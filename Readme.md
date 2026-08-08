简体中文 | 日本語
 
OwnDroid
 
通过安卓 DevicePolicyManager API 管理你的设备
 
下载
 
- IzzyOnDroid F-Droid 软件源
- GitHub 发布页面
 
注意：ColorOS 用户请前往 GitHub 发布页下载 testkey 测试密钥版本
 
功能特性
 
- 系统：禁用相机、关闭截图、主音量静音、禁用USB信号、锁定任务模式、清除设备数据……
- 网络：新增/修改/删除WiFi、网络统计、网络日志记录……
- 应用：冻结/隐藏应用、阻止应用卸载、授予/撤销权限、清空应用存储、安装/卸载应用……
- 用户限制：禁用短信、禁止呼出电话、关闭蓝牙、关闭NFC、禁止USB文件传输、禁止安装/卸载应用……
- 用户管理：查看用户信息、创建/启动/切换/停用/删除用户……
- 密码与锁屏：重置密码、设置屏幕超时时长……
 
工作模式
 
- 设备所有者模式（推荐）
激活方式：
- Shizuku
- Dhizuku
- Root权限
- ADB shell 命令：dpm set-device-owner com.bintianqi.owndroid/.Receiver
- Dhizuku 权限模式
- 工作资料模式
 
常见问题
 
设备中已存在账号
 
java.lang.IllegalStateException: Not allowed to set the device owner because there are already some accounts on the device
解决方案：冻结账号对应的宿主应用，或直接删除这些账号。
 
设备已有多个用户
 
java.lang.IllegalStateException: Not allowed to set the device owner because there are already several users on the device
解决方案：删除次级用户，包括工作资料、私密空间以及应用多开。
 
已设置其他设备所有者
 
java.lang.IllegalStateException: Trying to set the device owner (com.bintianqi.owndroid/.Receiver), but device owner (xxx) is already set.
一台设备仅可设置一个设备所有者，请先解除现有的设备所有者权限。
 
MIUI & 澎湃OS
 
java.lang.SecurityException: Neither user 2000 nor current process has android.permission.MANAGE_DEVICE_ADMINS.
解决方案：
 
- 在开发者选项中开启「USB调试（安全设置）」
- 或在Root权限终端执行激活命令
 
ColorOS系统
 
java.lang.IllegalStateException: Unexpected @ProvisioningPreCondition
解决方案：使用 OwnDroid testkey 版本
测试密钥版与正式签名版仅签名不同，功能完全一致。
 
三星设备
 
user limit reached
三星系统限制了安卓多用户功能，目前暂无解决方案。
 
创建工作资料 / 用户
 
当设备绑定设备所有者后，多数系统会禁止创建工作资料。
系统在设置设备所有者时，会自动添加 no_add_managed_profile 用户限制。
设备所有者无法修改系统级的用户限制；若设备已Root，可在ADB终端执行以下命令解除限制。
注意：设备所有者与工作资料所有者不能为同一个应用，否则重启后设备所有者权限会丢失。
 
pm set-user-restriction no_add_user 0
pm set-user-restriction no_add_managed_profile 0
pm set-user-restriction no_add_private_profile 0
pm set-user-restriction no_add_clone_profile 0
 
自行绕过限制风险自负，可能引发异常，例如系统在重启时静默删除新建的用户。
部分系统在设置设备所有者后，会隐藏系统设置内的新建用户入口。
你需要在OwnDroid内创建用户；若已Root，执行上方命令即可移除该限制。
 
进阶用户
 
API接口
 
OwnDroid提供基于Intent的API。你需要在设置中配置API密钥并开启API，括号内数字为最低支持的安卓版本。
 
- HIDE(包名: 字符串)
- UNHIDE(包名: 字符串)
- SUSPEND(包名: 字符串) (安卓7)
- UNSUSPEND(包名: 字符串) (安卓7)
- ADD_USER_RESTRICTION(限制项: 布尔值)
- CLEAR_USER_RESTRICTION(限制项: 布尔值)
- SET_PERMISSION_DEFAULT(包名: 字符串, 权限名: 字符串) (安卓6)
- SET_PERMISSION_GRANTED(包名: 字符串, 权限名: 字符串) (安卓6)
- SET_PERMISSION_DENIED(包名: 字符串, 权限名: 字符串) (安卓6)
- SET_SCREEN_CAPTURE_DISABLED()
- SET_SCREEN_CAPTURE_ENABLED()
- SET_CAMERA_DISABLED()
- SET_CAMERA_ENABLED()
- SET_USB_DISABLED() (安卓12)
- SET_USB_ENABLED() (安卓12)
- LOCK()
- REBOOT() (安卓7)
 
ADB shell 隐藏应用示例：
am broadcast -a com.bintianqi.owndroid.action.HIDE -n com.bintianqi.owndroid/.ApiReceiver --es key abcdefg --es package com.example.app
 
Kotlin 隐藏应用示例：
val intent = Intent("com.bintianqi.owndroid.action.HIDE")
.setComponent(ComponentName("com.bintianqi.owndroid", "com.bintianqi.owndroid.ApiReceiver"))
.putExtra("key", "abcdefg")
.putExtra("package", "com.example.app")
context.sendBroadcast(intent)
 
可用用户限制参数：https://developer.android.com/reference/android/os/UserManager#constants_1
 
开发者指南
 
编译构建
 
你可以在命令行使用Gradle编译OwnDroid项目
 
默认使用testkey密钥签名
 
./gradlew build
 
使用自定义 .jks 密钥文件签名
 
./gradlew build -PStoreFile="/path/to/your/jks/file" -PStorePassword="YOUR_KEYSTORE_PASSWORD" -PKeyPassword="YOUR_KEY_PASSWORD" -PKeyAlias="YOUR_KEY_ALIAS"
Windows系统请将 ./gradlew 替换为 gradlew.bat
 
代码贡献
 
请基于 dev 分支提交代码。
 
开源协议
 
License.md
 
版权所有 (C) 2026 BinTianqi
本程序为自由软件：你可以在 GNU 通用公共许可证第三版（或更新版本）的条款下重新分发、修改本程序。
发布本程序旨在发挥实用性，但不提供任何担保，包括适销性、特定用途适配性的默示担保。详情参阅许可证。
你应当随程序附带一份GPL许可证副本。若未收到，访问：https://www.gnu.org/licenses/