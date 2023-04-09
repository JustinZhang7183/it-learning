# Homebrew
- installing brew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- uninstall
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```
### Proxy of Homebrew
- Set http_proxy and https_proxy to environment variables
```
export http_proxy=http://ip:port
export https_proxy=http://ip:port
export socks_proxy=socks://ip:port
```
### Install xxx of Hombrew
- Use command: **brew install xxx** will create some config in following directories
```
# the other files will be changed by the file change in the directory which caontain Cellar
/usr/local/Cellar/xxx/{version}/homebrew.mxcl.xxx.plist
/usr/local/Cellar/xxx/{version}/homebrew.xxx.service
/usr/local/opt/xxx/homebrew.mxcl.xxx.plist
/usr/local/opt/xxx/homebrew.xxx.service
~/Library/LaunchAgents/homebrew.mxcl.xxx.plist
```
### The Principle of **brew services start xxx** in Homebrew
- Create a file like homebrew.mxcl.xxx.plist
- In this file, it designate the executable file and config file
- Run the executable file through these information
### Command Lines of Homebrew
```
# basis
brew -v
brew list
brew list --versions | grep xxx
brew doctor
brew info xxx
brew deps --installed --tree
brew install xxx
brew uninstall xxx
brew search xxx

# services relevent
brew services info xxx
brew services start xxx
brew services stop xxx
brew services list

# update brew
brew update

# check what need to update
brew outdated

# update all package
brew upgrade

# update one specific package
brew upgrade xxx

# check what can clean up
brew cleanup -n

# clean up all
brew cleanup

# clean one specific package
brew cleanup xxx

# lock the package which you don't want to upgrade
brew pin xxx
# unlock
brew unpin xxx

# clean invalid link
brew prune

# put symlink to homebrew
brew link xxx
```
### turn off the automatic update
- add environment variables
```
export HOMEBREW_NO_AUTO_UPDATE=true
```
# Environment variables
## Two default shells
- The default shell can be changed with **chsh** or in System Preferences ➞ Users and Groups
- to view the current default shell
```
dscl . -read ~/ UserShell
```
- to view the current shell
```
echo $SHELL
```
- enable environment variables
```
source /xxx
```
### zsh
- starting with macOS Catalina
- Order of reading startup files:
```
.zshenv -> .zprofile -> .zshrc -> .zlogin
```
- .zshrc is still effective after restart.
### bash
- in macOS Mojave and earlier
- Order of reading startup files:
```
/etc/profile -> .bash_profile -> .bash_login -> .profile
```
## About Java
### installing jdk
- using homebrew
```
brew install oracle-jdk / --cask oracle-jdk
brew install openjdk@11/17
```
- install directory
    1. /usr/local/opt/
    2. /Library/Java/JavaVirtualMachines/
    3. through "/usr/libexec/java_home -V" check

### environment configuration
- multiple versions(.zshrc)
```
#JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-20.jdk/Contents/Home
JAVA_HOME=/usr/local/opt/openjdk@17
#JAVA_HOME=/usr/local/opt/openjdk@11
PATH=${JAVA_HOME}/bin:${PATH}
export PATH
```
## About maven
### update jdk dependence
1. /etc/mavenrc
2. .mavenrc
```
JAVA_HOME=/usr/local/opt/openjdk@17
```
# Finder
- show hidden files(command + shift + .)


