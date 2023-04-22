# 《精灵宝可梦水晶版》汉化编译工程

## 环境准备

需要一个Linux环境，Win10以上可以使用 `WSL` 。在Linux上安装如下软件。

```
git libpng gcc bison pkg-config rsync pip3
```

对于Ubuntu来说，一般是如下命令安装缺失的工具。

```
sudo apt install git libpng-dev gcc bison pkg-config rsync python3-pip
```

另外，需要安装Python3的 `openpyxl` 库用于读写文本。

```
sudo pip3 install openpyxl
```

纯Windows编译环境理论上可行，但是还未完全实施，目前不提供。


## 下载

由于使用submodule，需要使用

```
git clone https://github.com/SnDream/pokecrystal_cn_build.git --recursive
```

下载仓库。


## 工程结构

- `pokecrystal_cn` 目录
    - 代码仓库。里面包含原始代码、汉化代码、系统使用的文本翻译。游戏主文本不包含在内。
- `rgbds` 目录
    - 工具链仓库。里面包含一个修改版本的 `rgbds` ，用于支持中文文本的编译。
- `tools` 目录
    - 文本导入的程序。
- `build` 目录
    - 将代码和文本合并编译的位置。ROM也将在这个路径中输出。
- `env-setup`
    - 环境初始化脚本
- `text.xlsx`
    - 游戏主文本。需要通过导入才能编译进ROM。


## 编译方法

在项目根目录（当前目录），执行如下命令

```
source env-setup
```

命令将自动编译 `rgbds` 工具链，并加入当前环境变量中。

之后，按顺序执行

```
pmc_isys
pmc_init
pmc_itext
pmc_build
```

最终将在 `build` 目录中输出相关的ROM。

命令的具体说明如下：

### 代码快速同步

执行 `pmc_init` 进行代码同步。将 `pokecrystal_cn` 中的代码更改同步到 `build` 中。

- 如果有导入过文本，内容将被删除，请重新导入文本。
- 不会删除 `build` 目录中为了编译生成的中间文件。

### 代码完整同步

执行 `pmc_finit` 进行完整代码同步。将 `pokecrystal_cn` 中的代码更改同步到 `build` 中。

- 与 `pmc_init` 的区别在于 `build` 目录中为了编译生成的中间文件也会被删除。
    - 相当于将 `build` 还原为和 `pokecrystal_cn` 完全一致的状态。
- 如果有导入过文本，内容将被删除，请重新导入文本。

### 系统文本导入

执行 `pmc_isys` 导入系统文本。

- 导入的目标是原始的 `pokecrystal_cn` 目录，而不是 `build` 目录。

### 系统文本导入

执行 `pmc_itext` 导入主文本。

- 导入的目标是 `build` 下的代码。

### 编译

执行 `pmc_itext` 开始编译。编译输出的ROM在 `build` 目录中。

- 如果需要定制编译，可以在执行 `source env-setup` 命令后自行进入 `build` 目录进行定制编译。
    - 如果关闭或者切换终端，需要重新执行 `source env-setup` 命令
