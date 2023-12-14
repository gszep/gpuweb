This page shows the current implementation status of the [WebGPU API spec](https://gpuweb.github.io/gpuweb/) in browsers. It also lists some resources (samples, demos) for enthusiastic web developers. Also note the [WebGPU Shading Language spec](https://gpuweb.github.io/gpuweb/wgsl/) that's hosted separately.

The [public-gpu@w3.org](https://lists.w3.org/Archives/Public/public-gpu/) mailing list is a good place to ask questions or provide feedback on the API.

You can also join the chat on Matrix in the "Web Graphics" Matrix Community: [#webgraphics:matrix.org](https://matrix.to/#/#webgraphics:matrix.org). The general WebGPU channel is [#WebGPU:matrix.org](https://matrix.to/#/#WebGPU:matrix.org).

# Implementation Status

## Chromium (Chrome, Edge, etc.)

WebGPU has begun shipping to Mac/Windows/ChromeOS in **Chrome 113 and Edge 113!**
As always, developers should develop against
[Chrome Canary](http://chrome.com/canary) or
[Edge Canary](https://www.microsoftedgeinsider.com/en-us/download).
Increased reach, other platforms, and bug fixes are ongoing.<sup>1</sup>

| Android  | Chrome OS | Linux | Mac | Windows |
| :------: | :-------: | :---: | :-: | :-----: |
| 👷 Behind a flag<sup>2</sup> | 113 | 👷 Behind a flag<sup>2,3</sup> | 113 | 113 |

<sup>1</sup> For details, look at the [Dawn bug tracker](https://crbug.com/dawn) and in the
    Chromium bug tracker [Blink&gt;WebGPU component](https://bugs.chromium.org/p/chromium/issues/list?q=component:Blink%3EWebGPU).
    Search these before filing new bugs.
<br><sup>2</sup> The `chrome://flags/#enable-unsafe-webgpu` flag must be enabled on these platforms (not `enable-webgpu-developer-features`). This flag is only available in Dev/Canary channels.
<br><sup>3</sup> Linux experimental support also requires launching the browser with `--enable-features=Vulkan`.

## Firefox and Servo

These browser implementations are based on [wgpu](https://github.com/gfx-rs/wgpu) project in Rust.

Platform-independent features:
- [x] WGSL shaders
  - [x] validation
  - [ ] overridable constants
  - [ ] behavioral analysis
  - [ ] uniformity analysis (present currently, but not up to spec)
- [x] buffer mapping
- [x] core features:
  - [x] render bundles
  - [x] compute
- [ ] errors
  - [x] error model
  - [x] error scopes
  - [ ] graceful device lost handling
- [x] presentation

| Platform-dependent features | Vulkan | D3D12 | Metal |
| --------------------------- | ------ | ----- | ----- |
| present surface sharing     |        |       |       |
| bounds checks               |:heavy_check_mark: | :heavy_check_mark: (not needed) | :heavy_check_mark: (missing vertex pulling) | 

### Firefox

In [Nightly](https://nightly.mozilla.org/) Firefox builds, WebGPU is enabled on Windows and Linux. For macOS and Android, users need to set the `dom.webgpu.enabled` pref to `true`.

All the issues and feature requests are tracked by the [Graphics: WebGPU](https://bugzilla.mozilla.org/buglist.cgi?product=Core&component=Graphics%3A%20WebGPU) component in Bugzilla. For more details, see the [Mozilla wiki](https://wiki.mozilla.org/Platform/GFX/WebGPU).

### Servo

Work [in progress](https://github.com/servo/servo/projects/24), enabled by "dom.webgpu.enabled" pref.

## Safari (In Progress)

Work is in progress in [Safari Technology Preview](https://developer.apple.com/safari/technology-preview/).

# Materials

## Samples

* [webgpu-samples](https://austineng.github.io/webgpu-samples/) for Chrome and Firefox (uses WGSL, or GLSL via SPIR-V)

* [wgpu-rs samples](https://wgpu.rs) for Firefox and Chrome (uses GLSL via SPIR-V), compiled from [Rust](https://github.com/gfx-rs/wgpu-rs)

* [WebKit/Safari Demos](https://webkit.org/demos/webgpu) (uses WSL)

* [hello-webgpu-compute.glitch.me](https://hello-webgpu-compute.glitch.me): simple demo with both the SPIR-V and WSL paths

## Demos

* [webgpu-clustered-shading](https://github.com/toji/webgpu-clustered-shading)
* [Meta-balls](https://toji.github.io/webgpu-metaballs/)
* [Spookyball](https://spookyball.com/) - 3D version of "Breakout", Halloween theme.
* [WebGPU Playground](https://webgpu-playground.netlify.app/) - a student project at Imperial College London. [feedback](https://forms.office.com/pages/responsepage.aspx?id=B3WJK4zudUWDC0-CZ8PTB-fvlzml-hFEprxqLaQ4CghUNUlDRzlRUFYwTVdBWlVVN1AzQzk2NjhNMS4u)

## Articles

* [Get started with GPU Compute on the Web](https://developers.google.com/web/updates/2019/08/get-started-with-gpu-compute-on-the-web)

## Frameworks

* [Babylon.js](https://www.babylonjs.com/) (uses SPIR-V)
  * [WebGPU documentation](https://doc.babylonjs.com/extensions/webgpu)
  * Performance comparison demo of [WebGL Forest](https://www.babylonjs.com/Demos/WebGPU/forestWebGL.html) vs [WebGPU Forest](https://www.babylonjs.com/Demos/WebGPU/forestWebGPU.html) (uses SPIR-V)