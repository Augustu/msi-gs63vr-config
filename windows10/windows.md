### Windows

#### 注册表中修改系统文件夹路径的方法

```powershell
运行："regedit"打开注册表编辑器，找到[ HKEY__CURRENT USER/Software/Microsoft/Windows/CurrentVersion/Explorer/User Shell Folders],在右侧的窗格找到并双击对应的健值，在打开的窗口输入你的路径，按F5刷新并退出注册表。

改变系统默认安装路径
多数朋友安装软件时都不愿意默认安装到c:program files文件夹下，需要更改一下默认安装路径，一次又一次的更改肯定很烦人，现在告诉大家个更改默认安装路径的小技巧，大多数软件的默认路径都可以更改
1、开始---运行------regedit 进入注册表
2、进入注册表后定位到HKEY_LOCAL_MACHINE/SOFTWARE/Microsoft/Windows/CurrentVersion这个目录。
3、找到ProgramFilesDir这个项值，把默认的“c:\Program Files”这个项值更改为你想安装到的文件夹，如更改为“d:\Program Files”。

```



