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

* Root Issue [#877147](https://bugs.chromium.org/p/chromium/issues/detail?id=877147) and blocking issues the authoritative reference. Search for [known bugs](https://bugs.chromium.org/p/chromium/issues/list?q=component:Blink%3EWebGPU) before filing [new bugs](https://bugs.chromium.org/p/chromium/issues/entry?components=Blink>WebGPU).
* As GPU sandboxing isn't implemented yet for the WebGPU API, it is possible to read GPU data for other processes.
* The `chrome://flags/#enable-unsafe-webgpu` flag must be enabled.

## Firefox (SPIR-V compatible)
N/A

## Safari (WHLSL compatible)
N/A

# Samples

* webgpu-samples ([source](https://github.com/austinEng/webgpu-samples)) (uses SPIR-V)
    * [hello_triangle.html](https://austineng.github.io/webgpu-samples/hello_triangle.html) ([source](https://github.com/austinEng/webgpu-samples/blob/master/hello_triangle.html))
    * [rotating_cube.html](https://austineng.github.io/webgpu-samples/rotating_cube.html) ([source](https://github.com/austinEng/webgpu-samples/blob/master/rotating_cube.html))
    * [texture_cube.html](https://austineng.github.io/webgpu-samples/textured_cube.html) ([source](https://github.com/austinEng/webgpu-samples/blob/master/textured_cube.html))
    * [compute_boids.html](https://austineng.github.io/webgpu-samples/compute_boids.html) ([source](https://github.com/austinEng/webgpu-samples/blob/master/compute_boids.html))

# Frameworks

* [Babylon.js](https://www.babylonjs.com/) (uses SPIR-V)
  * [WebGPU documentation](https://doc.babylonjs.com/extensions/webgpu)
  * Performance comparison demo of [WebGL Forest](https://www.babylonjs.com/Demos/WebGPU/forestWebGL.html) vs [WebGPU Forest](https://www.babylonjs.com/Demos/WebGPU/forestWebGPU.html) (uses SPIR-V)
