## Javascript: Execution Context là gì?Call Stack là gì?

**Execution Context in Javascript** là gì?

Tôi cá là bạn không biết câu trả lời.

Những thành phần nào là cơ bản nhất của một ngôn ngữ lập trình?

Các biến và hàm đúng không? Ai cũng có thể học những điều này.

Nhưng thứ gì ở xa hơn cả những điều cơ bản?

**Những nền tảng của Javascript** nào mà bạn nên làm chủ trước khi xem mình là một lập trình viên javascript trung cấp (hoặc thậm chí cao cấp)?

Có nhiều thứ như vậy: Scope, Closure, Callbacks, Prototype,...

Nhưng trước khi đi vào sâu hơn vào các khái niệm, ít nhất bạn nên hiểu được  **Javascript engine hoạt động như thế nào**.

Trong bài viết này ta sẽ tìm hiểu xung quanh 2 mảnh ghép cơ bản của bất cứ js engine nào: **Execution Context và the Call Stack**.

(Đừng sợ. Nó dễ dàng hơn bạn tưởng).

Sẵn sàng chưa?

Bài này là một phần của [Advanced Javascript class available both as remote 1 to 1 training](https://www.valentinog.com/) hoặc ở [on site training in Europe](https://www.servermanaged.it/formazione-javascript-react.html).

## Javascript: Execution Context là gì? Bạn sẽ học được gì?

Trong bài viết này bạn sẽ học được:

*   Javascript Engine hoạt động như thế nào
*   Execution Context trong Javascript
*   Call Stack là gì
*   sự khác biệt giữa Global Execution Context và Local Execution Context

## Javascript: Execution Context là gì? Javascript chạy Code của bạn như thế nào?

Javascript chạy Code của bạn như thế nào?

Nếu bạn là một lập trình viên cao cấp thì có thể bạn đã biết câu trả lời là gì.

Nếu bạn là người mới, chúng ta sẽ cùng tìm hiểu nó.

Sự thật là, bên trong Javascript không hề đơn giản.

Nhưng tôi đảm bảo bạn có thể học chúng.

Và ngay khi học chúng, bạn sẽ cảm thấy tự tin và thông minh hơn.

Bằng cách xem xét chức năng bên trong Javascript, bạn sẽ trở thành một nhà phát triển Javascript tốt hơn, ngay cả khi bạn không thể nắm vững từng chi tiết.

Bây giờ, hãy xem đoạn code sau:

<div class="highlight">

    var num = 2;

    function pow(num) {
        return num * num;
    }

</div>

Xong?

Nó không khó lắm nhỉ !

Giờ cho tôi biết: **browser sẽ định thứ tự như thế nào cho đọan code đó**?

Nói cách khác, nếu BẠN là browser, bạn sẽ đọc đoạn code đó thế nào?

Nghe có vẻ khả đơn giản.

Hầu hết mọi người nghĩ rằng "yeah , browser sẽ thực hiện hàm pow và trả về kết quả, sau đó gán 2 cho num."

Bạn muốn biết câu trả lời của các học trò tôi không?

_Trên xuống dưới_

_Browser sẽ bắt đầu tại hàm pow, tính toán num*num_

_JS engine sẽ chạy code dòng này sang dòng khác_ (đại loại thế)

Tôi đã mong đợi điều đó.

Tôi đã nói chính xác những điều như vậy nhiều năm trước.

Trong phần tiếp theo, bạn sẽ khám phá ra cơ quan bộ máy phía sau những **đoạn code tưởng chừng như đơn giản**.

## Javascript: Execution Context là gì? Các Javascript Engine

Bạn có muốn trở thành một nhà phát triển Javascript trung bình không?

Tôi cá là bạn sẽ không.

Nếu bạn muốn tạo ấn tượng tốt trong một cuộc phỏng vấn Javascript, ít nhất bạn nên biết **cách Javascript chạy code của bạn**.

(Và một loạt các thứ khác mà tôi sẽ không bao gồm ở đây).

Đừng vội lao vào những khái niệm này.

Bạn không thể học mọi thứ trong một ngày. Nó sẽ cần thời gian.

Tin tốt đây? Tôi sẽ làm cho mọi thứ dễ hiểu đối với mọi người (ít nhất tôi sẽ thử).

Để hiểu cách Javascript chạy code của bạn, chúng ta phải đối mặt điều đáng sợ đầu tiên: **Execution Context**.

Execution Context trong Javascript là gì?

**Mỗi khi bạn chạy Javascript trong browser** (hoặc trong Node) **engine thực hiện chuỗi các bước**.

Một trong những bước này liên quan đến việc **tạo ra Global Execution Context**.

Nhưng đợi đã, **engine là gì**?

Chính nó, Javascript engine là "engine" để chạy code Javascript.

Ngày nay có hai Javascript engine nổi bật: **Google V8** và **SpiderMonkey**.

V8 là công cụ JavaScript mã nguồn mở của Google, được sử dụng trong Google Chrome và Node.js.

SpiderMonkey là công cụ JavaScript của Mozilla, được sử dụng trong Firefox.

Như vậy chúng ta có Javascript engine và Execution Context.

Bây giờ là lúc để hiểu cách chúng làm việc với nhau.

## Javascript: Execution Context là gì? Nó hoạt động thế nào?

**Engine tạo ra Global Execution Context** mỗi khi bạn chạy Javascript code.

Execution Context là một từ ưa thích để mô tả môi trường mà Javascript code của bạn chạy.

Thật khó để hình dung những điều trừu tượng này, tôi thông cảm với bạn.

Bây giờ hãy nghĩ về Global Execution Context như một cái hộp:

[![Global Execution Context](https://res.cloudinary.com/practicaldev/image/fetch/s--jWWiNqCD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ptgf765a4989zd6twwfp.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--jWWiNqCD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ptgf765a4989zd6twwfp.png)

Hãy cùng nhìn lại đoạn code của chúng ta:

<div class="highlight">

    var num = 2;

    function pow(num) {
        return num * num;
    }

</div>

Engine đọc đoạn code đó thế nào?

Đây là một phiên bản đơn giản:

_**Engine**: Dòng 1. Có một biến! Tuyệt. Hãy lưu trữ nó trong bộ Global Memory._

_**Engine**: Dòng 3. Tôi thấy một khai báo hàm. Tuyệt. Hãy lưu trữ nó trong Global Memory!_

_**Engine**: Có vẻ như tôi đã xong._

Nếu tôi hỏi lại bạn: browser "xem" đọan code sau như thế nào, bạn sẽ nói gì?

Ừm, trong như là từ trên xuống dưới nhưng ...

Như bạn có thể thấy engine không chạy hàm pow!

Đó là một **khai báo hàm**, chứ không phải một lời gọi hàm.

Đoạn code trên sẽ dịch thành một số giá trị được lưu trong **Global Memory**: một khai báo hàm và một biến.

**Global Memory**?

Valentino, tôi đã mơ hồ với Execution Context và bây giờ bạn đang ném Global Memory vào tôi?

Vâng là tôi.

Hãy xem Global Memory là gì.

## Javascript: Execution Context là gì? The Global Memory

**Javascript engine cũng có một Global Memory**.

Global Memory chứa các biến toàn cục và khai báo hàm để sử dụng sau này.

Nếu bạn đọc "Scope and Closures" của Kyle Simpson, bạn có thể thấy rằng Global Memory trùng lặp với khái niệm Global Scope.

Trong thực tế, chúng là những điều tương tự.

Tôi đang bay cao 10.000 feet ở đây, vì một lý do chính đáng.

Đó là những khái niệm khó.

Nhưng bây giờ bạn không nên lo lắng.

Tôi muốn bạn hiểu hai phần quan trọng trong câu đố của chúng ta.

Khi công cụ Javascript chạy code của bạn, nó tạo ra:

* một Global Execution context
* một Global Memory (còn được gọi là Global Scope hoặc Global Variable Environment)

Mọi thứ đã rõ ràng chưa?

Nếu tôi là bạn vào thời điểm này tôi sẽ:

* viết một vài đoạn code Javascript
* phân tích code từng bước như bạn là engine
* tạo ra một biểu diễn đồ họa của cả Global Execution context và Global Memory trong quá trình thực hiện

Bạn có thể viết bài tập trên giấy hoặc bằng công cụ tạo mẫu.

Đối với ví dụ nhỏ của tôi, hình ảnh sẽ như sau:

[![Một biểu diễn đồ họa của Javascript Execution Context / Global Memory](https://res.cloudinary.com/practicaldev/image/fetch/s--Ae3LEIz8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/7sreqswm895lrphw4xy2.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Ae3LEIz8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/7sreqswm895lrphw4xy2.png)

Trong phần tiếp theo, chúng ta sẽ xem xét một điều đáng sợ khác: **Call Stack**.

## Javascript: Execution Context là gì? Call Stack là gì?

Bạn có hình dung rõ ràng về cách mà **Execution Context**, **Global Memory** và **Javascript engine ăn khớp với nhau** không?

Nếu không thì hãy dành thời gian của bạn để xem lại phần trước.

Chúng tôi sẽ giới thiệu một phần khác trong câu đố của chúng ta: **Call Stack**.

Hãy xem điều gì xảy ra trong quá trình thực thi code.

Trước tiên hãy tóm tắt lại những gì xảy ra khi Javascript engine chạy code của bạn.

Nó tạo ra:

* một Global Execution context
* một Global Memory

Ngoài ra trong ví dụ của chúng ta không có gì xảy ra nữa:

<div class="highlight">

    var num = 2;

    function pow(num) {
        return num * num;
    }

</div>

Đoạn code này là một phân bổ thuần túy các giá trị.

Hãy tiến thêm một bước nữa.

Điều gì xảy ra nếu tôi gọi hàm này?

<div class="highlight">

    var num = 2;

    function pow(num) {
        return num * num;
    }

    var res = pow(num);

</div>

Câu hỏi thú vị.

Hành động **gọi một hàm trong Javascript khiến engnie yêu cầu trợ giúp**.

Và sự trợ giúp đó đến từ một người bạn của Javascript engine: **Call Stack**.

Nó có thể không rõ ràng nhưng **Javascript engine cần phải theo dõi những gì đang xảy ra**.

Nó dựa trên Call Stack để làm điều đó.

Call Stack trong Javascript là gì?

**Call Stack giống như một bản ghi thực thi hiện tại của chương trình**.

Trong thực tế nó là một cấu trúc dữ liệu: [một stack] (https://en.wikipedia.org/wiki/Stack_ (abstract_data_type)).

Chính xác thì Call Stack hoạt động như thế nào?

Không có gì đáng ngạc nhiên khi nó có hai method: **push** và **pop**.

**Pushing** là hành động **đưa một cái gì đó vào stack**.

Tức là, **khi bạn chạy một hàm trong Javascript, engine sẽ đẩy hàm đó vào trong Call Stack**.

Mọi lời gọi hàm đều được push vào Call Stack.

**Điều đầu tiên được push là main()** (hoặc global()), luồng chính của việc thực thi chương trình Javascript của bạn.

Bây giờ, hình ảnh trước sẽ trông như sau:

[![Javascript Call Stack](https://res.cloudinary.com/practicaldev/image/fetch/s--u-ktmKSh--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ww11po2kybdij4zino4r.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--u-ktmKSh--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ww11po2kybdij4zino4r.png)

**Popping** ở đầu bên kia là hành động **xóa thứ gì đó khỏi stack**.

Khi một hàm kết thúc thực thi nó sẽ được pop khỏi Call Stack.

Và Call Stack của chúng ta sẽ giống như sau:

[![Javascript Call Stack Pop](https://res.cloudinary.com/practicaldev/image/fetch/s--XzP4eR0c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/2zbqkyan7uu67xrhpx1b.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--XzP4eR0c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/2zbqkyan7uu67xrhpx1b.png)

Và bây giờ bạn đã sẵn sàng để làm chủ mọi khái niệm Javascript ngoài kia.

Nhưng chưa xong! Hãy chuyển đến phần tiếp theo!

## Advanced Javascript: Execution Context là gì? Local Execution Context

Mọi thứ dường như rõ ràng cho đến lúc này.

Chúng ta có đang thiếu một cái gì đó?

Chúng ta biết rằng **Javascript engine tạo ra một Global Execution Context và Global Memory**.

Sau đó, khi bạn gọi một hàm trong code của bạn:

* Javascript engine yêu cầu trợ giúp
* sự trợ giúp đó đến từ một người bạn của công cụ Javascript: **Call Stack**
* **Call Stack theo dõi các hàm đang được gọi** trong code của bạn

Tuy nhiên, **một điều khác đã xảy ra khi bạn chạy một hàm** trong Javascript.

Đầu tiên, **hàm xuất hiện trong Global Execution Context**.

Sau đó, **một ngữ cảnh nhỏ khác xuất hiện cùng với hàm**.

Cái hộp nhỏ mới này được gọi là **Local Execution Context**.

Một Local Execution Context được tạo bên trong hàm đó!

Cái gì cơ??

Nếu bạn nhận thấy, trong hình trước đó một biến mới xuất hiện trong Global memory: **var _res _**.

Các biến **_ res _** có giá trị **không xác định lúc đầu**.

Sau đó, ngay sau **pow** xuất hiện trong **Global Execution Context, hàm thực thi và _res_ lấy giá trị trả về của nó**.

Trong suốt giai đoạn thực thi, một Local Execution Context được tạo ra, cùng với một Local Memory để giữ các biến cục bộ.

Quả là một khái niệm mạnh mẽ.

[![Local Execution Context](https://res.cloudinary.com/practicaldev/image/fetch/s--Zpkb7RDS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/8qoepadnbmo8alj8h0vi.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--Zpkb7RDS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/8qoepadnbmo8alj8h0vi.png)

Hãy ghi nhớ điều đó.

Hiểu cả **Global và Local Execution Context là chìa khóa để làm chủ Scope và Closures**.

## Javascript: Execution Context là gì? Call Stack là gì? Gói gọn lại

Bạn có thể tin những gì đằng sau 4 dòng code không?

Javascript engine tạo ra **Execution Context, Global Memory, và Call Stack**..

Nhưng một khi bạn gọi một hàm, engine sẽ tạo ra một **Local Execution Context có một Local Memory**.

Đến cuối bài viết này, bạn sẽ có thể hiểu những gì sẽ xảy ra khi bạn chạy một số Javascript code.

Thường bị bỏ qua, bên trong Javascript luôn được xem là những điều kì bí bởi các nhà phát triển mới.

Tuy nhiên, chúng là **chìa khóa để làm chủ các khái niệm Javascript nâng cao**.

Nếu bạn học được Execution Context, Global Memory, và Call Stack, **thì Sanpce, Closure, Callbacks và các nội dung khác sẽ dễ dàng hơn nhiều**.

Đặc biệt, sự hiểu biết Call Stack là tối quan trọng.

Tất cả các Javascript sẽ bắt đầu có ý nghĩa một khi bạn hình dung nó: cuối cùng bạn sẽ hiểu **tại sao Javascript là không đồng bộ** và tại sao chúng ta cần Callbacks.

Bạn có biết **những gì đằng sau 4 dòng code Javascript**?

Bây giờ bạn đã biết điều đó.

Cảm ơn vì đã đọc!
