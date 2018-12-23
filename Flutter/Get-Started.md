# Install
1. Download Flutter SDK

	```bash
	git clone -b master https://github.com/flutter/flutter.git
	./flutter/bin/flutter --version
	```
2. Add the `flutter` to PATH

	```bash
	export PATH=`pwd`/flutter/bin:$PATH
	```
3. Run flutter doctor

	```bash
	flutter doctor
	```
	跟着错误提示,把缺少的软件安装配置好.

## iOS setup
1. install Xcode 9.0+
2. Configure Xcode command-line-tools 

	```bash
	sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
	```
3. Make Xcode license agreement is signed
	
	```bash
	sudo xcodebuild -license
	```
4. Setup iOS simulator

	```bash
	open -a Simulator
	```
5. Deploy to ios Devices
	
	1. 安装[homebrew](http://brew.sh/)
	2. 安装工具
	
		```bash
		brew update
		brew install --HEAD libimobiledevice
		brew install ideviceinstaller ios-deploy cocoapods
		pod setup
		```
	3. 如果上面的命令执行出错, 执行 `brew doctor`修复对应的问题.
	
## Android setup

1. install Android Studio
2. install the latest Android SDK, Android SDK Platform-Tools, and Android SDK Build-Tools
3. setup Android device
	1. 手机上打开usb调试
	2. 用数据线把手机连上电脑,如果提示是否信任,信任
	3. 终端执行 `flutter devices` 看看设备是否 flutter 识别
4. setup Android emulator


## Visual Studio Code (VS Code) setup

1. install VS Code
2. install `Flutter` plugin   
3. Validate your setup with the Flutter Doctor
	1. Invoke View>Command Palette…
	2. Type ‘doctor’, and select the ‘Flutter: Run Flutter Doctor’ action
	3. Review the output in the ‘OUTPUT’ pane for any issues
	

