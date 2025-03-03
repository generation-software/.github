Paion Data Community Health Files
=================================

<!-- TOC -->
* [Paion Data Community Health Files](#paion-data-community-health-files)
  * [代码开发流程](#代码开发流程)
    * [新建分支](#新建分支)
    * [提交代码](#提交代码)
    * [新建 Pull Request](#新建-pull-request)
    * [Pull Request 合并之后一定要记得更新本地 `master` 分支](#pull-request-合并之后一定要记得更新本地-master-分支)
<!-- TOC -->

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
- `-S` 指定 GPG 签名，配置自己的 GPG 签名请参阅下文”创建并使用 GPG key 提交代码“

> [!TIP]
> 
> __创建并使用 GPG key 提交代码__
> 
> GPG key 的作用是对自己的代码进行签名认证，向 GitHub 上的开发者证明某一段代码确实是你自己写的，提高代码在公司团队中的的安全性
> 
> ![gpg-verified-with-expired-key](https://github.com/user-attachments/assets/d12c64d5-58e2-4e6b-a8b4-bba99b1c993f)
> 
> 下文的 Answer 详细罗列如何在本地新建一个 GPG key，将其上传到自己的 GitHub 账号，并在提交代码的时候用其进行签名。
> 
> 1. 打开终端，执行
> 
>    ```bash
>    gpg --full-generate-key
>    ```
> 
>    - key size 必须是 **4096** bits
>    - ***email 必须是 GitHub 注册邮箱***
> 
> 2. 获取 GPG key ID：
> 
>    ```bash
>    gpg --list-secret-keys --keyid-format=long
>    ```
> 
>    得到如下结果
> 
>    ```
>    $ gpg --list-secret-keys --keyid-format=long
>    /Users/hubot/.gnupg/secring.gpg
>    ------------------------------------
>    sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
>    uid                          Hubot <hubot@example.com>
>    ssb   4096R/4BB6D45482678BE3 2016-03-10
>    ```
> 
>    在上面的栗子中， **GPG key ID 是 `3AA5C34371567BD2`**。后续如果需要删除本地 GPG key 可使用
>    
>    ```bash
>    gpg --delete-secret-keys 3AA5C34371567BD2
>    ```
> 
> 3. 配置 Git，在提交代码的时候自动使用这个 GPG key
> 
>    ```bash
>    git config --global user.signingkey 3AA5C34371567BD2
>    ```
> 
>    然后，将这个 ID 复制粘贴到如下指令中，得到 GPG key 的 ASCII armor 原文格式：
> 
>    ```bash
>    gpg --armor --export 3AA5C34371567BD2
>    ```
> 
> 4. 复制 GPG key（包含 `-----BEGIN PGP PUBLIC KEY BLOCK-----` 和 `-----END PGP PUBLIC KEY BLOCK-----` 以及中间的内容）
> 5. 上传 GPG key 至 GitHub：请参照 [GitHub 官方文档](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
> 6. 使用 GPG key 提交代码。第一次提交代码之前，配置 Git 提交使用的邮箱：
> 
>    ```console
>    git config --global user.email "your_email@abc.example"
>    ```
>
>    - **`your_email@abc.example` 必须是 GitHub 注册邮箱**
>    - 如果是首次使用 `git`，还需要执行 `git config --global user.name "<你的 github username>"`
> 
> 7. 正式提交代码：
> 
>    ```bash
>    git commit -S -m "<commit message>"
>    ```
> 
>    - `-S` 代表使用 GPG key 进行签名
>    - `<commit message>` 根据每次代码提交的内容确定
> 
> __Troubleshooting__
> 
> - ❌ `gpg: signing failed: Inappropriate ioctl for device`：✅ 如果在 Mac 操作系统下 commit 遇到
>   **gpg: signing failed: Inappropriate ioctl for device** 的报错，
>   [先执行](https://github.com/keybase/keybase-issues/issues/2798#issue-205008630)
> 
>   ```bash
>   export GPG_TTY=$(tty)
>   ```
> 
>   再重新运行 `git commit -S -m "<commit message>"` 即可
> 
> - ❌ Commit 之时，出现 `Git commit failed : Couldn't load public key`：✅ 在 Windows 上使用 Gpg4Win 的时候出现
> 
>   ```console
>   error: Couldn't load public key 632EA751459C3A1A: No such file or directory?
> 
>   fatal: failed to write commit object
>   ```
> 
>   [执行](https://stackoverflow.com/questions/73726815/git-commit-failed-couldnt-load-public-key#comment135267740_73816112)
> 
>   ```console
>   git config --global --unset gpg.format
>   ```

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
