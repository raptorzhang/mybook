# git 分支

##查看分支 git branch -v

```
$ git branch -v
* master f370f61 第二次修改提交本地库

```

##创建分支 git branch name 

类似于创建一个副本；

```
$ git branch name
$ git branch -v
* master f370f61 第二次修改提交本地库
  name   f370f61 第二次修改提交本地库

```


##切换分支 git checkout name

```
rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (master)
$ git checkout name
Switched to branch 'name'

rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
$ git branch -v
  master f370f61 第二次修改提交本地库
* name   f370f61 第二次修改提交本地库

```

##修改分支上的内容

```
    $ vim hello.txt
   
   rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
   $ git status
   On branch name
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   hello.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
   $ git commmit -m "name分支的第一次修改！" name
   git: 'commmit' is not a git command. See 'git --help'.
   
   The most similar command is
           commit
   
   
   rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
   $ git add hello.txt
   
   rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (name)
   $ git commit -m "name分支的第一次修改！" hello.txt
   [name 9690b39] name分支的第一次修改！
    1 file changed, 3 insertions(+)
   
```

##合并分支 git merge name

    当前分支与name分支进行合并；
```
$ git checkout master
Switched to branch 'master'

rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (master)
$ git merge name
Updating f370f61..9690b39
Fast-forward
 hello.txt | 3 +++
 1 file changed, 3 insertions(+)

rapto@raptorzhangSurface MINGW64 /d/gitTest/gitDemo (master)

```    

当冲突合并的时候，你会处于1个以合并命名的临时分支，整理好代码commit就可以退出这个临时的分支
        
    
    