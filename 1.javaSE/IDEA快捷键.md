本文参考了 IntelliJ IDEA 的官网，列举了IntelliJ IDEA（Windows 版）的所有快捷键。并在此基础上，为 90% 以上的快捷键提供了动图演示，能够直观的看到操作效果。

该快捷键共分 16 种，可以方便的按各类查找自己需要的快捷键~~



# 一、构建/编译

### `Ctrl + F9`：构建项目

> 该快捷键，等同于菜单【Build】—>【Build Project】

![图片](https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIM3KqjG0riavLxK8uwjxhusC8x0gyYibF7GLhtcQZn08VxMsCicucS4icSoqjnqqqCPMOkhz04vA8eY6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

执行该命令后，IntelliJ IDEA 会编译项目中所有类，并将编译结果输出到`out`目录中。IntelliJ IDEA 支持增量构建，会在上次构建的基础上，仅编译修改的类。
![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCT4w7dcr5k9plxQzx6FEuRLBr6x88Wj2w9iaj1cbegfWbZgmVyicw3dOQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + F9`：重新编译当前类

> 该快捷键，等同于菜单【Build】—>【Recompile ‘class name’】

![图片](https://mmbiz.qpic.cn/mmbiz_png/QCu849YTaIM3KqjG0riavLxK8uwjxhusC7astlQLicGw6XVPADwIl3TO3ZuDSIzpFhBmEfUI0fX6ricdTBkpp9DkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
在IntelliJ IDEA 中打开要编译的类，执行该命令会编译当前类。
![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC1oicD5axBMicjtWHad4wzEJ8d81lOFAmXUCFLfevkdzFqLdicgoBOORZA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 二、文本编辑

### `Ctrl + X`：剪切

剪切选中文本，若未选中则剪切当前行。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusClrC3XR7fKiayeQTtCIYibqAhcq0yqhiagpQoSWEIMicLS3ichEu4CEXna2g/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + C`：复制

复制选中文本，若未选中则复制当前行。

### `Ctrl + V`：粘贴

### `Ctrl + Alt + Shift + V`：粘贴为纯文本

### `Ctrl + Shift + V`：从历史选择粘贴

从历史剪粘版中选择要粘贴的内容。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCgabMYrUdXDh69UbRkf2L2xibIlvS3yzZ9MjOkrib1H7ia9zU1C0Xx5FAA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + D`：复制行

复制光标所在行。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUBgyzZ01iaibe9aFlJkFMibQKZwRvLWUcWatpQPhQiaG9jc9a35libfz2OA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + C`：复制文件路径

复制选中文件所在路径。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCHkpl1vQMeXPp9MH2rN2c6a3fpncib2JZc2r8gNPYpIFvkbnJkpd7ESg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + C`：复制引用

复制包的路径，或者类的名称。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCYapVm0Z0VV0P7CCXYr8hd1moNHjuqXRia9lEicHhL7s7pPlwsY4nlueg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + S`：保存全部

### `Ctrl + Z`：撤销

撤销上一步操作内容。

### `Ctrl + Shift + Z`：重做

恢复上一步撤销内容。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUYfcibC28ucUuiaqBRBeNku6lPygY07CPwaMvTzEbKYwwdu26ufIjPQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Tab`：缩进

### `Shift + Tabl`：取消缩进

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + I`：自动缩进行

自动缩进至规范位置。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCzXIibSSHatbialfjwicNvukQficMndItSXic19UZMDrFtyhQxevNKmm4nXg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + Enter`：开始新行

无论光标是否在行尾，都开始新的行。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCKvBhia9rbRChRbJWibzmx5eXv39noUEWnjtaISackKiceXPBkIW561NPg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Enter`：在当前行之前开始新行

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC38nQIqgOOr56POvWXUEdakOUZbSeTLeA2LyiaqSKYHLsqaicNEFAbctw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Y`：删除行

删除当前行。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC8Pg43G9bZlW2LoBehSaY4mhvsuEH4lLQtF10jNKMoiac40v0CqnRvZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + U`：大小写转换

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC10STMYElibsogB6yC3L7y9gsyxoCBOZlt24sNu6MrPQZbPebJiauTW4A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + Insert`：创建临时文件

可以创建各种类型的临时文件，该临时文件不会保存到磁盘中。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Shift + F4`：在新窗口中打开

在新窗口打开当前文件。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUrNw6kwT8gcX2AFA7nI8m3PWxgibQwZBTBQ7PtoAibL0w1WS7Rj2GhVA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 三、光标操作

### `Ctrl + Left`：左移一个单词

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCpX9HhBbCBAib626icwia4WwIRibqRl4t7fIiaNHUE4ZXngRFqfhRIWWlIdg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Right`：右移一个单词

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFwxODc0q5JoNRxVD3YJeWBL87DLNbnYxQDWJkoVjicyGQiaicUSFRh7NA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Home`：移动至行首

### `End`：移动至行尾

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQIfKw81gicaibUhhia6RhicXrfI3WzMmbpMeVVsWCaw5bWSKJ6BRnell3Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + M`：移动至大括号

多次按下快捷键，可以在左右两个大括号间切换。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCf9vbTCBphk9Tq0b6Oh3rR7wf8IbjVE6LDOtRByXmia25N3uRCK6Jx1Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + [`：移动至代码块开始

### `Ctrl + ]`：移动至代码块末尾

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCPj9tajAU6Hta17FrNE2kicWMibn8rxBnYQT1pIpPOeLZysTcrT761a8A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Down`：下一个方法

### `Alt + Up`：上一个方法

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCYmdLsERruhaD3IUJEtadInv4fZhbBOS2Qib8r5hzviaAnLL5SZgicKKMg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + PageUp`：移动至页面顶部

### `Ctrl + PageDown`：移动至页面底部

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `PageUp`：向上翻页

### `PageDown`：向下翻页

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCh9GNDSsrUnoVcn8fyCoetA7BqjsBvMKp4SepGp3ibCVoy1Bwt5vZ0AA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Home`：移动至文件开头

### `Ctrl + End`：移动至文件末尾

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFaerIOSSGeibfCCeOyia2QyHkNfVLQjiatf4NDOmESDOibsvZqJyOGlCAw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 四、文本选择

### `Ctrl + A`：全选

### `Shift + Left`：向左选择

### `Shift + Right`：向右选择

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCa2Ilwh2rE2mDUyweYQolJ1Aq0M9CjDS0czDvI2H9PaeTNbiburCUyQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + Left`：向左选择一个单词

### `Ctrl + Shift + Right`：向右选择一个单词

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCTB4ogu8FZmWxcddF0nHd5ibG4fwxDNLnPGZfhd7ia1ziaTdiapArfzkGwA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + Home`：向左选择至行头

### `Shift + End`：向右选择至行尾

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Shift + Up`：向上选择

### `Shift + Down`：向下选择

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCNBENAyQR1uD9LCncsW8xlnuLCnDAa1dEWD5xxBMjSh1y30kADLwg4A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + [`：选择至代码块开头

### `Ctrl + Shift + ]`：选择至代码块结尾

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUkyaCEf6EuYjlicRuOj2vhI3icmoKVbBLQuvMLSg0V95D6MvbWfIeARw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + PageUp`：选择至页面顶部

### `Ctrl + Shift + PageDown`：选择至页面底部

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC6NEXIFoQWZr2ZC9ibDVzb2xzgjpXBY0P8usfbvflUB2Q0PCWBI2XoPQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + PageUp`：向上翻页选择

### `Shift + PageDown`：向下翻页选择

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCehaeQQxrypJlZf086iaChJcHDIia644F7I6WdrGeeK9zFw7a0KqYTrWA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + Home`：选择至文件开关

### `Ctrl + Shift + End`：选择至文件结尾

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + W`：扩展选择

### `Ctrl + Shift + W`：收缩选择

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCuBGsuOrvzr84dJ9aoPBxvfx9C2ibsEbnyMqb7o7BOI2USjjezNMEmbw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 五、代码折叠

### `Ctrl + NumPad+`：展开代码块

### `Ctrl + NumPad-`：折叠代码块

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC2nETmrJs1LiaaDiaIOExbPUkd6wMUibb38a9WvRGhhbtjpueatELmAAFA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + NumPad+`：递归展开

### `Ctrl + Alt + NumPad-`：递归折叠

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + NumPad+`：全部展开

### `Ctrl + Shift + NumPad-`：全部折叠

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCMAvUI3AF5HPtGEN1vehKiblROZufkJa2m3rlZcjClicg7aD0AbmYnWOA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + .`：折叠选择

# 六、多个插入符号和范围选择

### `Alt + Shift + Click`：添加/删除插入符号

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCGsjTjWmAKhHZDEBSAQM5JOTmiaacyjMkIUicvBVOWQlLp6tDNXvoiaAAg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Shift + Insert`：切换列选择模式

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### 双击`Ctrl` + `Up`：向上克隆插入符号

按`Ctrl`键两次，然后在不松开的情况下按向上箭头键。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC1u5c1d5egcYC50tWkxbYJMdyQ8JibFNBJ3Zhm0aRJ9IbdKrPy7CuxPw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### 双击`Ctrl` + `Down`：向下克隆插入符号

按`Ctrl`键两次，然后在不松开的情况下按向下箭头键。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCp24h97TUP6UCwG8G8W4RrXJwqibzAGplo40lt8RYknVaL7ObtOPL32A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Shift + G`：将插入符号添加到选择中的每一行

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Alt + J`：选择单位下次出现的位置

### `Alt + Shift + J`：取消最后一次选择

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCL97Scib5A2IRPCHEmAkPCylZiaMwicPnwjQJF5MXkTHDkNKL7QRlRbjjg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + J`：选择所有出现的位置

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCLcia59TXQoh70We2Nw2X19bzj57FyOxovhu1qRXxmOuqMrnwGsHmWNQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Shift + Middle-Click`：创建矩形选择

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Alt + Click`：拖拽以创建矩形选择区

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCWWcDiaVAHAk0F6Q8BYmoeHYFCoSmFy1Ckbjg2s2R9gvSVI1Ruibggomw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + Click`：拖拽以创建多个矩形选择区

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCJvHtJV5zU8rb6lib4wiajV9QvpLiaIKuZ99Bgmicj2PgjyBjl3SJ78RdSg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 七、辅助编码

### `Alt + Enter`：显示建议操作

该快捷键又称为“万通快捷键”，它会根据不同的语境建议不同的操作。下面这个演示只是其中的一种，还有很多种用法，你可以尝试一下。

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC9xJpBKicUkonwSEVHT8emQjicJ5KWLWgStHJP8x0hZ26oIxX37HwU6Nw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Space`：代码补全

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + Space`：类型匹配代码补全

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCnP6WY9bGDFTkujGFicmAE9M0CXe7WmQNRQODaeIBD1bgT4EeIUXJjFA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Space`：第二次代码补全

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC5N9ySicZbYzJuC5oluKQ14sEI7ZGjkjqbP8B31OfG0eibXGzkvE6129A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + Enter`：补全当前语句

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + L`：格式化代码

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCCn63nTDG2Jr1yQgIN9HePquInqkTEpcFSF2B6zVKnWfyd7F3JURS3g/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + P`：参数信息提醒

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCqrFTicKzYyibzb6aAS4b8EdWreXAhFZtrtPek3xpnoic6a3ia9OdlwMdDQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Q`：快速文档

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + Up`：向上移动语句

### `Ctrl + Shift + Down`：向下移动语句

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCOuBd6jo7icxGtPMkGnwEgcTACQ92jIDf9jLBchaibdYlicL0VVNdLoFkA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + Left`：向左移动元素

### `Ctrl + Alt + Shift + Right`：向右移动元素

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCwJfSXwvWokVGMjzyMxx7MTNKn4mPLseWMkBLSs2SA9ZgPLOJqjHhFA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Shift + Up`：向上移动队列

### `Alt + Shift + Down`：向下移动队列

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + /`：添加行注释

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCydgxWKPkfRB7WmJMsuJUv44gYDZTLlbjiac1mIl4rGoL1wX3wn4libqA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + /`：添加块注释

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCeJ6prGJ92T2jxAqX48lKYbLl4dDSxc8GuYGx9Bibw2Q2dCHiaZILrmCg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Insert`：生产语句

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChlIJm7v9UYSLUwj9zE0O2qxibeZSMjPRibtlYclz0yXW2X818LwLiapeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 八、上下文导航

### `Alt + Down`：跳转至下一个方法

### `Alt + Up`：跳转至上一个方法

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCP62vBcEX0afVVW4sY0O1yarrSXca2ms7Q7Rj4fYZgmfmFKqWQFQpkA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + G`：跳转到指定行

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCEmpNq7FZHBhJDFjCwlHrlbCS4IJAz9MRGicbUTxiaAQMmmvqjdkv0wZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Tab`：切换活动文件

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCN8zZIusQR2Xu26okcXtTZfWGqzMiaJovxTZnYdBibjb2ZxPN4Zia3IE8Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + F1`：选择文件的定位

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + E`：最近的文件

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCX7XA23pemWRqqUJeYE6cMuyR4XYKP3M9C7FUjgymicNsjE3Pz6LXJSQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + Backspace`：返回上次编辑位置

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaSnn3B7sr4s1EvsIRUjMdn7dBg7zRibqJ2Ch6icRzZpRwvU6J6sib3D4Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Left`：后退

### `Ctrl + Alt + Right`：前进

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + Down`：下一事件

### `Ctrl + Alt + Up`：上一事件

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC9eSia19sFAMwzvaqxAaUnymdJo1Z3AOEiaic6iaJrvWjniaiaLhTr5GaPRrA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Right`：选择下一个选项卡

### `Alt + Left`：选择下一个选项卡

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCRUhibelp6wBS7ib1NgjKEtMQNdPza03vMZYK0aicRRSVsOU6tEdcRofQA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `F11`：切换匿名书签

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + [digit]`：用数字切换书签

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCmibCuUxmxglAricjTxia4nZNE0WKSXcI567Zibp6Lpp4rMQEpIUqRsBVsA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + F11`：使用助词符切换书签

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCVJW96hjOfCdULb8rqPdOLwYuoPRibx3jqzKo7esaRKUvZvSGhfzRYhg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + F11`：显示所有书签

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + [digit]`：用数字跳转到书签

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC3YZt19MnGmCYiauaJhWiaeKyjGKiaGqngKeZw1ZTqyv5iaekAKMeoUicr1Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + 7`：显示结构窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCabQrofut1NqkPGHU4iaibRSCNknp5wUX3Ka40FUicTicmH17A3ddDAXMKQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + 3`：显示查找窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCemJMFDfeGbtmATLKxrQ4O8iasQ458HAQIiciaib0u3Fq4oJ4iaOkwyTQbjg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 九、查找操作

### 双击`Shift`：查找所有

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaw2EotlcgB5v0dLKzRxchKTRYHdYiaDN6GTC1O8ia9bW6cNltJF5aJBQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + F`：查找字符（当前文件）

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCZyTbuhP4pdclD7WVO5qGKl21MPy1cYqUE5LBick9fagqpPiaHd8t1eug/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `F3`：查找下一个

### `Shift + F3`：查找上一个

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCpdeaSKeuAiaVad1NeO2082r58HppuVIkvs2ImP96ic0MQrNhZibhcI4cA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + R`：替换字符（当前文件）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + F`：查找字符（所有文件）

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCIEoqtAnX3HjA02qFQjrNLM4gjQmAiaMLVy9RaWG5g0Ob5yXHgf0EQaA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + R`：替换字符（所有文件）

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCEXwEdxpbRFWwZXWSflRibyyEjooY5Da5GJQbsc9ziaSMLu2O2Cgjf5AQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + F3`：跳转到光标处单词的下一位置

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + N`：查找文件并跳转

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChLS9NIMA8icorQcao4GNvBPibgtLC0NKXBWxwUJnLLqWIndTNdeMORZw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + F12`：打开文件结构

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFQEia6IWykBXq7ASp921ibzCmeUAd0KUERVFic69icAErUZLiaMRrf007Yw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + N`：查找符号（变量、方法等）

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + A`：查找动作

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQZCHvPpaJ0dhAoVNiclZxASBIKvFVDsaJYb1faRlDlbica4YmZ0oht2A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 十、符号导航

### `Alt + F7`：查找用法

### `Ctrl + B`：跳转到声明处

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCUtzcWMjFCibWeumreI2gNLZKnbBEla5rgpqOo337icg1PWFkTKaKYT3Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + B`：跳转到声明类处

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + F7`：显示用法

### `Ctrl + U`：跳转到超级方法

### `Ctrl + Alt + B`：跳转到实现方法

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCynI3KDzPAaYPaia5fyibAUB2l3qdLoLpAVFAcwKPqJdVvDcdTVibvpiaeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + F7`：突出显示文件中的用法

# 十一、代码分析

### `Alt + Enter`：显示意图操作

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCJFLEaDDgxFaPOlkb9r4aLmt1OVH4AtmQEcwYAneWeh7vLmdibS6bGeg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + F1`：显示错误描述

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `F2`：下一个突出显示的错误

### `Shift + F2`：上一个突出显示的错误

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCvNXsHJxZ203nicqln0V36CdeIdQGU1ktBIR1jlrcDib5m9cf6eSPAAPg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + I`：按名称运行检查

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCsfLtqCJTbTtHrcBAibpia8XoKZbdVzK5zicDKbZEMoLbo2XO6OA1Efb9Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + 6`：显示问题窗口

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# 十二、运行和调试

### 双击`Ctrl`：运行所有

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCSibfZEianNOboZXPom9r3cU7qia6gGcY1V6RRKfdkkibdYfyyUDx0qXFZQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + F10`：运行上下文配置

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCdFshADA2RcaQia1QnYT9t25dQribykqIPF9DG9aj0ZSKB3XN0Vmdnhng/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Shift + F10`：打开运行窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC07Jiaq0jRafknzHQYoePke1NhuibicPDky4pqUnumJ6jZpRdHx4E6xE0A/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + F9`：调试上下文配置

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Alt + Shift + F9`：打开调试窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCA5YkybEia1n5PZXziaibUciaUyA5SJTX3Y27HLESQOzrqjgMGt2YG4m8Tw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + F5`：附加到进程

### `Ctrl + F2`：停止

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCiaG3mODSPdk9H2nlLJHiclMgwCRdu2DIZlrYkJJfPDGZmBibtD4D9YMgg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `F9`：运行至下一断点

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + F2`：停止后台进程

### `F8`：跨过调用

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCzpKYEBGDCTXsTsmE7NZGYADGic5cpISHEhuARQZA1Td3zJXD4PjPLAg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + Shift + F8`：强制跨过调用

### `F7`：进入调用

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCxjDrl26VSBVvlnp5VrpvD7KuPrUyn7DjHTiaLRKfOGjZ0BfjSdUrq9Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + F7`：智能进入调用

### `Alt + Shift + F7`：强制进入调用

### `Shift + F8`：跳出调用

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Alt + F9`：运行至光标处

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCAWxhSWswuDgRpomNOsNzXULPl6RuPhjsAFepZRibh3jNnicoic6l2ic6pw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + F9`：强制运行至光标处

### `Alt + F10`：显示执行点

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCQhy6k7qycFncbRiaiaEkhbMJJpaeicsr6Kc3HQ3ZMA1My7FCTVBa3352Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + F8`：评估表达式

### `Ctrl + Alt + F8`：快速评估表达式

### `Ctrl + F8`：切换行断点

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + Shift + F8`：切换临时行断点

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCZibKtEpY8QnxpD5sLr6TN7p0fJtUVc2kJicQfzL87icHDZ9V4byKkS1tA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + F8`：查看断点

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCbCSbjUTREyvyskib59GbiaDvmBt7trtVkCDWRt2whyPHBXIM8bUOIG4Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + F8`：编辑断点

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Alt + 4`：显示运行窗口

### `Alt + 5`：显示调试窗口

### `Alt + 8`：显示服务窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCcDVdnic4gNgyUemMv4icwqnibgg35n9Ov4o7Ltkm8oZpQtKbBaHc13mVw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

# 十三、代码重构

### `Ctrl + Alt + Shift + T`：打开重构列表

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCzhaVJnYRvFcfOzUTDnz8ibBqMTGrGvg4pYGfcMU39nKtibNa2RFaCtUw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Shift + F6`：修改名称

### `Ctrl + F6`：修改签名

### `Ctrl + Alt + N`：内联

### `F6`：移动

### `Ctrl + Alt + M`：提取方法

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + F`：引入域

### `Ctrl + Alt + P`：引入参数

### `Ctrl + Alt + V`：引入变量

### `Alt + Delete`：安全删除

# 十四、全局 CVS 操作

### `Alt + `` ：弹出 CVS 窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusC5ZM7BgNUUkplgMO6jMQwFdtJ3B4j5pf5TVoxicvAa0cp7QXOPohF0SQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + K`：提交

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCDo0GfqDrvSmIdWwGrkjSEOCOibW5YCcH0uNEXUn5YLdNLQbW46gpdpg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + T`：更新项目

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Alt + Z`：回滚

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCKvk2dN9oiagx8WiajGAEEu7dcSPVQq18WNNGZyJZxmXPogjdx66zV6ibw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + K`：拉取

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCLhbQ3rR3VHx2CUqVsr5rB1Kk9nEGNtvAiaMWMODZewTGiaLQRRZNLsiag/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + Down`：下一个修改

### `Ctrl + Alt + Shift + Up`：上一个修改

### `Alt + 9`：显示版本控制窗口

### `Alt + 0`：显示提交窗口

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

# 十五、差异查看器

### `F7`：下一个差异

### `Shift + F7`：上一个差异

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCZGibkiaueibrB1lAibwb2mls0pBvKsBsPeqevDhGYtiaERDFYibok5RV5APA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + R`：接受左侧

### `Ctrl + Alt + A`：接受右侧

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCFsjWycm6Z0DOsNowPnRichdNibZFYmodV04BUy6R92QYufoQO7Nj2iaWg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + Tab`：选择对面的差异窗格

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Ctrl + Shift + D`：显示差异设置窗口

# 十六、工具窗口

### `Shift + Escape`：隐藏活动的工具窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusClXRgfFq3NvaNgpicFLFuYpVFSlXssiciaiaRPDIpiak6tQvlpc0tNLdbJSg/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Shift + F12`：隐藏所有工具窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusChkhTnn1gicuu6JHZES64HBdLfaI0eXpVmWZTpaJLQSHdmdgdhBCxS2Q/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `F12`：跳转到最后一个工具窗口

### `Ctrl + Alt + Shift + Left`：向左延伸窗口大小

### `Ctrl + Alt + Shift + Right`：向右延伸窗口大小

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCWQtGtyDnTvGYKicshblHcoFFXRYLytzp39wtHjPUAjXjX0uaCYq60wA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Ctrl + Alt + Shift + Up`：向顶部延伸窗口大小

### `Ctrl + Alt + Shift + Down`：向底部延伸窗口大小

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCBfPJWNEy7ic4220NF72DIPCmvfh0ED5f019BfniaHws62oLStLoibKI9g/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)

### `Alt + 1`：显示项目窗口

### `Alt + 2`：显示书签窗口

### `Alt + 3`：显示查找窗口

### `Alt + 4`：显示运行窗口

### `Alt + 5`：显示调试窗口

### `Alt + 6`：显示问题窗口

### `Alt + 7`：显示结构窗口

### `Alt + 8`：显示服务窗口

### `Alt + 9`：显示版本控制窗口

### `Alt + 0`：显示提交窗口

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

### `Alt + F12`：显示终端窗口

![图片](https://mmbiz.qpic.cn/mmbiz_gif/QCu849YTaIM3KqjG0riavLxK8uwjxhusCO0PmZ97NAbU8BS8nFkKBpxMzKvzlBbzyxQUe0T4jgPekIq9U9zPsbA/640?wx_fmt=gif&wxfrom=5&wx_lazy=1)