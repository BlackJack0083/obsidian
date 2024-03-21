# Lecture 1  Course Overview + The Shell

## shell 是什么？

如今的计算机有着多种多样的交互接口让我们可以进行指令的的输入，从炫酷的图像用户界面（GUI），语音输入甚至是 AR/VR 都已经无处不在。 这些交互接口可以覆盖 80% 的使用场景，但是它们也从根本上限制了您的操作方式——你不能点击一个不存在的按钮或者是用语音输入一个还没有被录入的指令。 为了充分利用计算机的能力，我们不得不回到最根本的方式，使用文字接口：Shell

几乎所有您能够接触到的平台都支持某种形式的 shell，有些甚至还提供了多种 shell 供您选择。虽然它们之间有些细节上的差异，但是其核心功能都是一样的：它允许你执行程序，输入并获取某种半结构化的输出。

本节课我们会使用 Bourne Again SHell, 简称 “bash” 。 这是被最广泛使用的一种 shell，它的语法和其他的 shell 都是类似的。打开shell _提示符_（您输入指令的地方），您首先需要打开 _终端_ 。您的设备通常都已经内置了终端，或者您也可以安装一个，非常简单。
## 使用 shell

### 打开终端
注意，由于本课程基于Linux，所以Window系统需要使用虚拟机或者WSL
可以使用以下命令查看是否符合要求：
```shell
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo $SHELL
返回： /bin/bash    或者    /usr/bin/zsh
```

当您打开终端（进入终端方法：win + X + I ）时，您会看到一个提示符，它看起来一般是这个样子的：
```shell
missing:~$ 
我的是：hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$
```

这是 shell 最主要的文本接口。它告诉你，你的主机名是 `missing` 并且您当前的工作目录（”current working directory”）或者说您当前所在的位置是 `~` (表示 “home”)。 `$` 符号表示您现在的身份不是 root 用户（稍后会介绍）。在这个提示符中，您可以输入 _命令_ ，命令最终会被 shell 解析。最简单的命令是执行一个程序：

```shell
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ date
Tue Jan 23 11:12:43 CST 2024
```

这里，我们执行了 `date` 这个程序，不出意料地，它打印出了当前的日期和时间。然后，shell 等待我们输入其他命令。我们可以在执行命令的同时向程序传递 _参数_ ：
```shell
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo hello
hello
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo "hello world"
hello world
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo hello\ world
hello world
```

上例中，我们让 shell 执行 `echo` ，同时指定参数 `hello`。`echo` 程序将该参数打印出来。 shell 基于空格分割命令并进行解析，然后执行第一个单词代表的程序，并将后续的单词作为程序可以访问的参数。如果您希望传递的参数中包含空格（例如一个名为 My Photos 的文件夹），您要么用使用单引号，双引号将其包裹起来，要么使用转义符号 `\` 进行处理（`My\ Photos`，这里是将空格键进行转义，所以在空格前加转义字符）。

但是，shell 是如何知道去哪里寻找 `date` 或 `echo` 的呢？其实，类似于 Python 或 Ruby，shell 是一个编程环境，所以它具备变量、条件、循环和函数（下一课进行讲解）。当你在 shell 中执行命令时，您实际上是在执行一段 shell 可以解释执行的简短代码。如果你要求 shell 执行某个指令，但是该指令并不是 shell 所了解的编程关键字，那么它会去咨询 _环境变量_ `$PATH`，它会列出当 shell 接到某条指令时，进行程序搜索的路径：
```shell
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib:/mnt/c/Windows/system32:/mnt/c/Windows:/mnt/c/Windows/System32/Wbem:/mnt/c/Windows/System32/WindowsPowerShell/v1.0/:/mnt/c/Windows/System32/OpenSSH/:/mnt/c/Program Files (x86)/NVIDIA Corporation/PhysX/Common:/mnt/c/Program Files/NVIDIA Corporation/NVIDIA NvDLISR:/mnt/c/Program Files/dotnet/:/mnt/c/WINDOWS/system32:/mnt/c/WINDOWS:/mnt/c/WINDOWS/System32/Wbem:/mnt/c/WINDOWS/System32/WindowsPowerShell/v1.0/:/mnt/c/WINDOWS/System32/OpenSSH/:/mnt/c/Program Files/texlive/2023/bin/windows:/mnt/c/jdk-20.0.2/bin:/mnt/c/jdk-20.0.2/jre/bin:/mnt/c/Users/何嘉凯/Desktop/python/Scripts/:/mnt/c/Users/何嘉凯/Desktop/python/:/mnt/c/Users/何嘉凯/AppData/Local/Programs/Python/Python310/Scripts/:/mnt/c/Users/何嘉凯/AppData/Local/Programs/Python/Python310/:/mnt/c/Users/何嘉凯/AppData/Local/Microsoft/WindowsApps:/mnt/c/Program Files/JetBrains/PyCharm Community Edition 2023.2.3/bin:/mnt/c/altera/13.1/modelsim_ase/win32aloem:/mnt/c/Users/何嘉凯/AppData/Local/Programs/Microsoft VS Code/bin:/mnt/f/CLion 2023.3.2/bin:/snap/bin

missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
当我们执行 `echo` 命令时，shell 了解到需要执行 `echo` 这个程序，随后它便会在 `$PATH` 中搜索由 `:` 所分割的一系列目录，基于名字搜索该程序。当找到该程序时便执行（假定该文件是 _可执行程序_，后续课程将详细讲解）。确定某个程序名代表的是哪个具体的程序，可以使用 `which` 程序。我们也可以绕过 `$PATH`，通过直接指定需要执行的程序的路径来执行该程序

## 在shell中导航

shell 中的路径是一组被分割的目录，在 Linux 和 macOS 上使用 `/` 分割，而在Windows上是 `\`。路径 `/` 代表的是系统的根目录，所有的文件夹都包括在这个路径之下，在Windows上每个盘都有一个根目录（例如： `C:\`）。 我们假设您在学习本课程时使用的是 Linux 文件系统。如果某个路径以 `/` 开头，那么它是一个 _绝对路径_，其他的都是 _相对路径_ 。相对路径是指相对于当前工作目录的路径，当前工作目录可以使用 `pwd` 命令来获取。此外，切换目录需要使用 `cd` 命令。在路径中，`.` 表示的是当前目录，而 `..` 表示上级目录：

```shell
missing:~$ pwd
/home/missing
missing:~$ cd /home  
missing:/home$ pwd
/home

hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ cd /home
hejiakai@LAPTOP-EOPIIVUT:/home$ pwd
/home
hejiakai@LAPTOP-EOPIIVUT:/home$ cd /mnt
hejiakai@LAPTOP-EOPIIVUT:/mnt$ pwd
/mnt

missing:/home$ cd ..
missing:/$ pwd
/
missing:/$ cd ./home
missing:/home$ pwd
/home

missing:/home$ cd missing
missing:~$ pwd
/home/missing

hejiakai@LAPTOP-EOPIIVUT:/home$ cd /mnt
hejiakai@LAPTOP-EOPIIVUT:/mnt$ cd c
hejiakai@LAPTOP-EOPIIVUT:/mnt/c$

missing:~$ ../../bin/echo hello
hello
hejiakai@LAPTOP-EOPIIVUT:/mnt/c$ ../../bin/echo hello  
// 这里只下去了2级目录，所以返回上级目录也只需要2次
hello
```

注意，shell 会实时显示当前的路径信息。您可以通过配置 shell 提示符来显示各种有用的信息，这一内容我们会在后面的课程中进行讨论。

一般来说，当我们运行一个程序时，如果我们没有指定路径，则该程序会在当前目录下执行。例如，我们常常会搜索文件，并在需要时创建文件。

为了查看指定目录下包含哪些文件，我们使用 `ls` 命令：

```shell
missing:~$ ls
missing:~$ cd ..
missing:/home$ ls
missing
missing:/home$ cd ..
missing:/$ ls
bin
boot
dev
etc
home
...
```
![[微信截图_20240123114624.png]]
除非我们利用第一个参数指定目录，否则 `ls` 会打印当前目录下的文件。大多数的命令接受标记和选项（带有值的标记），它们以 `-` 开头，并可以改变程序的行为。通常，在执行程序时使用 `-h` 或 `--help` 标记可以打印帮助信息，以便了解有哪些可用的标记或选项。例如，`ls --help` 的输出如下：

```shell
  -l                         use a long listing format
```

```shell
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```
![[微信截图_20240123114741.png]]
这个参数可以更加详细地列出目录下文件或文件夹的信息。首先，本行第一个字符 `d` 表示 `missing` 是一个目录。然后接下来的九个字符，每三个字符构成一组。 （`rwx`）. 它们分别代表了文件所有者（`missing`），用户组（`users`） 以及其他所有人具有的权限。其中 `-` 表示该用户不具备相应的权限。从上面的信息来看，只有文件所有者可以修改（`w`），`missing` 文件夹 （例如，添加或删除文件夹中的文件）。为了进入某个文件夹，用户需要具备该文件夹以及其父文件夹的“搜索”权限（以“可执行”：`x`）权限表示。为了列出它的包含的内容，用户必须对该文件夹具备读权限（`r`）。对于文件来说，权限的意义也是类似的。注意，`/bin` 目录下的程序在最后一组，即表示所有人的用户组中，均包含 `x` 权限，也就是说任何人都可以执行这些程序。

在这个阶段，还有几个趁手的命令是您需要掌握的，例如 `mv`（用于重命名或移动文件）、 `cp`（拷贝文件）以及 `mkdir`（新建文件夹）。

```shell
mv -i source.txt /destination/folder/
mv file1.txt file2.txt /home/user/documents/   移动文件
mv oldname.txt newname.txt  重命名


cp file1.txt file2.txt 拷贝文件
cp file1.txt file2.txt directory/ 拷贝完移动到文件夹
```

如果您想要知道关于程序参数、输入输出的信息，亦或是想要了解它们的工作方式，请试试 `man` 这个程序。它会接受一个程序名作为参数，然后将它的文档（用户手册）展现给您。注意，使用 `q` 可以退出该程序。

```shell
missing:~$ man ls
```
![[微信截图_20240123114845.png]]
## 在程序间创建连接

在 shell 中，程序有两个主要的“流”：它们的输入流和输出流。 当程序尝试读取信息时，它们会从输入流中进行读取，当程序打印信息时，它们会将信息输出到输出流中。 通常，一个程序的输入输出流都是您的终端。也就是，您的键盘作为输入，显示器作为输出。 但是，我们也可以重定向这些流！

最简单的重定向是 `< file` 和 `> file`。这两个命令可以将程序的输入输出流分别重定向到文件：

```shell
missing:~$ echo hello > hello.txt  // 会创建一个hello.txt且往里写入hello
missing:~$ cat hello.txt  // 执行文件内容
hello
missing:~$ cat < hello.txt 输出流
hello
missing:~$ cat < hello.txt > hello2.txt  先输出到cat，再输入到hello2.txt
missing:~$ cat hello2.txt
hello
```

您还可以使用 `>>` 来向一个文件追加内容。使用管道（ _pipes_ ），我们能够更好的利用文件重定向。 `|` 操作符允许我们将一个程序的输出和另外一个程序的输入连接起来：

```shell
missing:~$ ls -l / | tail -n1  // 列举该目录的所有文件，并只显示最后一个(-n1)
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

我们会在数据清理一章中更加详细的探讨如何更好的利用管道。

## 一个功能全面又强大的工具

对于大多数的类 Unix 系统，有一类用户是非常特殊的，那就是：根用户（root user）。 您应该已经注意到了，在上面的输出结果中，根用户几乎不受任何限制，他可以创建、读取、更新和删除系统中的任何文件。 通常在我们并不会以根用户的身份直接登录系统，因为这样可能会因为某些错误的操作而破坏系统。 取而代之的是我们会在需要的时候使用 `sudo` 命令。顾名思义，它的作用是让您可以以 su（super user 或 root 的简写）的身份执行一些操作。 当您遇到拒绝访问（permission denied）的错误时，通常是因为此时您必须是根用户才能操作。然而，请再次确认您是真的要执行此操作。

有一件事情是您必须作为根用户才能做的，那就是向 `sysfs` 文件写入内容。系统被挂载在 `/sys` 下，`sysfs` 文件则暴露了一些内核（kernel）参数。 因此，您不需要借助任何专用的工具，就可以轻松地在运行期间配置系统内核。**注意 Windows 和 macOS 没有这个文件**

例如，您笔记本电脑的屏幕亮度写在 `brightness` 文件中，它位于

```shell
/sys/class/backlight
```

通过将数值写入该文件，我们可以改变屏幕的亮度。现在，蹦到您脑袋里的第一个想法可能是：

```shell
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```

出乎意料的是，我们还是得到了一个错误信息。毕竟，我们已经使用了 `sudo` 命令！关于 shell，有件事我们必须要知道。`|`、`>`、和 `<` 是通过 shell 执行的，而不是被各个程序单独执行。 `echo` 等程序并不知道 `|` 的存在，它们只知道从自己的输入输出流中进行读写。 对于上面这种情况， _shell_ (权限为您的当前用户) 在设置 `sudo echo` 前尝试打开 brightness 文件并写入，但是系统拒绝了 shell 的操作因为此时 shell 不是根用户。

明白这一点后，我们可以这样操作：

```shell
$ echo 3 | sudo tee brightness
```

因为打开 `/sys` 文件的是 `tee` 这个程序，并且该程序以 `root` 权限在运行，因此操作可以进行。 这样您就可以在 `/sys` 中愉快地玩耍了，例如修改系统中各种LED的状态（路径可能会有所不同）：

```shell
$ echo 1 | sudo tee /sys/class/leds/input6::scrolllock/brightness
```

# 作业中学到的内容
## touch命令
`touch` 命令在 Unix 和类 Unix 操作系统中用于更新文件的时间戳，或者如果文件不存在的话，它会创建一个空文件。以下是关于 `touch` 命令的基本用法：

### 基本语法
```bash
touch [选项] 文件名
```
- `文件名`：要更新时间戳的文件名，或者要创建的文件名。
### 更新文件时间戳
```bash
touch 文件名
```
这个命令会更新指定文件的访问时间和修改时间为当前时间。如果文件不存在，它会创建一个空的文件。
### 创建空文件
```bash
touch 新文件名
```
这个命令会创建一个空的文件，如果文件已经存在，它会更新文件的访问时间和修改时间为当前时间。
### 批量更新时间戳
```bash
touch 文件1 文件2 文件3
```
这个命令可以同时更新多个文件的时间戳，如果文件不存在，则创建空文件。
### 使用选项
- `-c`：仅在文件不存在时创建文件。
- `-t`：使用指定的时间戳，格式为 `[[CC]YY]MMDDhhmm[.ss]`。
### 示例
1. **更新文件时间戳**:
   ```bash
   touch myfile.txt
   ```
   这个命令会将 `myfile.txt` 的访问时间和修改时间更新为当前时间。
   
1. **创建空文件**:
   ```bash
   touch newfile.txt
   ```

   这个命令会创建一个名为 `newfile.txt` 的空文件。

3. **批量更新时间戳**:
   ```bash
   touch file1 file2 file3
   ```
   这个命令会同时更新 `file1`、`file2` 和 `file3` 的时间戳。

4. **指定时间戳**:
   ```bash
   touch -t 202201011200.00 myfile.txt
   ```
   这个命令会将 `myfile.txt` 的时间戳设置为 2022 年 1 月 1 日 12:00:00。

### 注意事项
- `touch` 命令通常用于创建空文件或更新文件的时间戳，但它并不修改文件的内容。
- 使用 `-t` 选项时，时间戳的格式应为 `[[CC]YY]MMDDhhmm[.ss]`。
- `-c` 选项可避免在文件已存在时创建新文件。

## chmod命令
`chmod` 是一个在 Unix 和类 Unix 操作系统中用于更改文件或目录权限的命令。它允许用户授予或撤销对文件的读取、写入和执行权限，以及对目录的访问权限。以下是 `chmod` 命令的基本用法：

### 基本语法
```bash
chmod [选项] 模式 文件名
```
- `模式`：权限模式，可以使用数字表示（如 755）或符号表示（如 u+rwx）。
- `文件名`：要更改权限的文件或目录名称。

### 使用数字表示权限
1. **数字模式**:
   - `4`：读权限（read）
   - `2`：写权限（write）
   - `1`：执行权限（execute）

2. **语法**:
   - `chmod XYZ 文件名`
   - `X` 表示所有者权限，`Y` 表示组权限，`Z` 表示其他用户的权限。

3. **示例**:
   - `chmod 755 文件名`：给所有者赋予读、写、执行权限，给组和其他用户赋予读、执行权限。

### 使用符号表示权限
1. **符号模式**:
   - `+`：添加权限
   - `-`：移除权限
   - `\=`：设置权限

2. **语法**:
   - `chmod [who] [+|-|=] [permissions] 文件名`
   - `who` 可以是 `u`（所有者）、`g`（组）、`o`（其他用户）或 `a`（所有用户）。
   - `permissions` 可以是 `r`（读取）、`w`（写入）、`x`（执行）的组合。

3. **示例**:
   - `chmod u+r 文件名`：给所有者添加读权限。
   - `chmod go-w 文件名`：移除组和其他用户的写权限。
   - `chmod a=x 文件名`：将所有用户的执行权限设置为与所有者相同。

### 使用 `chmod` 的特殊权限
1. **SUID、SGID、Sticky Bit**:
   - `u+s`：设置 SUID 位（Set User ID）
   - `g+s`：设置 SGID 位（Set Group ID）
   - `o+t`：设置粘滞位（Sticky Bit）

2. **语法**:
   - `chmod +s` 或 `chmod u+s`：给文件添加 SUID 位。
   - `chmod +s` 或 `chmod g+s`：给文件添加 SGID 位。
   - `chmod +t` 或 `chmod o+t`：给目录添加粘滞位。

3. **示例**:
   - `chmod +s 可执行文件`：将 SUID 位添加到可执行文件上。
   - `chmod g+s 目录`：将 SGID 位添加到目录上。
   - `chmod +t 目录`：将粘滞位添加到目录上。

### 注意事项
- 使用 `chmod` 命令时，权限模式可以使用数字（如 `755`）或符号（如 `u+rwx`）。
- 特殊权限（SUID、SGID、Sticky Bit）可以通过符号模式设置，例如 `chmod +s 文件`。
- 可以同时设置多个权限，例如 `chmod u+r,g+w 文件`。

使用 `chmod` 命令改变权限，使 `./semester` 能够成功执行，不要使用 `sh semester` 来执行该程序。您的 shell 是如何知晓这个文件需要使用 `sh` 来解析呢？
![[微信截图_20240123135042.png|500]]
## 写入含有特殊含义的符号
要通过 shell 命令将 `#!/bin/sh` 这一行写入一个文件，您可以使用 `echo` 命令结合重定向操作符。这里有几种不同的方法可以实现：

### 使用 echo 命令
1. **覆盖写入**:
   如果您希望将 `#!/bin/sh` 写入一个新文件或覆盖一个现有文件，可以使用 `>` 重定向符号。例如：
   ```bash
   echo '#!/bin/sh' > filename
   ```
   这将创建（或覆盖）名为 `filename` 的文件，并写入 `#!/bin/sh`。

2. **追加写入**:
   如果您想将 `#!/bin/sh` 添加到现有文件的末尾，可以使用 `>>` 重定向符号。例如：
   ```bash
   echo '#!/bin/sh' >> filename
   ```
   这将在现有 `filename` 文件的末尾添加一行 `#!/bin/sh`。

### 使用 printf 命令
`printf` 命令提供了更多的格式控制，同样可以用于写入文件：

1. **覆盖写入**:
   ```bash
   printf '#!/bin/sh\n' > filename
   ```
2. **追加写入**:
   ```bash
   printf '#!/bin/sh\n' >> filename
   ```
在这些示例中，`filename` 是您要写入的文件的名称。如果文件不存在，它将被创建。如果文件已存在，`>` 会覆盖它，而 `>>` 会向其追加内容。

### 注意事项
- 在 `echo` 或 `printf` 命令中使用单引号 `'` 是重要的，特别是当文本包含特殊字符（如 `#`）时，这样可以确保这些字符被正确地解释和写入。
- 根据您的需求选择使用 `>`（覆盖）或 `>>`（追加）。覆盖将删除文件中的现有内容，而追加将在文件的末尾添加新内容。

## 提取信息并输出
如果你想从上述输出中提取并保存最后更改日期信息到“last-modified.txt”文件中，你可以使用以下命令：

```bash
./semester | grep last-modified | cut -d ':' -f 2- | tr -d ' ' > ~/last-modified.txt
```

解释一下这个命令：

1. `./semester`: 执行 `semester` 程序，产生相应的输出。
2. `grep last-modified`: 使用 `grep` 命令筛选出包含“last-modified”这个词的行，即包含最后修改日期信息的行。
3. `cut -d ':' -f 2-`: 使用 `cut` 工具提取日期信息。`-d ':'` 指定冒号为字段分隔符，`-f 2-` 选择从第二个字段到最后一个字段。
4. `tr -d ' '`: 使用 `tr` 命令删除日期信息中的空格。
5. `> ~/last-modified.txt`: 将提取的日期信息重定向到主目录下名为“last-modified.txt”的文件中。

这个命令会提取最后修改日期信息并将其写入“last-modified.txt”文件中。确保运行时“semester”程序在当前工作目录中可执行。
## 获取电量信息
需要进入sys文件夹
`/sys` 文件夹是 Linux 操作系统中的一个虚拟文件系统，通常被称为 sysfs。它提供了对内核和设备的信息的访问，以及对内核参数的控制。sysfs 主要用于在用户空间和内核之间传递设备、驱动程序和其他内核信息。

以下是 `/sys` 文件夹的一些主要内容和用途：
1. **设备信息：** `/sys/class` 目录包含了系统上的不同类别的设备。例如，`/sys/class/net` 包含有关网络设备的信息，`/sys/class/power_supply` 包含有关电源供应设备（如电池）的信息。
2. **总线信息：** `/sys/bus` 目录包含了系统上不同总线（如 PCI、USB 等）的信息。
3. **设备树：** `/sys/firmware` 目录包含了 ACPI 和其他固件信息，以及 `/sys/devices` 目录提供了设备树中设备的信息。
4. **CPU 和内存信息：** `/sys/devices/system/cpu` 包含了有关系统上的 CPU 的信息，而 `/sys/class/thermal` 包含了有关热传感器和温度的信息。
5. **内核参数：** `/sys/kernel` 目录包含了有关内核配置和参数的信息。例如，`/sys/kernel/debug` 包含了内核调试信息。
6. **挂载点：** `/sys/fs` 包含了有关文件系统的信息。
`/sys` 文件夹提供了一种访问内核和设备信息的统一方式，使用户空间的工具和应用程序能够获取有关系统硬件和内核状态的详细信息。这对于调试、设备管理和系统监控等方面非常有用。请注意，用户通常只能读取这些信息，而对其进行写入通常需要超级用户权限。
![[微信截图_20240123142013.png|500]]

# Shell 工具和脚本

在这节课中，我们将会展示 bash 作为脚本语言的一些基础操作，以及几种最常用的 shell 工具。
# Shell 脚本

到目前为止，我们已经学习来如何在 shell 中执行命令，并使用管道将命令组合使用。但是，很多情况下我们需要执行一系列的操作并使用条件或循环这样的控制流。

shell 脚本是一种更加复杂度的工具。

大多数shell都有自己的一套脚本语言，包括变量、控制流和自己的语法。shell脚本与其他脚本语言不同之处在于，shell 脚本针对 shell 所从事的相关工作进行来优化。因此，创建命令流程（pipelines）、将结果保存到文件、从标准输入中读取输入，这些都是 shell 脚本中的原生操作，这让它比通用的脚本语言更易用。本节中，我们会专注于 bash 脚本，因为它最流行，应用更为广泛。

在bash中为变量赋值的语法是`foo=bar`，访问变量中存储的数值，其语法为 `$foo`。 需要注意的是，`foo = bar` （使用空格隔开）是不能正确工作的，因为解释器会调用程序`foo` 并将 `\=` 和 `bar`作为参数。 总的来说，在shell脚本中使用空格会起到分割参数的作用，有时候可能会造成混淆，请务必多加检查。

Bash中的字符串通过`'` 和 `"`分隔符来定义，但是它们的含义并不相同。以`'`定义的字符串为原义字符串，其中的变量不会被转义，而 `"`定义的字符串会将变量值进行替换。

```bash
foo=bar
echo "$foo"
# 打印 bar
echo '$foo'
# 打印 $foo
```

和其他大多数的编程语言一样，`bash`也支持`if`, `case`, `while` 和 `for` 这些控制流关键字。同样地， `bash` 也支持函数，它可以接受参数并基于参数进行操作。下面这个函数是一个例子，它会创建一个文件夹并使用`cd`进入该文件夹。

```bash
mcd () {
    mkdir -p "$1" #新建嵌套文件夹
    cd "$1"
}

hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ vim mcd.sh
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ mkdir test
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ cd $_
# cd $_可以进入上一条命令最后一个参数，即之前创建的test文件夹
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯/test$ cd ..
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ rmdir test
# 删除文件夹
```

这里 `$1` 是脚本的第一个参数。与其他脚本语言不同的是，bash使用了很多特殊的变量来表示参数、错误代码和相关变量。下面是列举来其中一些变量，更完整的列表可以参考 [这里](https://www.tldp.org/LDP/abs/html/special-chars.html)。

- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。

命令通常使用 `STDOUT`来返回输出值，使用`STDERR` 来返回错误及错误码，便于脚本以更加友好的方式报告错误。 返回码或退出状态是脚本/命令之间交流执行状态的方式。返回值0表示正常执行，其他所有非0的返回值都表示有错误发生。

退出码可以搭配 `&&`（与操作符）和 `||`（或操作符）使用，用来进行条件判断，决定是否执行其他程序。它们都属于短路[运算符](https://en.wikipedia.org/wiki/Short-circuit_evaluation)（short-circuiting） 同一行的多个命令可以用 `;` 分隔。程序 `true` 的返回码永远是`0`，`false` 的返回码永远是`1`。让我们看几个例子
```bash
false || echo "Oops, fail"
# Oops, fail

true || echo "Will not be printed"
#

true && echo "Things went well"
# Things went well

false && echo "Will not be printed"
#

false ; echo "This will always run"
# This will always run
```

另一个常见的模式是以变量的形式获取一个命令的输出，这可以通过 _命令替换_（_command substitution_）实现。

当您通过 `$( CMD )` 这样的方式来执行`CMD` 这个命令时，它的输出结果会替换掉 `$( CMD )` 。例如，如果执行 `for file in $(ls)` ，shell首先将调用`ls` ，然后遍历得到的这些返回值。
```bash
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ foo=$(pwd)
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo $foo
/mnt/c/Users/何嘉凯
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ echo "We are i
n $(pwd)"
We are in /mnt/c/Users/何嘉凯
```

还有一个冷门的类似特性是 _进程替换_（_process substitution_）， `<( CMD )` 会执行 `CMD` 并将结果输出到一个临时文件中，并将 `<( CMD )` 替换成临时文件名。这在我们希望返回值通过文件而不是STDIN传递时很有用。例如， `diff <(ls foo) <(ls bar)` 会显示文件夹 `foo` 和 `bar` 中文件的区别。
```bash
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ cat <(ls) <(ls..)
AppData
Application Data
Contacts
Cookies
Desktop
Documents
Downloads
ENVILiDARHeightPalettes
ENVILiDARTemplates
Edrawsoft
Favorites
Figure_1.png
IDLWorkspace
Links
Local Settings
Music
My Documents
NTUSER.DAT
NTUSER.DAT{5c6dd71c-bf8e-11ed-856d-9566695b9450}.TM.blf
NTUSER.DAT{5c6dd71c-bf8e-11ed-856d-9566695b9450}.TMContainer00000000000000000001.regtrans-ms
NTUSER.DAT{5c6dd71c-bf8e-11ed-856d-9566695b9450}.TMContainer00000000000000000002.regtrans-ms
NetHood
OneDrive
Pictures
PrintHood
PycharmProjects
Recent
Saved Games
Searches
SendTo
Templates
Videos
WPS Cloud Files
eclipse-workspace
kVWSiuXPDm8
ntuser.dat.LOG1
ntuser.dat.LOG2
ntuser.ini
quartus2.ini
「开始」菜单
新建文件夹
错误网络.png
All Users
Default
Default User
GLCache
Public
desktop.ini
ºÎ¼Î¿­
何嘉凯
浣曞槈鍑
浣曞槈鍑痋AppData
```
说了很多，现在该看例子了，下面这个例子展示了一部分上面提到的特性。这段脚本会遍历我们提供的参数，使用`grep` 搜索字符串 `foobar`，如果没有找到，则将其作为注释追加到文件中。

```bash
#!/bin/bash

echo "Starting program at $(date)" # date会被替换成日期和时间

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # 如果模式没有找到，则grep退出状态为 1
    # 我们将标准输出流和标准错误流重定向到Null，因为我们并不关心这些信息
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done

hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ ./example.sh mcd.sh
Starting program at Sun Feb 18 10:49:59 CST 2024
Running program ./example.sh with 1 arguments with pid 95
File mcd.sh does not have any foobar, adding one
```

在条件语句中，我们比较 `$?` 是否等于0。 Bash实现了许多类似的比较操作，您可以查看 [`test 手册`](https://man7.org/linux/man-pages/man1/test.1.html)。 在bash中进行比较时，尽量使用双方括号 `[[ ]]` 而不是单方括号 `[ ]`，这样会降低犯错的几率，尽管这样并不能兼容 `sh`。 更详细的说明参见[这里](http://mywiki.wooledge.org/BashFAQ/031)。

当执行脚本时，我们经常需要提供形式类似的参数。bash使我们可以轻松的实现这一操作，它可以基于文件扩展名展开表达式。这一技术被称为shell的 _通配_（_globbing_）

- 通配符 - 当你想要利用通配符进行匹配时，你可以分别使用 `?` 和 `*` 来匹配一个或任意个字符。例如，对于文件`foo`, `foo1`, `foo2`, `foo10` 和 `bar`, `rm foo?`这条命令会删除`foo1` 和 `foo2` ，而`rm foo*` 则会删除除了`bar`之外的所有文件。
- 花括号`{}` - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令。这在批量移动或转换文件时非常方便。

```bash
convert image.{png,jpg}
# 会展开为
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# 会展开为
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# 也可以结合通配使用
mv *{.py,.sh} folder
# 会移动所有 *.py 和 *.sh 文件

mkdir foo bar

# 下面命令会创建foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h这些文件
touch {foo,bar}/{a..h}
touch foo/x bar/y
# 比较文件夹 foo 和 bar 中包含文件的不同
diff <(ls foo) <(ls bar)
# 输出
# < x
# ---
# > y
```

编写 `bash` 脚本有时候会很别扭和反直觉。例如 [shellcheck](https://github.com/koalaman/shellcheck) 这样的工具可以帮助你定位sh/bash脚本中的错误。

注意，脚本并不一定只有用 bash 写才能在终端里调用。比如说，这是一段 Python 脚本，作用是将输入的参数倒序输出：

```bash
#!/usr/local/bin/python
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

内核知道去用 python 解释器而不是 shell 命令来运行这段脚本，是因为脚本的开头第一行的 [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))。

在 `shebang` 行中使用 [`env`](https://man7.org/linux/man-pages/man1/env.1.html) 命令是一种好的实践，它会利用环境变量中的程序来解析该脚本，这样就提高来您的脚本的可移植性。`env` 会利用我们第一节讲座中介绍过的`PATH` 环境变量来进行定位。 例如，使用了`env`的shebang看上去时这样的`#!/usr/bin/env python`。

shell函数和脚本有如下一些不同点：

- 函数只能与shell使用相同的语言，脚本可以使用**任意语言**。因此在脚本中包含 `shebang` 是很重要的。
- 函数仅在定义时被加载，脚本会在每次被执行时加载。这让函数的加载比脚本略快一些，但每次修改函数定义，都要重新加载一次。
- 函数会在当前的shell环境中执行，脚本会在单独的进程中执行。因此，函数可以对环境变量进行更改，比如改变当前工作目录，脚本则不行。脚本需要使用 [`export`](https://man7.org/linux/man-pages/man1/export.1p.html) 将环境变量导出，并将值传递给环境变量。
- 与其他程序语言一样，函数可以提高代码模块性、代码复用性并创建清晰性的结构。shell脚本中往往也会包含它们自己的函数定义。

### shellcheck
这个工具可以帮助检查shell函数当中是否有潜在的错误
安装方式：
- 需要先对apt-get进行升级(注意要在sudo模式下)
```bash
sudo apt-get update
sudo apt-get install shellcheck

hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ shellcheck mcd.sh

In mcd.sh line 1:
^-- SC2148 (error): Tips depend on target shell and yours is unknown. Add a shebang or a 'shell' directive.

In mcd.sh line 4:
        cd "$1"
        ^-----^ SC2164 (warning): Use 'cd ... || exit' or 'cd ... || return' in case cd fails.

Did you mean:
        cd "$1" || exit

For more information:
  https://www.shellcheck.net/wiki/SC2148 -- Tips depend on target shell and y...
  https://www.shellcheck.net/wiki/SC2164 -- Use 'cd ... || exit' or 'cd ... |...
```

# Shell 工具

## 查看命令如何使用

看到这里，您可能会有疑问，我们应该如何为特定的命令找到合适的标记呢？例如 `ls -l`, `mv -i` 和 `mkdir -p`。更普遍的是，给您一个命令行，您应该怎样了解如何使用这个命令行并找出它的不同的选项呢？ 一般来说，您可能会先去网上搜索答案，但是，UNIX 可比 StackOverflow 出现的早，因此我们的系统里其实早就包含了可以获取相关信息的方法。

在上一节中我们介绍过，最常用的方法是为对应的命令行添加`-h` 或 `--help` 标记。另外一个更详细的方法则是使用`man` 命令。[`man`](https://man7.org/linux/man-pages/man1/man.1.html) 命令是手册（manual）的缩写，它提供了命令的用户手册。

例如，`man rm` 会输出命令 `rm` 的说明，同时还有其标记列表，包括之前我们介绍过的`-i`。 事实上，目前我们给出的所有命令的说明链接，都是网页版的Linux命令手册。即使是您安装的第三方命令，前提是开发者编写了手册并将其包含在了安装包中。在交互式的、基于字符处理的终端窗口中，一般也可以通过 `:help` 命令或键入 `?` 来获取帮助。

有时候手册内容太过详实，让我们难以在其中查找哪些最常用的标记和语法。 [TLDR pages](https://tldr.sh/) 是一个很不错的替代品，它提供了一些案例，可以帮助您快速找到正确的选项。

例如，自己就常常在tldr上搜索[`tar`](https://tldr.ostera.io/tar) 和 [`ffmpeg`](https://tldr.ostera.io/ffmpeg) 的用法。
- 注意，要开sudo
```bash
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ sudo tldr conv
ert
convert
Image conversion tool.Part of ImageMagick.More information: https://imagemagick.org/script/convert.php.

 - Convert an image from JPG to PNG:
   convert {{path/to/input_image.jpg}} {{path/to/output_image.png}}

 - Scale an image to 50% of its original size:
   convert {{path/to/input_image.png}} -resize 50% {{path/to/output_image.png}}

 - Scale an image keeping the original aspect ratio to a maximum dimension of 640x480:
   convert {{path/to/input_image.png}} -resize 640x480 {{path/to/output_image.png}}

 - Horizontally append images:
   convert {{path/to/image1.png path/to/image2.png ...}} +append {{path/to/output_image.png}}

 - Vertically append images:
   convert {{path/to/image1.png path/to/image2.png ...}} -append {{path/to/output_image.png}}

 - Create a GIF from a series of images with 100ms delay between them:
   convert {{path/to/image1.png path/to/image2.png ...}} -delay {{10}} {{path/to/animation.gif}}

 - Create an image with nothing but a solid red background:
   convert -size {{800x600}} "xc:{{#ff0000}}" {{path/to/image.png}}

 - Create a favicon from several images of different sizes:
   convert {{path/to/image1.png path/to/image2.png ...}} {{path/to/favicon.ico}}
```
## 查找文件

程序员们面对的最常见的重复任务就是查找文件或目录。所有的类UNIX系统都包含一个名为 [`find`](https://man7.org/linux/man-pages/man1/find.1.html) 的工具，它是 shell 上用于查找文件的绝佳工具。`find`命令会递归地搜索符合条件的文件，例如：

```bash
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '*/test/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'
```

除了列出所寻找的文件之外，find 还能对所有查找到的文件进行操作。这能极大地简化一些单调的任务。
```bash
# 删除全部扩展名为.tmp 的文件
find . -name '*.tmp' -exec rm {} \;
# 查找全部的 PNG 文件并将其转换为 JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

尽管 `find` 用途广泛，它的语法却比较难以记忆。例如，为了查找满足模式 `PATTERN` 的文件，您需要执行 `find -name '*PATTERN*'` (如果您希望模式匹配时是不区分大小写，可以使用`-iname`选项）

您当然可以使用 alias 设置别名来简化上述操作，但 shell 的哲学之一便是寻找（更好用的）替代方案。 记住，shell 最好的特性就是您只是在调用程序，因此您只要找到合适的替代程序即可（甚至自己编写）。

例如，[`fd`](https://github.com/sharkdp/fd) 就是一个更简单、更快速、更友好的程序，它可以用来作为`find`的替代品。它有很多不错的默认设置，例如输出着色、默认支持正则匹配、支持unicode并且我认为它的语法更符合直觉。以模式`PATTERN` 搜索的语法是 `fd PATTERN`。

大多数人都认为 `find` 和 `fd` 已经很好用了，但是有的人可能想知道，我们是不是可以有更高效的方法，例如不要每次都搜索文件而是通过编译索引或建立数据库的方式来实现更加快速地搜索。

这就要靠 [`locate`](https://man7.org/linux/man-pages/man1/locate.1.html) 了。 `locate` 使用一个由 [`updatedb`](https://man7.org/linux/man-pages/man1/updatedb.1.html)负责更新的数据库，在大多数系统中 `updatedb` 都会通过 [`cron`](https://man7.org/linux/man-pages/man8/cron.8.html) 每日更新。这便需要我们在速度和时效性之间作出权衡。而且，`find` 和类似的工具可以通过别的属性比如文件大小、修改时间或是权限来查找文件，`locate`则只能通过文件名。 [这里](https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other)有一个更详细的对比。

## 查找代码

查找文件是很有用的技能，但是很多时候您的目标其实是查看文件的内容。一个最常见的场景是您希望查找具有某种模式的全部文件，并找它们的位置。

为了实现这一点，很多类UNIX的系统都提供了[`grep`](https://man7.org/linux/man-pages/man1/grep.1.html)命令，它是用于对输入文本进行匹配的通用工具。它是一个非常重要的shell工具，我们会在后续的数据清理课程中深入的探讨它。

`grep` 有很多选项，这也使它成为一个非常全能的工具。其中我经常使用的有 `-C` ：获取查找结果的上下文（Context）；`-v` 将对结果进行反选（Invert），也就是输出不匹配的结果。举例来说， `grep -C 5` 会输出匹配结果前后五行。当需要搜索大量文件的时候，使用 `-R` 会递归地进入子目录并搜索所有的文本文件。

但是，我们有很多办法可以对 `grep -R` 进行改进，例如使其忽略`.git` 文件夹，使用多CPU等等。

因此也出现了很多它的替代品，包括 [ack](https://beyondgrep.com/), [ag](https://github.com/ggreer/the_silver_searcher) 和 [rg](https://github.com/BurntSushi/ripgrep)。它们都特别好用，但是功能也都差不多，我比较常用的是 ripgrep (`rg`) ，因为它速度快，而且用法非常符合直觉。例子如下：

```bash
# 查找所有使用了 requests 库的文件
rg -t py 'import requests'
# 查找所有没有写 shebang 的文件（包含隐藏文件）
rg -u --files-without-match "^#!"
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

与 `find`/`fd` 一样，重要的是你要知道有些问题使用合适的工具就会迎刃而解，而具体选择哪个工具则不是那么重要。

## 查找 shell 命令

目前为止，我们已经学习了如何查找文件和代码，但随着你使用shell的时间越来越久，您可能想要找到之前输入过的某条命令。首先，按向上的方向键会显示你使用过的上一条命令，继续按上键则会遍历整个历史记录。

`history` 命令允许您以程序员的方式来访问shell中输入的历史命令。这个命令会在标准输出中打印shell中的里面命令。如果我们要搜索历史记录，则可以利用管道将输出结果传递给 `grep` 进行模式搜索。 `history | grep find` 会打印包含find子串的命令。

对于大多数的shell来说，您可以使用 `Ctrl+R` 对命令历史记录进行回溯搜索。敲 `Ctrl+R` 后您可以输入子串来进行匹配，查找历史命令行。

反复按下就会在所有搜索结果中循环。在 [zsh](https://github.com/zsh-users/zsh-history-substring-search) 中，使用方向键上或下也可以完成这项工作。

`Ctrl+R` 可以配合 [fzf](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r) 使用。`fzf` 是一个通用对模糊查找工具，它可以和很多命令一起使用。这里我们可以对历史命令进行模糊查找并将结果以赏心悦目的格式输出。
```bash
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯$ cat example.sh | fzf
                echo "# foobar" >> "$file"
```
另外一个和历史命令相关的技巧我喜欢称之为**基于历史的自动补全**。 这一特性最初是由 [fish](https://fishshell.com/) shell 创建的，它可以根据您最近使用过的开头相同的命令，动态地对当前对shell命令进行补全。这一功能在 [zsh](https://github.com/zsh-users/zsh-autosuggestions) 中也可以使用，它可以极大的提高用户体验。

你可以修改 shell history 的行为，例如，如果在命令的开头加上一个空格，它就不会被加进shell记录中。当你输入包含密码或是其他敏感信息的命令时会用到这一特性。 为此你需要在`.bashrc`中添加`HISTCONTROL=ignorespace`或者向`.zshrc` 添加 `setopt HIST_IGNORE_SPACE`。 如果你不小心忘了在前面加空格，可以通过编辑。`bash_history`或 `.zhistory` 来手动地从历史记录中移除那一项。

## 文件夹导航

之前对所有操作我们都默认一个前提，即您已经位于想要执行命令的目录下，但是如何才能高效地在目录 间随意切换呢？有很多简便的方法可以做到，比如设置alias，使用 [ln -s](https://man7.org/linux/man-pages/man1/ln.1.html) 创建符号连接等。而开发者们已经想到了很多更为精妙的解决方案。

由于本课程的目的是尽可能对你的日常习惯进行优化。因此，我们可以使用[`fasd`](https://github.com/clvv/fasd)和 [autojump](https://github.com/wting/autojump) 这两个工具来查找最常用或最近使用的文件和目录。

Fasd 基于 [_frecency_](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/Places/Frecency_algorithm) 对文件和文件排序，也就是说它会同时针对频率（_frequency_）和时效（_recency_）进行排序。默认情况下，`fasd`使用命令 `z` 帮助我们快速切换到最常访问的目录。例如， 如果您经常访问`/home/user/files/cool_project` 目录，那么可以直接使用 `z cool` 跳转到该目录。对于 autojump，则使用`j cool`代替即可。

还有一些更复杂的工具可以用来概览目录结构，例如 [`tree`](https://linux.die.net/man/1/tree), [`broot`](https://github.com/Canop/broot) 或更加完整的文件管理器，例如 [`nnn`](https://github.com/jarun/nnn) 或 [`ranger`](https://github.com/ranger/ranger)。

## 退出WSL的方法
对于WSL，无法直接通过如下命令退出
```bash
su -
poweroff
```
原因主要是因为 WSL 下的 Ubuntu 不在物理机或虚拟机中运行。您正在运行的实际上是托管（换句话说，我们通常无法直接与之交互）WSL2 VM 中的 Ubuntu 容器。

可以在PowerShell中使用如下语句：
```bash
wsl --shutdown
```
这样就可以正常终止Ubuntu了

## Diff format
Diff（差异）是一种文件比较工具，用于比较两个文本文件的差异。Diff format 是指 Diff 工具输出的文本格式，它显示两个文本文件之间的变更，包括添加、删除和修改的部分。

Diff 格式的基本形式是：

```plaintext
--- file1	时间戳
+++ file2	时间戳
@@ -m,n +m,n @@		# 描述变更的位置
```

其中：

- `--- file1` 和 `+++ file2` 分别表示两个文件的文件名。
- `@@ -m,n +m,n @@` 描述了发生变更的地方。`-m,n` 表示原文件中从第 m 行开始的 n 行，`+m,n` 表示新文件中从第 m 行开始的 n 行。

Diff 格式的示例：

```plaintext
--- old_file.txt	2022-02-24 10:00:00
+++ new_file.txt	2022-02-24 11:00:00
@@ -1,3 +1,3 @@
 This is the old file.
-It contains some content.
+It now contains different content.
 And it ends here.
```

上述例子表示 `old_file.txt` 和 `new_file.txt` 之间的差异。在第 3 行，原来的文本是 "It contains some content."，现在的文本是 "It now contains different content."。

Diff 格式常用于版本控制系统（例如 Git）和软件开发中，以显示源代码文件之间的更改。这样的格式便于程序理解和应用。

