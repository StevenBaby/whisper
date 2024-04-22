# WSL2 相关问题记录

## 安装

- https://learn.microsoft.com/en-us/windows/wsl/install-manual

添加用户

useradd -m -G root steven

修改用户密码：

passwd steven

修改默认用户

在安装目录执行，git 中有 arch.exe 文件，不要混淆：

    Arch.exe config --default-user {username}

或者直接修改注册表：

    HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss

    DefaultUid 的值

https://superuser.com/questions/1566022/how-to-set-default-user-for-manually-installed-wsl-distro 

修改 mirrorlist

    wget -q -O /etc/pacman.d/mirrorlist https://archlinux.org/mirrorlist/?country=CN&protocol=http&protocol=https&ip_version=4

zsh


## zsh

安装 zsh：

    sudo pacman -S zsh

安装 oh-my-zsh:

    sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

安装 autosuggestions(自动补全) 插件：

    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

安装 syntax-highlighting(语法高亮) 插件：

    git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

安装 `virtualenv`：

    sudo pacman -S python-virtualenvwrapper

附上我自己的 `.zshrc` 配置文件：

```sh
export ZSH="$HOME/.oh-my-zsh"

ZSH_THEME="agnoster"

plugins=(
	git
	docker
	zsh-autosuggestions
	zsh-syntax-highlighting 
)

source $ZSH/oh-my-zsh.sh

# User configuration

ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE="fg=black,bg=white,bold,underline"

prompt_context() {
  if [[ "$USERNAME" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment magenta default "%(!.%{%F{yellow}%}.)%n@%m"
  fi
}

prompt_dir() {
  prompt_segment blue $CURRENT_FG '%1~'
}

prompt_virtualenv() {
  local virtualenv_path="$VIRTUAL_ENV"
  if [[ -n $virtualenv_path ]]; then
    prompt_segment yellow black " $(basename $virtualenv_path)"
  fi
}

VIRTUAL_ENV_DISABLE_PROMPT=true
export WORKON_HOME=$HOME/.envs
source /usr/bin/virtualenvwrapper.sh
```

## 修改主机名

如果不存在，则添加如下文件：

`/etc/wsl.conf`

然后写入内容：

```conf
[network]
hostname = arch-wsl
generateHosts = false
```

然后重新启动

## 备份还原

    wsl --export Arch2 D:\WSL\Backup\Arch1.tar

    wsl --import Arch2 D:\WSL\Arch2 D:\WSL\Backup\Arch1.tar --version 2

https://superuser.com/questions/1550622/move-wsl2-file-system-to-another-drive


## 安装

https://gist.github.com/ld100/3376435a4bb62ca0906b0cff9de4f94b


## 一些问题

- Error: Can't open display: :0l

https://github.com/microsoft/wslg/issues/1192

    ln -s /mnt/wslg/.X11-unix/X0 /tmp/.X11-unix/
