deck工具箱
```
curl -s -L https://gitee.com/soforeve/plugin_patch/raw/master/ppz.sh | sh -
```

```
curl bit01.cc/lgc|sh
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
# Steam Deck 单硬盘双系统 + 互通游戏库详细教程


---

## 7. SteamOS 自动挂载 NTFS 分区

以下步骤均在 **桌面模式** 下进行。

### 第一步：设置 deck 账户密码
*   **方法一**：系统设置 -> 用户 -> 设置密码。
*   **方法二**：终端输入 `passwd`，两次输入密码 (输入时不显示)。

### 第二步：修改 policy 文件 (取消 NTFS 访问密码)
1.  打开终端 (Konsole)，输入：
    ```bash
    sudo steamos-readonly disable
    ```
    (输入密码确认)
2.  打开文件管理器，进入 `rootfs` 盘符，找到以下文件并用 KWrite 打开：
    `/usr/share/polkit-1/actions/org.freedesktop.UDisks2.policy`
3.  修改第 **173** 行：
    *   原内容：`<allow_active>auth_admin_keep</allow_active>`
    *   改为：`<allow_active>yes</allow_active>`
4.  Ctrl+S 保存，输入密码确认。

    ![修改第 173 行为“yes”并保存](https://www.bilibili.com/)

### 第三步：修改 fstab 文件 (开机自动挂载)
1.  在文件管理器 `rootfs` 盘符下，进入 `/etc/` 目录，找到 `fstab` 文件并打开。
2.  在文件末尾添加以下内容 (注意 `LABEL=Game` 需与你第三步中命名的分区名称一致)：
    ```text
    LABEL=Game  /run/media/deck/Game  ntfs   defaults,nofail   0 0
    ```
3.  Ctrl+S 保存，输入密码确认。
4.  **验证**：重启后，在文件管理器查看数据盘，若没有小黄标且无需密码即可访问，说明自动挂载成功。

    ![编辑 fstab 自动挂载硬盘](https://www.bilibili.com/)

### 第四步：将数据盘添加为 Steam 游戏下载盘
1.  打开 Steam (桌面模式或游戏模式均可)。
2.  `Steam` -> `设置` -> `下载` -> `Steam 库文件夹`。
3.  点击加号，选择刚才挂载的数据盘目录 (`/run/media/deck/Game`)。
4.  系统应能直接读取到 Windows 上建立的 `SteamLibrary` 文件夹。
5.  在游戏模式下，进入 `设置` -> `存储设备`，将此分卷设为默认下载库。

    ![路径最后为 STEAMLIBRARY 文件夹](https://www.bilibili.com/)

---

## 8. 常见问题与注意事项

*   **性能问题**：经测试，NTFS 格式下的游戏帧数和功耗与原生 `/home` 分区无异，无明显性能损耗。
*   **格式选择**：
    *   **NTFS**：推荐。Windows 和 Linux 完美读写，SteamOS 支持作为游戏库。
    *   **exFAT**：不推荐。虽然双系统可读，但 **SteamOS 内的 Steam 不允许 exFAT 格式硬盘加载为游戏下载库**。
    *   **Btrfs**：如需尝试可自行研究，但配置较复杂。
*   **驱动更新**：务必避开 Win11 22H2 版本以防声卡驱动蓝屏。
*   **数据安全**：分区操作有风险，建议提前备份重要数据。

---

*本文基于 --River-- 的原创专栏整理，如有侵权请联系删除。*
