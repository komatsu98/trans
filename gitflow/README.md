## 1. Git
![Git](branching.png)
<a name="some-git-rules"></a>

### 1.1 Some Git rules
Có một số luật mà bạn cần nhớ:
* Thể hiện công việc ở một nhánh tính năng.
    
    _Tại sao:_
    >Because this way all work is done in isolation on a dedicated branch rather than the main branch. It allows you to submit multiple pull requests without confusion. You can iterate without polluting the master branch with potentially unstable, unfinished code. [read more...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
    >Bởi vì theo cách này tất cả các công việc được thực hiện độc lập trên một nhánh chuyên dụng chứ không phải là nhánh chính. Nó cho phép bạn gửi nhiều pull request mà không bị nhầm lẫn. Bạn có thể lặp mà không làm rối nhánh chính với code chưa hoàn thành, có khả năng không ổn định. [xem thêm...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
* Tách khỏi nhánh `develop`
    
    _Tại sao:_
    >Bằng cách này, bạn sẽ chắc chắn rằng code ở master luôn được build mà không dính lỗi, và hầu như có thể được sử dụng trực tiếp cho các bản phát hành (điều này có thể là quá mức cần thiết đối với một số project).

* Không bao giờ đẩy code lên nhánh `develop` hay `master`. Hãy tạo pull request.
    
    _Tại sao:_
    > Điều này giúp thông báo cho các thành viên trong nhóm rắng họ đã hoàn tất một tính năng nào đó. Nó cho phép những nguời cùng review code một cách dễ dàng và tạo ra diễn đàn cho việc thảo luận các tính năng đã đề ra.

* Cập nhật nhánh `develop` ở local và rebase trước khi push tính năng và tạo pull request.

    _Tại sao:_
    > Việc rebase sẽ merge trong nhánh đã được yêu cầu (`master` hoặc `develop`) và áp dụng các commit mà bạn đã tạo ở local lên đầu tiên trong lịch sử mà không cần tạo một commit merge (giả sử rằng không có xung đột). Từ đó có một lịch sử đẹp và gọn gàng. [xem thêm ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

* Giải quyết xung đột tàng trong khi rebase và trước khi tạo Pull Request.
* Xóa local và remote các nhánh tính năng sau khi merge.
    
    _Tại sao:_
    > Các nhánh chết sẽ làm lộn xộn danh sách các nhánh của bạn. Điều này đảm bảo bạn chỉ merge nhánh vào (`master` hoặc` develop`) một lần. Các chi nhánh tính năng chỉ nên tồn tại khi công việc vẫn đang được tiến hành.

* Trước khi tạo một pull request, hãy chắc chắn rằng nhánh tính năng build thành công và qua được tất cả test (gồm cả các kiểm tra về code style)
    
    _Tại sao:_
    > Khi bạn muốn thêm code vào một nhánh đã ổn định. Nếu nhánh tính năng của bạn không qua được các test, khả năng cao việc build nhánh đích của bạn cũng sẽ thất b. Ngoài ra, bạn cần áp dụng kiểm tra code style trước khi thực hiện Pull request. Nó giúp code dễ đọc và hạn chế các chỉnh sửa định dạng được trộn lẫn với các thay đổi thực tế.

* Sử dụng file [này](./.gitignore) `.gitignore`.
    
    _Tại sao:_
    > File này chứa danh sách các file sẽ không được gửi lên repo cùng với code. Ngoài ra, nó loại tr cài đặt thư mục và tệp cho hầu hết các editor được sử dụng, cũng như các thư mục dependency phổ biến nhất.

* Bảo vệ các nhánh `develop` và `master` của bạn.
  
    _Tại sao:_
    > Điều này bảo vệ nhánh production chính của bạn khỏi việc nhận những thay đổi không mong đợi và không thể lấy lại. xem thêm ... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) và [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)

<a name="git-workflow"></a>
### 1.2 Git workflow
Với hầu hết các lí do trên, chúng tôi sử dụng [Feature-branch-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) với [Interactive Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) và một vài phần tử của [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (đặt tên và có một nhánh phát triển). Các bước chính như sau:

* Đối với một project mới, For a new project, khởi tạo một git repo trong thư mục project đó. __Đối với các thay đổi/tính năng tiếp theo thì bước này có thể được bỏ qua__.
   ```sh
   cd <project directory>
   git init
   ```

* Checkout nhánh tính năng/sửa lỗi mới.
    ```sh
    git checkout -b <branchname>
    ```
* Tạo các thay đổi.
    ```sh
    git add
    git commit -a
    ```
    _Tại sao:_
    > `git commit -a` sẽ bắt đầu một editor cho phép bạn tách tiêu đề ra khỏi phần thân. Đọc thêm về nó ở *phần 1.3*.

* Đồng bộ với nhánh đích để không bỏ qua các thay đổi.
    ```sh
    git checkout develop
    git pull
    ```
    
    _Tại sao:_
    > Nó sẽ cho bạn cơ hội xử lí các xung đột trên máy bạn trong khi rebase (sau này) hơn là tạo một pull request chứa các xung đột.

* Cập nhật nhánh tính năng của bạn với các thay đổi mới nhất bằng rebase.
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```
    
    _Tại sao:_
    > Bạn có thể dùng --autosquash để  gộp tất cả commit của bạn vào một commit duy nhất. Không ai muốn thấy một mớ commit cho một tính năng riêng lẻ trong nhánh develop cả. [xem thêm...](https://robots.thoughtbot.com/autosquashing-git-commits)
    
* Nếu bạn không gặp phải xung đột, hãy bỏ qua bước này. Nếu có, hãy [giải quyết chúng](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)  và tiếp tục rebase.
    ```sh
    git add <file1> <file2> ...
    git rebase --continue
    ```
* Push nhánh của bạn. Rebase sẽ thay đổi lịch sử, vì vậy bạn sẽ phải sử dụng `-f` để buộc các thay đổi thêm vào nhánh đích. Nếu người khác đang làm việc trên nhánh của bạn, hãy dùng `--force-with-lease`.
    ```sh
    git push -f
    ```
    
    _Tại sao:_
    > Khi rebase, bạn đang thay đổi lịch sử của nhánh tính năng. Và do đó Git sẽ từ chối `git push` như thường lệ. Thay vào đó, bạn sẽ cần sử dụng -f hoặc --force. [xem thêm...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
    
    
* Tạo một Pull Request.
* Pull request được chấp nhận, hợp nhất và đóng bởi người review.
* Xóa nhánh tính năng ở locl khi bạn hoàn thành.

  ```sh
  git branch -d <branchname>
  ```
  để xóa tất cả các nhanh không còn trên repo đích
  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```

<a name="writing-good-commit-messages"></a>
### 1.3 Viết các commit massage rõ ràng

Việc có những nguyên tắc khi tạo các commit và tuân theo nó sẽ khiến việc làm việc với Git và các đồng nghiệp dễ dàng hơn rất nhiều. Đây là một số quy tắc đã được đúc kết ([source](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * Tách tiêu đề khỏi phân thân với một dòng trống giữa chúng.

    _Tại sao:_
    > Git đủ thông minh để phân biejt dòng đầu tiên trong commit message của bạn như là tóm tắt cho commit đó. Thực tế, nếu bạn thử git shortlog, thay vì git log bạn sẽ thấy một danh sách dài các commit massage, chỉ gồm id và tóm tắt của commit.

 * Giới hạn tiêu đề 50 kí tự và xuống dòng ở phần thân là 72 kí tự.

    _Tại sao_
    > Các commit nên đủ tinh tế và thu hút nhất có thể, không nên viết dài dòng ở đây. [xem thêm...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

 * Viết hoa dòng tiêu đề.
 * Không kết thúc dòng tiêu đề bằng dấu chấm câu.
 * Sử dụng [imperative mood](https://en.wikipedia.org/wiki/Imperative_mood) ở dòng tiêu đề.

    _Tại sao:_
    > Thay v viết message nói rằng người thực hiện commit đã hoàn tất. Tốt hơn là nên xem xét các message như là hướng dẫn về những gì sẽ thay đổi sau commit đó đối với repo. [xem thêm...](https://news.ycombinator.com/item?id=2079612)


 * Sử dụng phần thân để giải thích **what** và **why** như là trái lại **how**.

 <a name="documentation"></a>
