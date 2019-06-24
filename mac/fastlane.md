

# 使用 [RVM](https://rvm.io/)

1. install rvm 

    ```
    curl -sSL https://get.rvm.io | bash -s stable
    rvm list known
    rvm install 2.6.3
    rvm use 2.6.3
    ```
2. install fastlane

    ```
    gem install fastlane -NV
    ```

3. install cocoapods

    ```
    gem install cocoapods
    pod setup
    ```


# 使用 rbenv

1. 安装[rbenv](https://github.com/rbenv/rbenv#homebrew-on-macos)


2. mac自带的ruby版本太低了， 使用`rbenv` 安装更高版本的ruby
```
# list all available versions:
$ rbenv install -l

# install a Ruby version:
$ rbenv install 2.6.0

# set ruby version for global/local/shell
$ rbenv global 2.6.0

```



3. 安装fastlane 

```
# Using RubyGems
gem install fastlane -NV
```