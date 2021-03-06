### 撤销、重做和重复
u  // 复原前一个动作

[Ctrl]+r // 重做上一个动作

. // 重复前一个动作

### 选中文本(可视模式)
- 要选择文本，需要先使用*Visual*命令切换到**可视模式**
- vi中提供了三种可视模式，可以方便程序员选择选中文本的方式
- 按 **ESC** 可以放弃选中，返回到命令模式

| 命令     | 模式       | 功能                               |
| :------- | :--------- | :--------------------------------- |
| v        | 可视模式   | 从光标位置开始按照正常模式选择文本 |
| V        | 可视行模式 | 选中光标经过的完整行               |
| Ctrl + v | 可视块模式 | 垂直方向选中文本                   |

- **可视模式**下，可以和 **移动命令** 连用，例如：*ggVG* 能够选中所有内容

### 复制一段文字
ctrl + v 进入块选择模式<br/>
复制选定块到缓冲区，用y;<br/>
剪切选定块到缓冲区，用d;<br/>
粘贴缓冲区中的内容，用p;

### 显示/取消行号
```
:set nu 	显示行号，设定之后，会在每一行的前缀显示该行的行号

:set nonu 	与 set nu 相反，为取消行号！
```

### 删除全部文件内容
```
:%d
```

