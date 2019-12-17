[TOC]

## shortcut
### linux terminal shortcut
| 命令 | 说明 |
| ------ | ------ |
| ctrl+a,ctrl+e | 行首行尾 |
| ctrl+left | 向前单词末 |
| ctrl+right | 向后单词末 |
| ctrl+u | 删除行首到光标 |
| ctrl+k | 删除光标到行末 |
| ctrl+w | 向前删除一个单词 |
| ctrl+y | 撤销 |

### windows desktop shortcut
| 命令 | 说明 |
| ------ | ------- |
| win+d | 显示桌面 |
| alt+left,alt+right | 前进，后退 |
| win+[0-9] | 打开在任务栏的对应数字的窗口 |
| win+[left, right, up, down] | 窗口[靠左，靠右，最大化，还原] |
| win+E | 运行资源管理器 |

#### run, windows system app
| 命令 | 说明 |
| ------ | ------- |
| msconfig | 开机启动管理 |
| inetcpl.cpl | internet选项 |
| taskmgr | 任务管理器 |
| perfmon | 性能检测 |
| resmon | 资源检测 |
| appwiz.cpl | 软件卸载 |
| control | 控制面板 |
| control system | 系统 |
| sysdm.cpl | 系统属性，环境变量 |
| timedate.cpl | 时间日期 |
| fonts | 字体管理 |
| msinfo32 | system information |
| dxdiag | DirectX Diagnostic tool |
| regedit | 注册表 |
| services.msc | 服务 |
| ncpa.cpl | 网络连接器 |
| lusrmgr.msc | 用户和组管理 |
| win+x, a | administrator powershell |
| devmgmt.msc | 设备管理器 |

- open path or run a exe

```
start path
explorer path

start path/xxx.ext
```

- find text in file or files
```
find/findstr "string" file
```

- task

```
tasklist
taskkill /F /IM nginx.exe
# start on background
start /B nginx
```

- chcp
<table>
<tbody><tr id="option">
<td width="10%">識別子</td>
<td width="20%">略称</td>
<td width="70%">名前</td>
</tr>
<tr>
<td>437</td>
<td>IBM437</td>
<td>OEM United States</td>
</tr>
<tr>
<td>500</td>
<td>IBM500</td>
<td>IBM EBCDIC International</td>
</tr>
<tr>
<td>720</td>
<td>DOS-720</td>
<td>Arabic (Transparent ASMO); Arabic (DOS)</td>
</tr>
<tr>
<td>874</td>
<td>windows-874</td>
<td>ANSI/OEM Thai (ISO 8859-11); Thai (Windows)</td>
</tr>
<tr>
<td>932</td>
<td>shift_jis</td>
<td>ANSI/OEM Japanese; Japanese (Shift-JIS)</td>
</tr>
<tr>
<td>936</td>
<td>gb2312</td>
<td>ANSI/OEM Simplified Chinese (PRC, Singapore); Chinese Simplified (GB2312)</td>
</tr>
<tr>
<td>950</td>
<td>big5</td>
<td>ANSI/OEM Traditional Chinese (Taiwan; Hong Kong SAR, PRC); Chinese Traditional (Big5)</td>
</tr>
<tr>
<td>1200</td>
<td>utf-16</td>
<td>Unicode UTF-16, little endian byte order (BMP of ISO 10646); available only to managed applications</td>
</tr>
<tr>
<td>1250</td>
<td>windows-1250</td>
<td>ANSI Central European; Central European (Windows)</td>
</tr>
<tr>
<td>1252</td>
<td>windows-1252</td>
<td>ANSI Latin 1; Western European (Windows)</td>
</tr>
<tr>
<td>1254</td>
<td>windows-1254</td>
<td>ANSI Turkish; Turkish (Windows)</td>
</tr>
<tr>
<td>1258</td>
<td>windows-1258</td>
<td>ANSI/OEM Vietnamese; Vietnamese (Windows)</td>
</tr>
<tr>
<td>10000</td>
<td>macintosh</td>
<td>MAC Roman; Western European (Mac)</td>
</tr>
<tr>
<td>10001</td>
<td>x-mac-japanese</td>
<td>Japanese (Mac)</td>
</tr>
<tr>
<td>12000</td>
<td>utf-32</td>
<td>Unicode UTF-32, little endian byte order; available only to managed applications</td>
</tr>
<tr>
<td>12001</td>
<td>utf-32BE</td>
<td>Unicode UTF-32, big endian byte order; available only to managed applications</td>
</tr>
<tr>
<td>20127</td>
<td>us-ascii</td>
<td>US-ASCII (7-bit)</td>
</tr>
<tr>
<td>20932</td>
<td>EUC-JP</td>
<td>Japanese (JIS 0208-1990 and 0121-1990)</td>
</tr>
<tr>
<td>50220</td>
<td>iso-2022-jp</td>
<td>ISO 2022 Japanese with no halfwidth Katakana; Japanese (JIS)</td>
</tr>
<tr>
<td>50222</td>
<td>iso-2022-jp</td>
<td>ISO 2022 Japanese JIS X 0201-1989; Japanese (JIS-Allow 1 byte Kana - SO/SI)</td>
</tr>
<tr>
<td>51932</td>
<td>euc-jp</td>
<td>EUC Japanese</td>
</tr>
<tr>
<td>65000</td>
<td>utf-7</td>
<td>Unicode (UTF-7)</td>
</tr>
<tr>
<td>65001</td>
<td>utf-8</td>
<td>Unicode (UTF-8)</td>
</tr>
</tbody>
</table>

#### powershell

- show all variables
```
gci env:
```

#### explore
| 命令 | 说明 |
| ------ | ------- |
| ctrl+L | 地址栏 |
| ctrl+tab | 下一个标签 |
| ctrl+shift+tab | 上一个标签 |

## vim
#### vim shortcut
| 说明 | 命令 |
| ------ | ------- |
| [n] | 0~9,hjkl,Enter... |
| 行首行末 | Home,End&0,$ |
| 屏幕中跳转 | H M L |
| 文章头行尾行 | gg,G |
| 文章行 | [n]gg |
| 上几个单词 | b, [n]b |
| 下个单词 | w,e,[n]w |
| 剪切整行 | cc, dd, [n]cc, [n]dd |
| 复制整行 | yy, [n]yy |
| 粘贴 | p |
| 光标前后insert | a, i |
| 句子前后insert | A, I |
| 句子前后行insert | o, O |
| 缩进 | >>,<< |
| 搜索,上一个,下一个 | /[关键字], n, N, [n]N |
| 执行shell | ![command] |
| undo, redo | u, ctrl+r |

- 分屏
splite 横向, vsplite纵向, ctrl+w切换
:tabnew 分页操作
:tabonly 关闭其他分页
:tabclose

#### vim setting

| 命令 | 说明 |
| ------ | ------- |
| set all | 查看所有配置 |
| set nu/set nonu | 行号显示 |
| set autoindent | 自动缩进 |
| set shiftwidth=[n] | >>,<<缩进的大小 |
| set tabstop=[n] | tab的时候缩进的大小 |
| set expandtab | tab to space |
| set nohls | 取消搜索选中 |
| set encoding=utf8, set fileencoding | 设置编码 |

v 选择
ctrl v 纵向选择
批量行缩进,批量操作
1, ctrl+v纵向选择; 2, shift+i批量输入; 3,空格字符任意填写; 4,esc
批行删除第一个字符
1, ctrl+v纵向选择; 选择一列; 2, x

q recording

设置变量
let 变量=值

map, nmapショートカットを設定する

##### ideal
```
ctrl+alt+T, 环绕代码块
alt+enter
ctrl+shift+enter: 分号

```



