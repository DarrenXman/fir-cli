FIR.im CLI
---
> FIR.im CLI 可以通过指令查看、上传、编译应用，同时还集成了第三方网站 [resign.tapbeta.com](http://resign.tapbeta.com) 进行企业签名以方便 inhouse 测试。

## 新指令 `build_ipa`
> 这个指令对 `xcodebuild` 这个原生指令进行了包装，将常用的参数名简化，同时支持全部的自带参数和设置。

### 编译并获得 ipa

```
$ fir build_ipa path/to/project -o path/to/output
> 欢迎使用 FIR.im 命令行工具，如需帮助请输入: fir help
> 正在编译
> 正在打包 app： demo.app 到 path/to/output/demo.ipa
> 完成
```

### 复杂一点
```
$ fir build_ipa path/to/workspace -o path/to/output -w -C Release -s allTargets GCC_PREPROCESSOR_DEFINITIONS="FOO=bar"
```

该指令在指向的目录中，找到第一个 workspace 文件，对其进行编译。使用 `Release` 设置，编译策略为 `allTargets`，同时设置了预编译参数 `FOO`。

### 一步，从源代码到 FIR.im
> 只需要多输入一个 -p

```
$ fir build_ipa path/to/project -p
> 欢迎使用 FIR.im 命令行工具，如需帮助请输入: fir help
> 正在编译
> 正在打包 app： demo.app 到 /var/folders/z7/7_cp03nx0535g0cd6ynfmxt80000gn/T/d20141218-13295-1oe0zlm/demo.ipa
> 完成
> 正在发布 demo.ipa
> 正在解析 ipa 文件...
> 正在获取 im.fir.demo@FIR.im 的应用信息...
> 上传应用...
> 上传应用成功
> 正在更新 fir 的应用信息...
> 更新成功
> 正在更新 fir 的应用版本信息...
> 更新成功
> 正在获取 im.fir.demo@FIR.im 的应用信息...
> http://fir.im/xxxx
```

## 使用入门
### 从安装入手

FIR.im CLI 使用 ruby 构建，只要安装相应 ruby gem 即可：

```shell
$ sudo gem install fir-cli
```

安装后，你可以在命令行执行指令

```shell
$ fir
> 欢迎使用 FIR.im 命令行工具，如需帮助请输入: fir help
Commands:
  fir build_ipa PATH [options] [settings]  # 编译 ios app 项目
  fir config                               # 配置全局设置
  fir help [COMMAND]                       # Describe available commands or one specific command
  fir info APP_FILE_PATH                   # 获取应用文件的信息（支持 ipa 文件和 apk 文件）
  fir login                                # 登录
  fir profile                              # 切换配置文件
  fir publish APP_FILE_PATH                # 将应用文件发布至 FIR.im（支持 ipa 文件和 apk 文件）
  fir resign IPA_FILE_PATH OUTPUT_PATH     # 使用 resign.tapbeta.com 进行企业签名
  fir upgrade                              # 更新 fir-cli 的所有组件
  fir version                              # 当前版本
```

### 发布一个应用

输入下面的指令便可轻松发布应用

```shell
$ fir publish 应用路径
```

这时系统会提示输入用户 token

```shell
> 欢迎使用 FIR.im 命令行工具，如需帮助请输入: fir help
> 正在解析 ipa 文件...
> 正在获取 im.fir.juo@FIR.im 的应用信息...
请输入用户 token：
```

输入用户 token 后，系统会自动上传

```shell
请输入用户 token：xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
> 上传应用...
> 上传应用成功
> 正在更新 fir 的应用信息...
> 更新成功
> 正在更新 fir 的应用版本信息...
> 更新成功
> http://fir.im/xxxxx
```

用户 token 可在[这里](http://fir.im/user/info)查看

### 方便一点

如果觉得每次都输入用户 token 很不方便，那么可使用登录命令

```shell
$ fir login
```

这时系统会提示输入用户 token

```shell
> 欢迎使用 FIR.im 命令行工具，如需帮助请输入: fir help
输入你的用户 token： 
```
输入用户 token，系统会自动获取你的用户 email
```shell
输入你的用户 token：xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
> 设置用户邮件地址为: dy@fir.im
> 当前登陆用户为：xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```


### 需要企业签名？

很多开发者需要一个企业签名的应用，来让更多的用户参与到测试中，这种行为并不符合企业证书的使用规范。但是我们还是集成了第三方签名网站 [resign.tapbeta.com](http://resign.tapbeta.com) 来帮助我们的用户更方便的进行测试。

```shell
$ fir resign ipa文件路径 输出文件路径
```

这条指令会输出一段风险提示；如果没有设置邮件地址，这里会让你输入邮件地址。输入邮件地址后，便开始进行企业签名了

```
> 欢迎使用 FIR.im 命令行工具，如需帮助请输入: fir help
! resign.tapbeta.com 签名服务风险提示
! 无法保证签名证书的长期有效性，当某种条件满足后
! 苹果会禁止该企业账号，所有由该企业账号所签发的
! 证书都会失效。你如果使用该网站提供的服务进行应
! 用分发，请注意：当证书失效后，所有安装了已失效
! 证书签名的用户都会无法正常运行应用；同时托管在
! fir.im 的应用将无法正常安装。
请输入你的邮件地址： dy@fir.im
> 正在申请上传令牌...
> 正在上传...
> 正在排队...
> 正在签名...
> 正在下载到 /path/to/output.ipa...
```


### 一步到位

企业签名后自动发布到 [FIR.im](http://fir.im)

```shell
$ fir publish ipa文件路径 -r
```

## 需要帮助？

输入以下指令获取全面功能介绍

```shell
$ fir help
```

如果还有疑问随时发邮件至[fir-cli](mailto:fir-cli@fir.im)

## 永远使用最新功能

下面的指令会自动更新 fir-cli 及所有扩展命令至最新状态

```shell
$ fir upgrade
```

随时更新以使用最新功能

## 更新记录
### FIR-cli 0.2.0
- 新指令 `build_ipa`

### FIR-cli 0.1.8
- 支持 ruby 1.9.x
- 规范输出参数选项，支持无颜色信息输出
  -`--verbose=v|vv|vvv`：设置输出级别
  -`--quiet` 与 `--no-quiet`：设置是否不输出辅助信息
  -`--color` 与 `--no-color`：设置输出是否携带颜色信息
- 修复 ipa 应用图标不清晰问题
- 增加切换配置文件功能：使用此功能可以在多个用户中切换使用
