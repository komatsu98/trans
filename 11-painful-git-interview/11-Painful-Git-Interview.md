# 11 câu hỏi khó nhằn về git trong phỏng vấn sẽ khiến bạn bật khóc
Theo cuộc khảo sát về dev mới nhất trên Stack Overflow, hơn 70% dev sử dụng Git, khiến nó trở thành VCS được sử dụng nhiều nhất trên thế giới. Git thường được sử dụng cho cả phát triển phần mềm thương mại và mã nguồn mở, với những lợi ích đáng kể cho các cá nhân, nhóm và doanh nghiệp.
### C1: Git fork là gì? Nêu sự khác nhau giữa fork, branch và clone.

> Topic: **Git**
> Difficulty: ⭐⭐

*   Mỗi **fork** là một bản sao từ xa, phía máy chủ của một repo, khác với bản gốc. Fork không phải là một khái niệm thực sự của Git, nó giống một ý tưởng về chính trị/xã hội hơn.   
*   Mỗi **clone** không phải là một fork; nó là một bản sao cục bộ của một repo từ xa nào đó. Khi bạn clone, bạn đang thực sự sao chép toàn bộ mã nguồn của repo, bao gồm cả lịch sử và các nhánh (branch).
*   Mỗi **branch** là một cơ chế để xử lý các thay đổi trong một repo duy nhất để rồi cuối cùng hợp nhất chúng với phần code còn lại. Branch là một thứ gì đó nằm trong repo. Về mặt khái niệm, nó đại diện cho một luồng phát triển.

🔗**Nguồn:** [stackoverflow.com](https://stackoverflow.com/questions/3329943/git-branch-fork-fetch-merge-rebase-and-clone-what-are-the-differences/)

### C2: Phân biệt "pull request" và "branch"

> Topic: **Git**
> Difficulty: ⭐⭐

*   Mỗi **branch** chỉ là một phiên bản riêng biệt của code.

*   Một **pull request** là khi ai đó lấy repo về, tạo nhánh riêng của họ, thực hiện một số thay đổi, sau đó cố gắng hợp nhất nhánh đó vào (đặt các thay đổi của họ vào repo của một người khác).

🔗**Nguồn:** [stackoverflow.com](https://stackoverflow.com/questions/19059838/whats-the-difference-between-a-pull-request-and-a-branch)

### C3: Sự khác biệt giữa "git pull" và "git fetch" là gì?

> Topic: **Git**
> Difficulty: ⭐⭐

Nói một cách đơn giản, `git pull` thực hiện` git fetch` và sau đó `git merge`.

*   Khi bạn sử dụng `pull`, Git sẽ cố gắng tự động làm việc đó cho bạn. **Đây là tình huống nhạy cảm**, vì vậy Git sẽ merge bất kỳ commit nào đã được pull về nhánh bạn đang làm việc. `Pull` ** tự động merge các commit mà không cho phép bạn xem xét chúng trước **. Nếu bạn không quản lý chặt chẽ các nhánh của mình, bạn có thể gặp phải các xung đột thường xuyên.
*   Khi bạn `fetch`, Git gom bất cứ commit nào từ nhánh đích không tồn tại trong nhánh hiện tại của bạn và **lưu trữ chúng trong kho repo cục bộ của bạn**. Tuy nhiên, **nó không hợp nhất chúng với nhánh hiện tại của bạn**. Điều này đặc biệt hữu ích nếu bạn cần cập nhật repo của mình nhưng lại đang làm việc với những thứ có thể bị hỏng nếu bạn thực hiện các cập nhật đó. Để tích hợp các commit vào nhánh master, hãy sử dụng `merge`.

🔗**Source:** [stackoverflow.com](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch)

### C4: Làm sao để trở về các commit trước đó trong git?

> Topic: **Git**
> Difficulty: ⭐⭐⭐

Giả sử bây giờ bạn có C là HEAD và (F) là trạng thái các file.

<div class="highlight">

       (F)
    A-B-C
        ↑
      master

</div>

*   Để tiêu hủy các thay đổi trong commit:

<div class="highlight">

    git reset --hard HEAD~1

</div>

Giờ B trở thành HEAD. Bởi vì bạn đã sử dụng --hard nên các file của bạn sẽ được đặt lại trạng thái của chúng ở commit B.

*   Để hủy bỏ commit nhưng vẫn giữ lại các thay đổi:

<div class="highlight">

    git reset HEAD~1

</div>

Bây giờ chúng ta bảo Git di chuyển con trỏ HEAD trở lại một commit (B) và để các file như thế, khi đó `git status` sẽ hiển thị những thay đổi mà bạn đã kiểm tra trong C.

*   Để hủy bỏ commit nhưng giữ lại các files và index

<div class="highlight">

    git reset --soft HEAD~1

</div>

Khi bạn thực hiện `git status`, bạn sẽ thấy cùng các file ở trong index trước đó.

🔗**Source:** [stackoverflow.com](https://stackoverflow.com/questions/927358/how-to-undo-the-most-recent-commits-in-git)

### C5: "Git cherry-pick" là gì?

> Topic: **Git**
> Difficulty: ⭐⭐⭐

Lệnh git _cherry-pick_ thường được sử dụng để chèn các commit cụ thể từ một nhánh trong một repo vào một nhánh khác. Cách sử dụng phổ biến là chuyển tiếp commit từ nhánh bảo trì đến nhánh phát triển.

Điều này trái ngược với các cách khác như merge và rebase nơi thường áp dụng nhiều commit vào một nhánh khác.

Để ý:

<div class="highlight">

    git cherry-pick <commit-hash>

</div>

🔗**Source:** [stackoverflow.com](https://stackoverflow.com/questions/9339429/what-does-cherry-picking-a-commit-with-git-mean)

### C6: Giải thích những ưu điểm của Forking Workflow

> Topic: **Git**
> Difficulty: ⭐⭐⭐

**Forking Workflow** cơ bản khác với các quy trình làm việc với Git phổ biến khác. Thay vì sử dụng một repo phía máy chủ duy nhất để hoạt động như là codebase "trung tâm", nó cung cấp cho mỗi nhà phát triển kho lưu trữ phía máy chủ của riêng họ. Forking Workflow thường thấy nhất trong các dự án nguồn mở công khai.

_Ưu điểm chính_ của Forking Workflow là sự đóng góp có thể được tích hợp mà không cần tất cả mọi người phải push lên một repo trung tâm duy nhất làm cho lịch sử dự án sạch sẽ. Các nhà phát triển push lên repo phía máy chủ của riêng họ và chỉ người duy trì dự án mới có thể push lên repo chính thức. 

🔗**Source:** [atlassian.com](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

### C7: Cho tôi biết sự khác nhau giữa HEAD, cây công việc và index trong Git?

> Topic: **Git**
> Difficulty: ⭐⭐⭐

*   **Working tree/working directory/workspace** là cây thư mục của các tệp (mã nguồn) mà bạn nhìn thấy và sửa.
*   **index/staging area** là một tệp nhị phân lớn, duy nhất trong /.git/index, liệt kê tất cả các tệp trong nhánh hiện tại, các checksum sha1 của chúng, dấu thời gian và tên tệp - nó không phải là một thư mục khác với các bản sao tệp trong đó.
*   **HEAD** là tham chiếu đến lần commit cuối cùng trong nhánh hiện đã check-out.

🔗**Source:** [stackoverflow.com](https://stackoverflow.com/questions/3689838/whats-the-difference-between-head-working-tree-and-index-in-git)

### C8: Bạn có thể giải thích quy trình làm việc Gitflow không?

> Topic: **Git**
> Difficulty: ⭐⭐⭐

Quy trình làm việc Gitflow sử dụng hai nhánh _long-running_ song song để ghi lại lịch sử của dự án, `master` và` develop`:

*   **Master** - luôn sẵn sàng để được phát hành trên LIVE, với mọi thứ được kiểm tra và phê duyệt đầy đủ (production-ready).

    *   **Hotfix** - Các nhánh bảo trì hoặc “hotfix” được sử dụng để nhanh chóng vá các bản phát hành sản phẩm. Các nhánh Hotfix giống như các nhánh phát hành và các nhánh tính năng, ngoại trừ việc chúng dựa trên `master` thay vì` develop`.

*   **Develop** - là nhánh mà tất cả các chi nhánh tính năng được hợp nhất và nơi tất cả các thử nghiệm được thực hiện. Chỉ khi mọi thứ được kiểm tra kỹ lưỡng và sửa chữa thì nó mới có thể được hợp nhất với `master`.

    *   **Feature** - Mỗi tính năng phải nằm trong nhánh riêng của nó, từ đó có thể được đẩy lên nhánh `develop` như là nhánh cha.

[![Quy trình Gitflow](https://res.cloudinary.com/practicaldev/image/fetch/s--pLQxGakq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wac-cdn.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%2520%282%29.svg%3FcdnVersion%3Dji)](https://res.cloudinary.com/practicaldev/image/fetch/s--pLQxGakq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wac-cdn.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%2520%282%29.svg%3FcdnVersion%3Dji)

🔗**Source:** [atlassian.com](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

### C9: Khi nào tôi nên sử dụng "git stash"?


> Topic: **Git**
> Difficulty: ⭐⭐⭐

Lệnh `git stash` lấy các thay đổi chưa được commit của bạn (cả staged và unstaged), lưu chúng lại để sử dụng sau này, và sau đó lấy lại chúng từ bản sao làm việc của bạn.

Xem:

<div class="highlight">

    $ git status
    On branch master
    Changes to be committed:
    new file: style.css
    Changes not staged for commit:
    modified: index.html
    $ git stash
    Saved working directory and index state WIP on master: 5002d47 our new homepage
    HEAD is now at 5002d47 our new homepage
    $ git status
    On branch master
    nothing to commit, working tree clean

</div>

Chúng ta có thể sử dụng stashing trong trường hợp nếu phát hiện ra ta quên một điều gì đó trong lần commit cuối cùng và đã bắt đầu làm việc trên commit tiếp theo trong cùng một nhánh:

<div class="highlight">

    # Assume the latest commit was already done
    # start working on the next patch, and discovered I was missing something

    # stash away the current mess I made
    $ git stash save

    # some changes in the working dir

    # and now add them to the last commit:
    $ git add -u
    $ git commit --ammend

    # back to work!
    $ git stash pop

</div>

🔗**Source:** [atlassian.com](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

### C10: Làm thế nào để loại bỏ một tập tin từ git mà không cần loại bỏ nó khỏi hệ thống của bạn?

> Topic: **Git**
> Difficulty: ⭐⭐⭐⭐

Nếu bạn không cẩn thận khi sử dụng `git add`, bạn có thể sẽ thêm các tệp mà bạn không muốn commit. Tuy nhiên, `git rm` sẽ loại bỏ nó khỏi cả vùng stage (index), cũng như hệ thống tệp của bạn (working tree), đây có thể không phải là thứ bạn muốn.

Thay vào đó hãy sử dụng `git reset`:

<div class="highlight">

    git reset filename          # or
    echo filename >> .gitingore # add it to .gitignore to avoid re-adding it

</div>

Điều này có nghĩa là `git reset <paths>` trái ngược với `git add <paths>`.

🔗**Source:** [codementor.io](https://www.codementor.io/citizen428/git-tutorial-10-common-git-problems-and-how-to-fix-them-aajv0katd)

### C11: Khi nào bạn sử dụng "git rebase" thay vì "git merge"?

> Topic: **Git**
> Difficulty: ⭐⭐⭐⭐⭐

Cả hai lệnh này được thiết kế để tích hợp các thay đổi từ một nhánh này sang một nhánh khác - chỉ là chúng thực hiện theo những cách rất khác nhau.

Hãy xem trước khi merge/rebase:

<div class="highlight">

    A <- B <- C    [master]
    ^
     \
      D <- E       [branch]

</div>

sau khi `git merge master`:

<div class="highlight">

    A <- B <- C
    ^         ^
     \         \
      D <- E <- F

</div>

sau khi `git rebase master`:

<div class="highlight">

    A <- B <- C <- D <- E

</div>

Với rebase bạn sẽ sử dụng một nhánh làm cơ sở mới cho công việc của bạn.

**Khi nào sử dụng:**

1.  Nếu bạn có bất cứ nghi ngờ nào, hãy dùng git merge.
2.  Sự lựa chọn giữa rebase và merge dựa trên việc bạn muốn lịch sử của bạn trông như thế nào.

**Nhiều yếu tố cần xem xét nữa:**

1.  **Nhánh của bạn có đang nhận được những thay đổi từ việc chia sẻ với các nhà phát triển khác bên ngoài nhóm của bạn (ví dụ: nguồn mở, công khai) không?** Nếu vậy, đừng rebase. Rebase phá hủy nhánh và các nhà phát triển đó sẽ phải nhận các repo bị hỏng / không nhất quán trừ khi họ sử dụng `git pull --rebase`.
2.  **Kĩ năng của nhóm phát triển của bạn như thế nào?** Rebase là một hoạt động có tính phá hủy. Điều đó có nghĩa, nếu bạn không áp dụng nó một cách chính xác, bạn có thể các công việc đã commit và / hoặc phá vỡ sự thống nhất của repo của nhà phát triển khác.
3.  **Bản thân nhánh có đại diện cho thông tin hữu ích không?** Một số nhóm sử dụng mô hình _branch-per-feature_ trong đó mỗi nhánh đại diện cho một tính năng (hoặc bugfix, hoặc sub-feature, vv) Trong mô hình này nhánh giúp xác định tập hợp các commit liên quan. Trong trường hợp mô hình _branch-per-developer_ bản thân chi nhánh không truyền tải bất kỳ thông tin bổ sung nào (commit đã có tác giả). Sẽ không có hại gì khi rebasing.
4.  **Bạn có muốn hoàn tác merge vì lí do nào không?** Hoàn tác (như trong hủy bỏ) một rebase là thực sự khó khăn và / hoặc không thể (nếu rebase có xung đột) so với hoàn tác merge. Nếu bạn nghĩ rằng có một cơ hội bạn sẽ muốn hoàn tác thì hãy sử dụng merge.
