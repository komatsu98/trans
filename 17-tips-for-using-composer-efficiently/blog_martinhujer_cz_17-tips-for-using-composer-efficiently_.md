#  [Martin Hujer blog](/)

## 17 lời khuyên để sử dụng Composer một cách hiệu quả

2018-01-05

Mặc dù hầu hết các lập trình viên PHP đều biết cách sử dụng Composer, nhưng không phải ai cũng dùng nó một cách hiệu quả hay hợp lý nhất.
Vì vậy tôi đã quyết định tóm tắt lại những thứ quan trọng cho quy trình làm việc hằng ngày của mình.

Luận điểm xuyên suốt phần lớn những lời khuyên trong bài này là _"Sử dụng nó một cách cẩn thận"_, nghĩa là nếu có nhiều cách để xử lí một vấn đề,
tôi sẽ dùng hướng tiếp cận mà ít có khả năng xảy ra lỗi nhất.


## Lời khuyên #1: Đọc tài liệu chính thức


Ý tôi chính là vậy đấy. [Tài liệu chính thức](https://getcomposer.org/doc/) rất tuyệt, chỉ với vài tiếng đọc nó có thể tiết kiệm cho bạn
rất nhiều thời gian về sau. Bạn sẽ bị ngạc nhiên về những gì Composer có thể làm được.


## Lời khuyên #2: Cảnh giác đối với sự khác nhau giữa một "project" và một "thư viện"


Quan trọng là bạn phải biết mình đang tạo ra một _"project"_ hay một _"thư viện"_. Mỗi thứ yêu cầu một tập kinh nghiệm, sự luyện tập tách biệt.

Thư viện là một package có thể tái sử dụng, bạn có thể thêm nó vào như một dependency - chẳng hạn như `symfony/symfony`, `doctrine/orm` hay
[`elasticsearch/elasticsearch`](https://github.com/elastic/elasticsearch-php).

Một project là một ứng dụng điển hình, dựa vào nhiều thư viện. Nó thường không thể tái sử dụng (không có project nào khác require nó như là một dependency).
Một vài ví dụ như website thương mại điện tử hay hệ thống hỗ trợ khách hàng...v.v

Tôi sẽ chỉ ra sự khác biệt giữa thư viện và project ở các lời khuyên bên dưới.


## Lời khuyên #3: Sử dụng các phiên bản dependency riêng biệt cho các ứng dụng

Nếu bạn đang tạo một ứng dụng, bạn nên sử dụng phiên bản chi tiết nhất để định nghĩa dependency. Nếu bạn cần phân tích các file YAML,
bạn nên viết chi tiết về dependency như thế này: `"symfony/yaml": "4.0.2"`.

Thậm chí thư viện sau [Semantic Versioning](https://semver.org/), có thể có những lỗi tương thích ngược trong các phiên bản thu nhỏ và đầy đủ.
Ví dụ như, nếu bạn đang sử dụng `"symfony/symfony": "^3.1"`, có thể có những thứ sẽ không còn dùng được nữa ở bản 3.2, điều đó có thể làm hỏng một số
trường hợp thử trong ứng dụng của bạn. Hoặc là có thể có một lỗi nào đó đã được sửa trong PHP_CodeSniffer và nó sẽ phát hiện các lỗi định dạng trong code của bạn,
và có thể dẫn đến hỏng cả thiết kế.

Bản cập nhật của các dependency nên được định trước, không phải tình cờ. Một trong số những lời khuyên bên dưới sẽ nói rõ hơn về điều này.

Điều này nghe có vẻ quá mức, nhưng nó sẽ ngăn những người đồng nghiệp của bạn khỏi việc cập nhật tất cả dependency một cách tình cờ khi thêm một thư viện mới vào project
(việc này có thể bạn sẽ k nhận ra khi xem xét lại code)

## Lời khuyên #4: Sử dụng các dãy phiên bản cho các thư viện dependency

Nếu bạn đang tạo một thư viện, bạn nên định nghĩa khoảng phiên bản rộng nhất có thể. Nếu bạn tạo ra một thư viện sử dụng thư viện `symfony/yaml` để phân tích YAML, bạn nên require nó như sau:

    
    
    "symfony/yaml": "^3.0 || ^4.0"
    

Điều này có nghĩa là thư viện của bạn có thể dùng `symfony/yaml` từ các phiên bản Symfony 3.x hoặc 4.x. Điều này quan trọng, bởi vì sự hạn chế này sẽ phải được thông qua bởi chương trình
ứng dụng nào sử dụng thư viện của bạn.

Trong trường hợp có 2 thư viện với mâu thuẫn trong việc require, chẳng hạn như một bên require `~3.1.0` và bên kia thì lại require `~3.2.0`, việc cài đặt sẽ thất bại.

## Lời khuyên #5: Bạn nên commit `composer.lock` lên git trong các ứng dụng

Nếu bạn đang tạo _một project_, bạn chắc chắn muốn commit `composer.lock` lên git. Việc này đảm bảo chắc chắn rằng mọi người - bạn, đồng nghiệp, CI server và production server của bạn - cùng
chạy ứng dụng đó với các phiên bản dependency như nhau.

Thoạt nhìn, điều này nghe có vẻ không cần thiết - bạn đã sử dụng một phiên bản cụ thể được giới hạn, như đã đề cập ở lời khuyên #3. Nhưng không, vẫn có những dependency trong các dependency của bạn
không được giới hạn với những ràng buộc này.
(chẳng hạn như `symfony/console` dựa trên `symfony/polyfill-mbstring`). Vì vậy nếu không commit `composer.lock`, bạn sẽ không thể nhận được tập các dependency chính xác.

## Lời khuyên #6: Đặt `composer.lock` vào `.gitignore` trong các thư viện

Nếu bạn đang tạo _một thư viện_ (hãy gọi nó là `acme/my-library`), bạn không nên commit file `composer.lock`. Nó [không có chút tác động nào](https://getcomposer.org/doc/02-libraries.md#lock-file) lên
các projects sử dụng thư viện của bạn.

Hãy tưởng tượng rằng `acme / my-library` sử dụng` monolog / monolog` như là một dependency. Nếu bạn đã commit `composer.lock`, bất cứ ai đang phát triển ` acme / my-library`
sẽ phải sử dụng phiên bản cũ hơn của Monolog. Nhưng khi thư viện đã hoàn tất, và bạn sử dụng nó trong một project thực sự, một phiên bản mới hơn của Monolog có thể
được cài đặt và nó có thể không tương thích với thư viện. Nhưng bạn đã không chú ý nó từ trước, vì `composer.lock`!

Tốt nhất là đặt `composer.lock` vào `.gitignore` của bạn để bạn sẽ không commit nó một cách tình cờ.

Nếu bạn muốn chắc chắn rằng thư viện sẽ tương thích với những phiên bản dependency khác của nó, hãy đọc lời khuyên tiếp theo!

## Lời khuyên #7: Chạy Travis CI thiết kế với những phiên bản dependency khác nhau
> Lời khuyên này chỉ dành cho các thư viện (vì bạn sử dụng các phiên bản chi tiết cho các ứng dụng)

Nếu bạn đang xây dựng một thư viện mã nguồn mở, bạn có thể sử dụng Travis CI để chạy các bản thiết kế của nó.

Theo mặc định, Composer cài đặt các phiên bản dependency mới nhất được cho phép bởi các ràng buộc trong `composer.json`. Có nghĩa là với
ràng buộc dependency `^ 3.0 || ^ 4.0`, bản thiết kế sẽ luôn sử dụng phiên bản mới nhất của bản phát hành v4. Vì 3.0 chưa bao giờ được thử, thư viện có thể không
tương thích với nó và điều đó sẽ làm cho người dùng của bạn không vui vẻ gì.

May mắn thay, Composer cung cấp một công tắc để cài đặt các phiên bản thấp nhất có thể của các dependency `--prefer-low` (nên được sử dụng với` --prefer-stable` để
ngăn chặn cài đặt các phiên bản không ổn định).

Cấu hình của file `.travis.yml` đã cập nhật có thể trông như thế này:

    
    
    language: php
    
    php:
      - 7.1
      - 7.2
    
    env:
      matrix:
        - PREFER_LOWEST="--prefer-lowest --prefer-stable"
        - PREFER_LOWEST=""
    
    before_script:
      - composer update $PREFER_LOWEST
    
    script:
      - composer ci
    

Xem trực tiếp trên [my mhujer/fio-api-php library](https://github.com/mhujer/fio-
api-php/blob/master/.travis.yml) và [bản xây dựng ma trận Travis CI](https://travis-ci.org/mhujer/fio-api-php)

Mặc dù giải pháp này sẽ bắt được hầu hết sự không tương thích, hãy nhớ rằng có nhiều tổ hợp dependency giữa phiên bản cũ nhất và mới nhất.
Và chúng có thể không tương thích với nhau.

## Lời khuyên #8: Sắp xếp các package trong require và require-dev theo tên

Việc giữ các package trong `require` và` require-dev` được sắp xếp theo tên là một thói quen tốt. Nó có thể ngăn chặn các xung-đột-khi-hợp-nhất không cần
thiết khi rebase một nhánh. Bởi vì nếu bạn đã thêm một package vào cuối danh sách trong 2 nhánh thì sẽ có xung-đột-khi-hợp-nhất mọi lúc.

Thật không thú vị gì khi cứ phải tự tay làm điều đó, tốt hơn hết là [cấu hình nó](https://getcomposer.org/doc/06-config.md#sort-packages) trong
`composer.json`:

    
    
    {
    ...
        "config": {
            "sort-packages": true
        },
    â¦
    }
    


Lần tới, bạn 'require' một package mới, nó sẽ được thêm vào đúng chỗ (và không phải ở cuối)

## Lời khuyên #9: Không cố gắng hợp nhất `composer.lock` khi rebase hay merge

Nếu bạn thêm một dependency mới vào `composer.json` (và` composer.lock`) và trước khi nhánh của bạn được merge,
có một dependency khác được thêm vào trong master, bạn cần phải rebase nhánh của mình. Và bạn sẽ nhận được một xung-đột-khi-hợp-nhất trong
`composer.lock`.

Bạn không bao giờ nên cố gắng giải quyết xung đột này theo cách thủ công, bởi vì file `composer.lock` chứa một hàm băm của các dependency được định nghĩa trong
`composer.json`. Vì vậy, ngay cả khi bạn giải quyết xung đột, kết quả file lock vẫn sẽ không chính xác.

Tốt nhất là tạo `.gitattributes` trong project root bằng
lệnh sau, bằng cách này git sẽ không merge file `composer.lock`:
    
    
    /composer.lock -merge
    

Bạn có thể khắc phục vấn đề này bằng cách sử dụng các nhánh có tính năng ngắn hạn như đã được đề xuất
trong [Trunk Based Development] (https://trunkbaseddevelopment.com/) (bạn nên
đang làm điều này bằng mọi giá). Khi bạn có một nhánh ngắn hạn, được merge
ngay lập tức, nguy cơ xung-đột-khi-hợp-nhất trong `composer.lock` sẽ nhỏ nhất. Thậm chí bạn có thể tạo chi nhánh chỉ
để thêm dependency và merge nó ngay lập tức.


Nhưng sẽ phải làm gì nếu bạn gặp phải xung-đột-khi-hợp-nhất trong `composer.lock` khi
rebase? Giải quyết nó với phiên bản ở master, khi đó bạn sẽ có thay đổi
chỉ trong `composer.json` (package mới được thêm vào). Và sau đó chạy `composer update --lock`, sẽ cập nhật file `composer.lock` với các thay đổi
từ `composer.json`. Bây giờ bạn có thể đưa ra file `composer.lock` đã cập nhật và tiếp tục với rebase.

## Lời khuyên #10: Nhận biết sự khác nhau giữa 'require' và 'require-dev'

Việc ý thức được sự khác biệt biệt giữa các khối 'require' và 'require-dev' là quan trọng.

Các package cần thiết để chạy ứng dụng hoặc thư viện nên được định nghĩa trong `require` (ví dụ: Symfony, Doctrine, Twig, Guzzle, â ?? ¦).
Nếu bạn đang tạo một thư viện, hãy cẩn thận với những thứ bạn cho vào `require`. Bởi vì mỗi dependency trong phần này cũng là một dependency của ứng dụng,
nơi sử dụng thư viện.

Các package cần thiết để phát triển ứng dụng (hoặc thư viện) nên được định nghĩa trong `require-dev` (ví dụ: PHPUnit, PHP_CodeSniffer, PHPStan).

## Lời khuyên #11: Cập nhật các dependency một cách an toàn

Tôi đoán chúng ta có thể đồng ý về thực tế rằng các dependency nên được cập nhật thường xuyên. Những gì tôi muốn thảo luận ở đây là
việc cập nhật các dependency nên được thực hiện tường minh và thận trọng, không phải là một cách tiện tay khi làm việc khác. Nếu bạn tái cấu trúc một cái gì đó và
đồng thời cập nhật một số thư viện, bạn không thể dễ dàng biết được ứng dụng đã bị hỏng do việc cấu trúc lại hay bởi bản cập nhật.

Bạn có thể sử dụng lệnh `composer outdated` để dependency nào có thể được cập nhật. Bạn nên thêm công tắc `--direct` (hoặc` -D`) vào danh sách
dependency được chỉ định trong `composer.json`. Ngoài ra có thể dùng `-m` để liệt kê ra những cập nhật chỉ có ở phiên bản thu nhỏ.

**Với mỗi dependency hết hạn hãy làm theo những bước sau**:

  1. Tạo một nhánh mới
  2. Cập nhật phiên bản dependency trong `composer.json` lên phiên bản mới nhất
  3. Chạy `composer update phpunit/phpunit --with-dependencies` (thay `phpunit/phpunit` bằng thư viện bạn đang cập nhật)
  4. Kiểm tra CHANGELOG trong repository trên Github để tìm ra nếu có bất kì sự thay đổi nào không. Nếu có, cập nhật lại ứng dụng.
  5. Kiểm thử ứng dụng ở local (nếu bạn đang dùng Symfony, bạn có thể tìm các cảnh báo về việc không được sử dụng trong thanh Debug)
  6. Commit các thay đổi (`composer.json`, `composer.lock` và bất cứ thứ gì cần thiết cho phiên bản mới hoạt động)
  7. Chờ cho đến khi CI được build xong
  8. Hợp nhất và triển khai

Đôi khi, bạn nên cập nhật nhiều phụ thuộc hơn cùng một lúc, ví dụ: khi bạn
đang cập nhật Doctrine hoặc Symfony. Trong trường hợp này, bạn có thể liệt kê tất cả trong lệnh cập nhật:

    
    composer update symfony/symfony symfony/monolog-bundle --with-dependencies
    

Hoặc bạn có thể sử dụng kí hiệu để cập nhật tất cả dependency trong một namespace cụ thể:
    
    
    composer update symfony/* --with-dependencies
    

Tôi biết rằng tất cả điều này nghe có vẻ nhàm chán, nhưng bạn có thể sẽ chỉ cập nhật các dependency
trong vài dịp thôi, vì vậy đáng để làm nó một cách an toàn hơn.

Một cách ngắn gọn là cập nhật tất cả các dependency `require-dev` cùng một lúc (nếu họ không yêu cầu thay đổi code, ngược lại tôi đề xuất
sử dụng các nhánh riêng để việc xem lại code dễ dàng hơn).

## Lời khuyên #12: Bạn có thể định nghĩa những loại dependency khác trong `composer.json`

Ngoài việc định nghĩa thư viện như là các dependency, bạn cũng có thể định nghĩa các những thứ khác ở đó.

Bạn có thể định nghĩa các phiên bản PHP mà ứng dụng/thư viện của bạn hỗ trợ:

    
    
    "require": {
        "php": "7.1.* || 7.2.*",
    },
    


Bạn cũng có thể định nghĩa các extension nào được require cho ứng dụng/thư viện. Nó cực kì có ích khi bạn đang cố cập nhật ứng dụng của mình hoặc khi một đồng nghiệp mới
muốn cài đặt ứng dụng đó lần đầu.

    
    
    "require": {
        "ext-mbstring": "*",
        "ext-pdo_mysql": "*",
    },
    

(Bạn nên dùng `*` đối với các phiên bản extension như [chúng có thể không nhất quán](https://getcomposer.org/doc/01-basic-usage.md#platform-packages)).

## Lời khuyên #13: Xác thực `composer.json` trong quá trình tạo CI

`composer.json` và` composer.lock` phải luôn được giữ đồng bộ. Bạn nên có một cách kiểm tra tự động cho nó. Chỉ cần thêm lệnh này vào khi tạo script và nó sẽ đảm bảo rằng
`composer.lock` được đồng bộ với `composer.json`:
    
    
    composer validate --no-check-all --strict
    

## Lời khuyên #14: Sử dụng Composer plugin trong PHPStorm

Có một plugin [composer.json cho PHPStorm] (https://plugins.jetbrains.com/plugin/7631-php-composer-json-support). Nó thêm  bộ tự động hoàn thành và một số xác thực khi thay đổi
`composer.json` theo cách thủ công.

Nếu bạn đang sử dụng IDE khác (hoặc chỉ là một trình soạn code), bạn có thể thiết lập xác thực
chống lại [lược đồ JSON của nó] (https://getcomposer.org/schema.json).

## Lời khuyên #15: Ghi rõ phiên bản sản phẩm PHP trong `composer.json`

Nếu bạn giống tôi và bạn đôi khi [chạy các phiên bản PHP chưa được phát hành ở local] (https://blog.martinhujer.cz/php-7-2-is-due-in-november-whats-new/),
bạn có nguy cơ cập nhật các dependency lên các phiên bản không hoạt động trong sản phẩm. Bây giờ tôi đang sử dụng PHP 7.2.0, có nghĩa là, tôi có thể cài đặt
các thư viện mà sẽ không hoạt động trên 7.1. Khi sản phẩm đang chạy 7.1, cài đặt sẽ thất bại ở đó.

Nhưng không cần phải lo lắng, có một cách dễ dàng. Chỉ cần xác định phiên bản PHP trong phần `config` của` composer.json`:
    
    
    "config": {
        "platform": {
            "php": "7.1"
        }
    }
    

Đừng nhầm lẫn với phần `require`, chúng hoạt động khác nhau. Ứng dụng của bạn có thể chạy trên 7.1 hoặc 7.2 hay đồng thời chỉ định 7.1
làm platform (có nghĩa là các dependency sẽ luôn được cập nhật thành phiên bản tương thích với 7.1):

    
    
    "require": {
        "php": "7.1.* || 7.2.*"
    },
    "config": {
        "platform": {
            "php": "7.1"
        }
    },
    

## Lời khuyên #16: Sử dụng các package riêng tư từ Gitlab của bạn

Chúng tôi khuyên bạn nên sử dụng `vcs` làm loại repository và Composer nên xác định được đúng cách lấy package. Ví dụ, nếu bạn đang thêm một fork từ Github,
nó sẽ sử dụng API của nó để tải xuống file .zip thay vì clone toàn bộ repository.

Nhưng sẽ phức tạp hơn đối với một cài đặt Gitlab riêng tư. Nếu bạn sử dụng `vcs`
làm kiểu repository, Composer sẽ phát hiện rằng có một cài đặt Gitlab sẽ cố tải xuống package bằng API (yêu cầu khóa API.
tôi không muốn thiết lập nó, vì vậy tôi đã quyết định cấu hình như thế này (sử dụng SSH cho
clone):

Trước hết chỉ định reposity với kiểu 'git':

    
    
    "repositories": [
        {
            "type": "git",
            "url": "git@gitlab.mycompany.cz:package-namespace/package-name.git"
        }
    ]
    

Sau đó sử dụng package như bình thường:

    
    
    "require": {
        "package-namespace/package-name": "1.0.0"
    }
    

## Lời khuyên #17: Làm sao để tạm thời sử dụng một nhánh với bản vá lỗi từ fork

Nếu bạn tìm thấy một lỗi trong một số thư viện công khai và bạn sửa nó trong fork của bạn trên
Github, bạn cần phải cài đặt thư viện từ repository này thay vì bản chính thức của nó (cho đến khi bản vá lỗi được hợp nhất và phiên bản đã sửa chữa được phát hành).

Nó có thể được thực hiện dễ dàng với [inline aliasing] (https://getcomposer.org/doc/articles/aliases.md#require-inline-alias):

    
    
    {
        "repositories": [
            {
                "type": "vcs",
                "url": "https://github.com/you/monolog"
            }
        ],
        "require": {
            "symfony/monolog-bundle": "2.0",
            "monolog/monolog": "dev-bugfix as 1.0.x-dev"
        }
    }
    

Bạn có thể kiểm thử bản vá lỗi ở local trước khi đẩy nó lên bằng việc [sử dụng `path` như là kiểu repository](https://getcomposer.org/doc/05-repositories.md#path).

## Cập nhật 01/08/2018:

Sau khi đặng bài báo, tôi nhận được những gợi ý cho nhiều lời khuyên khác. Và chúng đây:

## Lời khuyên #18: Cài đặt prestissimo để tăng tốc độ cài đặt package

Có một plugin Composer [hirak / prestissimo] (https://github.com/hirak/prestissimo) giúp tăng tốc độ cài đặt dependency bằng cách tải xuống chúng song song.

Và điều tuyệt nhất là bạn chỉ cần cài đặt nó một lần, một cách toàn cục và nó sẽ
làm việc tự động cho tất cả các project:

    
    
    composer global require hirak/prestissimo

## Lời khuyên #19: Kiểm thử các ràng buộc phiên bản của bạn nếu bạn không chắc chắn

Việc viết các ràng buộc phiên bản chính xác đôi khi có thể khó khăn ngay cả sau khi đọc
[tài liệu chính thức] (https://getcomposer.org/doc/articles/versions.md#writing-version-constraints).

May mắn thay, đã có [Checkagist Semver Checker] (https://semver.mwl.be/), ở đó bạn có thể kiểm tra phiên bản nào khớp với ràng buộc đã chỉ định. Thay vì chỉ
phân tích ràng buộc phiên bản, nó tải dữ liệu từ Packagist xuống để hiển thị các phiên bản đã thực sự được phát hành.

Xem [Kết quả cho `symfony/symfony:^3.1`](https://semver.mwl.be/#?package=symfony%2Fsymfony&version=%5E3.1&minimum-stability=stable).

## Lời khuyên #20: Sử dụng các sơ đồ class có phân quyền trong sản phẩm

Bạn nên [tạo ra sơ đồ class có phân quyền] (https://getcomposer.org/doc/articles/autoloader-optimization.md # optimization-level-2-a-authoritative-class-maps)
trong sản phẩm. Nó sẽ tăng tốc độ tải class bằng cách bao gồm tất cả mọi thứ trong sơ đồ class và bỏ qua tất cả kiểm tra file hệ thống.

Bạn có thể làm điều đó bằng cách chạy lệnh này trong tạo sản phẩm:
    
    
    composer dump-autoload --classmap-authoritative
    

## Lời khuyên #21: Cấu hình `autoload-dev` để kiểm thử

Bạn không muốn bao gồm các file thử nghiệm trong sơ đồ class của sản phẩm(vì vấn đề kích thước và bộ nhớ). Nó có thể được thực hiện bằng cách cấu hình `autoload-dev`
(tương tự như `autoload`):

    
    
    "autoload": {
        "psr-4": {
            "Acme\\": "src/"
        }
    },
    "autoload-dev": {
        "psr-4": {
            "Acme\\": "tests/"
        }
    },
    

## Lời khuyên #22: Thử Composer scripts

Composer scripts là các công cụ nhẹ để tạo các thiết kế script. Tôi đã viết [một bài báo khác về chúng](/have-you-tried-composer-scripts/).

## Kết luận

Nếu bạn đồng ý với những lời khuyên trên, tôi sẽ rất vui nếu bạn có thế thể hiện nó trong các bình luận (đừng quên để lại số thứ tự của lời khuyên).

** Viết bởi [Martin Hujer](https://www.martinhujer.cz) **

** [Theo dõi tôi trên Twitter](https://twitter.com/MartinHujer) **
** [Đăng ký RSS](/feed/) **


