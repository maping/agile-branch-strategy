# Agile Branch Strategy

## 1. 在 Github 上创建 Repo
点击 Repositories，点击 New，输入 agile-branch-strategy

## 2. 初始化项目
```console
$ mkdir agile-branch-strategy
$ cd agile-branch-strategy
$ echo "# agile-branch-strategy" >> README.md
$ git init
$ git add README.md
$ git commit -m "first commit"
$ git remote add origin https://github.com/maping/agile-branch-strategy.git
$ git push -u origin master
```
> 说明：git remote add origin 这一步是添加远程仓库 origin。

> 说明：git push -u origin master 这一步是把代码推送到 远程仓库 origin 的 master 分支。

查看远程仓库
```console
$ git remote -v
origin	https://github.com/maping/agile-branch-strategy.git (fetch)
origin	https://github.com/maping/agile-branch-strategy.git (push)
```

查看分支
```console
$ git branch -v
* master b86be18 first commit
```

## 3. 创建 feature 分支
feature 分支是根据需求来的，每一个用户故事可以作为一个分支。
```console
$ git checkout -b feature/request-20191205-01 master 
Switched to a new branch 'feature/request-20191205-01'
$ git branch -v
* feature/request-20191205-01 b86be18 first commit
  master                      b86be18 first commit
```

### 3.1 在 feature 分支下工作
编写功能代码、单元测试代码、集成测试代码；执行单元测试和集成测试，完成后推送到 feature 分支。
```console
$ git add -A
$ git commit -m "finish feature/request-20191205-01"
$ git push origin feature/request-20191205-01
```
> 说明：git push origin feature 这一步是让 feature 分支远程可见。

### 3.2 合并 feature 分支到 master 分支
第一步，先合并 master 分支到 feature 分支，因为 master 分支在这个功能开发过程中很可能发生了更新
```console
$ git merge master  
Already up to date.
```
第二步，马上再对功能做一次进行单元测试和集成测试，确认合并 master 分支后，依然可以通过测试

第三步，合并 feature 分支到 master 分支
```console
$ git checkout master
$ git merge --no-ff feature/request-20191205-01
$ git push origin master // 推送 commit
```
> 说明：git merge --no-ff feature 这一步是合并提交 feature 分支的代码。

第四步，删除 feature 分支
feature 分支是临时分支，开发完毕后，应该删除
```console
$ git push origin :feature/request-20191205-01
```

## 4. 创建 bug 分支
测试人员在系统测试或验收测试环节测试出 bug 后，会提交一个新需求，开发人员领取任务，创建 bug 分支，修复该 bug。
```console
$ git checkout -b bug/bug-20191205-01 master 
Switched to a new branch 'bug/bug-20191205-01'
$ git branch -v
* master b86be18 first commit
```

### 4.1 在 bug 分支下工作
编写 bug 修复代码、单元测试代码、集成测试代码；执行单元测试和集成测试，完成后推送到 bug 分支。
```console
$ git add -A
$ git commit -m "finish bug/bug-20191205-01"
$ git push origin bug/bug-20191205-01
```
> 说明：git push origin bug 这一步是让 bug 分支远程可见。

### 4.2 合并 bug 分支到 master 分支
第一步，先合并 master 分支到 bug 分支，因为 master 分支在这个 bug 修复过程中很可能发生了更新
```console
$ git merge master  
Already up to date.
```
第二步，马上再对功能做一次进行单元测试和集成测试，确认合并 master 分支后，依然可以通过测试

第三步，合并 bug 分支到 master 分支
```console
$ git checkout master
$ git merge --no-ff bug/bug-20191205-01
$ git push origin master
```
> 说明：git merge --no-ff bug 这一步是合并提交 bug 分支的代码。

第四步，删除 bug 分支
bug 分支是临时分支，开发完毕后，应该删除
```console
$ git push origin :bug/bug-20191205-01
```

## 5. 创建 release 分支
正常情况下，所有测试通过之后，所有 bug 修复之后，可以正式发版了。
```console
$ git checkout -b release/1.0 master 
Switched to a new branch 'release/1.0'
$ git tag -a 1.0
```

## 6. 创建 hotfix 分支
用户在生产环境中使用过程中发现 bug 后，会提交一个新需求，开发人员领取任务，创建 hotfix 分支，修复该 bug。
```console
$ git checkout -b hotfix/hot-20191205-01 release/1.0 
Switched to a new branch 'hotfix/hot-20191205-01'
$ git branch -v
* master b86be18 first commit
```

### 6.1 在 hotfix 分支下工作
编写 bug 修复代码、单元测试代码、集成测试代码；执行单元测试和集成测试，完成后推送到 bug 分支。
```console
$ git add -A
$ git commit -m "finish bug/bug-20191205-01"
$ git push origin bug/bug-20191205-01
```
> 说明：git push origin bug 这一步是让 bug 分支远程可见。

### 6.2 合并 hotfix 分支到 master 分支
第一步，先合并 master 分支到 bug 分支，因为 master 分支在这个 bug 修复过程中很可能发生了更新
```console
$ git merge master  
Already up to date.
```
第二步，马上再对功能做一次进行单元测试和集成测试，确认合并 master 分支后，依然可以通过测试

第三步，合并 bug 分支到 master 分支
```console
$ git checkout master
$ git merge --no-ff bug/bug-20191205-01
$ git push origin master
```
> 说明：git merge --no-ff bug 这一步是合并提交 bug 分支的代码。

第四步，删除 bug 分支
bug 分支是临时分支，开发完毕后，应该删除
```console
$ git push origin :hotfix/hot-20191205-01
```

### 6.3 合并 hotfix 分支到 release 分支
第一步，先合并 master 分支到 bug 分支，因为 master 分支在这个 bug 修复过程中很可能发生了更新
```console
$ git merge master  
Already up to date.
```
第二步，马上再对功能做一次进行单元测试和集成测试，确认合并 master 分支后，依然可以通过测试

第三步，合并 bug 分支到 master 分支
```console
$ git checkout master
$ git merge --no-ff bug/bug-20191205-01
$ git push origin master
```
> 说明：git merge --no-ff bug 这一步是合并提交 bug 分支的代码。

第四步，删除 bug 分支
bug 分支是临时分支，开发完毕后，应该删除
```console
$ git push origin :hotfix/hot-20191205-01
```
