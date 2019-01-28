# Clean Install macOS Mojave

**Important**  
If you like to follow these instructions keep in mind that you need to use your own information like username and folder locations.   
I use Dropbox for backup/restore and also my symlinked files (like gitconfig and some dotiles) are located on Dropbox.    

---

## Create Bootable USB Disk

- [Download macOS Mojave](https://itunes.apple.com/tr/app/macos-mojave/id1398502828?mt=12 )
- Format USB (min 8GB):
    - Open **Disk Utility**
    - Click **Erase**
    - Select **OS X Extended (Journaled)**
    - Rename USB to **Untitled**
- To Create a OS X bootable USB disk enter this command to your terminal:

```bash
sudo /Applications/Install\ macOS\ Mojave.app/Contents/Resources/createinstallmedia --volume /Volumes/Untitled --applicationpath /Applications/Install\ macOS\ Mojave.app
```

### Backup
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


### Restore your files from Dropbox

- Download [Dropbox](https://www.dropbox.com/downloading) and install it. 
- Login to your Acoount. 
- The application will create a Dropbox folder and begin downloading files from your account. Stop the download immediately by clicking the Dropbox icon in your menu bar, clicking the gear icon, and selecting ***Pause Syncing*** from the menu.
- Copy the files from your portable drive into the Dropbox folder. The default location of the folder on your Mac is /Users/yourUserName/Dropbox (such as /Users/Robert/Dropbox).
- Resume syncing by clicking the Dropbox icon in your menu bar, clicking the gear icon, and selecting ***Resume Syncing*** from the menu.

  *[Source](https://www.dropbox.com/en/help/1941)*

### Install Brew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install Brew Packages

```bash
brew install bash bash-completion coreutils curl fabric gettext gdrive git git-extras grep guetzli jpeg libffi libpng libyaml mas mysql node nvm openssl openssl@1.1 pcre peco readline ssh-copy-id stormssh stormssh-completion tree wget xz youtube-dl
# Restart Terminal
```

### Restore .ssh files 

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


### Install [Dotfiles](https://github.com/vigo/dotfiles-light)


```bash
git clone https://github.com/tarikkavaz/dotfiles-light.git $HOME/Dotfiles
bash $HOME/Dotfiles/scripts/install.bash
# Restart Terminal
```

Fork the repo to your account. use your github username as `USER_NAME`

```bash
cd ~/Dotfiles/
git remote rename origin upstream 
git remote add origin git@github.com:USER_NAME/dotfiles-light.git
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
ln -s ~/Dropbox/Dotfiles/private/my-ps1 ~/Dotfiles/private/my-ps1
```

Symlink git aliases, settings

```bash
ln -s ~/Dropbox/Dotfiles/configs/gitconfig ~/.gitconfig
ln -s ~/Dropbox/Dotfiles/configs/gitignore_global ~/.gitignore_global
```


### Install [Ruby](https://github.com/rbenv/rbenv)

```bash
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
# Restart Terminal

git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
git clone https://github.com/rbenv/rbenv-default-gems.git $(rbenv root)/plugins/rbenv-default-gems
```

Create default-gems file

```bash
nano $(rbenv root)/default-gems
echo "bundler" >> $(rbenv root)/default-gems
echo "pry" >> $(rbenv root)/default-gems
```

Install Ruby 2.4.0 

```bash
RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl) --with-readline-dir=$(brew --prefix readline) --with-libyaml-dir=$(brew --prefix libyaml)" rbenv install 2.4.0
rbenv global 2.4.0 
# Restart Terminal
```


### Install [Python](https://github.com/pyenv/pyenv)
```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv 
# Restart Terminal

git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
git clone https://github.com/pyenv/pyenv-virtualenvwrapper.git $(pyenv root)/plugins/pyenv-virtualenvwrapper
CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" CONFIGURE_OPTS="--with-readline-dir=$(brew --prefix readline)" 
pyenv install 3.6.0
pyenv global 3.6.0
# Restart Terminal
```

For some of my older projects I need `Python 2.7`.   
If you need a different Python version; do this on that project folder:

```bash
CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" CONFIGURE_OPTS="--with-readline-dir=$(brew --prefix readline) --with-openssl-dir=$(brew --prefix)" PYTHON_CONFIGURE_OPTS=--enable-unicode=ucs2 pyenv install 2.7.12
pyenv local 2.7.12
# Restart Terminal
```

### Install [nvm](https://github.com/creationix/nvm)

Install script
```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

Install required version
```bash
nvm install 6.3
nvm alias default 6.3
```

### Install your npm modules

```bash
npm install -g bower
npm install -g gulp-cli
npm install -g electron
npm install -g @vue/cli
```

---
## Install Apps

Change the default Gatekeeper behavior to run apps downloaded from anywhere.

```bash
sudo spctl --master-disable
```

### From Brew Cask

#### Install Homebrew Cask
```bash
brew tap caskroom/cask
```
The code below Should be in your `.bash_profile` or Private Dotfile.  
Check if you have it already in your symlinked files like `env`

```bash
export HOMEBREW_CASK_OPTS="--appdir=/Applications --fontdir=/Library/Fonts"
```

#### Install [Cask Updater](https://github.com/buo/homebrew-cask-upgrade)
```
brew tap buo/cask-upgrade
```

#### Install Cask Apps

[Search available Cask Apps](https://caskroom.github.io/search) 

- [1Password](https://1password.com/)
- [Appcleaner](https://freemacsoft.net/appcleaner/)
- [BackgroundMusic](https://github.com/kyleneideck/BackgroundMusic)
- [Bartender 2](https://www.macbartender.com/) *
- [Calibre](https://calibre-ebook.com/download)
- [Chrome](https://www.google.com/chrome/browser/desktop/)
- [CodeKit](https://incident57.com/codekit/) *
- [Commander One](http://mac.eltima.com/file-manager.html) *
- [Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Goofy](http://www.goofyapp.com/)
- [iTerm2](https://www.iterm2.com/downloads.html)
- [Liya](https://cutedgesystems.com/software/liya/)
- [Padbury Clock](http://padbury.me/clock/)
- [Poedit](https://poedit.net/)
- [qlImageSize](https://github.com/Nyx0uf/qlImageSize)
- [QuickLook Markdown](https://github.com/toland/qlmarkdown/)
- [Screens Connect](http://edovia.com/screensconnect/download/)
- [Skype](http://www.skype.com/en/download-skype/skype-for-mac/downloading/)
- [Spotify](https://www.spotify.com/us/download/)
- [Sequel-Pro](https://www.sequelpro.com/)
- [Sip](http://sipapp.io/)
- [Slack](https://slack.com/)
- [TextMate](https://macromates.com/download) *
- [TeamViewer](https://www.teamviewer.com/en/download/mac/)
- [Telegram](https://telegram.org/)
- [TripMode](https://www.tripmode.ch) *
- [Typora](https://typora.io/)
- [The Unarchiver](http://unarchiver.c3.cx/unarchiver)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Visual Studio Code](https://code.visualstudio.com/)
- [WALTR](https://softorino.com/waltr) *
- [WhatsApp Messenger](https://www.whatsapp.com/download/)
- [ZenMate](https://secure.zenmate.com/)

Or install it with a single line:

```bash
brew cask install 1password 1password-cli atom appcleaner background-music bartender calibre google-chrome codekit commander-one firefox goofy iterm2 liya padbury-clock Poedit qlImageSize qlmarkdown screens-connect skype spotify sequel-pro sip slack textmate teamviewer telegram tripmode typora the-unarchiver vlc virtualbox visual-studio-code waltr whatsapp zenmate-vpn
```



### From App Store

- [Airmail 3](http://airmailapp.com/)
- [Feedly](http://feedly.com/)
- [Keynote](http://www.apple.com/mac/keynote/)
- [Moom](https://manytricks.com/moom/)
- [Numbers](http://www.apple.com/mac/numbers/)
- [Pages](http://www.apple.com/mac/pages/)
- [Parcel](https://web.parcelapp.net/)
- [Pixelmator Pro](http://www.pixelmator.com/pro/)
- [TweetDeck](https://tweetdeck.twitter.com/)
- [Unclutter](http://unclutterapp.com/)
- [Wunderlist](https://www.wunderlist.com/)

#### Or use [mas](https://github.com/mas-cli/mas)

```bash
mas install 918858936 577085396 441258766 639968404 409183694 485812721 409201541 865500966 747648890 409203825 1289583905 410628904
```

#### Homebrew Bundle
Create your own [Homebrew Bundle](https://github.com/Homebrew/homebrew-bundle) for installing all your apps and dependencies with one single file 

### From Web
- [Adobe Creative Cloud](http://www.adobe.com/creativecloud/desktop-app.html) *
- [Screens](http://edovia.com/screens/) *
- [Transmit](https://panic.com/transmit/) *
- [TrCharChange](https://github.com/tarikkavaz/TrCharChange)
- [Virtual Machines](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/mac/)

### Themes
- [Solarized color palette](http://ethanschoonover.com/solarized)
- [iTerm themes](http://iterm2colorschemes.com/)

`*` = Paid App 
