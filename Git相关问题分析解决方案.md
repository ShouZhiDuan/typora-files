# Git相关问题分析解决方案

## 1、Force Checkout和Smart Checkout区别

```tex
1. 本地分支切换的时候（例如A切到B），会弹出来Restore workspace on branch switching 对话框，如果选择是的话，在切换分支的时候，你在当前分支（A）所做的一些还未add或commit/push的文件改动（包括断点等的设置）会带到切换后的分支（B）上；
2. 如果本地工作空间没有uncommitted changes， 分支会顺利切换
3. 如果本地工作空间（分支A）有些文件会被分支B改动，IDEA会弹出对话框，让你选择Force Checkout 或 Smart Checkout;
如果选择Force Checkout， 本地工作空间（分支A）的一些未提交的修改会被覆盖（被分支B覆盖），会有很大可能丢代码！！！
如果选择Smart Checkout，IDEA会先执行stash命令，贮存这些未提交的修改，然后checkout 到分支B，在切换到分支B后，unstash 这些修改，所以A分支本地的这些修改会带到B分支上。
to see https://blog.csdn.net/shiyuehit/article/details/83010956
```

