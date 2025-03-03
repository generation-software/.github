Paion Data Community Health Files
=================================

代码开发流程
----------

无论是开发一项新功能还是修一个 bug，都要以 pull request 的形式提交代码，并且通过审核之后，才能合并到 `master` 主线分支

### 新建分支

```console
git checkout -b <分支名>
```

`<分支名>` 没有严格的要求，一般的形式是 `issue号码-描述`，英文空格用中划线代替，比如 `issue7-implement-login-form`

### 提交代码

一个好的习惯是先用

```console
git status
```

看一下修改的文件是否符合预期，没问题就可以提交代码：

```console
git add .
git commit -S -m "<commit message>"
git push origin <分支名>
```

- `<commit message>` 可以是中英文（但给国际开源社区提交代码则必须是英文），主要描述一下这次提交的内容
- `-S` 指定 GPG 签名，请参阅[这个贴子](https://stackoverflowteams.com/c/paion-data/q/22/1)配置自己的 GPG 签名


### [新建 Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request#creating-the-pull-request)

新建 Pull Request 之前我们会看到这样的内容：

```
Changelog
---------

### Added

### Changed

### Deprecated

### Removed

### Fixed

### Security

Checklist
---------

- [ ] Test
- [ ] Self-review
- [ ] Documentation
```

这是每一个 PR 的 [Changelog](https://keepachangelog.com/en/1.1.0/)，这是写给代码审核人看的，我们要填写一下这个 PR 做了一件什么事情，比如，如果是修一个 Bug，就在 `### Fixed` 栏目下介绍一下修了一个什么样的 Bug。

- Added：实现了一个新功能
- Changed：修改了一项功能的实现
- Deprecated：一个函数/模块/用户功能不再使用，将在下一个发布版本删除
- Removed：正式删除了一个不再使用的函数/模块/用户功能
- Fixed：修复了一个 Bug
- Security：修复了一个安全漏洞

**Checklist** 是自己做记录用的：

- Test：有没有给代码写测试？
- Self-review：有没有自审代码？
- Documentation：有没有文档

### Pull Request 合并之后一定要记得更新本地 `master` 分支

```console
git checkout master
git pull origin master
```
