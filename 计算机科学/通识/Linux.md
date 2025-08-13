## `tmux`

`tmux` 是一个终端复用器（terminal multiplexer），它允许你在一个终端窗口中创建和管理多个独立的终端会话。这对于远程工作、长时间运行任务以及保持工作环境非常有用。

### 为什么使用 `tmux`？

  * **会话保持**：即使你断开 SSH 连接，在 `tmux` 中运行的程序也会继续在后台运行。你可以随时重新连接并恢复到之前的会话。
  * **多窗口管理**：你可以在一个 `tmux` 会话中创建多个窗口，每个窗口就像一个独立的终端。
  * **分屏**：你可以在一个窗口中创建多个窗格（pane），实现分屏效果，同时查看多个终端的输出。
  * **结对编程**：多个用户可以连接到同一个 `tmux` 会话，实时共享同一个终端视图。

-----

### 安装 tmux

根据你的操作系统，选择相应的安装命令：

  * **macOS (使用 Homebrew):**

    ```bash
    brew install tmux
    ```

  * **Ubuntu / Debian:**

    ```bash
    sudo apt-get update
    sudo apt-get install tmux
    ```

  * **CentOS / RHEL:**

    ```bash
    sudo yum install tmux
    ```

-----

### 基本概念

`tmux` 的结构分为三层：

1.  **会话 (Session)**：一个 `tmux` 服务器可以管理多个会话。每个会话是一个独立的工作空间，包含一个或多个窗口。
2.  **窗口 (Window)**：一个会话可以包含多个窗口。窗口占据整个屏幕，你可以像使用浏览器标签页一样在不同窗口间切换。
3.  **窗格 (Pane)**：一个窗口可以被分割成多个窗格。每个窗格都是一个独立的伪终端。

-----

### 核心：前缀键 (Prefix)

`tmux` 的所有快捷键都需要先按下一个“前缀键”，然后再按其他键来触发相应的功能。`tmux` 的默认前缀键是 `Ctrl-b` (按住 `Ctrl` 键，再按 `b` 键)。

**操作流程**：

1.  按下 `Ctrl-b`。
2.  松开 `Ctrl-b`。
3.  按下另一个功能键 (例如 `c`、`%`、`"` 等)。

在接下来的教程中，`Prefix` 就代表 `Ctrl-b`。

-----

### 会话管理 (Session)

这些命令通常在你的常规终端（`tmux` 之外）中执行。

  * **启动一个新的会话：**

    ```bash
    tmux
    ```

  * **启动一个命名会话：**

    ```bash
    tmux new -s <session_name>
    ```

    *示例：* `tmux new -s my_project`

  * **列出所有会话：**

    ```bash
    tmux ls
    ```

  * **断开（分离）当前会话 (Detach)：**
    在 `tmux` 中，按下 `Prefix` + `d`。这会让你返回到常规终端，但会话仍在后台运行。

  * **重新连接（附加）到上一个会-话 (Attach)：**

    ```bash
    tmux attach
    ```

    或者

    ```bash
    tmux a
    ```

  * **连接到指定的会话：**

    ```bash
    tmux attach -t <session_name>
    ```

    *示例：* `tmux attach -t my_project`
    note：`-t` 是 **`target-session`** 的缩写

  * **关闭一个会话：**

    ```bash
    tmux kill-session -t <session_name>
    ```

  * **关闭所有会话：**

    ```bash
    tmux kill-server
    ```

-----

### 窗口管理 (Window)

这些快捷键在 `tmux` 会话内部使用。

  * **创建一个新窗口：**
    `Prefix` + `c` (create)

  * **切换到下一个窗口：**
    `Prefix` + `n` (next)

  * **切换到上一个窗口：**
    `Prefix` + `p` (previous)

  * **通过编号切换窗口：**
    `Prefix` + `<number>` (例如 `Prefix` + `0`, `Prefix` + `1`...)

  * **重命名当前窗口：**
    `Prefix` + `,` (逗号)

  * **关闭当前窗口：**
    `Prefix` + `&` (会提示确认)
    或者直接在窗口中输入 `exit` 命令。

  * **列出所有窗口以供选择：**
    `Prefix` + `w` (window)

-----

### 窗格管理 (Pane)

这些快捷键在 `tmux` 会话内部使用。

  * **水平分割窗格 (左右)：**
    `Prefix` + `%`

  * **垂直分割窗格 (上下)：**
    `Prefix` + `"`

  * **在窗格之间切换：**
    `Prefix` + `方向键` (`↑`, `↓`, `←`, `→`)

  * **交换窗格位置：**
    `Prefix` + `{` (与前一个窗格交换)
    `Prefix` + `}` (与后一个窗格交换)

  * **关闭当前窗格：**
    `Prefix` + `x` (会提示确认)
    或者直接在窗格中输入 `exit` 命令。

  * **最大化/恢复当前窗格 (全屏/取消全屏)：**
    `Prefix` + `z` (zoom)

  * **显示窗格编号：**
    `Prefix` + `q` (按显示的数字可快速切换)

  * **调整窗格大小：**
    `Prefix` + `Ctrl + 方向键` (按住 `Ctrl` 和前缀键，然后按方向键)

-----

### 其他常用功能

  * **进入滚动/复制模式：**
    `Prefix` + `[`
    进入此模式后，你可以像在 Vim 中一样使用方向键或 `j`/`k` 上下滚动。

      * 按 `q` 退出此模式。
      * 按 `空格键` 开始选择。
      * 按 `Enter` 复制选中的内容。

  * **粘贴复制的内容：**
    `Prefix` + `]`

  * **显示时钟：**
    `Prefix` + `t`

  * **显示所有快捷键帮助：**
    `Prefix` + `?`

-----

### 自定义 tmux (`~/.tmux.conf`)

你可以通过创建一个 `~/.tmux.conf` 文件来高度自定义 `tmux` 的行为。

**一个简单的示例配置文件：**

```tmux
# 将前缀键从 Ctrl-b 改为 Ctrl-a
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix

# 开启鼠标支持 (推荐)
set -g mouse on

# 使用更直观的分屏快捷键
bind | split-window -h
bind - split-window -v
unbind '"'
unbind %

# 使用 Vim 键位在窗格间移动
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# 设置窗口和窗格的起始编号为 1
set -g base-index 1
setw -g pane-base-index 1

# 重新加载配置文件，无需重启 tmux
bind r source-file ~/.tmux.conf \; display "Reloaded!"
```

**如何应用配置？**

1.  保存文件后，在 `tmux` 中按下 `Prefix` + `r` (根据上面的自定义配置) 来重新加载。
2.  或者，关闭并重新启动 `tmux`。

## `curl`

`curl` 是一个功能强大的命令行工具，用于通过 URL 与服务器进行数据传输。它支持多种协议，包括 HTTP, HTTPS, FTP, FTPS, SCP, SFTP, IMAP, SMTP 等等。由于其灵活性和强大的功能，`curl` 是开发人员、系统管理员和网络爱好者必备的工具之一。

### 1\. `curl` 的基本用法

`curl` 最简单的用法是在其后直接跟上一个 URL。默认情况下，`curl` 会执行一个 GET 请求，并将服务器返回的内容输出到标准输出（通常是你的终端屏幕）。

**语法：**

```bash
curl [options] [URL]
```

**示例：获取网页内容**

```bash
curl https://www.google.com
```

这个命令会获取 Google 首页的 HTML 内容并显示在终端上。

-----

### 2\. 保存输出到文件

如果你想将 `curl` 的输出保存到文件中，而不是显示在屏幕上，可以使用 `-o` 或 `-O` 选项。

  * `-o` (小写o): 将内容保存到指定的文件名。

    ```bash
    curl -o google.html https://www.google.com
    ```

    这会将网页内容保存为 `google.html` 文件。

  * `-O` (大写O): 将内容保存到文件，文件名使用 URL 中最后的部分。

    ```bash
    curl -O https://curl.se/logo/curl-logo.svg
    ```

    这会下载文件并将其命名为 `curl-logo.svg`。

-----

### 3\. 发送不同类型的 HTTP 请求

`curl` 不仅仅能发送 GET 请求。通过 `-X` 选项，你可以指定不同的 HTTP 请求方法。

#### **GET 请求 (默认)**

如前所述，不加任何选项就是 GET 请求。

#### **POST 请求**

POST 请求通常用于向服务器提交数据。使用 `-d` 或 `--data` 选项来发送数据。

**示例：发送表单数据**

```bash
curl -d "name=John&age=30" https://api.example.com/users
```

这个命令会发送一个 POST 请求，请求体内容为 `name=John&age=30`。默认情况下，`Content-Type` 会被设置为 `application/x-www-form-urlencoded`。

**示例：发送 JSON 数据**
在现代 API 中，发送 JSON 数据非常普遍。你需要手动设置 `Content-Type` 头。

```bash
curl -d '{"name":"John", "age":30}' -H "Content-Type: application/json" https://api.example.com/users
```

#### **PUT 请求**

PUT 请求通常用于更新服务器上的资源。

```bash
curl -X PUT -d '{"status":"active"}' -H "Content-Type: application/json" https://api.example.com/users/123
```

#### **DELETE 请求**

DELETE 请求用于删除服务器上的资源。

```bash
curl -X DELETE https://api.example.com/users/123
```

-----

### 4\. 处理 HTTP 头部 (Headers)

HTTP 头部包含了关于请求和响应的元数据。

  * **查看响应头部 (`-i` 或 `-v`)**

      * `-i` (`--include`): 在输出中包含 HTTP 响应头部。
        ```bash
        curl -i https://www.google.com
        ```
      * `-v` (`--verbose`): 显示详细的通信过程，包括请求头和响应头。这对于调试非常有用。
        ```bash
        curl -v https://www.google.com
        ```

  * **自定义请求头部 (`-H` 或 `--header`)**
    你可以使用 `-H` 选项添加或修改请求头部。

    ```bash
    # 模拟浏览器发送请求
    curl -H "User-Agent: My-Awesome-Browser/1.0" https://www.example.com

    # 发送一个带认证令牌的请求 (常用于 API)
    curl -H "Authorization: Bearer YOUR_ACCESS_TOKEN" https://api.example.com/data
    ```

-----

### 5\. 文件上传

你可以使用 `-F` 或 `--form` 选项来模拟 HTML 表单提交，从而实现文件上传。

**语法：**

```bash
curl -F "name=@/path/to/your/file" [URL]
```

`@` 符号告诉 `curl` 后面跟着的是一个文件路径，需要读取该文件的内容作为字段的值。

**示例：**

```bash
curl -F "profile_picture=@/home/user/images/avatar.jpg" https://api.example.com/upload
```

-----

### 6\. 处理 Cookies

`curl` 可以很方便地处理 Cookies。

  * **发送 Cookies (`-b` 或 `--cookie`)**

    ```bash
    curl -b "session_id=abc123; user=john" https://www.example.com
    ```

  * **保存服务器返回的 Cookies (`-c` 或 `--cookie-jar`)**

    ```bash
    # 将服务器设置的 cookies 保存到 cookies.txt 文件
    curl -c cookies.txt https://www.example.com/login
    ```

  * **读取并发送 Cookies**

    ```bash
    # 使用之前保存的 cookies.txt 文件来访问需要登录的页面
    curl -b cookies.txt https://www.example.com/dashboard
    ```

-----

### 7\. 认证 (Authentication)

如果一个页面需要用户名和密码认证，可以使用 `-u` 或 `--user` 选项。

**语法：**

```bash
curl -u username:password [URL]
```

**示例：**

```bash
curl -u admin:123456 https://admin.example.com
```

`curl` 会自动进行 Base64 编码并添加到 `Authorization` 头部。

-----

### 8\. 其他常用选项

  * **`-L` 或 `--location`**: 当服务器返回 3xx 重定向时，`curl` 默认不会跟随跳转。使用 `-L` 选项可以让 `curl` 跟随重定向。

    ```bash
    curl -L http://google.com  # 会重定向到 https://www.google.com/
    ```

  * **`-s` 或 `--silent`**: 静默模式。不显示进度条或错误信息。

    ```bash
    # 只输出最终结果
    curl -s https://www.example.com > page.html
    ```

  * **`-k` 或 `--insecure`**: 忽略 SSL/TLS 证书验证。 **警告：** 这会带来安全风险，只在你知道自己在做什么的情况下使用（例如，在本地开发环境中使用自签名证书）。

    ```bash
    curl -k https://self-signed.local
    ```

  * **`--proxy [PROTOCOL://]HOST[:PORT]`**: 通过指定的代理服务器发送请求。

    ```bash
    curl --proxy http://proxy.example.com:8080 https://www.google.com
    ```

### 总结

`curl` 是一个极其强大的工具，以上只是其众多功能的一部分。要了解所有选项和功能，你可以随时查阅其官方文档或使用 `man curl` 命令查看手册页。

**常用命令速查表:**

| 功能 | 命令 |
| :--- | :--- |
| 获取 URL 内容 | `curl [URL]` |
| 保存到文件（指定文件名） | `curl -o filename.txt [URL]` |
| 保存到文件（使用远程文件名） | `curl -O [URL]` |
| 显示响应头 | `curl -i [URL]` |
| 显示详细过程（用于调试） | `curl -v [URL]` |
| 发送 POST 数据 | `curl -d "key=value" [URL]` |
| 发送 JSON 数据 | `curl -d '{"key":"value"}' -H "Content-Type: application/json" [URL]` |
| 自定义请求头 | `curl -H "Header-Name: Header-Value" [URL]` |
| 跟随重定向 | `curl -L [URL]` |
| 使用用户名和密码认证 | `curl -u user:pass [URL]` |
| 文件上传 | `curl -F "file=@/path/to/file" [URL]` |

### `grep` 的基本用法

`grep` 最核心的功能是在文件或标准输入中查找包含特定模式的行。

#### 基本语法

```bash
grep [选项] "要搜索的模式" [文件名...]
```

  - `[选项]`: 用来控制 `grep` 的行为，例如大小写不敏感、显示行号等。
  - `"要搜索的模式"`: 你想要查找的文本或正则表达式。建议总是用双引号 `"` 包裹起来，以避免特殊字符被 Shell 误解。
  - `[文件名...]`: 你想在哪个或哪些文件中进行搜索。如果省略，`grep` 会从标准输入（通常是管道的上一级命令的输出）中读取内容。

#### 简单示例

假设我们有一个名为 `story.txt` 的文件，内容如下：

```
Hello world, this is a simple story.
The quick brown fox jumps over the lazy dog.
Linux is powerful, and linux is fun.
Learning GREP is a good starting point.
The end.
```

**1. 在单个文件中查找**

查找包含 "fox" 的行：

```bash
grep "fox" story.txt
```

输出：

```
The quick brown fox jumps over the lazy dog.
```

**2. 在多个文件中查找**

如果你还有另一个文件 `notes.txt`，可以同时搜索它们：

```bash
grep "linux" story.txt notes.txt
```

`grep` 会在输出的每一行前加上文件名，以示区分。

**3. 配合管道使用**

`grep` 的强大之处在于它可以作为过滤器，处理其他命令的输出。

例如，查看系统中与 `systemd` 相关的进程：

```bash
ps aux | grep "systemd"
```

这个命令会将 `ps aux` 列出的所有进程信息通过管道传给 `grep`，`grep` 再筛选出含有 "systemd" 的行。

-----

### 常用选项 (Options)

选项极大地扩展了 `grep` 的功能。

| 选项 | 长选项 | 描述 | 示例 |
| :--- | :--- | :--- | :--- |
| `-i` | `--ignore-case` | **忽略大小写**。 | `grep -i "linux" story.txt` 会匹配 "Linux" 和 "linux"。 |
| `-v` | `--invert-match` | **反向查找**，只显示不匹配的行。 | `grep -v "lazy" story.txt` 会显示所有不含 "lazy" 的行。 |
| `-n` | `--line-number` | 在输出的每一行前**显示行号**。 | `grep -n "story" story.txt` 会显示 `1:Hello world, this is a simple story.` |
| `-c` | `--count` | **只输出匹配的行数**，不显示匹配的内容。 | `grep -c "is" story.txt` 会输出 `3`。 |
| `-w` | `--word-regexp` | **匹配整个单词**，而不是字符串的一部分。 | `grep -w "is" story.txt` 只会匹配 " is "，而不会匹配 "this"。 |
| `-o` | `--only-matching` | **只显示匹配到的部分**，而不是整行。 | `grep -o "is" story.txt` 会每行一个 "is" 地输出三次。 |
| `-r`\<br\>`-R` | `--recursive` | **递归搜索**目录下的所有文件。`-R` 还会跟随符号链接。 | `grep -r "error" /var/log/` 会搜索 `/var/log` 目录及其所有子目录中的 "error"。 |
| `-l` | `--files-with-matches`| **只列出包含匹配项的文件名**，而不是匹配的行。 | `grep -l "dog" story.txt notes.txt` |
| `-A N`| `--after-context=N` | 显示匹配行及其**后 N 行**。 | `grep -A 1 "fox" story.txt` (显示匹配行和它后面1行) |
| `-B N`| `--before-context=N`| 显示匹配行及其**前 N 行**。 | `grep -B 1 "fox" story.txt` (显示匹配行和它前面1行) |
| `-C N`| `--context=N` | 显示匹配行及其**前后各 N 行**。 | `grep -C 1 "fox" story.txt` (显示匹配行和它前后各1行) |

-----

### 使用正则表达式 (Regular Expressions)

`grep` 的真正威力来自于它对正则表达式的支持。正则表达式是一种描述文本模式的强大语言。

#### 基础正则表达式元字符

| 元字符 | 描述 | 示例 |
| :--- | :--- | :--- |
| `.` | 匹配**任意单个字符**（除了换行符）。 | `grep "l..y" story.txt` 会匹配 "lazy"。 |
| `*` | 匹配**前一个字符零次或多次**。 | `grep "s*tory" story.txt` 会匹配 "story", "story", "ssstory" 等。 |
| `^` | 匹配**行首**。 | `grep "^The" story.txt` 会匹配以 "The" 开头的行。 |
| `$` | 匹配**行尾**。 | `grep "end.$" story.txt` 会匹配以 "end." 结尾的行。 |
| `[]` | 匹配方括号中**任意一个字符**。 | `grep "[lL]inux" story.txt` 会匹配 "Linux" 或 "linux"。 |
| `[^]` | 匹配**不在**方括号中的任意一个字符。 | `grep "[^A-Z]og" story.txt` 会匹配 "dog"，但不会匹配 "Log"。 |
| `\` | **转义字符**，用于匹配特殊字符本身。 | `grep "end\." story.txt` 只会匹配 "end."，而不是 "end" 加上任意字符。 |

#### 示例：使用正则表达式

1.  **查找以大写字母开头的行：**

    ```bash
    grep "^[A-Z]" story.txt
    ```

    输出：

    ```
    Hello world, this is a simple story.
    Linux is powerful, and linux is fun.
    Learning GREP is a good starting point.
    The end.
    ```

2.  **查找空行：**

    ```bash
    grep "^$" filename.txt
    ```

3.  **查找包含数字的行：**

    ```bash
    grep "[0-9]" filename.txt
    ```

#### 扩展正则表达式 (Extended Regular Expressions)

`grep` 默认使用基础正则表达式。要使用功能更强的扩展正则表达式，需要加上 `-E` 选项 (或者直接使用 `egrep` 命令，效果相同)。

| 扩展元字符 | 描述 | 示例 |
| :--- | :--- | :--- |
| `+` | 匹配前一个字符**一次或多次**。 | `grep -E "go+d" story.txt` 会匹配 "god", "good", 但不匹配 "gd"。 |
| `?` | 匹配前一个字符**零次或一次**。 | `grep -E "colou?r"` 会匹配 "color" 和 "colour"。 |
| `\|` | **或**操作符，匹配 `|` 两边的任意一个模式。 | `grep -E "fox|dog" story.txt` 会匹配包含 "fox" 或 "dog" 的行。 |
| `()` | **分组**，将多个字符作为一个整体。 | `grep -E "(lazy|quick) dog"` 会匹配 "lazy dog" 或 "quick dog"。 |

-----

### 实践案例

1.  **在 Nginx 配置文件中查找所有监听的端口，并排除注释行：**

    ```bash
    grep -E "^[[:space:]]*listen" /etc/nginx/nginx.conf | grep -v "#"
    ```

      - `grep -E "^[[:space:]]*listen"`: 查找以任意数量的空白字符开头，后跟 "listen" 的行。
      - `| grep -v "#"`: 将结果通过管道传给下一个 `grep`，排除掉包含 `#` 的注释行。

2.  **从 `dmesg`（内核日志）中高亮显示错误或失败信息：**

    ```bash
    dmesg | grep -i -E "error|fail|warning"
    ```

    这个命令会在启动日志中查找不分大小写的 "error", "fail" 或 "warning"。很多系统会自动给 `grep` 加上 `--color=auto` 别名，这样匹配到的关键词会高亮显示，非常直观。

3.  **递归地在项目代码中查找所有包含 `TODO` 的地方：**

    ```bash
    grep -rnw "TODO" ./src
    ```

      - `-r`: 递归。
      - `-n`: 显示行号。
      - `-w`: 匹配整个单词 "TODO"，避免匹配到 "TODOLIST" 这样的词。
      - `./src`: 在当前目录下的 `src` 文件夹中查找。

### 总结

`grep` 是一个极其灵活的工具。掌握它的关键在于：

  - 熟悉常用选项，特别是 `-i`, `-v`, `-n`, `-r`, `-w`。
  - 理解基础正则表达式 `^, $, ., *, []` 的含义。
  - 知道何时使用 `-E` 开启扩展正则来使用 `|`, `+`, `?` 等更方便的元字符。
  - 勤于通过管道 `|` 将 `grep` 和其他命令（如 `ps`, `ls`, `cat`, `dmesg`）结合起来，解决实际问题。

通过不断练习，`grep` 会成为你在命令行中进行文本处理和信息挖掘的得力助手。

在 Linux 和其他类 Unix 系统中，管道运算符 `|` 是一个非常强大且常用的功能。它允许你将一个命令的输出（标准输出）直接作为另一个命令的输入（标准输入）。通过这种方式，你可以将多个简单的命令组合起来，完成复杂的任务。

这完美体现了 Unix 的设计哲学：“做一件事，并把它做好”（Do one thing and do it well）。每个小程序专注于一个功能，而管道则将它们粘合在一起。

-----

### 工作原理

当你使用管道时，Shell 会在内存中创建一个管道，它就像一个临时的“水管”，连接着前后两个命令。

1.  `命令1` 执行后，它产生的任何输出（本应显示在屏幕上的内容）都不会显示出来。
2.  相反，这些输出被直接送入管道中。
3.  `命令2` 启动后，它会从管道中读取数据，就像从键盘或文件中读取一样，并将这些数据作为自己的输入来处理。

**基本语法非常简单：**

```bash
命令1 | 命令2 | 命令3 ...
```

-----

### 实用示例

让我们通过一些由浅入深的例子来理解管道的威力。

#### 示例 1：过滤文件列表

假设你想在当前目录下查找所有的 `.log` 文件。

  - `ls -l` 会列出当前目录下所有文件和文件夹的详细信息。
  - `grep ".log"` 会从其输入中筛选出包含 `.log` 的行。

将它们用管道连接起来：

```bash
ls -l | grep ".log"
```

**执行过程：**

1.  `ls -l` 的输出（文件列表）被送到管道。
2.  `grep ".log"` 从管道中读取这个列表，并只打印出包含 `.log` 的那些行。

#### 示例 2：统计文件数量

如果你想知道当前目录（包括隐藏文件）下有多少个文件和目录。

  - `ls -a` 列出所有条目（包括以 `.` 开头的隐藏文件）。
  - `wc -l` (word count - lines) 会统计其输入内容的行数。

```bash
ls -a | wc -l
```

**执行过程：**

1.  `ls -a` 将每个文件/目录名作为一行输出到管道。
2.  `wc -l` 读取这些行并计算总数，最终只输出一个数字。

#### 示例 3：查看特定进程

当你需要检查某个特定程序（例如 Nginx）是否正在运行时，可以使用 `ps` 和 `grep`。

  - `ps aux` 会列出系统中当前正在运行的所有进程。
  - `grep "nginx"` 会筛选出包含 "nginx" 关键词的行。

```bash
ps aux | grep "nginx"
```

**执行过程：**

1.  `ps aux` 的庞大进程列表被输出到管道。
2.  `grep "nginx"` 从中快速找到你关心的那几行（通常包括 nginx 主进程和工作进程，以及 grep 本身）。

#### 示例 4：多级管道（进阶）

这是管道真正发挥威力的地方。假设你想分析一个 web 服务器的访问日志（`access.log`），找出访问量最高的 10 个页面。

```bash
cat access.log | awk '{print $7}' | sort | uniq -c | sort -nr | head -n 10
```

让我们分解这个复杂的命令：

1.  `cat access.log`

      - 读取 `access.log` 文件的内容并将其输出到管道。

2.  `| awk '{print $7}'`

      - `awk` 是一个强大的文本处理工具。这条命令会接收日志内容，并提取出每一行的第 7 个字段（通常是访问的 URL 路径）。

3.  `| sort`

      - 接收 URL 列表并按字母顺序对其进行排序。这是为了让相同的 URL 排列在一起，为下一步做准备。

4.  `| uniq -c`

      - `uniq` 用于去除重复的行。`-c` 选项会额外在每行前面加上该行重复出现的次数。
      - 现在我们得到一个列表，格式为 `次数 URL`。

5.  `| sort -nr`

      - 再次进行排序。`-n` 表示按数值大小排序（而不是按字符串），`-r` 表示逆序（从大到小）。
      - 这样，访问次数最多的 URL 就排在了最前面。

6.  `| head -n 10`

      - `head` 命令用于显示文件或输入的开头部分。`-n 10` 表示只显示前 10 行。

**最终结果**：你得到了 `access.log` 中访问量排名前 10 的 URL 列表及其访问次数。如果没有管道，你需要编写一个脚本或者手动分步执行很多次才能完成这个任务。

-----

### 处理标准错误 (stderr)

默认情况下，管道只传递**标准输出 (stdout)**。命令产生的**标准错误 (stderr)**（例如 "Permission denied" 或 "File not found" 等错误信息）仍然会直接显示在你的终端上，而不会进入管道。

如果你希望将标准错误也一并送入管道，可以使用 `2>&1` 这个重定向语法。它表示将文件描述符 `2` (stderr) 重定向到文件描述符 `1` (stdout)。

**示例：**
假设你在查找一个文件，但过程中会产生很多 "Permission denied" 的错误信息，而你不想看到它们。

```bash
# 这个命令的错误信息会直接打印在屏幕上
find / -name "myfile.txt" | grep "some_pattern"

# 使用 2>&1 将错误信息和正确结果都送入管道
find / -name "myfile.txt" 2>&1 | grep -v "Permission denied"
```

在这个例子中：

1.  `find / -name "myfile.txt" 2>&1` 会执行查找，并将找到的路径（stdout）和遇到的错误（stderr）都送入管道。
2.  `grep -v "Permission denied"` 会接收所有输入，并过滤掉（`-v` 选项表示反向匹配）包含 "Permission denied" 的行，最终只显示你想要的结果。

### 总结

管道 `|` 是 Linux 命令行最核心、最精髓的工具之一。它让你能够：

  - **组合命令**：将简单工具链接起来，形成强大的处理流水线。
  - **提高效率**：避免创建不必要的中间文件来保存临时结果。
  - **简化脚本**：用一行命令代替复杂的脚本逻辑。

熟练掌握管道的使用，是从 Linux 新手迈向高手的关键一步。

### 核心概念：模式 (Modes)

与普通编辑器（如记事本、VS Code）不同，Vim 是一个**模态编辑器 (modal editor)**。这意味着在不同的“模式”下，同一个按键会有不同的功能。初学者最需要掌握的是以下四种模式：

1.  **普通模式 (Normal Mode)**

      - 这是你打开 Vim 后默认进入的模式。
      - 此模式下，你按下的键**不会**直接输入文本，而是被解释为**命令**，例如移动光标、删除文本、复制文本等。
      - 这是 Vim 的主要模式，大部分时间你都应该待在这个模式下。
      - **如何进入**：任何模式下按 `Esc` 键即可返回普通模式。

2.  **插入模式 (Insert Mode)**

      - 此模式下，Vim 的行为和你熟悉的普通编辑器一样，你按下的键会直接**插入**到文本中。
      - **如何进入**：在普通模式下按 `i`, `a`, `o` 等键（详见下文）。
      - **如何退出**：按 `Esc` 键返回普通模式。

3.  **命令模式 (Command-Line Mode)**

      - 此模式用于执行更复杂的操作，如保存文件、退出 Vim、全局替换等。
      - **如何进入**：在普通模式下按冒号 `:`。光标会移动到屏幕左下角，等待你输入命令。
      - **如何退出**：按 `Esc` 键返回普通模式，或者按 `Enter` 执行命令后自动返回。

4.  **可视模式 (Visual Mode)**

      - 此模式用于**选中文本块**，然后可以对选中的文本执行操作（如复制、删除、替换等）。
      - **如何进入**：在普通模式下按 `v` (字符选择), `V` (行选择), 或 `Ctrl+v` (块选择)。
      - **如何退出**：按 `Esc` 键返回普通模式。

**记忆技巧**：无论在哪，迷路了就**狂按 `Esc`**，总能回到最安全的“普通模式”。

-----

### 第一步：打开与退出

  - **打开文件**：在终端中输入 `vim 文件名`。如果文件不存在，Vim会创建一个新文件。
    ```bash
    vim my_script.py
    ```
  - **退出 Vim**：这是初学者遇到的第一个“天坑”。你必须在**普通模式**下，进入**命令模式**来操作。
      - `:q` - **退出 (Quit)**。如果文件被修改过但未保存，Vim 会警告你并阻止退出。
      - `:q!` - **强制退出 (Quit\!)**，不保存任何修改。非常常用！
      - `:w` - **保存 (Write)**。
      - `:wq` - **保存并退出**。
      - `:x` - 功能与 `:wq` 相同，但更智能一些（仅在文件有修改时才写入）。

-----

### 第二步：基本操作（全在普通模式下）

#### 移动光标 (Navigation)

忘记方向键！使用主键盘区的 `h, j, k, l` 来移动，这是 Vim 高效的基石。

  - `h` - 左
  - `j` - 下 (像一个向下的箭头)
  - `k` - 上
  - `l` - 右

**单词级移动:**

  - `w` - (word) 移动到下一个**单词的开头**。
  - `b` - (back) 移动到上一个**单词的开头**。
  - `e` - (end) 移动到当前**单词的结尾**。

**行内移动:**

  - `0` - (数字0) 移动到**行首**。
  - `^` - 移动到本行第一个非空白字符。
  - `$` - 移动到**行尾**。

**文档级移动:**

  - `gg` - 移动到文件的**第一行**。
  - `G` - 移动到文件的**最后一行**。
  - `[行号]G` - 例如 `15G` 会跳转到第 15 行。

#### 进入插入模式 (Entering Insert Mode)

  - `i` - (insert) 在**光标前**开始插入文本。
  - `a` - (append) 在**光标后**开始插入文本。
  - `I` - 在**当前行的行首**开始插入文本。
  - `A` - 在**当前行的行尾**开始插入文本。
  - `o` - 在**当前行的下方**新建一行并进入插入模式。
  - `O` - 在**当前行的上方**新建一行并进入插入模式。

#### 编辑文本 (Editing)

这些命令通常由一个**操作符**和一个**动作**组成。例如 `d` (delete) 是操作符，`w` (word) 是动作。

  - **删除 (Delete)**

      - `x` - 删除光标所在的单个字符。
      - `dw` - 删除一个单词 (delete word)。
      - `d$` - 从光标处删除到行尾。
      - `dd` - **删除整行**。
      - `[N]dd` - 例如 `3dd` 表示删除 3 行。

  - **复制 / 拷贝 (Yank)** - Vim 中不叫 Copy，叫 Yank (y)。

      - `yw` - 复制一个单词 (yank word)。
      - `y$` - 从光标处复制到行尾。
      - `yy` - **复制整行**。
      - `[N]yy` - 例如 `2yy` 表示复制 2 行。

  - **粘贴 (Paste)**

      - `p` - (paste) 将剪贴板中的内容粘贴到**光标之后**。
      - `P` - 将内容粘贴到**光标之前**。

  - **修改 (Change)** - `c` 操作符是 `d` (删除) 和 `i` (插入) 的结合。

      - `cw` - 删除一个单词，并立即进入插入模式 (change word)。
      - `cc` - 删除整行，并立即进入插入模式。

  - **撤销与重做**

      - `u` - (undo) **撤销**上一步操作。
      - `Ctrl+r` - (redo) **重做**被撤销的操作。

-----

### 第三步：命令模式的高级功能

在普通模式下按 `:` 进入。

#### 搜索 (Search)

  - `/模式` - 向下搜索指定的“模式”。按 `n` 跳转到下一个匹配项，`N` 跳转到上一个。

      - 示例: `/error`

  - `?模式` - 向上搜索指定的“模式”。

#### 替换 (Substitute)

语法: `:[范围]s/旧内容/新内容/[标志]`

  - **范围**:
      - `%` - 整篇文章。
      - `1,10` - 第1到10行。
  - **标志**:
      - `g` - (global) 替换行内的所有匹配项，不加则只替换每行第一个。
      - `c` - (confirm) 每次替换前都进行确认。
      - `i` - (ignore case) 忽略大小写。

**常用示例:**

  - `:%s/foo/bar/g` - 将文件中所有的 "foo" 替换为 "bar"。
  - `:10,20s/cat/dog/gc` - 在第10到20行中，替换所有的 "cat" 为 "dog"，每次替换前需要你按 `y` (yes) 确认。

-----