# Homebrew
### Proxy of Homebrew
- Set http_proxy and https_proxy to environment variables
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