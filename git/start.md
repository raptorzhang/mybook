#git的初始化操作

###用户签名:
```git
git config --global user.name raptorzhang
git config --global user.email raptorzhang@gmail.com
```

查看用户目录下面的 .gitconfig文件里面就有保存了！
###初始化 git init
```
$ pwd
/d/gitTest

$ mkdir -p gitDemo

$ cd gitDemo/

$ git init
Initialized empty Git repository in D:/gitTest/gitDemo/.git/

```
###查看状态 git status

```
$ git status
\On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
$ echo "hello \n hello git!" >> hello.txt

$ ll
total 1
-rw-r--r-- 1 rapto 197609 20 Mar 22 23:24 hello.txt

$ cat hello.txt
hello \n hello git!

$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.txt

nothing added to commit but untracked files present (use "git add" to track)

```

###添加到暂存区 git add hello.txt
```
$ git add hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt
```
###删除暂存区 git rm -f hello.txt
```

$ cp hello.txt hello2.txt
$ ll
total 2
-rw-r--r-- 1 rapto 197609 20 Mar 22 23:24 hello.txt
-rw-r--r-- 1 rapto 197609 20 Mar 22 23:30 hello2.txt

$ git add hello2.txt
warning: in the working copy of 'hello2.txt', LF will be replaced by CRLF the next time Git touches it

$ git rm hello2.txt
error: the following file has changes staged in the index:
    hello2.txt
(use --cached to keep the file, or -f to force removal)

$ git rm -f hello2.txt
rm 'hello2.txt'

```
###提交本地库 git commit -m "描述信息" hello.txt

```
$ git commit -m "描述信息" hello.txt
warning: in the working copy of 'hello.txt', LF will be replaced by CRLF the next time Git touches it
[master (root-commit) 1c0e522] 描述信息
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt

$ git status
On branch master
nothing to commit, working tree clean

```

###查看log
```
$ git reflog
1c0e522 (HEAD -> master) HEAD@{0}: commit (initial): 描述信息

$ git log
commit 1c0e522f0b3b8b1e7d54ace43e21cc787a72fa00 (HEAD -> master)
Author: raptorzhang <raptorzhang@gmail.com>
Date:   Wed Mar 22 23:35:57 2023 +0800
    描述信息
```
###版本穿梭 git reset --hard 版本号
```
$ git reflog
513f64c (HEAD -> master) HEAD@{0}: commit: 第三次修改！
f370f61 HEAD@{1}: commit: 第二次修改提交本地库
1c0e522 HEAD@{2}: commit (initial): 描述信息

$ git reset --hard f370f61
HEAD is now at f370f61 第二次修改提交本地库

$ git reflog
f370f61 (HEAD -> master) HEAD@{0}: reset: moving to f370f61
513f64c HEAD@{1}: commit: 第三次修改！
f370f61 (HEAD -> master) HEAD@{2}: commit: 第二次修改提交本地库
1c0e522 HEAD@{3}: commit (initial): 描述信息
```



