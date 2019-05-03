This page shows the current implementation status of the WebGPU API in browsers. It also lists some resources (samples, demos) for enthusiastic web developers.

# Implementation Status

## Google Chrome (SPIR-V compatible)

Work is in progress in [Chrome Canary](http://chrome.com/canary).

Feature/Platform          | Android  | Chrome OS | Linux | Mac | Windows |
------------------------- | :------: | :-------: | :---: | :-: | :-----: |
Device                    |          |           |       | 👷  |         |
Rendering                 |          |           |       | 👷  |         |
└ Canvas                  |          |           |       | 👷  |         |
└ Textures                |          |           |       | 👷  |         |
└ Multisampling           |          |           |       | 👷  |         |
└ Dynamic Buffer Offset   |          |           |       |     |         |
Compute                   |          |           |       | 👷  |         |
└ Basic Compute           |          |           |       | 👷  |         |
└ Texture Storage         |          |           |       |     |         |

* Root [Issue #877147](https://bugs.chromium.org/p/chromium/issues/detail?id=877147) and blocking issues are most authorative on status. Search for [known bugs](https://bugs.chromium.org/p/chromium/issues/list?q=component:Blink%3EWebGPU) before filing [new bugs](https://bugs.chromium.org/p/chromium/issues/entry?components=Blink>WebGPU).
* As GPU sandboxing isn't implemented yet for the WebGPU API, it is possible to read GPU data for other processes
* On Mac, the `chrome://flags/#enable-unsafe-webgpu` flag must be enabled.

## Firefox (SPIR-V compatible)
N/A

## Safari (WHSL compatible)
N/A

# Resources

* SPIR-V compatible samples:
  * https://github.com/austinEng/webgpu-samples