主要借助react native嵌入h5页面打出IOS和Android包。h5也可直接用于微信、手机浏览器等

# ReactNative Mac环境搭建

## 安装 homebrew
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

## 安装 node
`brew install node`

## 安装 Yarn和React Native的命令行工具
`npm install -g yarn react-native-cli`

## 配置Android环境
### 安装Android studio
https://developer.android.com/studio/

Android Studio包含了运行和测试React Native应用所需的Android SDK和模拟器

安装完新建一个hello Word跑到手机，坏境就好了

### 配置ANDROID_HOME
```bash
vim .bash_profile
export ANDROID_HOME=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools
source ~/.bash_profile
```

## 配置IOS环境
### 在App Store安装xcode

## 测试安装

```bash
react-native init ReactNativeDemo
cd ReactNativeDemo
# androud
## 确保有可用的设备adb devices
react-native run-android

# ios
react-native run-ios
```
即可看到在模拟器或者真机上面运行


# 基于H5模式开发
## 使用WebView显示h5页面
修改App.js
```javascript
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, {Component} from 'react';
import {
    Platform,
    StyleSheet,
    Text,
    View,
    WebView,
    Dimensions
} from 'react-native';

const deviceHeight = Dimensions
    .get("window")
    .height;
const deviceWidth = Dimensions
    .get("window")
    .width;

var MARGIN_TOP = Platform.OS === "ios"
    ? 20
    : 0;

// 请求动态访问的接口地址
var getUrl = "http://xxx.com";


export default class App extends Component {
    constructor(props) {
        super(props);
        this.state = {url: ''};
        this.getRequest(getUrl);
    }

    getRequest(url) {
        /*网络请求的配置*/
        var opts = {
            method: "GET"
        };

        fetch(url, opts)
            .then((response) => {
                return response.text();
            })
            .then((responseText) => {
                this.setState(previous => {
                    return {url: responseText};
                });
            })
            .catch((error) => {
                alert(error);
            })
    }

    render() {

        var url = this.state.url;
        return (
            <View style={styles.container}>
                <WebView
                    style={styles.content}
                    source={{
                        uri: url
                    }}/>
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF'
    },
    content: {
        width: deviceWidth,
        height: deviceHeight,
        marginTop: MARGIN_TOP
    }
});
```
## 参考
- [搭建开发环境](https://reactnative.cn/docs/0.51/getting-started.html#content)

## 参考项目
- [ReactNativeDemo](https://github.com/Mark-PHP/ReactNativeDemo)