# Git的基本使用

Git相关文档：https://git-scm.com/book/zh/v2

1、安装git

https://git-scm.com/

2、设置git用户名、邮箱

```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

3、查看git用户名、邮箱

```
git config user.name
git congig user.email
```

4、克隆代码

```
git clone https://github.com/inseeker/ChatGPT-Next-Web.git
```

5、查看当前状态

```
git status
```

6、查看本地及远程所有分支

```
git branch -a
```

7、进行远程的更新

```
git remote update
```

8、创建一个新分支并切换

```
git checkout -b dev

or

git branch dev
git checkout dev
```

9、添加增删改/提交

```
git add -A
git commit -m "What has changed"
```

10、把本地分支推到远程

```
git push --set-upstream origin dev
```