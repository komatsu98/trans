
[Source](https://en.m.wikipedia.org/wiki/Design_by_contract "Permalink to Design by contract - Wikipedia")

# Thiết kế theo hợp đồng - Wikipedia

![][1]

Quy trình thiết kế theo hợp đồng

**Thiết kế theo hợp đồng** (**DbC**), còn được biết đến như là **lập trình hợp đồng**, **lập trình theo hợp đồng** hay **lập trình thiết-kế-theo-hợp-đồng**, là một hướng tiếp cận cho việc thiết kế [phần mềm][2]. Kĩ thuật này quy định rằng các nhà thiết kế nên xác định [formal][3], các thông số kĩ thuật giao diện chính xác và có thể kiểm chứng được cho [các thành phần của phần mềm][4], mở rộng định nghĩa thông thường của [kiểu dữ liệu abstract][5] với [các điều kiện tiên quyết][6], [điều kiện sau][7] và [bất biến][8]. 	Các thông số kỹ thuật này được gọi là "hợp đồng", phù hợp với [phép ẩn dụ khái niệm][9] với các điều kiện và nghĩa vụ của hợp đồng kinh doanh. 

Cách tiếp cận DbC giả định tất cả các thành phần client gọi một hoạt động trên một thành phần server sẽ đáp ứng các điều kiện tiên quyết được xác định theo yêu cầu cho hoạt động đó. Trường hợp giả định này được coi là quá rủi ro (như trong server đa kênh hoặc server phân tán) thì phương pháp _ "[defensive design][10]" _ được thực hiện, nghĩa là một thành phần server kiểm tra (trước hoặc trong khi xử lý request của client) rằng tất cả các điều kiện tiên quyết có liên quan đều đúng và trả lời bằng thông báo lỗi phù hợp nếu không. 

## Nội dung

Ý tưởng trung tâm của DbC là một phép ẩn dụ về cách các phần tử của một hệ thống phần mềm cộng tác với nhau trên cơ sở _nghĩa vụ_ lẫn _lợi ích_ chung. Ẩn dụ xuất phát từ cuộc sống kinh doanh, nơi một "khách hàng" và "nhà cung cấp" đồng ý về "hợp đồng" xác định, ví dụ,: 

* Nhà cung cấp phải cung cấp một sản phẩm nhất định (nghĩa vụ) và có quyền mong đợi rằng khách hàng thanh toán phí (lợi ích) của mình.
* Khách hàng phải trả phí (nghĩa vụ) và được quyền nhận sản phẩm (lợi ích).
* Cả hai bên phải đáp ứng các nghĩa vụ nhất định, chẳng hạn như luật và quy định, áp dụng cho tất cả các hợp đồng.

Tương tự, nếu một thường trình từ [class][11] trong [lập trình hướng đối tượng][12] cung cấp một chức năng nhất định, nó có thể:

* Mong đợi một điều kiện nhất định được đảm bảo khi nhập vào bởi bất kỳ mô-đun khách hàng nào gọi nó là: điều kiện tiên quyết [6] — nghĩa vụ cho khách hàng, và lợi ích cho nhà cung cấp (bản thân thường trình), vì nó giải phóng nó để xử lý các trường hợp ngoài điều kiện tiên quyết.
* Đảm bảo một tài sản nhất định khi thoát ra: [điều kiện tiên quyết][7] của thường trình — nghĩa vụ cho nhà cung cấp, và rõ ràng là một lợi ích (lợi ích chính của việc gọi thường trình) cho khách hàng.
* Duy trì một tài sản nhất định, giả định khi vào và được bảo đảm khi thoát: [class bất biến][13].

Hợp đồng tương đương về mặt ngữ nghĩa với [Hoare triple] [14], hợp thức hóa các nghĩa vụ. Điều này có thể được tóm tắt bằng "ba câu hỏi" mà nhà thiết kế phải nhiều lần trả lời trong hợp đồng:

* Hợp đồng kỳ vọng điều gì?
* Hợp đồng đảm bảo điều gì?
* Hợp đồng duy trì điều gì?

Nhiều [ngôn ngữ lập trình][15] có cơ sở để tạo ra [assertions][16] như thế này. Tuy nhiên, DbC coi các hợp đồng này là rất quan trọng đối với [tính chính xác phần mềm][17] rằng chúng phải là một phần của quá trình thiết kế. Trong thực tế, DbC chủ trương viết các xác nhận đầu tiên. Hợp đồng có thể được viết bởi [code comments][18], được thi hành bởi [test suite][19], hoặc cả hai, ngay cả khi không có hỗ trợ ngôn ngữ đặc biệt cho các hợp đồng.

Khái niệm về hợp đồng kéo dài xuống mức phương thức/thủ tục; hợp đồng cho mỗi method thường sẽ chứa các thông tin sau:[[_trích dẫn cần thiết][20]_]

* Các giá trị hoặc kiểu input được chấp nhận hoặc không được chấp nhận và ý nghĩa của chúng
* Trả lại giá trị hoặc kiểu và ý nghĩa của chúng
* Lỗi và [exception] [21] giá trị điều kiện hoặc kiểu có thể xảy ra và ý nghĩa của chúng
* [Tác dụng phụ][22]
* [Điều kiện tiên quyết][6]
* [Điều kiện sau][7]
* [Bất biến][8]
* (hiếm khi hơn) Đảm bảo hiệu suất, ví dụ: cho thời gian hoặc không gian được sử dụng

Các lớp con trong [phân cấp thừa kế][23] được phép làm suy yếu các điều kiện tiên quyết (nhưng không tăng cường chúng) và tăng cường các điều kiện và bất biến (nhưng không làm suy yếu chúng). Những quy tắc này gần đúng [subtyping behaviory][24].

Tất cả các mối quan hệ class là giữa các class client và các class cung cấp. Một class client có nghĩa vụ thực hiện các lời gọi đến các tính năng của nhà cung cấp, nơi trạng thái kết quả của nhà cung cấp không bị vi phạm bởi lời gọi của khách hàng. Sau đó, nhà cung cấp có nghĩa vụ cung cấp trạng thái trả lại và dữ liệu không vi phạm các yêu cầu của trạng thái của khách hàng. Ví dụ, bộ đệm dữ liệu của nhà cung cấp có thể yêu cầu dữ liệu đó có trong bộ đệm khi một tính năng xóa được gọi. Sau đó, nhà cung cấp đảm bảo cho khách hàng rằng khi một tính năng xóa kết thúc công việc của nó, mục dữ liệu sẽ, thực sự, sẽ bị xóa khỏi bộ đệm. Các hợp đồng thiết kế khác là các khái niệm về "bất biến lớp". Lớp bảo đảm bất biến (đối với lớp địa phương) rằng trạng thái của lớp sẽ được duy trì trong các dung sai được chỉ định ở cuối mỗi thực thi tính năng.

Khi sử dụng hợp đồng, nhà cung cấp không nên cố gắng xác minh rằng các điều kiện hợp đồng được thỏa mãn; ý tưởng chung là code nên "thất bại", với xác minh hợp đồng là mạng lưới an toàn. Thuộc tính "fail hard" của DbC đơn giản hoá việc gỡ rối hành vi hợp đồng, vì hành vi dự định của mỗi thường trình được xác định rõ ràng. Điều này phân biệt nó một cách rõ rệt từ một thực hành liên quan được gọi là [defensive programming][25], nơi mà nhà cung cấp chịu trách nhiệm tìm ra những việc cần làm khi điều kiện tiên quyết bị phá vỡ. Thường xuyên hơn không, nhà cung cấp ném một ngoại lệ để thông báo cho khách hàng rằng điều kiện tiên quyết đã bị phá vỡ, và trong cả hai trường hợp - DbC và defensive programming — khách hàng phải tìm ra cách để đáp ứng điều đó. DbC giúp công việc của nhà cung cấp dễ dàng hơn. 

Thiết kế theo hợp đồng cũng xác định tiêu chí cho tính chính xác cho một module phần mềm:

* Nếu lớp bất biến và điều kiện tiên quyết là đúng trước khi một nhà cung cấp được gọi bởi một khách hàng, sau đó là bất biến và điều kiện sau sẽ là đúng sau khi dịch vụ đã được hoàn thành.
* Khi thực hiện cuộc gọi đến nhà cung cấp, mô-đun phần mềm không được vi phạm các điều kiện tiên quyết của nhà cung cấp.

Thiết kế theo hợp đồng cũng có thể tạo điều kiện tái sử dụng mã, vì hợp đồng cho từng đoạn mã được ghi chép đầy đủ. Các hợp đồng cho một mô-đun có thể được coi là một dạng [tài liệu phần mềm][26] cho hành vi của mô-đun đó.

Các điều kiện hợp đồng sẽ không bao giờ bị vi phạm trong khi thực hiện chương trình không có lỗi. Các hợp đồng do đó thường chỉ được kiểm tra trong chế độ gỡ lỗi trong quá trình phát triển phần mềm. Sau khi phát hành, kiểm tra hợp đồng bị vô hiệu hóa để tối đa hóa hiệu suất.

Trong nhiều ngôn ngữ lập trình, các hợp đồng được thực hiện với [assert][16]. Assert được mặc định biên dịch trong chế độ phát hành trong C/C ++, và tương tự như bị vô hiệu hóa trong C # [[8]][27] và Java. Điều này có hiệu quả loại bỏ các chi phí thời gian chạy của hợp đồng phát hành. 

Thiết kế theo hợp đồng không thay thế các chiến lược thử nghiệm thường xuyên, chẳng hạn như [kiểm tra đơn vị][28], [thử nghiệm tích hợp][29] và [thử nghiệm hệ thống][30]. Thay vào đó, nó bổ sung cho thử nghiệm bên ngoài với các phép kiểm thử nội bộ có thể được kích hoạt cho cả các thử nghiệm riêng biệt và trong mã sản xuất trong giai đoạn thử nghiệm. Lợi thế của tự kiểm tra nội bộ là chúng có thể phát hiện lỗi trước khi chúng tự biểu hiện dưới dạng kết quả không hợp lệ được khách hàng quan sát. Điều này dẫn đến phát hiện lỗi sớm hơn và cụ thể hơn.

Việc sử dụng các xác nhận có thể được coi là một dạng [test oracle] [31], một cách để kiểm tra thiết kế bằng cách thực hiện hợp đồng.

[1]: https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Design_by_contract.svg/220px-Design_by_contract.svg.png
[2]: https://en.m.wikipedia.org/wiki/Software "Software"
[3]: https://en.m.wikipedia.org/wiki/Formal_methods "Formal methods"
[4]: https://en.m.wikipedia.org/wiki/Component-based_software_engineering#Software_component "Component-based software engineering"
[5]: https://en.m.wikipedia.org/wiki/Abstract_data_type "Abstract data type"
[6]: https://en.m.wikipedia.org/wiki/Precondition "Precondition"
[7]: https://en.m.wikipedia.org/wiki/Postcondition "Postcondition"
[8]: /wiki/Invariant_(computer_science) "Invariant (computer science)"
[9]: https://en.m.wikipedia.org/wiki/Conceptual_metaphor "Conceptual metaphor"
[10]: https://en.m.wikipedia.org/wiki/Defensive_design "Defensive design"
[11]: /wiki/Class_(computer_programming) "Class (computer programming)"
[12]: https://en.m.wikipedia.org/wiki/Object-oriented_programming "Object-oriented programming"
[13]: https://en.m.wikipedia.org/wiki/Class_invariant "Class invariant"
[14]: https://en.m.wikipedia.org/wiki/Hoare_triple "Hoare triple"
[15]: https://en.m.wikipedia.org/wiki/Programming_language "Programming language"
[16]: /wiki/Assertion_(software_development) "Assertion (software development)"
[17]: /wiki/Correctness_(computer_science) "Correctness (computer science)"
[18]: /wiki/Comment_(computer_programming) "Comment (computer programming)"
[19]: https://en.m.wikipedia.org/wiki/Test_suite "Test suite"
[20]: https://en.m.wikipedia.org/wiki/Wikipedia%3ACitation_needed "Wikipedia:Citation needed"
[21]: https://en.m.wikipedia.org/wiki/Exception_handling "Exception handling"
[22]: /wiki/Side_effect_(computer_science) "Side effect (computer science)"
[23]: /wiki/Inheritance_(object-oriented_programming) "Inheritance (object-oriented programming)"
[24]: https://en.m.wikipedia.org/wiki/Liskov_substitution_principle "Liskov substitution principle"
[25]: https://en.m.wikipedia.org/wiki/Defensive_programming "Defensive programming"
[26]: https://en.m.wikipedia.org/wiki/Software_documentation "Software documentation"
[27]: https://en.m.wikipedia.org#cite_note-8
[28]: https://en.m.wikipedia.org/wiki/Unit_testing "Unit testing"
[29]: https://en.m.wikipedia.org/wiki/Integration_testing "Integration testing"
[30]: https://en.m.wikipedia.org/wiki/System_testing "System testing"
[31]: https://en.m.wikipedia.org/wiki/Test_oracle "Test oracle"

  