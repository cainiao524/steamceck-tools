deck工具箱
```
curl -s -L https://gitee.com/soforeve/plugin_patch/raw/master/ppz.sh | sh -
```

https://github.com/panyiwei-home/Friendeck

https://github.com/panyiwei-home/Freedeck

[直读 NTFS 文档](直读ntfs.md)
第二步：修改policy文件，取消ntfs分区访问密码

    首先，打开steamos终端（桌面模式左下角steam标志菜单-系统-Konsole），输入如下命令：（会要求输入密码，此时输入内容是隐藏的，属于正常现象。输入密码后回车就可以）

```
sudo steamos-readonly disable
```

   使用steamos自带的文件管理器，在左侧竖向的选择栏最下面找到进入rootfs盘符，根据如下目录，找到对应文件，右键打开方式，使用kwrite打开文件。
```
/usr/share/polkit-1/actions/org.freedesktop.UDisks2.policy
```
    修改第173行
```
<allow_active>auth_admin_keep</allow_active>
改为
<allow_active>yes</allow_active>
```
    按ctrl+s保存，会弹出要求输入密码的框，输入此前设置的账户密码，保存成功。

修改第173行为“yes”并保存


第三步：修改fstab文件实现开机自动挂载

11月14日修改：此部分此前使用UUID指定分区实现挂载，在参考了这位大佬的教程后采用更简单的LABEL指定分区。

SteamDeck Windows 和 Steamos 共享游戏库教程

为了更好的阅读体验，建议访问我的原博客 https://blog.njzydark.com/posts/steamdeck-windows-steamos-share-games完整的 SteamDeck Windows 和 Steamos 共享游戏库教程，实现 Windows 和 steamos 可以运行同一个分区上的游戏，避免重复下载安装如何安装双系统可见我之前的教程 SteamDeck Windows 单盘双系统安装教程 https://www.bilibili.com/read/cv19


方法：还是文件管理器，找到你名为rootfs的盘符，进入/etc/目录，找到名为fstab的文件，双击打开。将以下内容添加到fstab内容的最后，如图所示。此处LABEL=你在本文第三部分第二步中给互通分区的命名。
```
rootfs/etc/fstab
```
```
/run/media/deck/Game
```
为分区的挂载点，并非指定某文件夹，请谨慎修改。
```
LABEL=Game  /run/media/deck/Game  ntfs   defaults,nofail   0 0
```
编辑fstab自动挂载硬盘
    4. Ctrl+s，输入密码保存文件，至此开机自动挂载数据盘分区的操作完成。在文件管理器访问你的数据盘测试一下，原本盘符应该带了一个小黄标，如果访问此盘不需要密码，且小黄标消失即为成功。或者重启一下，进文件管理器查看，如果在不点击的情况下数据盘没有小黄标，即说明此硬盘开机自动挂载了。 作者：--River-- https://www.bilibili.com/read/cv18881740/?from=search&spm_id_from=333.337.0.0&opus_fallback=1 出处：bilibili
