[TOC]

# 记录关于git的一些问题

## 关联远程仓库为origin
+ Q: 将远程仓库代码克隆到本地后遇到远程仓库代码更新,与此同时需要更新本地代码;执行`git pull origin master`提示错误`'origin' does not appear to be a git repository`
+ A: 原因是还未与远程仓库关联为origin,因此执行`git remote add origin git@git.leadstecgz.com:fltang/oppo-campaign-site.git`关联远程仓库为origin
