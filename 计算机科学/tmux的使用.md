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