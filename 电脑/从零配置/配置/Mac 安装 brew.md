
# 方法一:

自动脚本(全部国内地址)（在Mac os终端中复制粘贴回车下面这句话)

```java
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

下面是下载时的效果：

![img](https://img2020.cnblogs.com/blog/1681961/202004/1681961-20200422164413625-522091467.jpg)


# 方法二:
## 快速开始

如果之前尝试过其他脚本安装，请移除`Homebrew`后再安装，具体请参考 [FAQ](https://brew.idayer.com/guide/faq/#%E5%A6%82%E4%BD%95%E5%88%A0%E9%99%A4Homebrew) 。

```
/bin/bash -c "$(curl -fsSL https://gitee.com/ineo6/homebrew-install/raw/master/install.sh)"
```

> 默认使用中科大源，如果需要换源参考 [换源](https://brew.idayer.com/guide/change-source/)。

如果命令执行中卡在下面信息：
```
==> Tapping homebrew/core
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...
```

请`Control + C`中断脚本执行如下命令：
```
cd "$(brew --repo)/Library/Taps/"
mkdir homebrew && cd homebrew
git clone https://mirrors.ustc.edu.cn/homebrew-core.git
```

成功执行之后重新执行安装命令。

Homebrew 4.0 版本后默认`JSON API`获取仓库信息，因此在大部分情况下都不再需要处理下面的`cask`。

> **安装`cask` 同样也有安装失败或者卡住的问题，解决方法也是一样：**
> 
> cd "$(brew --repo)/Library/Taps/"
> 
> cd homebrew
> 
> git clone https://mirrors.ustc.edu.cn/homebrew-cask.git
> 
> 成功执行之后重新执行安装命令。

最后看到`🎉 恭喜，安装成功！`就说明安装成功了。

然后更新：

`brew update`

## 如何卸载 Homebrew

使用官方脚本同样会遇到`uninstall`地址无法访问问题，可以使用下面脚本：

`/bin/bash -c "$(curl -fsSL https://gitee.com/ineo6/homebrew-install/raw/master/uninstall.sh)"`