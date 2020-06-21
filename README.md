# Fedora setup

# Update the system

```sh
sudo dnf -y update
```

# Install basic tools

```sh
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo dnf install -y curl dnf-plugins-core entr exfat-utils expect fuse-exfat git httpie jq make moreutils util-linux-user vim wget
sudo dnf install -y geary mpv telegram-desktop the_silver_searcher transmission-gtk vim-X11
```

# Change shell to ZSH

```sh
sudo dnf install -y zsh zsh-syntax-highlighting
git clone https://github.com/sindresorhus/pure.git /tmp/pure
sudo cp /tmp/pure/async.zsh /usr/share/zsh/site-functions/async
sudo cp /tmp/pure/pure.zsh /usr/share/zsh/site-functions/prompt_pure_setup
wget -O ~/.zshrc https://git.grml.org/f/grml-etc-core/etc/zsh/zshrc
cat << EOF > ~/.zshrc.local
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source ~/.profile
PURE_CMD_MAX_EXEC_TIME=1
PURE_PROMPT_SYMBOL=‣
autoload -U promptinit; promptinit
prompt pure
EOF
chsh -s $(which zsh)
```

# Change WM to i3

```sh
sudo dnf install -y lightdm lightdm-gtk-greeter-settings
sudo systemctl disable gdm
sudo systemctl enable lightdm

sudo dnf install -y blueman compton i3 i3lock ImageMagick network-manager-applet scrot @xfce-desktop-environment
curl -s https://api.github.com/repos/Ulauncher/Ulauncher/releases/latest \
	| jq -r '.assets[].browser_download_url' \
	| grep rpm \
	| tail -n1 \
	| wget -O /tmp/ulauncher.rpm -i -
sudo dnf install /tmp/ulauncher.rpm
```

# Install Docker

```sh
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io
sudo usermod -a -G docker $USER
```

# Install Haskell

```sh
sudo dnf copr enable -y petersen/stack
sudo dnf install -y haskell-platform stack
```

# Change angular style

If you are using the latest angular-cli, which is in my case 6.2.6 (I think starting from 6.0.7, this would apply), use the following command to change the configuration.

```sh
ng config schematics.@schematics/angular:component.styleext scss
```

If you are using mac or linux, open the src folder from the terminal, and type the following command to rename the css files

```sh
find . -name "*.css" -exec bash -c 'mv "$1" "${1%.css}".scss' - '{}' \;
```
