# GPU Web meeting 2022-11-09/10 APAC-timed

Chair: KG

Scribe: KR

Location: Google Meet

Time: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WebGPU+Meeting&iso=20221109T16&p1=137&ah=1](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WebGPU+Meeting&iso=20221109T16&p1=137&ah=1) 


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * Myles C. Maxfield
* Cocos
    * **Huabin Ling**
    * **Zeqiang Li**
    * **Zhenglong Zhou**
* Google
    * Austin Eng
    * Ben Clayton
    * **Brandon Jones**
    * Corentin Wallez
    * Dan Sinclair
    * David Neto
    * Gregg Tavares
    * **Kai Ninomiya**
    * **Ken Russell**
    * Loko Kung
    * Shannon Woods
    * Shrek Shao
    * Stephen White
    * Ryan Harrison
* Intel
    * Brandon Jones
    * Bryan Bernhart
    * Hao Li
    * Jiajia Qin
    * **Jiawei Shao**
    * Jie Chen
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Jesse Natalie
    * Rafael Cintron
* Mozilla
    * **Erich Gubler**
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* Snap
    * Corvyn Kusuma
    * Dhritiman Sagar
    * Sumant Hanumante
* Unity
    * Brendan Duncan
    * Dave Shreiner
* Connor Fitzgerald
* Francois Daoust
* Mehmet Oguz Derin
* Michael Shannon
* Rob Conde
* Russell Campbell


# Administrivia



* KG: Welcome to Cocos!
* HL: Very glad to join the meeting. Cocos is officially supporting WebGPU now as part of our platform. Met Kai in 2019 at GDC. I lead Cocos' engine team - three generations of product. Colleagues here in this meeting are working on WebGPU. We have a presentation to offer about how we're using WebGPU.
* 
* 
* 
* 


# CTS Update



* KN: Gyuyoung's working on a few operation tests, including render operation tests like blending
* WGSL side: Ryan's fixed some precision tests. Looking into test performance; some WGSL tests have a lot of test overhead in parameter generation.
* 
* 
* 


# “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)



* Nothing on the queue. Two candidates are already on the agenda. 6 total candidates.
* 
* 
* 
* 


# How do we specify the timing of external texture expiration? [#3324](https://github.com/gpuweb/gpuweb/issues/3324)



* KN: thought we resolved this last week. Were going to allow importExternalTexture to return the same object, and were going to propose a PR doing this, but we ran out of time.
* KG: didn't see perhaps one area of consensus?
* KN: I have concerns about tryRefresh. Lets you import the same texture multiple times. Adds complexity.
* KN: thoughts since last meeting:
* KN: when you import an external texture now, we take some texture owned by the decoder and use interop to import it as a Vulkan texture into e.g. Dawn/WebGPU. Maybe multiple, for multiple planes. To use them, we BeginAccess before using it, and EndAccess afterward. If you can import the same frame multiple times and don't cache the results, you can end up importing the same underlying texture multiple times. Use both in same draw call - will need to BeginAccess on both for synchronization, and EndAccess. They may conflict because you're trying to lock the same lock twice. Will probably say, if holding this lock, don't hold it again. Similar cache we'd need to let import() return the same object. I don't think we avoid any impl complexity with tryRefresh. Talked with Corentin too - he was in favor of tryRefresh, but he's convinced by this argument.
* KG: would also make it easier for you if someone imported the same texture twice?
* KN: yes. I think it's a pretty good solution. When I write a proposal - I'll propose a version that returns the same object. No objections to this idea last week.
* KG: **think we can resolve on that. Waiting for a PR**.


# Cocos’s feedback



* Presentation: [Cocos @ GPU for the Web.pdf](https://drive.google.com/file/d/1Ce9H0xNEezg_ATOSzKkBGl8b6XUIpUMh/view?usp=share_link)
* (HL presenting screen)
* HL: Cocos is most popular for our 2D game engine. Now have full featured game editor with 3D and 2D rendering combined. Vulkan, Metal, now WebGPU. Support web natively. And native platforms with our native branch (not the same source code).
* [https://preview.cocos.com/lake/](https://preview.cocos.com/lake/)
* Ability to customize render pipeline
* Customizable features coming by EOY - including render graph. Easy to customize render pipeline.
* Editor based on the web. Running in Electron. Target is to simplify work on the editor itself. Editor, debugger, etc. are written in HTML/CSS. We suggest developing on the web; then you can publish to native platforms later.
* With WebGPU we think our code bases will finally merge together.
* Our GFX API abstraction is very much Vulkan-like. Allowed supporting WebGPU easily.
* Support Metal, OpenGL ES, Vulkan - and WebGL and WebGPU.
* Serious performance issues with WebGPU. API invocation from .ts to Wasm is too expensive. Predictable.
    * Lot of potential from raising our binding level and reduce invocations between TS and Wasm.
    * Maintenance friendly. One code base for all modern rendering; no JS in rendering process.
    * Later - want to bring render-pipeline into C++ and into renderer.wasm.
* Ran into unexpected problems.
    * GPU cost of WebGPU is higher than WebGL.
    * Even after optimization, finding GPU cost of WebGPU is 4 ms, and WebGL is 1 ms (for one particular user case). Still investigating why.
* Discussions:
    * Proper way to investigate performance for WebGPU including CPU and GPU side?
        * Using Pix, but only for WebGPU - WebGL doesn't use DX12.
        * RenderDoc, other tools supported?
    * Need to reduce the cost of JS->Wasm
* Our SPIR-V compilation is very expensive. Aim to compile to WGSL offline, but need to preserve macros. Suggestions on how to do this?
* JB: on SPIR-V compilation cost - Tint compiler was designed to be heavy on validation, haven't started looking at performance. 
* KR: Can we talk a little more about cost going JS->WASM?
* KR: So the big cost is typescript->wasm?
* ZL: I have a test, there was a time I was using a property from TS to judge if two objects are the same. Every frame is []. Once I tried to cache the property in TS, and don’t invoke the [] from wasm, it saves a lot. In the demo, we’ve tried 1000 models in this demo, it’s about 24ms/frame, but once I cache the property, cost decreases to 16ms (-8ms). 
* HL: This is one property right?
* ZL: Yes, the object ID.
* HL: The previous soln was to bind the webgpu from TS. Internally the most effective soln would be increasing the binding layer, so only [] logic is needed in the renderer. 
* KN: Thanks for diving into that. Anywhere where you can split the code, keeping fewer calls between JS and WASM is better. Either keeping low level low, or high level high. Can’t say what perf will be for webgpu bindings here, we don’t have a good idea of how perf it is yet. We do expect it to be similar to webgl, due to similar number of calls being made.
* KR: Is it possible to extract some kind of benchmark to show this JS->WASM perf issue? IIRC in Mozilla’s engine, this is basically free because the JS engine is the same.
* KR: We’re interested in solving this kind of problem, like what we’ve run into for e.g. webgl also. We do want to find ways to fix things so that they’re fast. In particular, if we have a benchmark where Chrome is slower than Firefox, that’d be compelling for us to fix in Chrome.
    * [https://hacks.mozilla.org/2018/10/calls-between-javascript-and-webassembly-are-finally-fast-%F0%9F%8E%89/](https://hacks.mozilla.org/2018/10/calls-between-javascript-and-webassembly-are-finally-fast-%F0%9F%8E%89/)
* HL: Yeah it’d be great to compare Firefox to Chrome. Currently testing on Chromium.
* KG: Firefox's general Web IDL binding code is also faster than Blink's. Not specifically what's being discussed here.
* JB: are you using RenderBundles?
* HL: not yet.
* ZL: not yet. Have it planned.
* JB: it makes a big difference when JS call overhead is significant.
* KG: can we get a link to this slide deck?
* HL: will share.
* KG: other ways to keep in touch with us - https://github.com/gpuweb/gpuweb , the Matrix #webgpu room, [https://matrix.to/#/#WebGPU:matrix.org](https://matrix.to/#/#WebGPU:matrix.org) 
* 
* 
* 


# Multiple components cannot effectively share a GPUDevice because it is stateful [#3250](https://github.com/gpuweb/gpuweb/issues/3250)



* KN: Brandon and I discussed more. Last week - thought we needed to add an item to the prototype chain. That's not possible, but there are alternatives which aren't breaking changes. After that discussion, we're satisfied this isn't a problem we have to deal with right now. Can punt this to post-V1.


# Should "mark adapters stale" be called more quickly? [#1630](https://github.com/gpuweb/gpuweb/issues/1630)



* KN: Other one which Brandon and I discussed Monday. Thought through device creation cases. Gist: want to steer developers toward, when they need a new device, request an adapter first and create a device from it. Gives browser opportunity to select the best adapter. Right now the spec says, adapters may expire whenever, and browsers should do it often. But we couldn't find a case where you needed to be able to hold onto an adapter for a long time.
* KN: each adapter object will be one-time use. Create one device from it. If you don't do that, browser won't necessarily do anything with the adapter object (it can stay alive, or not). Unusual for an app to fetch an adapter and use it 3 days later. If that fails, should trigger retry logic in the app. We think this simple API, making the adapters one-shot, is best.
* BJ: little more context. Imp't: how this interacts with getAdapterInfo. Should we expire adapters with same cadence as canvas textures? At end of task? Discussed use cases. Developers, if they want to look at the adapter info for workarounds - that's potentially a long-running Promise. Browser UI may pop up dialogs, get user consent. Would need exception to expiration there. It got messy fast, and didn't seem to give much benefit - if you can postpone expiration by calling getAdapterInfo, would be awful, and developers might start doing it. Can achieve the result by making it one-shot for creating the device. Giving you a drop-down to select an adapter - would make that more practical, too, since the adapters would be kept alive until you did something with them.
* KR: any reason to have a close() or similar on GPUAdapter? To release GPU resources?
* Discussion.
* KN: don't think that being able to close the adapter would make that meaningfully easier - e.g. closing graphics drivers.
* KG: not sure it's a good idea to make it more difficult to create multiple GPUDevices against the same GPUAdapter.
* KN: agree, makes you pause. Think we should address needs for specific adapters to be "I need the adapter you give me to be compatible with this XR device." Use-case driven. When we find cases where this'll be useful, can make them work.
* KG: what pushed us in the direction to make adapters expire? To make people do the right thing?
* KN: when an app's reinitializing, we want to make sure they let the browser select an adapter at a recent point. If you get an adapter and write some code - if device is lost and you try again on the same adapter - OK if you ran out of memory, etc. But if the adapter was unplugged from the system - your recovery logic won't work. People should always request an adapter when you create a device.
* KG: need to think about this more.


# Add HTMLImageElement and ImageData to copyExternalImageToTexture [#3430](https://github.com/gpuweb/gpuweb/pull/3430)



* 
* 
* 


# Forbid any ‘\0’ in all string? [#3576](https://github.com/gpuweb/gpuweb/issues/3576)



* 
* 
* 


# Agenda for next meeting



* Take a look at the upcoming agenda items, let's try to make progress offline.
* November 16: Non-APAC
* 
* Upcoming WebGPU API schedule:
    * November 09: APAC
    * November 16: Non-APAC
    * _November 23: _APAC
    * (US Thanksgiving Nov24, Thursday)
    * November 30: APAC
    * December 07: Non-APAC
    * December 14: APAC
    * _December 21: _Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 28: No Meeting_
    * January 04: APAC
* 