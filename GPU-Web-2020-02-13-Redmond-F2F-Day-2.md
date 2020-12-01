So as to not be anonymous animals, the doc is shared for writing with the google accounts that are invited to the meeting. If you can’t edit let [cwallez@google.com](mailto:cwallez@google.com) know. Also be sure to be in “Edit” mode and not “Suggest” mode.




Chair: Corentin

Scribe: Austin

Location: Google Meet


## [Minutes from Day 1](https://docs.google.com/document/d/1vQPA1JSOvfCHjBrkAEDLA1qCqQXe72vGen_1quoHZV8)


## Tentative agenda

Thu morning:



*   WebML synopsis (Rafael)

Thu afternoon:



*   Meeting with D3D Team (after lunch)
    *   DM: How do you decide to make the feature tiers that you have?
    *   CW: Shader combinatorial explosion problem? In your experience what are useful tools at the API and ecosystem level to help limit that explosion?
    *   RC: Robust buffer access. In D3D11 it’s everywhere. When is D3D12 robust/not? (E.g. for raytracing etc, you just hand it an arbitrary virtual address.)
        *   DP: Security boundaries and process isolation.
    *   MM: What is the state of the art for picking threads-per-threadgroup when shipping an app/game/whatever to unknown devices? Because HLSL bakes this into the shader source, mechanically, how do ISVs set it?
    *   MM: When designing an API-agnostic solution for keeping data on-chip (for tiling devices), is there anything I should keep in mind?
    *   RC: Multi-GPU
    *   

    **Unprioritized topic bucket**

    *   Is it still important to support occlusion queries? [#72](https://github.com/gpuweb/gpuweb/issues/72)
    *   fxc / dxc questions. Performance issues from WebGL. Questions (again) about using DXC compiler pipeline with D3D11. (Ken)
    *   **More from looking through gpuweb issues (KN):**
    *   Opinions on inclusion in WebGPU 1.0 (are they useful? fast across architectures? etc):
        *   [#431](https://github.com/gpuweb/gpuweb/issues/431) ExecuteIndirect
        *   drawIndirect/dispatchIndirect: opinion on inclusion in the API?
        *   [#442](https://github.com/gpuweb/gpuweb/issues/442) [#395](https://github.com/gpuweb/gpuweb/issues/395) programmable blending, fragment shader interlock, raster order views, etc.
        *   #455 sparse resources
        *   #450 variable rasterization rate
        *   #445 tessellation
        *   #380 bindless resources
        *   [#162](https://github.com/gpuweb/gpuweb/issues/162) uniform texel buffer / storage texel buffer?
        *   [#275](https://github.com/gpuweb/gpuweb/issues/275) Picking a good value for threads-per-threadgroup
        *   [#284](https://github.com/gpuweb/gpuweb/issues/284) Consider associating the texture with its clear color value
        *   [#72](https://github.com/gpuweb/gpuweb/issues/72) occlusion queries?
        *   pipeline statistics queries?
        *   Geometry shaders???
    *   ~~[crbug.com/dawn/336](https://crbug.com/dawn/336) Should we expect any differences between “desktop” and ARM/64 D3D12?~~
    *   ~~What about Xbox D3D12?~~
*   ~~Chat with David Catuhe from Babylon.js (3PM ish?) (not coming)~~

Other topics with no timing constraints:



*   SetBufferSubData (Corentin?)
*   Multiqueue (Corentin?)
*   Timer query and other queries (occlusion query, pipeline statistics query)
*   Storage texture ([#513](https://github.com/gpuweb/gpuweb/pull/513),[ #541](https://github.com/gpuweb/gpuweb/pull/541),[ #532](https://github.com/gpuweb/gpuweb/pull/532),[ #542](https://github.com/gpuweb/gpuweb/pull/542))

Finally:



*   Agenda for next meeting


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



*   Alibaba
    *   Qian Qian Yuxuan
*   Apple
    *   **Dean Jackson**
    *   Jason Aftosmis
    *   JF Bastien
    *   **Justin Fan**
    *   **Myles C. Maxfield**
    *   Robin Morisset
    *   Theresa O'Connor
    *   Thomas Denney
*   Google
    *   **Austin Eng**
    *   **Corentin Wallez**
    *   **Dan Sinclair**
    *   David Neto
    *   Idan Raiter
    *   James Darpinian
    *   John Kessenich
    *   **Kai Ninomiya**
    *   Ken Russell
    *   Shrek Shao
    *   **Ryan Harrisson**
*   Graphistry
    *   Miller Hooks
*   Intel
    *   Brandon Jones
    *   **Bryan Bernhart**
    *   **Yunchao He**
*   Microsoft
    *   **Chas Boyd**
    *   **Damyan Pepper**
    *   **Rafael Cintron**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
*   Yandex
    *   Kirill Dmitrenko
*   Elviss Strazdiņš 
*   Joshua Groves
*   Markus Siglreithmaier
*   Mateusz Kielan
*   **Mehmet Oguz Derin**
*   Samuel Williams
*   Timo de Kort
*   Tyler Larson


## WebML Synopsis

RC: Intel Shanghai started experimenting with exposing their ML chips to the web. Ideas about loading a model, or using compute shaders, etc. Ken Russell suggested to expose as low-level of an API as possible, because the ecosystem of models is changing and shifting. Graph API where in JS, the app would put together a graph of ops. Ningxin tried this and made a prototype based loosely on NN API (Android). Performance looked really good. Community group started, Google joined after. Suggestion was to not do a graph, just operators. Then group started thinking about doing it has a WebGL or WebGPU extension. Take existing buffers and have them be inputs into Ops, or a compute shader of your own. Microsoft’s position has always been that LoadModel is better. Few people want to do a graph. Microsoft wants LoadModel to take the ONNX format. All the IHVs are in the ONNX group. It’s two things: a file format, and an operator spec. There are hundreds of ops, TensorFlow has many, many more. If LoadModel is not tenable, then Microsoft’s idea would be that the graph is good, or in addition to a file format. Google changed position again and thinks that a file format would be good as well. Group needs to decide what the model format will be. MLIR has been actively developed, etc.

JD: Google has unfortunately changed their minds a lot. The last time was because we met with Microsoft and talked to them about it, and came closer to what Microsoft was proposing. As RC said, people at Google aren’t super happy with ONNX as a standard, but we don’t have a mature alternative to it.

RC: Are you thinking of doing something like TF lite?

JD: I think something like LoadModel with different backends, and you could choose which one you’d like -- prototype of course.

CW: Two questions: If it’s a LoadModel API, it will probably be extensible to have hundreds of Ops. How do you deal with fragmentation where one browser understands fancy ops and others don’t?

DJ: Also, the model could specify ops that are hardware accelerated on some platforms, and not on others. There could be a drastic performance divergence. In general browser vendors would not like this.

JD: Huge problem. Huge number of operations, growing every year. Obviously we wouldn’t want that in the standard. It’s unclear if there’s a restricted subset of ops that would be useful and still accelerates on all underlying APIs. Lots of investigation needs to happen to determine this sort of thing.

Chai: lead for DirectML. Background has been in OS. DirectML started two years ago and was released last year. Firsthand experience implementing many of the Ops. DirectML aims to accelerate any op on any GPU. Performance varies, but it’s an abstraction layer. Joined conversations on WebML with the perspective: What should the OS provide to the browser so the web developer experience remains uniform, the ML models can carry out faithfully within the scope of how the model is trained. While performance may vary, we hope the experience is uniform. When I started joining the WebML biweekly meetings, we had talked about LoadModel and the convenience it provides. But to me, this is simply a convenience layer. The real discussion topic is what is inside the file format. That’s the contract between the web developer and the browser. Very likely, that will become the browser and the OS that runs it. What’s inside is the most important aspect. The rest of whether it’s a graph API or LoadAndRun, that’s a next layer of abstraction. The Graph API, there’s more of a guarantee because it can be translated to formats. That’s the benefit of Model-less programming model. Google, of course, has a lot of advances around TensorFlow and how ML models help and what’s useful. ONNX came later, so there’s less breadth and is still a subset of what TensorFlow can do. My team, being the GPU accelerator of ML, we looked at all the frameworks. We work closely with the ONNX runtime team, and understand it quite deeply. When we started ONNX, they really wanted to be an interchangeable format that can handle models trained by different frameworks. TF, PyTorch, etc. The spirit was for it to be universal. In practice, it’s still to be proven whether that goal can be realized. In my view, the spec is decent, and the topic about what format the browser should support -- I don’t want to start format wars -- is the right abstraction level the operator, the graph, or even lower than that? Johnathan has left the option open and intends to bring up this issue. We’ll have to do a lot of work toward that meeting.

CW: okay, so you’re saying that the shape of the API and the way you access the operators, is independent of the list of operators. What you care more about is what is exposed, and whether they’re expressed uniformly. The question is what is the better abstraction? Op level or even more primitive?

Chai: In my opinion the Op level is a good abstraction.

CW: What’s lower level than ops?

Chai: There are big ops like convolution. There are smaller pieces like mat mul, sigmoid, etc. The sub-operators can be thought of in many ways. Say you have a mean-variance op. That’s composed of log, sigmoid, etc. Can be broken down into primitive math ops. Then, you can break it down even further. If you want to implement mat mul, you can compile it down to a long list of instructions. #1 should it be lower than operator. #2 if it should be, how low? The lower you go, the harder it is to maintain the contract between the browser and OS.

DJ: We agree in general. On our platforms, we have multiple bits of hardware that do different things, with different power implications. It’s important for us to decide what hardware to use at what time. The higher up in the stack, the better we are to efficiently decide what to do. The iPhone has an image processing chip for the camera. Some ops might be better for that. We have dedicated ML chips for other stuff, and the GPU.

Chai: The way we implement DirectML is exactly what you said. At runtime, we pick the GPU to run on. It’s important for the OS to optimize based on what hardware you have.

DJ: If we have the low-level instructions, it’s hard to reverse engineer what to do.

Chai: And it’s also unfair for the user to not be able to use dedicated ops.

DJ: Also, what data representations? Some of the hardware is optimized for float32 and some float16. The lower level it is, the harder it is for us to decide float32 or float16 or float8. I don’t know how this maps to the requests on the WebGPU group.

CW: Yes, that was my next question. Is there any interaction with WebGPU like D3D meta commands or importing buffers, etc?

YH: Previously I worked in Shanghai on the same team. Ningxin wants to know whether we can choose a compute-only device. Like a VGPU. 

JG: I want that. I think that’s something we are going to have.

MM: One way to answer: If it is exposed to Vulkan or D3D or Metal, that makes sense for us to target it. If it’s exposed via a fourth API, that’s a harder decision to make.

YH: You can get it on D3D if you enumerate adapters.

Chai: Working with Ningxin on this, and yes on D3D you can enumerate different hardware. I don’t know if that’s equivalent in WebGPU or not. I agree there needs to be a way to select them.

YH: His proposal is to have requestAdapters allow selection of the VGPU. And then, if we do this, how can we share data across adapters?

RC: If we expose a very low-level operator API, if ops aren’t supported, maybe we can use WebGPU compute shaders.

CW: We looked briefly. In Metal at least, there’s a way to do this, and in other APIs as well. The most difficult part will be synchronizing  work between accelerators and the GPU. Ideally we’ll want to give all the synchronization to D3D and DirectML, especially if we’re using metacommands there. Making it fast and avoiding synchronization will be very difficult.

Chai: Synchronization issue: It could happen within DirectML, the issue we ran into was that there are some cases where the user needs some way to tweak their models -- build custom kernels to move data faster in the graph. In those cases, if they use WebGPU directly to make their own data and push it back into the graph, the synchronization needs to be in the hands of the user. We try to define DirectML at the level of D3D. We want flexibility to all the callers of the API. If you’re looking at gaming scenarios -- game titles want ML more and more -- they need full control over synchronization, how resources are managed, etc.

CW: Sorry, what I meant by synchronization: web page takes a buffer, gives it the the browser -- the browser should have to go back and forth to send this to/from DirectML and D3D.

Chai: That could make the synchronization simpler so the user doesn’t have to manage binding resources etc.

CW: Okay and there are also Metal performance shaders and D3D metacommands. Is there an appetite for a metacommand like thing in WebGPU?

JD: I proposed something like this, but I don’t think there was a big appetite for that in the group. They wanted to build a separate API.

DJ: I think there’s use in the idea of Metal performance shaders. If you were to implement that functionality yourself, you definitely wouldn’t get as much performance across all hardware. The MPS API is really tuned depending on what hardware you’re running on.

JD: To me, the big win of doing an ML API is not really running neural nets on GPUs. We can always write shaders to do this though they may be slower. The big win is to allow access to non-GPU accelerators that are especially important in phones for low power consumption. You wouldn’t get access to that with a WebGPU extension. Makes sense to focus on those special accelerators.

Chai: Design of metacommands: You’re right that generally there are two kinds of hardware devices. 1. Designed for compute. Once you have that, metacommands are less important because they’re just another way to reach the hardware. At first Intel devices were not interested in just general compute, and more fixed-function. For that, it’s hard to map onto the general compute model. Metacommands give a way for the OS to tap into the fixed-function hardware very quickly without the driver spending a lot of time to make the hardware work on a general compute model. That said, while it allows quick use and experimentation, the problem becomes it’s very good at one thing in one situation with certain constraints. But when the use case is a little different, it falls apart. Very fast to very slow performance cliff. Not designed to be generic. In the case of DirectML, we try to be very careful of deciding what hardware to execute the metacommand on and when. It’s not a silver bullet. If WebGPU has it, to use it is not straightforward. It’s tricky and we generally discourage it. Better to use DirectML.

BB: We’re thinking about exposing VPUs as compute-only devices. 

JD: VPU is one case which is not that common. The common case is many phones with specific ML hardware that is not general purpose that could not be exposed to WebGPU.

Chai: The issue with a model like MobileNet. But say you want to tweak it to have another convolution or unusual striding, you suddenly go from really fast to really slow.

JD: Yea that’s a huge problem. Work needs to be done to determine if there’s a lowest common denominator so we can expose the hardware. 

Chai: Not sure about that. The problem with Web programming is that if the web developer wants something to run, they do expect it to run.

JD: Yes, we shouldn’t expose a bunch of Ops that can’t be implemented on a large part of the accelerators. Maybe there’s an LCD that can be supported on most/all hardware. Maybe some people can’t run their special strided convolution, but there is some subset that can definitely be accelerated on these chips.

Chai: One of the things the WebML workgroup is trying to do is to find that overlap. ex..) Convolution2D we did find an overlap of the APIs. You’re right that not everything can be supported. I’m still a little bit nervous on the performance cliff.

RC: Versioning of ONNX?

Chai: I’m not familiar with it very much. Though it’s challenging because there are a lot of Ops. You can start by thinking it’s not that hard and just version check every op. But then you have to do crazy version checks on everything before running the model. You also can’t have model versioning either because sometimes the difference between different versions of a model is very small. Bumping the whole version is not realistic. There’s a challenge to create a versioning story that is both practical and the lesser of all the evils. Think they settled on the Offset model. Their model depends on the fact that, if within your graph every op can version independently, the highest version op in your graph defines the version of the graph. That allows the runtime to determine if it can run the model. The problem with this is that if you introduce custom ops, which anyone can define, when you do that, it bumps to entire Offset version of the model. Then if you’re the runtime, you’re not sure whether or not you can run the model. So, what they do, they first check the IR version, then they check the highest version number of the ops in the graph. Then they determine if they can run. It’s not perfect.

CW: Going back to WebGPU. What can this group do for / with the WebML group?

RC: Can’t think of anything. Keep on keeping on. If we do decide to do an extension or expose something that’s just operators, we can then consider doing an extension to WebGPU. The most important thing is to first ship WebGPU.

CW: I heard from BB about compute-only devices. Is that something we want to do?

RC: Yes. Maybe after. In addition to all the enums you provide, we consider additional options to ask for a device.

JD: I think the most likely thing is that WebGPU will have some way to share buffers, but I think it will be a while.

Chai: I think the WebML group should have an exercise at some point to try to implement a custom up with WebGPU and make sure it interops with existing ops. That would be a way to validate buffer sharing and stuff. People should be able to do it.

YH: Intel Shanghai is also working on some ML operators via compute shaders with TF.js, and I think if we use WebGPU compute shaders, it’s not difficult to share the data. If we use accelerators or VGPUs, it could be a hard problem. Experience shows that after you do inference, you want to render something in some cases like hand/gesture recognition and tracking.

CW: Yes, so interop is important.


## Query APIs

**(Presentation by Yunchao)**

[Slides](https://docs.google.com/presentation/d/1XYeFUnwTS1gpchxTSCegHRC-wzDmg3qmwg3y3gJlBLM/edit) and other[ investigations](https://drive.google.com/drive/u/1/folders/1AAFpnJ7J8KrCrwfhlBGu2xvri11n7tWj)

MM: Missing from analysis, iOS (10.3+) does have timing info: GPUStartTime and GPUEndTime on the MTLCommandBuffer after execution.

AE: Also added in MacOS 10.15

(Discussion about whether timestamp and other query data should come back on the GPU instead of on the CPU.)

(Discussion about pass.begin/endQuery api. Doesn’t work for occlusion queries on metal begin render pass which requires the visibilityResultBuffer.)

MM: GPUDevice.getQueryResults is on the device, but it needs to occur on the queue timeline. Should put it on GPUQueue (or add an argument to specify the queue).

DP: getQueryResults returns results for every query in the GPUQuerySet? So you can’t have a giant one - need to have small ones that are close to the number used in one pass.

CW: nit: destroyQuerySet could probably be handled by garbage collection.

MM: Actually disagree. They represent GPU objects so should have a .destroy().

MM: Could provide a buffer and an index to getQueryResults, and change its return type to void. Then the author can copy the data to the CPU themselves if they want.

DP: …

MM: Metal has something similar, in begin/end there’s a boolean argument that says “insert a barrier or not”. If you say no, it’s less clear what you measure, but if you say yes, it’s slower.

MM: Good analysis. Happy to collaborate to refine the design.


## D3D Team Q&A

(Intros)

**DM: How do you decide to make the feature tiers that you have?**

MMc: How we add features to D3D. Close partnership with major hardware vendors. Looking many years ahead before they’ve even started hardware design. Things that would improve games or UI rendering, etc. Or could be solving developer problems. Engagement refines to hardware roadmaps and when stuff will be in market. Usually vendors thinking about the same things at the same time. We try to play a role in creating alignment and tiering in how things will be valuable for customers. For a given feature, they may be classes of implementations that solve the core basic problem, or further refine it. Rasterization is a good example. General feature talked about for a while. Published in research before hardware. There were varying levels of precision which informed how we tier an individual feature. Sort of the next phase in defining feature levels is involved is understanding the total available market for Windows and understanding what things will reach a certain set of availability. Those feature levels are around giving developers an easy way to target a segment of users. The groupings are around scenarios: ex.) doing physics simulation on GPUs. 

Shawn: Addressable market also an important part. If there’s a significant set of machines things will run on, instead of checking hundreds of capabilities you can just proceed with a feature level.

CW: That’s for the coarse-grained feature levels. What about things like Binding model tier 1, 2, etc. Does that follow the same process  just at a same level?

MMc: Yea, that’s sort of how it is with Conservative Rasterization. The binding model was a significant one in the development of D3D12. The hardware vendors have significant variation in how they flow parameters. More registers, cache? if you exceed a limit, is it a cliff or smooth degradation, etc. One of the top three design challenges of 12 is a binding model that has easy to predict behavior for developers. Based on what they wanted to do, would they get the expected performance. Did that binding model fully expose the capabilities of the hardware? Also want to make sure we’re not hiding useful hardware vendor differentiation. End of with tiering which lets us target five different hardware vendors across 3-4 generations each. That turned into three binding tiers.

DM: The main takeaway here is that scnario use-cases drove the specification. Do you have an idea of where we would start getting those scenarios to get those tiers?

MMc: Understanding what software ecosystem you’re going after. For DX, we tend to go after the AAA games more for the folks who push the bleeding edge of what’s possible. There’s equally important categories like the creative space like Adobe, as well as simply efficiently rendering text on the GPU. The first thing is understanding the software driving the design. If the answer is everything, it will be hard to create focus. When I first heard about WebGPU, we had Ben Constable. My hope was a commoditization of basic GPU capability over time. Wanted to make sure D3D12 could serve the basic needs of that commoditization. I didn’t necessarily think that it would play in the AAA space. D3D12 had 20 million calls a second. That’s not the starting point for a Web API.

CW: Makes a lot of sense. WebGPU is basically a commoditization of graphics APIs, especially for example: Raytracing! Short answer is that we can’t have it in core API until the native ecosystem matures and coalesces around something standard. Difficulty is that we don’t have a pre-existing relationship with customers as much. And, we can’t drive native API development the same way you suggest features or alignment through GPU vendors. 

MMc: There’s high level alignment of purpose which is interesting. I could put a proxy like: can WebGPU render all of Chromium. But also capable of loading Unity? May not be like native Unity, but is it possible.

DM: We’ll need to talk to people, gather scenarios, and go from there.

**CW: Shader combinatorial explosion problem? In your experience what are useful tools at the API and ecosystem level to help limit that explosion?**

MMc: Don’t think this is a solved problem in the industry. One of the core things necessary for D3D12 to get lower overhead was the PSO. Though, if we do this, the big question was how many PSOs are there? It could have been possible that we wouldn’t have enough memory to store all the pipelines. We evaluated this and saw how many pipelines per frame, how many created total. This was one of the top apps in 2012. It was around 1000 unique pipelines per frame, changing 5-10% per frame. Something like 10,000 unique pipelines. That was a AAA game at the time.

Shawn: A few different problems. Very consistent is that managing shaders is very difficult. And it varies very much between developers. There are some games where there’s 24 hours to compile shaders, or there’s 2GB of shader binaries. A lot of that comes down to design decisions in the beginning of GPU design. There used to be tiny shaders and performance was extremely important -- like expanding out all permutations instead of parameterizing. Some of that is a need for the industry to shift. GPUs can do ifs. Secondary problem is that some workflows given to content creators have led to an explosion. Shaders were tiny and people started treating them as data packed with assets. Whole pipelines were built on this assumption. Now, developers duplicate shaders and just change a couple constants. Little you can do in a runtime or driver model to solve that. The industry has scaled things to a point where it doesn’t make sense. Beyond that, there’s a ton of technical problems. Performance matters and there’s time to load it and pass it to the driver and hardware. But the games have tons of shaders which can lead to hitches. Or really long time to first frame. Unsolved problem and no magic solution.

CW: Is the fact that shaders are a single translation unit and no linking and re-optimization…

Shawn: Maybe. Some things that have been successful: caching shaders. Lots built into the runtime which the drivers all support. Look at a hash and know it’s already compiled. There are some APIs that let apps manage the cache (like clearing for testing). Doesn’t help first run, but does help second and third. But then, every layer needs to cache as well. The second thing which we shipped a little more than a year ago. D3D12 drivers can kick off background threads to recompile shaders with more aggressive optimizations in the background. We want compiles fast, but we want performance faster as well. So progressively optimizing. 

On linking:  seems desirable. Still a research problem. GPUs can’t do function calls like CPUs. a lot of challenges around register occupancy, etc.

DM: Does that apply to splitting per stage, if multiple stages together?

Shawn: Not as bad. Mostly the combinatorics problem. There are some drivers that will remove things from the vertex shader if the pixel shader doesn’t use it.

MMc: Usual problem with cross-stage is that there’s a fixed-function state in between them.

MD: To quickly add: There’s absolutely pressure between how many shaders people want to compile and efficiency. Even the branching problem is a good point. Usually results in higher register pressure. The best way to think about it is Max’s point about what your intent is. Question about how advanced game engines handle this and what their use case is. Other applications might solve it differently with runtime compilation and more flexibility. We have different sets of customers and they want totally different things. Who your customer is is critical.

MMc: In the number of attempts taken to solve the shader combinatorics problem, we’ve had API designs where there’s a flag to ask the driver to compile fast without optimizing, the moment someone misuses it in a game, and then it becomes a valuable title, then that flag suddenly gets ignored. One of the fun parts is that factors like this influence how you change API and system design. How do you make a durable flag? How do we manage the ecosystem such that these flags mean the same thing? If the flag isn’t dependable, then we shouldn’t have done it. There are a lot of parts where because it’s not a stable minima of the design, the API no longer does what we expect.

MM: The idea of kicking off a compile to the background, browsers do this all the time.

MMc: The benefit of the compiler in the browser is that presumably the person building the code base owns it. For us, the hardware vendor is a black box which makes stuff not durable. The thing with the background thing from the OS is more durable so far.

CW: Was looking at things like disableOptimization, or deferCompile in Vulkan. I can see how they’ll be no-ops in the future.

MMc: If there’s nothing else pinning down the behavior, it won’t mean anything.

DM: I think the situation with native APIs is very different from what we have. The user can set some bytes to not bind a resource. In our case, we don’t have GPU-based validation and we have to statically know that a pipeline layout or root signature needs everything bound. Even if there’s a branch, the user needs to still bind everything.

Shawn: One possibility is to just bind dummy resources. That’s a pretty common path. Doesn’t have to be a real resource like a 1x1 texture. Don’t need to combine all the shaders, that’s too much. That would have the worst occupancy. Maybe a good bucketing is ~100 shaders. A lot of the specializations are how many lights are on: 3 or 4 and unroll the loops for that.

CW: We touched a lot about what the API can do to alleviate that. Is there anything at the ecosystem level -- like compiler toolchains that can help with his?

Shawn: It’s a multiprong problem with multiple causes and possible solutions. One thing we’re doing is talking to developers about specializing shaders less, and thinking about them as code not data.

CW: An idea we’ve been toying with is the ability to do linking in the shaders, in the browser. Common lib compiled and then linked against more code with bindings. This would also help payload size which is huge on the web.

MMc: Being able to shine the right amount of daylight in the right direction in a problem space helps. We might not have enough daylight. Developers aren’t seeing what they’re trading off perhaps. We don’t have the best tools for highlighting the creation overhead. I do have a sense that if we illuminate the right things, we have to do less engagement.

Shawn: interesting is payload size, even if the output is just passed to the driver. For a AAA game, they ship everything compiled. Makes sense to me that the distribution is important for the Web.

CW: David Catuhe was talking about shipping Powerpoint using Babylon meant that everything needed to be under 100kb. 

**RC: Robust buffer access. In D3D11 it’s everywhere. When is D3D12 robust/not? (E.g. for raytracing etc, you just hand it an arbitrary virtual address.) \
DP: Security boundaries and process isolation.**

Shawn: D3D12 has a very explicit philosophy of saving nobody ever.

MM: Metal is that way too.

MMc: 11 had a core design point at least for accessing out of bounds, they would all have default values. That came out in D3D10. That was 2003 shipped in 2006. during that time D3D9 was very successful and getting a lot of traction on Windows. While it was great for games, compositing needed a different degree of stability and reliability to get all applications on windows using GPUs. That design point was about forcibly driving up quality and consistency. D3D9 didn’t really have an out of bounds spec. But it was designed starting in 2001. 10 built an awesome hardware spec that had a lot of thought about precision, about what was optional. Detailed conversations with hardware vendors, and all the tradeoffs were made to get high quality software. As Sean mentioned, D3D12 does nothing for you. It’s a processor with an address space, and code accesses it. It’s that simple. The cache management is explicit with barriers. If you get them wrong, you get corrupt data.

Shawn: 9 underspecified everything. 10 and 11 specified everything. 12 retains the rigor if you’re within the lines. Everything outside the lines is undefined. Everything within that process is fair game.

JG: A lot of times we care about things like UndefinedData, or UndefinedYourOwnData.

MMc: Our security boundary is at the process level. To the extent that your data and someone else's data is in the same process, it’s all fair game. The other thing to consider: what are your target application classes for WebGPU? What do you want now, and what do you want in the future? The direction for 12 is that the GPU is more and more capable. If WebGPU wants to be as capable, the security boundary is going to be tough.

CW: We already have a lot of friction between WebGPU and the native API because our barriers have to be absolutely perfect, whereas some IHVs say they ship with incorrect barriers, but it works and eh it’s fine. Right now WebGPU’s feature level is D3D11-ish, except we have command lists and resource binding.. but there’s a lot of interest in this group for bindless for GPU-driven commands, and that will start stinging.

MMc: You may find you need to support process-level isolation -- which is resource intensive for a browser. It’s not just what your target application scenario is.

Shawn: Automatic barriers is feasible for an 11-ish API. The moment you add bindless and multithreaded, it becomes impossible to do automatic barriers correctly.

JD: It may not be out of the question to have a process boundary. If WebGPU 2 has bindless, maybe we will create a new process.

MMc: Also, what are the precise differentiations from WebGL? How is it a leap forward? If that leap forward is to be like Metal or D3D12, then the security boundary is rough. If the difference is just efficient multithreaded rendering, then that’s something different to inform API design. Need to be crisp about target applications.

CW: So D3D12 has no guarantee, not even in vertex fetch?

Shawn: There are some places. But those places have never been viewed as a security boundary. The specific case of an index buffer that goes past the end of a vertex buffer: I believe that depends on how the vertex buffer is bound and what flags are used.

Jesse: For 12, we wanted to make sure previous APIs could be on top of it. So a lot of the things you have in 11 are there in 12 to put it on top. You have a pointer and size, if you get the pointer wrong you’ll read out of bounds.

**MM: What is the state of the art for picking threads-per-threadgroup when shipping an app/game/whatever to unknown devices? Because HLSL bakes this into the shader source, mechanically, how do ISVs set it?**

MM: In HLSL the threads per threadgroup is baked into the shader. We’ve found that it really matters what those numbers are. The correct numbers are different between devices. We’ve also found that getting the numbers wrong is worse than WebGL. 

CW: Also, there was a discussion that you can ask threads per workgroup is 2000, but the compiler can just error. Choosing a number is hard, but also reacting when the driver fails.

Shawn: Lots of game devs tune it for Xbox, ship on PC, and make someone else care.

MMc: People tune for multiple profiles and ship all. Or dynamic profile. One of the historical issues in compute design was this. This particular aspect does not abstract the hardware. Between dynamic profiling and what Shawn said, lots of content ships. Compute is probably waiting on a rethink on our side that has a different way of expressing shaders that doesn’t expose this.

MM: Say someone makes a microbenchmark and runs it on load time. Do they string concat to get the numbers in?

MMc: Some just have a few sets and ship them all.

DM: What to do when the user asks things not supported? For us it’s all async.

Shawn: Don’t think there’s really anything you can do. You can’t really mechanistically transform the code.

CW: Yea, we’ve run into this with subgroups.

**MM: When designing an API-agnostic solution for keeping data on-chip (for tiling devices), is there anything I should keep in mind? **One thing that is important to Apple is we have tiling GPUs and flushes to main memory are slow. Would be cool to keep data on chip. Metal has ImageBlocks. Vulkan has Subpasses. D3D12 doesn’t look like it has something?

MMc: History of tiling on DX. in the mid to late 90s, Imagination had this chip called KYRO. That actually shipped on PCs. D3D was an immediate rendering API and there was a question about retrofitting GPUs for things that did tiling. Discard flags became awesome. Problem is people set them, don’t test on that hardware, things break, so then the flag has to get ignored. When correct, it was good, but it made other apps break. Fast forward through the 2000s, not interesting. Then Metal/Vulkan/D3D12, the high level idea of renderpasses is the better way of doing discard. 12 does have renderpasses, but the PC marketplace doesn’t have a lot of full on explicit tilers. There are some Qualcomm devices, but not a lot. Lots of developers wouldn’t want to change their codebases to support tilers. The high-level idea of (non-sub) renderpasses is what was put into 12. That was the sweet spot. \
How do you do something agnostic? We don’t have explicit tilers, but desktop GPUs still get benefit from cache management. Lots can detect that for a given render target and simple pipelines that don’t have random writes to stuff, the driver can know how to split it up into blocks so everything fits in L2 which gets you 90% of a full on tiler. So:



*   Have a high level idea of passes
*   Use simple pipelines, and the right things happen.

Shawn: Actually in the 2000s we had Windows Phone. And we were doing the discard flags, but don’t do that. It’s not fun. It’s not a hint that benefits hardware that can take advantage. It has to be the mechanism you use.

CW: Like Myles said, luckily what we have now is close to D3D12 or Metal passes, and our discard is a lazy clear so you can’t do it wrong. The question was more with explicit tile management -- like storing to a gbuffer, etc.

Shawn: What we’re looking for is a feature being clearly used on software that really matters. Where we landed is what we thought was the sweet spot.

MMc: we did look at all options, including full subpasses, and it became very clear that 90% of developers didn’t want to touch it. 

**ExecuteIndirect in addition to just dispatch or drawIndirect**

Shawn: Crazy useful. Massive demand by game developers. More demand to go even further. Not loved by driver or tools devs.

MMc: Bleeding edge of GPU work creation which breaks a lot of tools which didn’t handle that.

MM: If we did have something like that, it would probably match what D3D has.

MMc: part of the reason why it’s restrictive is that it was designed in 2013 or 14, and it had to work on GPUs that didn’t really have programmable frontend command processors. The implementation had to work where the GPU would run as unprivileged shader to translate the fixed layout buffer into the hardware-specific commands, and then flush the command processor cache. Designed for GPUs that didn’t have anything like ExecuteIndirect. Most GPUs aren’t that restricted and we can do something more flexible.

KN: Vulkan essentially has nothing. Nvidia has an experimental Vulkan extension, but Vulkan doesn’t have anything else. Don’t think we would be able to use an experimental extension in WebGPU.

Shawn: FWIW, can see it as an area of most growth. Lots of games are just Dispatch and ExecuteIndirect.

MMc: Again, know your target audience.

JG: Crazy hypothetical question: how crazy would it be to run a compute shader to clip offsets on indirect calls?

MMc: Hard to predict because it depends on workload so much. I would say that the initial impl of ExecuteIndirect which had to work by running a compute shader ahead of it, the drivers did crazy stuff by pre-patching things so they didn’t need to flush the command flush. If they couldn’t it would be a full GPU stall. I don’t know if this is true, but what you're saying sounds like you could be causing big end2end stalls.

**JG: Removing vertex pushing**

JG: That’s great. Follow up: Other crazy idea was a PR to remove vertex input and do pure vertex pulling.

MMc: You should talk to your tiled renderer friends. We call that like mesh-shaders (ish). Some of the tiled renderers don’t like vertex pulling. They like knowing ahead of time. I don’t know if this is true of Apple’s hardware. I think ARM and Imagination told us they like the vertex pipeline.

**Programmable blending, interlock, and friends**

CW: Is there demand?

Shawn: Yes. Between software and hardware people. Lots of creative apps like this. If you switch to the hardware perspective, there’s tremendous perf to be had by not doing this. Long term, there’s a trend that rasterization becomes less and less important. In games, compute is growing and rasterization is shrinking. The interest in adding new things for rasterization is shrinking and the solution is to write to UAVs.

CW: On mobile we can just do everything in the tile?

MMc: Again, though what’s the target? If you want to render Chromium, then rasterization still important.

Shawn: Filters are the easy case, because even if it’s complex blend, it’s a single layer. The complexity is when you have depth buffers involved, etc.

**Sparse resources**

limited adoption to date. Cool for big worlds. AAA games and upcoming consoles

**VRR**

totally useful. Lots of perf

JG: it seems like you can get some of that with multi-view style extensions. You can build your own Variable shading on top of that. Dumb idea? not as fast?

MMc: I think the answer is Yes, you should have it. It is a formulation of that class of perf gain that is most flexibly mappable to any type of engine.

Shawn: With multiview, don’t see how that would give you the same flexibility. Have a composition problem with depth buffer being a different res. VRS makes it very flexible and robust. Easy to integrate as well.

MM: Isn’t it hard to figure out which part of the screen to render at which rate?

Shawn: Depends, Civilization was using the previous frame as feedback and did a sobel filter to determine the rate.

**Tessellation**

Really cool, not a lot of adoption. Mesh shaders better

**Bindless**

future evolution of the pipeline

**uniform texel buffer, storage texel buffer**

not sure

**creating texture with clear color value**

don’t do it. regret it :-/

**occlusion queries**

used and useful.  \
Used same frame as a pipeline thing?

Yes. Object culling. Draw a world with bounding boxes, then redraw. Same frame with predication on the command list. No readback.

Use case other than that?

Have used them for GPU hit testing in editors.

DP: Offline occlusion baking.

God rays. Needs non-binary (e.g. fades out behind foliage). Class of similar effects. Somewhat dated technique. Now can use visibility masks but it’s very expensive.

**pipeline statistics queries**

CW: Two use cases: in production and during development. Is it used in prod?

Shawn: not heard of retail uses. Games will have an overlay in development. Reading that back is neat

**geometry shaders**

like tessellation shaders but older. Mesh shaders just much better.

But you have to worry about the years of hardware without it.

CW: Yea we’ve had ideas of emulating mesh and task on compute shaders, but..

MMc: Bunch of people skipped tesc and geo and used compute instead. 

Shawn: Mesh shaders are a hardware impl of a compute shader paper from siggraph.

JD: So mesh shaders are the future?

Shawn: Yes, even on tilers.

**Raytracing**

Shawn: Target market again!

Multiple AAA titles using it.

Not primary rays. Soft shadows, etc. Used for offline baking GI.

Lots also doing machine learning models for reconstruction.

This caused us to introduce bindless textures into D3D. Truly dynamic selection.

**Async Compute**

Shawn: Every AAA game. Outside of that, maybe some ML stuff. Anything derived from Unity or Unreal. Probably Lumberyard.

MMc: Gets related to high priority compute. For VR we can for what’s important, etc.

**RC: Multi-GPU**

RC: What advice for handling devices with multiple GPUs? One way is to move everything to the high power, another way is to render on high power and composite on low. You’re going to say it depends. What does it depend on?

MMc: Depends how informed the developer is.

Shawn: Regardless of how informed, they can’t possibly know the configuration they’re running on.

JG: Difference is mostly power usage.

Shawn: really the question you want to ask, which there isn’t a good way to ask, is you want to run a workload meeting perf requirements with the least power possible.

MMc: I think the high/low power is not quite right. should be low-power and high-performance. Hard for us as well. We build expectations into the API that integrated is lower power but slower than discrete. And external is faster than both and no one cares about power. Dev might have a sense that they care most about video and want low power. If you have a web version of a game, you probably want to run high performance for UI responsiveness.

JG: We’re going to get content that wants to be on the high-performance GPU and be as fast as possible. Should we shut down everything and transition the compositor to that adapter?

Shawn: At some point, content has to transition because it has to be on the adapter driving the display. Someone has to move it there.

CW: Direct Composition is able to show content from the GPU not driving the display.

MMc: Yes, but there’s a copy. If you leave everything on the same GPU, I wouldn’t even offer the choice. If you do give the choice, you’ll have to think about what the flag means. We did a demo of split-frame rendering in Unreal. We split the graph between the scene render and the post process. That got 10-20% frame rate improvement because the NVIDIA GPU on the surface book is that much faster. Unrealistic on web to do a split frame render. If you do expose low-power or high-performance, I think the intent would be to move everything across except the final result.

JG: In the end, we’re giving them to choice to pick the high performance mode, and we’ll figure out what we need to move over.

MMc: I suspect it’ll be hard to reason about unless you can decide for a given region on screen you have a cost metric for. My gut instinct is that you should move everything to one side or the other. It’ll be hard to get something that’s easy for web devs to reason about. If you can’t set clear expectations, it’s probably not stable enough to target.

RC: Should we use EnumAdaptersByPowerPreference?

MMc: If you expose this, I would use the underlying APIs.

Shawn: Interesting nuance that the regular EnumAdapters gives you what the whole system has decided for you. Can change based on system policy or user preference.

**Other regrets?**

depthBias in PSO. Was put there for some old hardware.


## buffer.writeSubData [#509](https://github.com/gpuweb/gpuweb/pull/509)

DM: Uploading data is the most complained-about feature in wgpu. People are confused. When I say we don’t have we don’t have a good story, just create a new buffer, they are concerned about performance. Today in WebGPU you have two options: either you’re lazy and create a buffer every time, or you have a complicated machinery to recycle buffers. Neither is good for demonstrating how to use the API. And the complex machinery doesn’t even provide the most efficient way.

MM: We should have it.

DM: There were concerns that this could be implemented on top of buffer recycling.

JG: The main thing is I don’t want an API where we have to get all of our optimizations right. I want users to set themselves up for success instead of relying on us to win for them.

CW: Ties into generateMipmap. Would be nice if we had a set of criteria to choose for what “nice-to-have” helpers we should/shouldn’t have in the API that developers could do themselves. I tentatively think that would be: we can do it for little cost, and there’s only one way to do it correctly. I think this falls into that.

JG: Previously, there was concern about having to track whether buffers are immediately available for mapping on the client side. That’s incompatible with implementing writeSubData. So if there’s a strong desire to avoid that, …. there’s going to be extreme market pressure to optimize the heck out of this.

MM: I think there’s a counterpoint: I expect there will be a lot of content that gets buffer uploads wrong, and then we’ll have to optimize that content. \
JG: I think there are extant proposals that make it harder to mess up.

  [https://hackmd.io/d6sXDReIQE-9njvHQdPUTA](https://hackmd.io/d6sXDReIQE-9njvHQdPUTA) 

JG: Not willing to continue championing this position.

...

MM: … Happy to add the boolean to the buffer for that tracking.

CW: Complex cross-process.

MM: I see. Extra time when the client can’t know the buffer is available?

…

CW: If you want to be able to do the correct copy in writeSubData, the client needs to know from the GPU process when the buffer is no longer in use. But it also needs duplicate/parallel state tracking on the client to immediately mark when they _become_ in-use.

CW: So the most efficient path for writeSubData forces a lot of tracking on you that you don’t want to do.

JG: Most of these proposals are not impossible, just have tradeoffs. We should pick the one where we can address the tradeoffs most effectively.

CW: We’ve spent a huge amount of person-time trying to design in this space. There is just no good solution. There are only bad tradeoffs.

JG: Want to acknowledge the issues with writeSubData. Chromium will be able to optimize this and everyone else will be forced to.

RC/MM: Want to give Jeff’s document another read.

MM: I think in 2 weeks we can have a constructive debate.

DJ: In general, if Jeff or anyone has made a proposal and it hasn’t been sufficiently reviewed, we need to be responsible to listen to that and review it.

CW: When we get back to this, how about we focus the conversation on buffers and not textures?

DM: But won’t we be missing important data about handling textures?

CW: Aren’t they similar? We can add the complexity if you want. But my feeling is that all of these translate directly to textures.

DM: OK.

DJ: Your hackmd doesn’t mention #509.

MM: That’s basically queue.setSubData.

MM: JG’s point to focus not on the problems, but on how we deal with the problems, is important.

**homework: review [https://hackmd.io/d6sXDReIQE-9njvHQdPUTA](https://hackmd.io/d6sXDReIQE-9njvHQdPUTA) **


## Storage textures ([#513](https://github.com/gpuweb/gpuweb/pull/513), [#541](https://github.com/gpuweb/gpuweb/pull/541), [#532](https://github.com/gpuweb/gpuweb/pull/532), [#542](https://github.com/gpuweb/gpuweb/pull/542))

DM: We have a good investigation by Intel that shows that support for storage textures to be readable/writable in the same shader is much more limited than readonly or writeonly. Currently the api only has storage usage. The things we want to support are: stating in the API that you’re only using something for reading (so you can combine it with other readonly usages). We want to report early if a shader tries to readwrite a format it can’t. Not at draw time.

DM: If we have separate usages for readonly, writeonly, and readwrite, we could validate this ahead. Problem is that for other places, just “storage” is good enough.

DM: The other spot we can do this is at bind group creation. We know the layout and the texture view.

DM: Finally, if we don’t do that, we can only do it at the draw call.

DM: In the shaders, there are access qualifiers for readonly, writeonly, readwrite. We can detect that at pipeline creation time.

DM: So, this is the investigation part. Having reviewed all of this, I propose: a single “storage” usage for resource creation, but an extended storage semantic for the bind group layout. A setting per binding in the bind group layout. Since in the proposal we only have one “storage” usage, in D3D12 we would have to create both SRV and UAV, which is the compromise. I think we can pay that.

CW: This would mean creating storage textures with D3D12_RESOURCE_FLAG_ALLOW_UNORDERED_ACCESS always even if they’re only ever used as readonly.

RC: Why is this necessary if you have additional flags?

CW: The storage itself must be created with the flag. This could be skipped if it were only ever used as readonly.

RC: But we have sampled.

CW: Can’t be used as readonly storage.

RC: Why not?

CW: Vulkanism. Texture cache vs data cache in some hardware. So in Vulkan not allowed to use sampled texture for getTexelPtr, only through texelFetch. So you can’t do a load, only a fetch.

RC: So if you had STORAGE_READ (#532) then you wouldn’t have to create the UAV.

DM: Yes. But this isn’t my proposal.

CW: readwrite is not supported on some of our target iOS hardware.

YH: It is an optional feature.

CW: OK. We add two binding types for textures. writeonly and readonly. 

DM: For buffers?

CW: It’s ok because the storage of readonly isn’t more optimized than readwrite.

YH: To clarify, your suggestion is to and readonly writeonly stuff to binding type, not texture usage …

CW: Usage would be only STORAGE (no STORAGE_READ). Binding types would be writeonly and readonly.

YH: We would still use the possibly-unused UAV flag.

DP: Should be OK. Think it would be very unusual for it to be unused anyway. Usually you use a storage texture because you want to write to it.

YH: What about readonly bindings with UAV flag set for ML operators, say we need to read data from a readonly texture/image from random location? Not quite sure whether we have such cases though

CW: Can use texelFetch on SAMPLED.

RC: Going to have an extension with readwrite storage texture binding type?

CW: Yes.

DM: What do people do with it?

DP: Can implement (something about) blending.

MM: Isn’t UAV unordered?

RC: Pretty sure it’s ok within one dispatch/draw, just need UAV barriers between dispatches/draw.

DP: What’s the target that doesn’t have it?

MM: Some metal. [KN: see readWriteTextureSupport]

MM: … It will analyze at pipeline creation time. Not at shader compile time.

CW: to conclude, readonly and writeonly binding type, and texture format in bindings. 


## Multi-queue

JG: No concrete proposal.

CW: In metal you can create as many queues as you want and use them in parallel. But if you happen to be race on the same resource it races.

CW: More recently, Metal added ways to synchronize between queues: MTLFence and MTLEvent (64 bit counter you can signal/wait). If you use this you can protect yourself from races. Couldn’t find anything about reading from multiple queues.

MM: Think it’s ok, will get back to you.

DM: In particular storage/sampled changes the layout.

CW: Also there is sample

DM: Argument buffers require you to do so also. 

CW: Is useResource a write usage since it might decompress clear planes etc.

MM: Inside useResource there is read, write, and sample.

DM: That’s only on newer Metal?

MM: Docs say no `useResource` that doesn’t take args

CW: Move on because Metal was the simplest. Can create a timeline using u64, signal it, wait on it, valid to read multiple at once, but undefined mutual write/read.

CW: MTLEvent vs MTLFence, former is lighter and inside metal only. Fence can be used to sync with outside of metal.

CW: D3D is somewhat similar. Fence object is u64, queue that signals (not encoder) and also the CPU. (it’s magic) Read/write behavior the same as metal.

CW: Three queues: graphics > compute > copy.

DP: Resource states are different for different queue types.

CW: There are a few different things that require knowing which queue *type* they’re created on. Command lists. Not resources (unlike vulkan).

CW: Synchronization. Resource states are same between graphics and compute. Copy queue is different (because it’s using a small subset of the hardware). So some complexity for memory barriers. Copy source/destination state … are not the same.

CW: If you do nothing, you can use a resource in multiple queues provided it’s in a state they all understand. Read-only. If you want to mix read/write, you can add a resource flag. But WebGPU  will probably not have this to start.

Each subresource is in a specific state, one usage at a time or “common”. 

Copy queue doesn’t have texture units and can’t handle any kind of compression planes.

DP: If you transition to a direct queue to COPY_SRC, it’s not really COPY_SRC on a copy queue. “Actual” usage is dependent on the queue type that the transition is from.

CW: Vulkan is similar-ish. Graphics/Compute/Copy(Blit) split again, have to be created at device creation. Queue family important when creating a resource, and command pool. (which queue family commands will be enqueued on)

By default a resource is allowed to be on a single queue only. You have to use pipeline barriers to transfer the ownership (Queue Ownership Transfer). There’s another mode: concurrent, can be used  by all queues at the same time, almost  exactly the same as d3d12 except weirdness with copy engines.

When you have a concurrent resource, e.g.: You do a barrier on queue A to make the writes available for sampling…. (something about queue B).

JG: My reading is that it’s specific to the queue family not the queue. Concurrent is just for concurrent access from different *families*. … In some ways it feels like vulkan queue families are similar to queues in other APIs. In other APIs it’s as if there’s only one of each family. But in Vulkan you can have many of each family.

CW: In exclusive access, the barriers are only between a single family, however if you want to switch to another family you do a Queue Family Ownership Transfer.

CW &lt;draws on the whiteboard>

For QFOT, you need to initiate the transfer on the source family, handle synchronization between the two queue (families), then finalize the transfer on the destination family.

JG: Only have to do a transfer if you want to preserve the data. If you want uninitialized data, the destination queue family can just forcibly take the resource.

MM: I figured out the answer about fences. MTLFences are not strong enough to put dependencies between queues. MTLQueues represent multiple hardware queues. This doesn’t work if resources are aliased. So you use MTLFences to do what the metal runtime would have done for you. MTLFences can’t go across queues. MTLEvents can go across queues.

CW: MTLSharedEvent…

MM: Then the CPU can also use it.

CW: So the D3D12 fence?

MM: I think so.

CW: Synchronization between queues in Vulkan uses semaphores (Q2Q). They encode some memory barrier at the same time I think. Vulkan recently added timeline semaphores which are the same as D3D12Fence or MTLSharedEvent.

MM: So we can’t rely on it?

CW: It’s polyfillable on other stuff.

MM/CW: weird.

CW: My suggestion is have 64 bit counters ... ...

MM: Warnings all over docs to not use MTLSharedEvent if you can avoid it. So would be nice if we can have two options.

CW: OK. Suggest having 64 bit counters, and requiring a value to be signalled (on the client side) before a wait can be enqueued. Which makes it impossible to wait on GPU and signal on CPU.

MM: …

CW: cpu waitSignal returns a promise so it’s ok if it never comes. But if a gpu waitSignal never receives a signal then bad things will happen.

DP: In which APIs? In d3d12, the queue will stop making forward progress, but it won’t hang/TDR.

CW: In Vulkan, if you wait on gpu + signal on cpu, the gpu can decide to not wait for the signal and keep going anyway.

> **6.5. Events.** A device can wait for an event to become signaled before executing further operations.

MM: So it sounds like we can’t include it in the API.

CW: We could virtualize the wait. Don’t even submit the commands until the signal. But would be expensive to hold onto those.

MM: Right. An app makes a simple mistake and it enqueues forever.

CW: Yes. Although you can also not signal properly and the same queueup happens in the GPU/driver instead.

MM: Sounds like we can start with GPU->GPU as it’s the most necessary.

CW: OK. 64-bit value seems obvious. Requiring DAGs seems nice (forbid deadlocks).

On vulkan there will be some fun to use semaphores to do this. But if timeline semaphores can be polyfilled then we should be able to do it.

CW: something about marking them for compute-only.

MM: Don’t want to be a jerk, but would like to know the overhead of saying every resource is for every queue.

CW: On desktop, compute+graphics is probably same as graphics. Compute+graphics+copy is a problem because resource will be decompressed always.

MM: Would like to see numbers though because Metal acts as if this is OK and it gets pretty good performance.

RC: It says here “use of these flags can compromise … prevents compression”

MM: Just would love to see that in number form.

DM: We can reason about how efficient resource compression is without numbers.

CW: Can at least set a default so people who don’t care don’t need to worry about it.

DP: I will try to find more information.

CW: for queue transitions, can do it implicitly or explicitly.

CW: Also command buffers need the queue type so maybe we should move createCommandEncoder to the GPUQueue object.

DM: Don’t expose families, right?

CW: Haven’t said anything about querying the queues on the system and …. My 2 cents is just say what type of queue something goes on.

DM: Need to …

JG: From the Vulkan side, one of the possibilities is to be able to eagerly indicate that you’re indicating to transfer between queues. So if you had exclusive resources and you wanted to transfer them between queues, in the naive case, we would only recognize when you submit using a resource to a different queue, which means going back to the source queue and inserting an implicit release. Not great because that’s potentially already busy doing something.

So we could have an optional queue transfer “hint” on the source queue (and warn if hint wrong). Gives us an opportunity to eagerly migrate between queues.

DM: Think it’s a good idea to consider.

CW: Think it would almost work but only concerned possibly about the readonly usages (sampled + storage).

CW: So that was an overview. Need to write it down and make proposals.

CW: I will work on the detailed buffer mapping design investigation first. Then this.

JG: Within about a month.

CW: with some missing bits.


## Agenda for next meeting 2/24



*   Query API
*   Buffer mapping (postpone until Corentin is available?)
*   Shading language
    *   Language name
    *   Meeting times
    *   Meeting attendees
    *   Meeting frequency
    *   Editors