# Clean Install macOS

**Important**
If you like to follow these instructions keep in mind that you need to use your own information like username and folder locations.
I use Dropbox for backup/restore and also my symlinked files (like gitconfig and some dotiles) are located on Dropbox.

---

## Create Bootable USB Disk

- [Download macOS Catalina 10.15](https://apps.apple.com/us/app/macos-catalina/id1466841314?mt=12)
- Format USB (min 8GB):
    - Open **Disk Utility**
    - Click **Erase**
    - Select **OS X Extended (Journaled)**
    - Rename USB to **Untitled**
- To Create a OS X bootable USB disk enter this command to your terminal:

```bash
# Catalina
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Untitled
```

## Backup

Backup Your files to Dropbox.

*don't forget your important files like ssh keys:*

```bash
cp -R ~/.ssh ~/Dropbox/
```

---

## Start Clean Install

***YOU ARE GOING TO DELETE EVERYTHING ON YOUR MAC IN THE NEXT STEP !!!***

Write down the next 4 steps. You won't be able to to open your Mac until OS is installed.

1. Restart & hold down the Option **⌥ alt** key.
2. Choose **Install OS X Mojave**.
3. Select **Disk Utility** from the menu and **erase** you main HDD.
4. Go back to the main menu; select **Install OS X** and choose your HDD when prompted.

Continue installation with your credentials.

## Restore your files from Dropbox

- Download [Dropbox](https://www.dropbox.com/downloading) and install it.
- Login to your Acoount.
- The application will create a Dropbox folder and begin downloading files from your account. Stop the download immediately by clicking the Dropbox icon in your menu bar, clicking the gear icon, and selecting ***Pause Syncing*** from the menu.
- Copy the files from your portable drive into the Dropbox folder. The default location of the folder on your Mac is /Users/yourUserName/Dropbox (such as /Users/Robert/Dropbox).
- Resume syncing by clicking the Dropbox icon in your menu bar, clicking the gear icon, and selecting ***Resume Syncing*** from the menu.

    *[Source](https://www.dropbox.com/en/help/1941)*

## Install Xcode From [Appstore](https://apps.apple.com/tr/app/xcode/id497799835?mt=12)

## Install Command Line Tools

```bash
xcode-select --install
```

## Install Brew

```bash
/usr/bin/ruby -e "$(curl -fsSL <https://raw.githubusercontent.com/Homebrew/install/master/install>)"
```

Install Brew Packages

```bash
brew install bash bash-completion gettext libffi openssl@1.1 xz cask coreutils curl fabric icu4c jpeg ffmpeg gdrive git git-extras grep guetzli highlight hr imagemagick libyaml mas mono mysql nvm pkg-config postgresql rsync ssh-copy-id stormssh stormssh-completion tree unar wget youtube-dl
# Restart Terminal

# brew install autoconf automake bash bash-completion coreutils curl openssl@1.1 readline xz fabric gettext git git-extras grep guetzli highlight hr htop icu4c jpeg libffi libksba libtool libyaml mas mysql node@12 ntfs-3g nvm openssh peco postgresql postgresql@9.4 rclone rsync ruby ssh-copy-id stormssh stormssh-completion tmate tree wget yarn youtube-dl zlib
```

## Restore .ssh files

```bash
cd
mkdir ~/.ssh
cd Dropbox/.ssh 
cp id_rsa* ~/.ssh/
ln -s ~/Dropbox/Dotfiles/configs/ssh_config ~/.ssh/config
```

Set Permissions

```bash
chmod 400 ~/.ssh/id_rsa
```

## Install [Dotfiles](https://github.com/vigo/dotfiles-light)

```bash
git clone https://github.com/tarikkavaz/dotfiles-light.git $HOME/Dotfiles
bash $HOME/Dotfiles/scripts/install.bash
# Restart Terminal
```

Fork the repo to your account. use your github username as `USER_NAME`

```bash
cd ~/Dotfiles/
git remote rename origin upstream 
git remote add origin git@github.com:tarikkavaz/dotfiles-light.git
git push -u origin master
```

Dotfiles Private files

```bash
cd ~/Dotfiles/
mkdir private
```

Symlink private Dotfiles.
I have added sample `env` and `my-ps1` files to the repo. You can create your own `alias` and `function` files if needed.

```bash
ln -s ~/Dropbox/Dotfiles/private/alias ~/Dotfiles/private/alias
ln -s ~/Dropbox/Dotfiles/private/env ~/Dotfiles/private/env
ln -s ~/Dropbox/Dotfiles/private/functions ~/Dotfiles/private/functions
ln -s ~/Dropbox/Dotfiles/private/my-colors ~/Dotfiles/private/my-colors
ln -s ~/Dropbox/Dotfiles/private/my-ps1 ~/Dotfiles/private/my-ps1
```

Symlink git aliases, settings

```bash
ln -s ~/Dropbox/Dotfiles/configs/gitconfig ~/.gitconfig
ln -s ~/Dropbox/Dotfiles/configs/gitignore_global ~/.gitignore_global
```

## Install [Ruby](https://github.com/rbenv/rbenv)

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
# Restart Terminal

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
git clone https://github.com/rbenv/rbenv-vars.git ~/.rbenv/plugins/rbenv-vars
git clone https://github.com/rbenv/rbenv-default-gems.git ~/.rbenv/plugins/rbenv-default-gems
```

Create default-gems file

```bash
nano $(rbenv root)/default-gems
echo "bundler" >> $(rbenv root)/default-gems
echo "pry" >> $(rbenv root)/default-gems
```

Install Ruby 2.6.2

```bash
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl) --with-readline-dir=$(brew --prefix readline) --with-libyaml-dir=$(brew --prefix libyaml)" rbenv install 2.6.2
rbenv global 2.6.2 
# Restart Terminal
```

## Install [Python](https://github.com/pyenv/pyenv)

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv 
# Restart Terminal
```

If you need a different Python version; do this on that project folder:

```bash
CONFIGURE_OPTS="--with-readline-dir=$(brew --prefix readline)" PYTHON_CONFIGURE_OPTS=--enable-unicode=ucs2 pyenv install 3.7.1
pyenv global 3.7.1
# Restart Terminal
```

Install Plug-in's

```bash
git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
git clone https://github.com/yyuu/pyenv-virtualenvwrapper.git ~/.pyenv/plugins/pyenv-virtualenvwrapper
git clone https://github.com/yyuu/pyenv-pip-rehash.git ~/.pyenv/plugins/pyenv-pip-rehash
```

## Install [nvm](https://github.com/creationix/nvm)

Install script

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

Install required version

```bash
nvm install 6.3
nvm alias default 6.3
```

## Install your npm modules

```bash
npm install -g bower
npm install -g gulp-cli
npm install -g electron
npm install -g @vue/cli
```

---

## Install [Postgres Version Manager](https://github.com/guedes/pgvm)

```bash
curl -s -L https://raw.github.com/guedes/pgvm/master/bin/pgvm-self-install | bash
```

---

## Install Apps

Change the default Gatekeeper behavior to run apps downloaded from anywhere.

```bash
sudo spctl --master-disable
```

## From Brew Cask

## Install Homebrew Cask

The code below Should be in your `.bash_profile` or Private Dotfile.
Check if you have it already in your symlinked files like `env`

```bash
export HOMEBREW_CASK_OPTS="--appdir=/Applications --fontdir=/Library/Fonts"
```

## Install [Cask Updater](https://github.com/buo/homebrew-cask-upgrade)

```bash
brew tap buo/cask-upgrade
```

## Install Cask Apps

[Search available Cask Apps](https://caskroom.github.io/search)

```bash
brew cask install alfred appcleaner bartender betterzip brooklyn clipy codekit copyclip db-browser-for-sqlite docker figma fliqlo flotato google-chrome iterm2 liya moom notion padbury-clock qlcolorcode qlimagesize qlmarkdown qlprettypatch qlstephen quicklook-csv quicklook-json screens-connect sequel-pro skype slack spotify sublime-text suspicious-package teamviewer telegram-desktop the-unarchiver tripmode typora visual-studio-code vlc webpquicklook whatsapp xquartz
```

## From App Store

- AdBlock
- Airmail
- Keynote
- Microsoft Excel
- Microsoft PowerPoint
- Microsoft Word
- Numbers
- Pages
- Pixelmator Pro
- Surfshark
- TweetDeck

## Or use [mas](https://github.com/mas-cli/mas)

```bash
mas install 1402042596 918858936 409183694 462058435 462062816 462054704 409203825 409201541 1289583905 1437809329 485812721
```

## Homebrew Bundle

Create your own [Homebrew Bundle](https://github.com/Homebrew/homebrew-bundle) for installing all your apps and dependencies with one single file

## From Web

- [Adobe Creative Cloud](http://www.adobe.com/creativecloud/desktop-app.html) *
- [Screens](http://edovia.com/screens/) *
- [Transmit](https://panic.com/transmit/) *
- [TrCharChange](https://github.com/tarikkavaz/TrCharChange)
- [Virtual Machines](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/mac/)

## Themes

- [Solarized color palette](http://ethanschoonover.com/solarized)
- [iTerm themes](http://iterm2colorschemes.com/)

`*` = Paid App
