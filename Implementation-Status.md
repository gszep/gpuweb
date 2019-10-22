This page shows the current implementation status of the [WebGPU API spec](https://gpuweb.github.io/gpuweb/) in browsers. It also lists some resources (samples, demos) for enthusiastic web developers.

The [public-gpu@w3.org](https://lists.w3.org/Archives/Public/public-gpu/) mailing list is a good place to ask questions or provide feedback on the API.

# Implementation Status

## Google Chrome (SPIR-V compatible - In Progress)

Work is in progress in [Chrome Canary](http://chrome.com/canary).

Feature/Platform          | Android  | Chrome OS | Linux | Mac | Windows |
------------------------- | :------: | :-------: | :---: | :-: | :-----: |
Device                    |          |           |       | 👷  |         |
Rendering                 |          |           |       | 👷  |         |
└ Canvas                  |          |           |       | 👷  |         |
└ Textures                |          |           |       | 👷  |         |
└ Multisampling           |          |           |       | 👷  |         |
└ Dynamic Buffer Offset   |          |           |       | 👷  |         |
Compute                   |          |           |       | 👷  |         |
└ Basic Compute           |          |           |       | 👷  |         |
└ Texture Storage         |          |           |       |     |         |

* Root Issue [#852089](https://bugs.chromium.org/p/chromium/issues/detail?id=852089), and blocking issues, are the authoritative reference. Search for [known bugs](https://bugs.chromium.org/p/chromium/issues/list?q=component:Blink%3EWebGPU) before filing [new bugs](https://bugs.chromium.org/p/chromium/issues/entry?components=Blink>WebGPU).
* As GPU sandboxing isn't implemented yet for the WebGPU API, it is possible to read GPU data for other processes. **Avoid leaving it enabled when browsing the untrusted web.**
* The `chrome://flags/#enable-unsafe-webgpu` flag must be enabled.

## Edge
N/A

## Firefox (SPIR-V compatible)

Work is in progress to provide a secure DOM interface to [wgpu-native](https://github.com/gfx-rs/wgpu) via IPC in Gecko.

All the issues and feature requests are tracked by the [Graphics: WebGPU](https://bugzilla.mozilla.org/buglist.cgi?product=Core&component=Graphics%3A%20WebGPU) component in BugZilla.

## Safari (WHLSL compatible - In Progress)

Work is in progress in [Safari Technology Preview](https://developer.apple.com/safari/technology-preview/).

To enable WebGPU, first make sure the Develop menu is visible using `Safari` → `Preferences` → `Advanced` → `Show Develop menu in menu bar`. Then, in the `Develop` menu, make sure `Experimental Features` → `WebGPU` is checked. **Avoid leaving it enabled when browsing the untrusted web.**

Bugs can be viewed and filed [here](https://bugs.webkit.org/buglist.cgi?bug_status=UNCONFIRMED&bug_status=NEW&bug_status=ASSIGNED&bug_status=REOPENED&component=WebGPU).

# Samples

* [webgpu-samples](https://austineng.github.io/webgpu-samples/) for Chrome (uses GLSL via SPIR-V)

* [WebKit/Safari Demos](https://webkit.org/demos/webgpu) (uses WSL)

* [hello-webgpu-compute.glitch.me](https://hello-webgpu-compute.glitch.me): simple demo with both the SPIR-V and WSL paths

# Articles

* [Get started with GPU Compute on the Web](https://developers.google.com/web/updates/2019/08/get-started-with-gpu-compute-on-the-web)


# Frameworks

* [Babylon.js](https://www.babylonjs.com/) (uses SPIR-V)
  * [WebGPU documentation](https://doc.babylonjs.com/extensions/webgpu)
  * Performance comparison demo of [WebGL Forest](https://www.babylonjs.com/Demos/WebGPU/forestWebGL.html) vs [WebGPU Forest](https://www.babylonjs.com/Demos/WebGPU/forestWebGPU.html) (uses SPIR-V)