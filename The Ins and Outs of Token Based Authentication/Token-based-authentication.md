# The Ins and Outs of Token Based Authentication ― Scotch

## Giới thiệu

Token based authentication nổi lên ở khắp nơi trên web  ngày nay. Với hầu hết các công ty web sử dụng API, các token là cách tốt nhất để xử lý xác thực cho đa người dùng. 

Có một vài yếu tố rất quan trọng khi lựa chọn token based authentication cho ứng dụng của bạn. Lý do chính cho token là:

Server không có trạng thái và có thể mở rộng

Sẵn sàng tích hợp với ứng dụng di động

Truyền xác thực tới các ứng dụng khác

Bảo mật tốt

## Ai sử dụng Token Based Authentication?

Bất kỳ hệ thống lớn API hoặc ứng dụng web nào mà bạn đã gặp phải hầu hết đều sử dụng token. Các ứng dụng như Facebook, Twitter, Google+, GitHub và rất nhiều khác nữa sử dụng token.

Hãy cùng xem nó hoạt động như nào.

## Why Tokens Came Around

Trước khi chúng ta có thể thấy cách Token based authentication hoạt động và lợi ích của nó, ta phải xem xét cách mà xác thực đã được thực hiện trong quá khứ.

### Server Based Authentication (Phương pháp truyền thống)

> Bởi giao thức HTTP là _không có trạng thái_, điều này có nghĩa là nếu chúng ta xác thực người dùng bằng username và password, thì ở request tiếp theo, ứng dụng sẽ không biết ta là ai. Dó đó ta phải xác thực lại.

Cách truyền thống để ứng dụng nhớ chúng tôi là ai đó là ** lưu trữ thông tin người dùng đăng nhập  trên máy chủ **. Điều này có thể được thực hiện theo một vài cách khác nhau trong session, thường là trong bộ nhớ hoặc được lưu trữ trên đĩa.

Đây là biểu đồ về quy trình làm việc của Server Based Authentication:

![tokens-traditional](https://cask.scotch.io/2014/11/tokens-traditional.png)

Khi web, ứng dụng và sự nổi lên của ứng dụng di động xuất hiện, phương pháp xác thực này đã cho thấy các vấn đề, đặc biệt là trong khả năng mở rộng.

## Các vấn đề với Server Based Authentication

Một số vấn đề lớn nảy sinh với phương pháp xác thực này.

**Sessions**: Mỗi khi người dùng được xác thực, máy chủ sẽ cần phải tạo bản ghi ở đâu đó trên máy chủ. Điều này thường được thực hiện trong bộ nhớ và khi có nhiều người dùng xác thực, chi phí trên máy chủ của bạn tăng lên.

**Khả năng mở rộng**: Vì các phiên được lưu trữ trong bộ nhớ, điều này tạo ra các vấn đề với khả năng mở rộng. Khi các nhà cung cấp dịch vụ đám mây của chúng tôi bắt đầu sao chép các máy chủ để xử lý load ứng dụng, việc có các thông tin quan trọng trong bộ nhớ session sẽ hạn chế khả năng mở rộng của chúng tôi.

**CORS**: Vì chúng tôi muốn mở rộng ứng dụng của mình để cho phép dữ liệu của chúng tôi được sử dụng trên nhiều thiết bị di động, chúng tôi phải lo lắng về việc chia sẻ tài nguyên gốc (CORS). Khi sử dụng các lần gọi AJAX để lấy các tài nguyên từ một domain khác (điện thoại di động đến máy chủ API của chúng tôi), chúng tôi có thể gặp sự cố với các yêu cầu bị cấm.
**CSRF**: Chúng tôi cũng sẽ có được sự bảo vệ chống lại [cross-site request forgery] [1] (CSRF). Người dùng dễ bị tấn công CSRF vì họ có thể đã được xác thực với một trang web ngân hàng và điều này có thể được tận dụng khi truy cập các trang web khác.

Với những vấn đề này thì khả năng mở rộng là vấn đề chính, bạn nên thử một cách tiếp cận khác.

## Token Based hoạt động như thế nào?

Token based authentication là **không trạng thái**. Chúng tôi không lưu trữ bất kỳ thông tin nào về người dùng trên máy chủ hoặc trong một session.

Khái niệm này sẽ một mình xử lý nhiều vấn đề với việc phải lưu trữ thông tin trên máy chủ.

> Không có thông tin session có nghĩa là ứng dụng của bạn có thể mở rộng quy mô và thêm nhiều máy hơn nếu cần mà không phải lo lắng về nơi người dùng đăng nhập.

Mặc dù việc triển khai này có thể thay đổi, nhưng luôn gồm các ý chính sau:

1. Người dùng yêu cầu truy cập với Username / Password
2. Ứng dụng xác thực thông tin đăng nhập
3. Ứng dụng cung cấp token đã được ký cho client
4. Client lưu trữ token đó và gửi nó theo cùng mỗi request 
5. Server xác thực token và phản hồi lại với data 

** Mỗi request sẽ yêu cầu token **. Token này sẽ được gửi trong HTTP header để chúng tôi tiếp tục với ý tưởng về các request HTTP không trạng thái. Chúng ta cũng sẽ cần thiết lập máy chủ của mình để chấp nhận các request từ tất cả các miền bằng cách sử dụng `Access-Control-Allow-Origin: *`. Điều thú vị về việc chỉ định `*` trong tiêu đề ACAO là nó không cho phép yêu cầu cung cấp thông tin xác thực như xác thực HTTP, chứng chỉ SSL phía client hay cookie.

Đây là một hình ảnh minh họa để giải thích quá trình:

![tokens-new](https://cask.scotch.io/2014/11/tokens-new.png)

Khi chúng tôi đã xác thực thông tin của mình và có token, chúng tôi có thể thực hiện nhiều việc với token này.

Chúng tôi thậm chí có thể tạo một giấy phép based token và truyền nó cùng với ứng dụng của bên thứ ba (chẳng hạn một ứng dụng dành cho thiết bị di động mới mà chúng tôi muốn sử dụng) và họ sẽ có thể truy cập dữ liệu của chúng tôi - ** nhưng chỉ có thông tin chúng tôi đã cho phép với token cụ thể đó **.

## Các lợi ích của Tokens

### Không trạng thái và có thể mở rộng

Các token được lưu trữ ở phía client. Hoàn toàn không có trạng thái và sẵn sàng để được thu nhỏ. Cân bằng tải của chúng tôi có thể chuyển người dùng đến bất kỳ máy chủ nào của chúng tôi vì không có thông tin trạng thái hoặc phiên nào ở bất kỳ đâu.

Nếu chúng tôi giữ thông tin về session trên người dùng đã đăng nhập, điều này sẽ yêu cầu chúng tôi tiếp tục gửi người dùng đó đến máy chủ _họ đã đăng nhập_ (được gọi là tính liên thông session).

Điều này đem đến vấn đề kể từ việc một số người dùng sẽ bị buộc phải cùng một máy chủ và điều này có thể mang lại một điểm lưu lượng truy cập lớn.

Đừng lo lắng! Những vấn đề đó đã biến mất với token vì bản thân token mạng dữ liệu cho người dùng đó.

### Bảo mật

Token, không phải cookie, được gửi theo mọi request và vì không có cookie nào được gửi, điều này giúp ngăn chặn các cuộc tấn công CSRF. Ngay cả khi việc triển khai cụ thể của bạn lưu trữ token trong một cookie ở phía client thì cookie cũng chỉ là một cơ chế lưu trữ thay vì một cơ chế xác thực. Không có thông tin dựa trên session để thao tác vì chúng tôi không có session!

Token cũng hết hạn sau một khoảng thời gian nhất định, do đó, người dùng sẽ được yêu cầu đăng nhập lại một lần nữa. Điều này giúp chúng tôi luôn an toàn. Ngoài ra còn có khái niệm về [thu hồi token] [2] cho phép chúng tôi vô hiệu hóa token cụ thể và thậm chí là một nhóm token dựa trên cùng một sự cho phép.

### Khả năng mở rộng (Bạn của bạn và Sự cho phép)

Tokens sẽ cho phép chúng ta xây dựng các ứng dụng chia sẻ quyền với nhau. Ví dụ: chúng tôi đã liên kết các tài khoản xã hội ngẫu nhiên với các tài khoản chính của chúng tôi như Facebook hoặc Twitter.

Khi chúng tôi đăng nhập vào Twitter thông qua một dịch vụ (hãy gọi nó là Buffer), chúng tôi đang cho phép Buffer đăng lên luồng Twitter của chúng tôi.

Bằng cách sử dụng token, đây là cách chúng tôi ** cung cấp quyền chọn lọc cho các ứng dụng của bên thứ ba **. Chúng tôi thậm chí có thể xây dựng API của riêng mình và cung cấp các token quyền đặc biệt nếu người dùng của chúng tôi muốn cấp quyền truy cập vào dữ liệu của họ cho một ứng dụng khác.

### Đa nền tảng và domain

Chúng tôi đã nói một chút về CORS trước đó. Khi ứng dụng và dịch vụ của chúng tôi mở rộng, chúng tôi sẽ cần cung cấp quyền truy cập vào tất cả các loại thiết bị và ứng dụng (vì ứng dụng của chúng tôi chắc chắn sẽ trở nên phổ biến!).

Khi API của chúng tôi chỉ phân phối dữ liệu, chúng tôi cũng có thể đưa ra lựa chọn thiết kế để phân phát nội dung từ CDN. Điều này giúp loại bỏ các vấn đề mà CORS mang lại sau khi chúng tôi thiết lập cấu hình tiêu đề nhanh cho ứng dụng của chúng tôi.
    
    
    Access-Control-Allow-Origin: *
    

Dữ liệu và tài nguyên của chúng tôi sẵn có cho các yêu cầu từ bất kỳ miền nào ngay bây giờ ** miễn là người dùng có token hợp lệ **.

### Standards Based

Khi tạo token, bạn có một vài tùy chọn. Chúng ta sẽ đi sâu hơn vào chủ đề này khi chúng ta bảo mật một API trong một bài viết tiếp theo, nhưng tiêu chuẩn để sử dụng sẽ là [JSON Web Tokens] [3].

Biểu đồ thư viện và trình gỡ lỗi tiện dụng này hiển thị hỗ trợ cho các JSON Web Tokens. Bạn có thể thấy rằng nó có một số lượng lớn hỗ trợ trên nhiều ngôn ngữ. Điều này có nghĩa là bạn có thể thực sự chuyển đổi cơ chế xác thực của mình nếu bạn chọn làm như vậy trong tương lai!

## Kết luận

Đây chỉ là xem xét cách thức và lý do của token based authentication. Như mọi khi trong thế giới an ninh, có nhiều, nhiều, nhiều, nhiều (quá nhiều?) Nhiều hơn cho mỗi chủ đề và nó thay đổi theo từng ca sử dụng. Thậm chí, chúng tôi cũng nghiên cứu một số chủ đề về khả năng mở rộng xứng đáng với cuộc trò chuyện của riêng họ.

Đây là một tổng quan nhanh ở mức độ cao, vì vậy xin vui lòng chỉ ra bất cứ điều gì đã bị bỏ lỡ hoặc bất kỳ câu hỏi nào bạn có về vấn đề này.
