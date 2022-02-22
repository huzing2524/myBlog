### 【Git】重写log历史
- [官方文档地址](https://link.zhihu.com/?target=https%3A//git-scm.com/book/zh/v2/Git-%25E5%25B7%25A5%25E5%2585%25B7-%25E9%2587%258D%25E5%2586%2599%25E5%258E%2586%25E5%258F%25B2)
- 方法1：修改log历史记录
    ```bash
    $ git rebase -i [log记录commit id]
    $ git commit --amend
    $ git rebase --continue
    ```
- 方法2：全局修改邮箱地址   
另一个常见的情形是在你开始工作时忘记运行 git config 来设置你的名字与邮箱地址， 或者你想要开源一个项目并且修改所有你的工作邮箱地址为你的个人邮箱地址。 任何情形下，你也可以通过 filter-branch 来一次性修改多个提交中的邮箱地址。 需要小心的是只修改你自己的邮箱地址，所以你使用 --commit-filter。  
这会遍历并重写每一个提交来包含你的新邮箱地址。因为提交包含了它们父提交的 SHA-1 校验和，这个命令会修改你的历史中的每一个提交的 SHA-1 校验和， 而不仅仅只是那些匹配邮箱地址的提交。
    ```bash
    $ git filter-branch -f --commit-filter '
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
            GIT_AUTHOR_NAME="Scott Chacon";
            GIT_AUTHOR_EMAIL="schacon@example.com";
            git commit-tree "$@";
        else
            git commit-tree "$@";
        fi' HEAD 
    ```
