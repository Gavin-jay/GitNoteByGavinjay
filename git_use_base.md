> ## The usage of Git
>
> ## init
>
> * git init
>
> ## check
>
> * git status
>   查看当前文件状态
> * git add text.c
>   将文件放入暂存区，即跟踪新文件。命令使用文件或目录的路径作为参数，如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“精确地将内容添加到下一次提交中
> * git checkout -- <file> <file/》
>
>   discard changes in working directory
> * git diff
>
>   查看尚未暂存的文件更新了哪些部分,此命令比较的是工作目录中当前文件和暂存区域快照之间的差异
> * git diff --staged
>
>   查看已暂存的将要添加到下次提交里的内容
> * git commit -m "description"
>
>   提交更新
> * git commit -a
>
>   Git 会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤
>
> ## Ignore
>
> ```
> cat .gitignore
> .[oa]
> ~
> ```
>
> 第一行告诉 Git 忽略所有以 `.o` 或 `.a` 结尾的文件。 第二行告诉 Git 忽略所有名字以波浪符（\~）结尾的文件
>
> ```
> # 忽略所有的 .a 文件
> *.a
>
> # 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
> !lib.a
>
> # 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
> /TODO
>
> # 忽略任何目录下名为 build 的文件夹
> build/
>
> # 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
> doc/*.txt
>
> # 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
> doc/**/*.pdf
>
> ```
>
> 星号（`*`）匹配零个或多个任意字符；
>
> `[abc]` 匹配任何一个列在方括号中的字符；
>
> 问号（`?`）只匹配一个任意字符；
>
> 如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）
>
> 使用两个星号（`**`）表示匹配任意中间目录，比如 `a/**/z` 可以匹配 `a/z` 、 `a/b/z` 或 `a/b/c/z` 等
>
> ## remove file
>
> ```
> rm project.md
> git rm project,md
> git status
> ```
>
> 下一次提交时，该文件就不再纳入版本管理了。 如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 `-f`
>
> ```
> git em --cached README
> ```
>
> 会让文件保留在磁盘，但是并不想让 Git 继续跟踪
>
> ```
> git rm log/\*.log
> git rm \*~
> ```
>
> 此命令删除 `log/` 目录下扩展名为 `.log` 的所有文件,下面的命令会删除所有名字以~结尾的文件
>
> ## change name
>
> ```
> git mv file_name file_to
> ```
>
> 会重命名这个文件
>
> ## 查看提交历史
>
> ```
> git log
> ```
>
> check the commit history
>
> * 选项 `-p` 或 `--patch` ，它会显示每次提交所引入的差异（按 **补丁** 的格式输出）。 你也可以限制显示的日志条目数量，例如使用 `-2` 选项来只显示最近的两次提交
> * 看到每次提交的简略统计信息，可以使用 `--stat` 选项
>
> ```
> git log --pretty=oneline
> ```
>
> * 另一个非常有用的选项是 `--pretty`。 这个选项可以使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如 `oneline` 会将每个提交放在一行显示，在浏览大量的提交时非常有用。 另外还有 `short`，`full` 和 `fuller` 选项，它们展示信息的格式基本一致，但是详尽程度不一
>
> ```
> $ git log --pretty=format:"%h - %an, %ar : %s"
>
> ca82a6d - Scott Chacon, 6 years ago : changed the version number
> 085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
> a11bef0 - Scott Chacon, 6 years ago : first commit
> ```
>
> * 可以定制记录的显示格式
>
> ```
> $ git log --pretty=format:"%h %s" --graph
> * 2d3acf9 ignore errors from SIGCHLD on trap
> *  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
> |\
> | * 420eac9 Added a method for getting the current branch.
> * | 30e367c timeout code and tests
> * | 5a09431 add timeout protection to grit
> * | e1193f8 support for heads with slashes in them
> |/
> * d6016bc require time for xmlschema
> *  11d191e Merge branch 'defunkt' into local
> ```
>
> * 这个选项添加了一些 ASCII 字符串来形象地展示你的分支、合并历史
>
> ```
> git log --since=2.weeks
> ```
>
> * 列出最近两周的所有提交，该命令可用的格式十分丰富——可以是类似 `"2008-01-15"` 的具体的某一天，也可以是类似 `"2 years 1 day 3 minutes ago"` 的相对日期
>
> ```
> git log -S function_name
> ```
>
> * 另一个非常有用的过滤器是 `-S`, 它接受一个字符串参数，并且只会显示那些添加或删除了该字符串的提交。 假设你想找出添加或删除了对某一个特定函数的引用的提交，可以调用
>
> ```
> -<n>                  仅显示最近的 n 条提交。
>
> --since, --after      仅显示指定时间之后的提交。
>
> --until, --before     仅显示指定时间之前的提交。
>
> --author              仅显示作者匹配指定字符串的提交。
>
> --committer           仅显示提交者匹配指定字符串的提交。
>
> --grep                仅显示提交说明中包含指定字符串的提交。
>
> -S                    仅显示添加或删除内容匹配指定字符串的提交。
>
>
> ```
>
> * the command above is example
>
> ```
> eg:
>
> $ git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" \
>    --before="2008-11-01" --no-merges -- t/
> 5610e3b - Fix testcase failure when extended attributes are in use
> acd3b9e - Enhance hold_lock_file_for_{update,append}() API
> f563754 - demonstrate breakage of detached checkout with symbolic link HEAD
> d1a43f2 - reset --hard/read-tree --reset -u: remove unmerged new paths
> 51a94af - Fix "checkout --track -b newbranch" on detached HEAD
> b0ad11e - pull: allow "git pull origin $something:$current_branch" into an unborn branch
> ```
>
> * 一个实际的例子，如果要在 Git 源码库中查看 Junio Hamano 在 2008 年 10 月其间, 除了合并提交之外的哪一个提交修改了测试文件.
>
> ## Undo
>
> ```
> $ git commit -m 'initial commit'
> $ git add forgotten_file
> $ git commit --amend
> ```
>
> * 文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息,最终只会有一个提交——第二次提交将代替第一次提交的结果
>
> ```
> $ git add *
> $ git status
> On branch master
> Changes to be committed:
>   (use "git reset HEAD <file>..." to unstage)
>
>     renamed:    README.md -> README
>     modified:   CONTRIBUTING.md
>
>
> $ git reset HEAD CONTRIBUTING.md
> Unstaged changes after reset:
> M	CONTRIBUTING.md
> $ git status
> On branch master
> Changes to be committed:
>   (use "git reset HEAD <file>..." to unstage)
>
>     renamed:    README.md -> README
>
> Changes not staged for commit:
>   (use "git add <file>..." to update what will be committed)
>   (use "git checkout -- <file>..." to discard changes in working directory)
>
>     modified:   CONTRIBUTING.md
>
> ```
>
> * git reset HEAD file to unstage
>
> ## remote
>
> ```
> $ git remote
> origin
> $ git remote -v
> origin	https://github.com/schacon/ticgit (fetch)
> origin	https://github.com/schacon/ticgit (push)
> ```
>
> * check the remote repository
>
> ```
> $ git remote
> origin
> $ git branch -M main
> $ git remote add pb https://github.com/paulboone/ticgit
> $ git remote -v
> origin	https://github.com/schacon/ticgit (fetch)
> origin	https://github.com/schacon/ticgit (push)
> pb	https://github.com/paulboone/ticgit (fetch)
> pb	https://github.com/paulboone/ticgit (push)
>
> $ git fetch pb
> remote: Counting objects: 43, done.
> remote: Compressing objects: 100% (36/36), done.
> remote: Total 43 (delta 10), reused 31 (delta 5)
> Unpacking objects: 100% (43/43), done.
> From https://github.com/paulboone/ticgit
>  * [new branch]      master     -> pb/master
>  * [new branch]      ticgit     -> pb/ticgit
>
> $ git push pb main
>
>
>
> ```
>
> * 可以在命令行中使用字符串 `pb` 来代替整个 URL
> * 将 `main` 分支推送到 `pb` 服务器
