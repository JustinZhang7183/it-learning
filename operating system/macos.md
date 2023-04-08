# Homebrew
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
brew install xxx
brew services info xxx
brew services start xxx
brew services stop xxx
brew services list
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
