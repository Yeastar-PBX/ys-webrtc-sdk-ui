# ys-webrtc-sdk-ui

[中文](./README_zh-CN.md) | [English](./README.md) 

Yeastar WebRTC SDK separates the PBX web calling component UI and call logic. 

**Yeastar WebRTC SDK UI** is pre-integrated UI components that require no additional coding, while the core logic for the calling functionality is based on the integration of  [Yeastar WebRTC SDK Core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core#readme).

## Installation

```bash
npm install ys-webrtc-sdk-ui --save
```

## Getting started

Initialize the Yeastar WebRTC SDK UI and render the UI components,
```js
import { init } from 'ys-webrtc-sdk-ui';
import 'ys-webrtc-sdk-ui/lib/ys-webrtc-sdk-ui.css';
const container = document.getElementById('container');
// Initialization
init(container, {
    username: '1000',
    secret: 'sdkshajgllliiaggskjhf',
    pbxURL: 'https://192.168.1.1:8088'
}).then(data => {
// Obtain the exposed instances for additional business needs
const { phone, pbx, destroy, on } = data;
    // ...
}).catch(err=>{
    console.log(err)
})
```

## Use HTML tags to reference the Yeastar WebRTC SDK UI

```html
<!-- Loading styles -->
<link rel="stylesheet" href="ys-webrtc-sdk-ui.css">

<div id="test"></div>
<!-- Loading Yeastar WebRTC SDK UI-->
<script src="./ys-webrtc-sdk-ui.js"></script>
<script>
    const test = document.getElementById('test');
    // Initialize Yeastar WebRTC SDK UI with the 'YSWebRTCUI' object. 
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

Initialize Yeastar WebRTC SDK UI using the `init` method. Upon successful initialization, two instantiated Operator objects are returned: 

+ **phone**: The object contains methods and attributes related to the call handling, such as 'call', 'hangup', and others.
+ **pbx**: The object contains methods and attributes related to the PBX operations, such as querying  CDR.

	**Note**: For more information, see [ys-webrtc-sdk-core](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core#readme).

The `init` function requires two parameters:

+ **container**: Applicable for rendering UI components, and the data type is 'HTMLElement'.
+ **rtcOption**: Initialization options.

### rtcOption Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| username | string | Yes | Extension number. |
| secret | string | Yes | Login signature, which can be obtained using OPEN API. For more information, see [Obtain a Server-side Signature](https://github.com/Yeastar-PBX/ys-webrtc-sdk-core/blob/main/docs/CreateSign.md). |
| pbxURL | URL \| string | Yes | The URL for accessing your PBX system, including the transfer protocol and the port number.<br />For example, https://192.168.1.1:8088 or https://xx.xxx.com. |
| enableLog | boolean | No | Whether to enable log output and report error logs to PBX. This feature is enabled by default. |
| reRegistryPhoneTimes | number | No | Define the number of attempts to reconnect to the SIP service. By default, it is unlimited. |
| deviceIds | { cameraId?: string; microphoneId?: string; speakerId:string; volume:number } | No | Specify the IDs of the audio and video input devices, including the camera ID, microphone ID, and speaker ID.<br />Volume refers to the volume level for calls, incoming call ringtones, and keypad tones, ranging from 0 to 1. The default value is 0.6. |
| incomingOption | { style?: React.CSSProperties; class?: string;  } | No | Adjust the styling of the 'incoming call component'. |
| dialPanelOption | {  style?: React.CSSProperties; class?: string; hideVideoBtn?: boolean } | No | Adjust the styling of the 'dial panel component'. `hideVideoBtn` determines whether to hide the video button, default is false. |
| sessionOption | [SessionOption](#session-option) | No | Adjust the position and size of the 'call window component'. |
| hiddenIncomingComponent | boolean | No | Hide the 'incoming call component'. |
| hiddenDialPanelComponent | boolean | No | Hide the 'dial panel component'. |
| disableCallWaiting | boolean | No | Whether to disable call waiting. When setting this value to `true`, the PBX call waiting value does NOT take effect and PBX only handles single calls. |
| intl | { local: string; messages: Record\<string, string\>} | No | Internationalization (multilingual) settings.<br>For more information, see [intl  (Internationalization) description](#intl-settings). |

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

### <a id="intl-settings">intl  (Internationalization) description</a>

- `local`: Name of the current language, connected with a '**-**'. For example, `en-US` or `zh-CN`.
- `messages`: Configurable text content, which is presented in key-value pairs. Refer to the following content for specific configuration items and their default values.
	```json
	{
    "common.cancel": "Cancel",
    "common.confirm": "Confirm",
    "dial_panel.input.placeholder": "Please input number",
    "dial_panel.tip.connect_failed": "Failed to connect to server, you cannot initiate or answer a call. Trying to reconnect to the server.",
    "incoming.btn.hang_up": "Hang up",
    "incoming.btn.video": "Video",
    "incoming.btn.audio": "Audio",
    "session.calling": "Calling...",
    "session.ringing": "Ringing...",
    "session.talking": "Talking...",
    "session.connecting": "Connecting...",
    "session.hang_up": "End Call",
    "session.new_call": "New call",
    "session.record": "Record",
    "session.mute": "Mute",
    "session.video": "Video",
    "session.hold": "Hold",
    "session.resume": "Resume",
    "session.dialpad": "Dialpad",
    "session.transfer": "Transfer",
    "session.attended_transfer": "Attended Transfer",
	"session.blind_transfer": "Blind Transfer",
    "session.modal.change_to_video.title": "Request",
    "session.modal.change_to_video.content": "{0} invites you switch to video call. Do you accept?",
    "session.error.client_error": "Client Error: {0}",
    "session.tip.recording": "Recording the Audio...",
    "session.tip.pause": "The recording is paused.",
    "session.tip.can_no_use_video": "Unlock this feature with Ultimate Plan.",
    "error.code_200": "Unknown Error.",
    "error.code_202": "No available communication device found.(no permissions)",
    "error.code_205": "Call failed. Cannot process more new calls now.",
    "error.code_206": "No available communication device found.",
    "error.code_207": "Attended Transfer Failed.",
    "error.code_208": "Call failed. Called too many times.",
    "error.code_209": "Call failed. Invalid Number.",
    "error.code_210": "Operation failed with pending calls.",
    "error.code_211": "Answer failed",
    "error.code_290": "No available microphone found.",
    "error.code_291": "No available camera found."
	}
	```

