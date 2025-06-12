# ys-webrtc-sdk-ui

[中文](./README_zh-CN.md) | [English](./README.md) 

本项目抽离了 PBX web 通话组件 ui 及通话逻辑，使用集成好的ui组件无需额外编码。

通话核心逻辑基于 [ys-webrtc-sdk-core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core/blob/main/README_zh-CN.md)集成。

## Install

```bash
npm install ys-webrtc-sdk-ui --save
```

## Getting started

初始化sdk，渲染UI组件。
```js
import { init } from 'ys-webrtc-sdk-ui';
import 'ys-webrtc-sdk-ui/lib/ys-webrtc-sdk-ui.css';
const container = document.getElementById('container');
// 初始化
init(container, {
    username: '1000',
    secret: 'sdkshajgllliiaggskjhf',
    pbxURL: 'https://192.168.1.1:8088'
}).then(data => {
// 可以在这里获取暴露出的实例，处理更多业务
const { phone, pbx, destroy, on } = data;
    // ...
}).catch(err=>{
    console.log(err)
})
```

## 标签方式引用

```html
<!-- 加载样式 -->
<link rel="stylesheet" href="ys-webrtc-sdk-ui.css">

<div id="test"></div>
<!-- 加载UI集成 SDK -->
<script src="./ys-webrtc-sdk-ui.js"></script>
<script>
    const test = document.getElementById('test');
    // 加载成功后通过YSWebRTCUI对象进行初始化
    window.YSWebRTCUI.init(test, {
        username: '1000',
        secret: 'sdkshajgllliiaggskjhf',
        pbxURL: 'https://192.168.1.1:8088'
    })
        .then(data => {
            const { phone, pbx } = data;
        })
        .catch(error => {
            console.log(error);
        });
</script>
```

## API

通过 init 方法进行初始化，初始化成功后会的到两个实例化后的 Operator 对象：phone 和 pbx。
- phone 对象包含通话相关的方法和属性，如：call、hangup 等方法。
- pbx 对象包含 PBX 的相关方法和属性，如：查询 cdr。
- 更多方法和属性请查看 [ys-webrtc-sdk-core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core#readme)。

init 函数需要两个参数 container和rtcOption。
- container 类型是 HTMLElement，用于渲染 UI 组件。
- rtcOption 为初始化选项。

### rtcOption 参数

| 属性名 | 类型 | 是否必填 | 说明 |
| --- | --- | --- | --- |
| username | string | 必填 | 分机号。 |
| secret | string | 必填 | 登录签名，通过 OPEN API 获取，[获取签名流程](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core/blob/main/docs/zh-CN/CreateSign.md)。 |
| pbxURL | URL \| string | 必填 | PBX 的访问地址可以为 URL 对象，地址要求包含协议以及端口如：https://192.168.1.1:8088或 https://xx.xxx.com。 |
| enableLog | boolean | 可选 | 是否开启日志输出以及错误日志上报至 PBX，默认开启。 |
| reRegistryPhoneTimes | number | 可选 | 尝试重新连接 sip 服务次数，默认无限制。 |
| deviceIds | { cameraId?: string; microphoneId?: string; speakerId:string; volume:number } | 可选 | 指定音视频输入设备 id，包含摄像头 id 、麦克风 id、扬声器 id；volume为通话、来电、拨号盘音量，音量范围0-1直接的浮点数，默认为0.6。 |
| incomingOption | { style?: React.CSSProperties; class?: string;  } | 可选 | 调整Incoming组件样式 |
| dialPanelOption | {  style?: React.CSSProperties; class?: string; hideVideoBtn?: boolean } | 可选 | 调整DialPanel组件样式。`hideVideoBtn`用于控制视频按钮的显示与隐藏，默认false。 |
| sessionOption | [SessionOption](#session-option) | 可选 | 调整通话窗口组件位置和尺寸 |
| hiddenIncomingComponent | boolean | 可选 | 隐藏来电弹屏组件 |
| hiddenDialPanelComponent | boolean | 可选 | 隐藏拨号盘组件 |
| disableCallWaiting | boolean | 可选 | 是否禁用callWaiting，为true时不使用pbx callWaiting值且只处理单通电话。|
| intl | { local: string; messages: Record\<string, string\>} | 可选 | 多语言选项，可根据自身需要替换文案，message的键值可见下方详细描述。|

### Types

#### SessionOption
```ts
type SessionOption = {
    sessionSetting?: {
        width?: number;
        height?: number;
        miniWidth?: number;
        miniHeight?: number;
        x?: number;
        y?: number;
    };
    style?: React.CSSProperties;
    class?: string;
}
```

### Intl

- `local`：该属性用于标识当前正在使用的语言，例如：en、zh-CN、en-US。不可以使用下划线形式，例如：zh_cn、en_us。
- `messages`：文案配置，键值对形式，键值可见下方详细描述。其中\{0\}表示占位符，会被替换为具体的值。
	
	```json
  {
      "common.cancel": "Cancel", // 取消操作提示文案
      "common.confirm": "Confirm", // 确认操作提示文案
      "dial_panel.input.placeholder": "Please input number", // "输入号码" 提示文案
      "dial_panel.tip.connect_failed": "Failed to connect to server, you cannot initiate or answer a call. Trying to reconnect to the server.", //服务器连接失败提示文案
      "incoming.btn.hang_up": "Hang up", // "挂断通话" 按钮文案
      "incoming.btn.audio": "Audio", // 选择 "语音通话" 按钮文案
      "session.calling": "Calling...", // "呼叫中" 文案
      "session.ringing": "Ringing...", // "对方振铃中" 文案
      "session.talking": "Talking...", // "通话中" 文案
      "session.connecting": "Connecting...", // "连接中" 文案
      "session.hang_up": "End Call", // "结束通话" 文案
      "session.new_call": "New call", // "新呼叫" 文案
      "session.record": "Record", // "录音" 文案
      "session.mute": "Mute", // "静音通话" 文案
      "session.hold": "Hold", // "保持通话" 文案
      "session.resume": "Resume", // "恢复通话" 文案
      "session.dialpad": "Dialpad", // "拨号键盘" 文案
      "session.transfer": "Transfer", // "转接通话" 文案
	    "session.attended_transfer": "Attended Transfer", // "咨询转接" 文案
      "session.blind_transfer": "Blind Transfer", // "盲转接" 文案
      "session.error.client_error": "Client Error: {0}", // "客户端异常" 文案。"{0}" 为占位符
      "session.tip.recording": "Recording the Audio...", // "通话录音中" 文案
      "session.tip.pause": "The recording is paused.", // "通话录音已暂停" 文案
      "error.code_200": "Unknown Error.", // "未知错误" 提示文案
      "error.code_202": "No available communication device found.(no permissions)", // "无权获取通话设备" 提示文案
      "error.code_205": "Call failed. Cannot process more new calls now.", // "通话失败 (已达最大通话数)" 提示文案
      "error.code_206": "No available communication device found.", // "无可用的通话设备" 提示文案
      "error.code_207": "Attended Transfer Failed.", // "咨询转接失败" 提示文案 
      "error.code_208": "Call failed. Called too many times.", // "通话失败 (已达呼出次数上限)" 提示文案
      "error.code_209": "Call failed. Invalid Number.", // "通话失败 (无效号码)" 提示文案
      "error.code_210": "Operation failed with pending calls.", // "有待处理来电，操作失败" 提示文案
      "error.code_211": "Answer failed", // "接听来电失败" 提示文案
      "error.code_290": "No available microphone found.", // "无可用的麦克风" 提示文案
	    "error.code_291": "No available camera found." // "无可用的摄像头" 提示文案
	}
	```
