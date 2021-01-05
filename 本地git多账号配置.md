# Windows下配置多个Git账号

**1. 为什么会用多个git账号？**

- 不同git账号对应不同代码托管平台，如：github、bitbucket、gitlab、gitee（码云）等
- 2个GitHub账号，用于测试（最近需求，之前没研究过github协同开发，所以注册了个小号来测试）

**2. 不同git账号对应不同代码托管平台，以github和bitbucket为例**

- github：https://github.com/，你在github使用的账号用户名为：github_user
- bitbucket：https://bitbucket.org/，你在bitbucket使用的账号用户名为：bct_user

1）假设你已经生成了不同的 sshkey，其中针对不同的平台可以命名如下：

- github：id_github_rsa
- bibucket：id_bitbucket_rsa
- 注意：为了管理方便，把id_github_rsa、id_github_rsa.pub 和 id_bitbucket_rsa、id_bitbucket_rsa.pub都移到同一目录下

2）我的移动到 /c/Users/xxx/.ssh/目录下，在该目录下手动创建文件“config”（无后缀名），编辑config文件，如下：

```tex
# github account [github_user]
Host github.com
HostName github.com
User github_user
IdentityFile /c/Users/xxx/.ssh/id_github_rsa
IdentitiesOnly yes

# bitbucket account [bct_user]
Host bitbucket.org
HostName bitbucket.org
User btc_user
IdentityFile /c/Users/xxx/.ssh/id_bitbucket_rsa
IdentitiesOnly yes
```

3）通过上述配置后，你就可以使用多个git账号操作对应的代码托管平台（假设你使用同一个email注册不同的平台，否则的话，需要取消全局email和username设置，并且在使用前通过git config命令切换账号，具体请看 **3. 同一个平台GitHub下的不同git账号**）

**3. 同一个平台（GitHub为例）下的不同git账号**

1）config 配置如下：

```tex
# github account [user1]
Host github.com
HostName github.com
User user1
IdentityFile /c/Users/xxx/.ssh/id_user1_rsa
IdentitiesOnly yes

# github account [user2]
Host github.com
HostName github.com
User user2
IdentityFile /c/Users/xxx/.ssh/id_user2_rsa
IdentitiesOnly yes
```

2）在使用时需要注意，不能设置全局的 username 和 email

```tex
# 取消全局 username, email
>git config --global --unset user.name
>git config --global --unset user.email
```

3）如要在repo中使用 user1 进行操作，进入repo目录后，先设置username 和 email，再进行其他操作

```tex
>git config user.name "user1"
>git config user.email "user1登陆GitHub的email"
# 同样的，如果想在repo中切换为 user2 进行操作，则重新设置username和email（同上）后，再进行其他操作
```

**4. 可能遇到的问题**

**1）git push：remote: Permission to XXXA/xxxx.git denied to XXXB**

原因：

- 之前为了测试GitHub的一些机制，我在同一个电脑上配置了两个GitHub账号：southday | lcxv
- 最近在向southday账号下的项目push代码时，出现：remote: Permission to XXXA/xxxx.git denied to lcxv
- 这是由于电脑使用git bash配过SSH，系统已经将指向[github.com](http://github.com/)的用户设置为了lcxv，每次push操作的时候，默认读取保存在本地的用户lcxv

解决方法：

- 解决这个问题最简单的办法是删除本机中GitHub的lcxv登陆session
- 重新提交的时候，git会要求你输入新的用户名和密码，输入后就可以成功提交了

**5. 参考内容**

- [多git帐号的SSH key切换](http://justcoding.iteye.com/blog/2265628)
- [Windows下Git多账号配置，同一电脑多个ssh-key的管理](https://www.cnblogs.com/popfisher/p/5731232.html)
- [git 指定sshkey访问远程仓库](https://segmentfault.com/a/1190000005349818)

转载于：https://www.cnblogs.com/southday/p/10011261.html