
[Source](https://developers.google.com/web/tools/chrome-user-experience-report/?authuser=2 "Permalink to Chrome User Experience Report  |  Tools for Web Developers  |  Google Developers")

# Báo cáo trải nghiệm người dùng Chrome  |  Công cụ cho web developer  |  Google Developers

![][1]

Báo cáo trải nghiệm người dùng Chrome cung cấp các số liệu về trải nghiệm người dùng về cách người dùng Chrome trong thực tế trải nghiệm các điểm đến phổ biến trên web.

## Phương pháp luận

Báo cáo trải nghiệm người dùng Chrome được hỗ trợ bởi việc đo lường số liệu trải nghiệm người dùng chính trên web công khai, được tổng hợp từ những người dùng đã chọn tham gia đồng bộ hóa lịch sử duyệt web của họ, chưa thiết lập mật khẩu đồng bộ hóa và có [báo cáo thống kê sử dụng] [2] được bật. Dữ liệu kết quả được cung cấp qua:

1. [PageSpeed Insights][3], cung cấp số liệu trải nghiệm người dùng ở cấp URL cho các URL phổ biến được trình thu thập dữ liệu web của Google biết đến.
2. [Dự án Public Google BigQuery][4], tổng hợp các số liệu trải nghiệm người dùng theo nguồn gốc, cho tất cả các nguồn được trình thu thập thông tin web của Google biết và phân tách trên nhiều thứ nguyên được nêu bên dưới.

### Các số liệu

Số liệu được cung cấp bởi Báo cáo trải nghiệm người dùng Chrome công khai được lưu trữ trên Google BigQuery được cung cấp bởi các API nền tảng web tiêu chuẩn được các trình duyệt hiện đại hiển thị và được tổng hợp theo độ phân giải gốc. Chủ sở hữu trang web muốn phân tích chi tiết hơn (độ phân giải cấp độ URL) và hiểu rõ hơn về hiệu suất trang web của họ và có thể sử dụng cùng một API để thu thập dữ liệu đo lường thực tế chi tiết (RUM) cho các nguồn của riêng họ.

**Lưu ý:** Hiện tại, Báo cáo trải nghiệm người dùng Chrome tập trung vào hiệu suất tải. Theo thời gian, chúng tôi hy vọng sẽ thêm nhiều số liệu và thứ nguyên hơn, cả hai đều cung cấp thông tin chi tiết hơn về tải và [các yếu tố quan trọng khác ảnh hưởng nhiều nhất đến trải nghiệm người dùng] [5].

Để được hướng dẫn về số liệu nào cần theo dõi và tối ưu hóa và các phương pháp hay nhất về cách diễn giải dữ liệu đo lường của người dùng thực, hãy tham khảo tài liệu [tâm điểm người dùng] [5] của chúng tôi.

#### First Paint

Được miêu tả bởi [Paint Timing API][6] và [khả dụng trên Chrome M60+][7]:

> "First Paint báo cáo thời gian khi trình duyệt lần đầu được hiển thị sau khi điều hướng. Điều này loại trừ sơn nền mặc định, nhưng bao gồm sơn nền không mặc định. Đây là thời điểm quan trọng đầu tiên mà các developer quan tâm đến khi tải trang - khi trình duyệt bắt đầu hiển thị trang."

#### First Contentful Paint

Được định nghĩa bởi [Paint Timing API][6] và [khả dụng trên Chrome M60+][7]:

> "First Contentful Paint báo cáo thời gian khi trình duyệt hiển thị bất kỳ văn bản, hình ảnh nào (bao gồm hình nền), canvas không phải màu trắng hoặc SVG. Điều này bao gồm văn bản với webfonts đang chờ xử lý. Đây là thời điểm đầu tiên người dùng có thể bắt đầu sử dụng nội dung trang."

#### DOMContentLoaded

Được miêu tả bởi [Đặc tả HTML][9]:

> "DOMContentLoaded báo cáo thời gian khi tài liệu HTML ban đầu đã được tải và phân tích cú pháp hoàn toàn, mà không cần đợi các stylesheet, hình ảnh và các subframe để hoàn tất việc tải." - [MDN][10].

#### onload

Được miêu tả bởi [Đặc tả HTML][11]:

> "Sự kiện tải được kích hoạt khi trang và các tài nguyên phụ thuộc của nó tải xong." - [MDN][12].

### Dimensions

Hiệu suất của nội dung web có thể thay đổi đáng kể dựa trên loại thiết bị, thuộc tính của mạng và các biến khác. Để giúp phân đoạn và hiểu trải nghiệm người dùng trên các phân đoạn chính như vậy, Báo cáo trải nghiệm người dùng Chrome cung cấp các thứ nguyên sau.

#### Loại kết nối hiệu quả

Được miêu tả bởi [Network Information API][13] và [khả dụng trên Chrome M62+][14]:

> "Cung cấp loại kết nối hiệu quả ("2g chậm", "2g", "3g", "4g" hoặc "ngoại tuyến") được xác định bằng giá trị vòng và băng thông dựa trên các quan sát đo lường thực tế của người dùng."

#### Loại thiết bị

Phân loại thiết bị thô ("điện thoại", "máy tính bảng" hoặc "máy tính để bàn"), như [được kết nối thông qua User-Agent] [15].

#### Quốc gia

Vị trí địa lý của người dùng ở cấp quốc gia, được suy ra theo địa chỉ IP của họ. Các quốc gia được xác định theo [mã số ISO 3166-1 alpha-2 tương ứng] [16].

### Định dạng dữ liệu

Báo cáo được cung cấp qua [Google BigQuery] [17] dưới dạng tập hợp các tập dữ liệu chứa các số liệu trải nghiệm người dùng được tổng hợp thành độ phân giải gốc. Mỗi tập dữ liệu đại diện cho một quốc gia duy nhất, `country_rs` nắm bắt dữ liệu trải nghiệm người dùng cho người dùng ở Serbia (` rs` là mã [ISO 31611-1] [16] cho Serbia). Ngoài ra, có một tập dữ liệu tổng hợp toàn cầu (`all`) thu thập trải nghiệm trên toàn thế giới. Mỗi hàng trong tập dữ liệu chứa một bản ghi trải nghiệm người dùng lồng nhau cho một nguồn gốc cụ thể, được chia cho các thứ nguyên chính.

| ----- |
| Dimension |  
| `origin` |  "https://example.com" |  
| `effective_connection_type.name` |  4G |  
| `form_factor.name` |  "phone" |  
| `first_paint.histogram.start` |  1000 |  
| `first_paint.histogram.end` |  1200 |  
| `first_paint.histogram.density` |  0.123 | 

Ví dụ: ở trên cho thấy một bản ghi mẫu từ Báo cáo trải nghiệm người dùng Chrome, cho biết rằng 12,3% tải trang có đo lường "thời gian first paint" trong khoảng 1000-1200 mili giây khi tải "http://example.com "trên thiết bị" điện thoại "qua kết nối giống như" 4G ". Để có được giá trị tích lũy của người dùng trải qua thời gian sơn đầu tiên dưới 1200 mili giây, bạn có thể thêm tất cả các bản ghi có giá trị "end" của biểu đồ nhỏ hơn hoặc bằng 1200. 

**Lưu ý:** Báo cáo trải nghiệm người dùng Chrome không cung cấp giá trị định lượng (ví dụ: trung vị). Các giá trị như vậy có thể xấp xỉ từ dữ liệu được cung cấp, nhưng không được lộ ra trực tiếp bởi báo cáo.

## Bắt đầu

Báo cáo trải nghiệm người dùng Chrome được cung cấp dưới dạng dự án công khai trên [Google BigQuery] [17]. Để truy cập dự án, bạn cần có tài khoản Google và Dự án Google Cloud: [tham khảo hướng dẫn từng bước của chúng tôi] [18] và [hướng dẫn về cách truy vấn dự án] [19].

## Mẹo phân tích và các phương pháp hay nhất

### Xem xét sự khác biệt về sự tập trung trên nguồn

Các số liệu được cung cấp bởi Báo cáo trải nghiệm người dùng Chrome được cung cấp bởi dữ liệu đo lường người dùng thực. Kết quả là, dữ liệu phản ánh cách người dùng thực sự trải nghiệm nguồn truy cập và không giống như thử nghiệm tổng hợp hoặc thử nghiệm cục bộ nơi thử nghiệm được thực hiện trong điều kiện cố định và mô phỏng, nắm bắt đầy đủ các yếu tố bên ngoài hình thành và đóng góp cho trải nghiệm người dùng cuối cùng.

Ví dụ: sự khác biệt về lượng người dùng truy cập nguồn cụ thể có thể đóng góp những khác biệt có ý nghĩa cho trải nghiệm người dùng. Nếu trang web thường xuyên được nhiều khách truy cập hơn với các thiết bị hiện đại hơn hoặc thông qua mạng nhanh hơn, kết quả có thể xuất hiện "fast" ngay cả khi trang web không được tối ưu hóa tốt. Ngược lại, một trang web được tối ưu hóa tốt thu hút một lượng người dùng rộng hơn hoặc dân số có số lượng người dùng lớn hơn trên các thiết bị hoặc mạng chậm hơn, có thể xuất hiện "slow".

Khi thực hiện so sánh trực tiếp giữa các nguồn, điều quan trọng là phải tính toán và kiểm soát sự khác biệt về lượng người dùng: phân đoạn theo thứ nguyên được cung cấp, chẳng hạn như loại thiết bị và loại kết nối và xem xét các yếu tố bên ngoài như quy mô dân số, quốc gia mà từ đó nguồn gốc được truy cập, v.v.

### Xem xét sự khác biệt về quy mô tập trung trên nguồn

Báo cáo trải nghiệm người dùng Chrome tổng hợp dữ liệu cho mỗi nguồn, với các giá trị "mật độ" trên tất cả các biểu đồ thứ nguyên-số liệu tổng hợp với giá trị là "1.0". Điều này cung cấp thông tin chi tiết về phân phối trải nghiệm trên các thứ nguyên chính cho một nguồn duy nhất.

Tuy nhiên, khi tổng hợp dữ liệu từ nhiều nguồn, ví dụ trong ngành dọc hoặc khu vực địa lý, hãy cẩn thận với các loại kết luận được rút ra: việc tăng mật độ cho cùng một số liệu trên nhiều nguồn không tính đến sự khác biệt về sự tập trung tương đối trên nguồn.

Ví dụ: trang web A có thể có mười triệu khách truy cập, trong khi trang web B có mười nghìn. Trong cả hai trường hợp, mật độ biểu đồ cho mỗi tổng nguồn là "1.0" và tập dữ liệu không cung cấp bất kỳ số liệu tuyệt đối nào về quy mô tập trung của nguồn riêng lẻ hoặc sự khác biệt về kích thước sự tập trung tương đối trên nguồn. Kết quả là, nếu bạn cộng các mật độ từ A và B, và trung bình kết quả, bạn sẽ coi chúng là bằng nhau mặc dù A có ba đơn vị lưu lượng truy cập lớn hơn.

### Xem xét sự khác biệt về tập trung của Chrome

Báo cáo trải nghiệm người dùng Chrome được hỗ trợ bởi đo lường người dùng thực được tổng hợp từ những người dùng Chrome đã chọn tham gia đồng bộ hóa lịch sử duyệt web của họ, chưa thiết lập mật khẩu đồng bộ hóa và đã bật báo cáo thống kê sử dụng. Dân số này có thể không đại diện cho cơ sở người dùng rộng hơn cho một nguồn cụ thể và nhiều nguồn có thể có sự khác biệt về sự tập trung giữa chúng. Hơn nữa, dữ liệu này không tính đến người dùng có trình duyệt khác nhau và sự khác biệt trong việc chấp nhận trình duyệt ở các khu vực địa lý khác nhau.

Do đó, hãy cẩn thận với các loại kết luận được rút ra khi nhìn vào mặt cắt ngang của nguồn và khi so sánh nguồn gốc cá nhân: tránh sử dụng so sánh tuyệt đối và xem xét các yếu tố tập trung khác được nêu trong các phần ở trên.

## Phản hồi và đề xuất

Chúng tôi muốn nghe phản hồi, các câu hỏi và đề xuất của bạn để giúp chúng tôi cải thiện Báo cáo trải nghiệm người dùng Chrome. Vui lòng tham gia cuộc trò chuyện trên [Nhóm Google công khai của chúng tôi] [20].

## Giấy phép

Bộ dữ liệu "Báo cáo trải nghiệm người dùng Chrome" của Google được cấp phép theo [Creative Commons Attribution 4.0 International License] [21].

[1]: https://developers.google.com/web/tools/chrome-user-experience-report/images/dataset.png?authuser=2
[2]: https://www.google.com/chrome/browser/privacy/whitepaper.html?authuser=2#usagestats
[3]: https://developers.google.com/speed/pagespeed/insights/?authuser=2
[4]: https://bigquery.cloud.google.com/dataset/chrome-ux-report:all?authuser=2
[5]: https://developers.google.com/web/updates/2017/06/user-centric-performance-metrics?authuser=2
[6]: https://w3c.github.io/paint-timing/#first-paint
[7]: https://www.chromestatus.com/feature/5688621814251520
[8]: https://w3c.github.io/paint-timing/#first-contentful-paint
[9]: https://html.spec.whatwg.org/#event-domcontentloaded
[10]: https://developer.mozilla.org/en-US/docs/Web/Events/DOMContentLoaded
[11]: https://html.spec.whatwg.org/#event-load
[12]: https://developer.mozilla.org/en-US/docs/Web/Events/load
[13]: https://wicg.github.io/netinfo/#dfn-effective-connection-types
[14]: https://www.chromestatus.com/feature/5108786398232576
[15]: https://developer.chrome.com/multidevice/user-agent
[16]: https://en.wikipedia.org/wiki/ISO_3166-1#Officially_assigned_code_elements
[17]: https://cloud.google.com/bigquery/?authuser=2
[18]: https://developers.google.com/web/tools/chrome-user-experience-report/getting-started?authuser=2#access-dataset
[19]: https://developers.google.com/web/tools/chrome-user-experience-report/getting-started?authuser=2#example-queries
[20]: https://groups.google.com/a/chromium.org/forum/?authuser=2#!forum/chrome-ux-report
[21]: https://creativecommons.org/licenses/by/4.0/

  