#github 

网址:  <https://github.com>;

    
   ###别名 git remote
   相当于一url简写记忆的记号。
```
rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
$ git remote -v

rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
$ git remote add nav https://github.com/raptorzhang/code-edit-online.git

rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
$ git remote -v
nav     https://github.com/raptorzhang/code-edit-online.git (fetch)
nav     https://github.com/raptorzhang/code-edit-online.git (push)

```
   ###推送到github服务器 git push nav master
   首次需要授权，网页登录自己的git帐号；
   
```
    $ git push nav master
    Enumerating objects: 1389, done.
    Counting objects: 100% (1389/1389), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (1377/1377), done.
    Writing objects: 100% (1389/1389), 2.80 MiB | 1.35 MiB/s, done.
    Total 1389 (delta 138), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (138/138), done.
    To https://github.com/raptorzhang/code-edit-online.git
     * [new branch]      master -> master
```
   ###拉取github 到本地； git pull nav master

```
rapto@raptorzhangSurface MINGW64 /d/navfile (master)
$ git pull nav master
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 686 bytes | 76.00 KiB/s, done.
From https://github.com/raptorzhang/code-edit-online
 * branch            master     -> FETCH_HEAD
   1fc0fc7..1e375dd  master     -> nav/master
Updating 1fc0fc7..1e375dd
Fast-forward
 config.php | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

```   
   ###克隆，和push不一样的是你开源理解是拿别人的代码 clone
   你不需要账号验证登录，如果分支是public 甚至不需要初始化, 也不需要创建别名；
    下面有一个安全因素排除后，只要进入目录就自动进入新的master分支；
   
```
rapto@raptorzhangSurface MINGW64 /f/gitcolontest
$ git clone https://github.com/raptorzhang/code-edit-online.git
Cloning into 'code-edit-online'...
remote: Enumerating objects: 1392, done.
remote: Counting objects: 100% (1392/1392), done.
remote: Compressing objects: 100% (1242/1242), done.
remote: Total 1392 (delta 140), reused 1387 (delta 138), pack-reused 0
Receiving objects: 100% (1392/1392), 2.80 MiB | 60.00 KiB/s, done.
Resolving deltas: 100% (140/140), done.
Updating files: 100% (1336/1336), done.

rapto@raptorzhangSurface MINGW64 /f/gitcolontest

$ cd code-edit-online/
rapto@raptorzhangSurface MINGW64 /f/gitcolontest/code-edit-online
$ git remote -v
    fatal: detected dubious ownership in repository at 'F:/gitcolontest/code-edit-online'
   'F:/gitcolontest/code-edit-online' is on a file system that does not record ownership
   To add an exception for this directory, call:
   
    git config --global --add safe.directory F:/gitcolontest/code-edit-online
   
rapto@raptorzhangSurface MINGW64 /f/gitcolontest/code-edit-online
$ git config --global --add safe.directory F:/gitcolontest/code-edit-online
rapto@raptorzhangSurface MINGW64 /f/gitcolontest/code-edit-online (master)

```
###ssh免密登录 ssh-keygen -t rsa -C raptorzhang@163.com
    完成敲3次回车
   生成成功后把user/.ssh/id_rsa.pub 添加到github的后台的 setting/SSH and GPG keys
然后就开以登录了，首次会有一个confirm.以后就正常了！
```
$ ssh-keygen -t rsa -C raptorxxxxx@163.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/rapto/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/rapto/.ssh/id_rsa
Your public key has been saved in /c/Users/rapto/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:JmrzymI1fVjn6Wa6lfnWxyT/qXZsWfu3PHQxSyyRDFk raptorxxxxx@163.com
The key's randomart image is:
+---[RSA 3072]----+
|           o=H.  |
|           . o   |
|              o  |
|        . .  . = |
|     ..oSo .  o +|
|    o.oo. o  i.o+|
|   .+. . .  + o*=|
|  oo o    =. ooBB|
| . .o..  +...o=+X|
rapto@raptorzhangSurface MINGW64 /d/navfile (master)
$  git pull git@github.com:raptorzhang/code-edit-online.git master
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
From github.com:raptorzhang/code-edit-online
 * branch            master     -> FETCH_HEAD
Updating 6969aca..758fcb4
Fast-forward
 index.php | 5 +++++
 1 file changed, 5 insertions(+)

rapto@raptorzhangSurface MINGW64 /d/navfile (master)

```