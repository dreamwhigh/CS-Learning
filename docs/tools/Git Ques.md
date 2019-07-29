#### Git 常见问题解决方法

##### 推送到 GitHub 报错

fatal: remote origin already exists. 报错远程起源已经存在

先移除当前远程库，再重新关联远程库

```
$ git remote rm origin
$ git remote add origin git@github.com:dreamwhigh/test.git
```

