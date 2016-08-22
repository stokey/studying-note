##IOS通过cocoaPods管理第三方类库

##1. 安装cocoaPods［ruby >= 2.2］
###1.1 更新Ruby
-------------------------------
1. curl -L https://get.rvm.io | bash -s stable --ruby
2. rvm list known:查看可选版本 [rvm command not found: source ~/.rvm/scripts/rvm --> type rvm | head -n 1]
3. rvm install ruby-2.3.0:安装特定版本
4. rvm use ruby-2.3.0:使用特定Ruby版本

###1.2 安装cocoaPods
-------------------------------
1. 查询Ruby的源: gem source -l
2. 删除Ruby的国外源: gem source --remove https://rubygems.org/
3. 替换Ruby源为淘宝源: gem source -a https://ruby.taobao.org/
4. 执行安装命令: sudo gem install cocoapods

##2. 创建Podfile文件
###2.1 于项目根目录创建Podfile文件：vim Podfile（或者执行pod init命令自动初始化）
###2.2 添加需要的第三方类库：
```
target 'SwiftWeather' do
  pod 'AFNetworking', '~> 3.0'
  pod 'FBSDKCoreKit', '~> 4.9'
end
```
##3. 安装第三方类库：pod install 
##4. 在项目根目录打开.xcworkspace文件进入项目

#### AFNetworking安装失败，提示无法找到：
1. pod repo remove master
2. pod repo add master https://github.com/CocoaPods/Specs.git
3. pod setup
4. pod install

#### pod install 无法成功解决方法：
1. 进入master文件夹：cd ~/.cocoapods/repos/master
2. clone项目：git clone https://github.com/CocoaPods/Specs.git 
3. pod setup
4. pod install
