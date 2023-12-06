# WGSL 2023-12-05 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (**Pacific-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-11-28 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1mTo5eZEuEWsYiVdmzqRO5CV3I1JQ_wG1H_loaZv8ORs) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Mike Wyrzykowski **
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * **Hao Li**
    * Jia A Chen
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
* **Dominic Cerisano**
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb


---


# 📢 Announcements/Meta


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* 


---


# ⏳ Timeboxes (until XX:30)


## Google’s requests for issue milestone changes



* [https://docs.google.com/spreadsheets/d/1bBASnGyBzktAuIKaWHYpVp5CT_Ln67jVkQTkP9AP2Bw/edit#gid=391469536](https://docs.google.com/spreadsheets/d/1bBASnGyBzktAuIKaWHYpVp5CT_Ln67jVkQTkP9AP2Bw/edit#gid=391469536) 


### rcp, rsqrt, fast reciprocal (couple this to relaxed conversions?) [#4902](https://github.com/gpuweb/gpuweb/issues/4092)



* AB: We’ve had requests for some of this stuff on Mac, but we likely want this to be more general than doing it piecemeal function-by-shortened-mangled-function.
* JB: Distinction between “make this accurate vs fast” should be done by knobs not by funky names. Mozilla doesn’t have an urgent need for these.Happy to push these to M3.
* MW: Agree.
* _Agree to **M3+**_


### Early Depth/Stencil test directive or flag in pipeline state creation [#398](https://github.com/gpuweb/gpuweb/issues/398)



* AB: Do others want to champion this?  Touched on it last time.  If it’s flag int he pipeline state then we are happy to drop tracking it here. It’s here because sometimes it’s a an attribute in the shader.  Not the top of our list.
* JB: We don't have partners or oss users for this.
* _Agree: push to **M3+**_


### arrays of textures, arrays of buffers [#822](https://github.com/gpuweb/gpuweb/issues/822)



* DN: Think it’s important gap in functionality now. Coupled to sketching design of array of buffer bindings. Have design issues with uniformity of indexing, but let’s keep that to design.
* AB: We also think it’s important to do this at the same time as #2105.
* JB: Mozilla was expecting to discuss this early in 2024.  Either M1 or M2 is fine with us.
* **->M1**


### sketch design array of buffer bindings [#2105](https://github.com/gpuweb/gpuweb/issues/2105)



* **->M1**


### add texel buffers [#162](https://github.com/gpuweb/gpuweb/issues/162)



* AB: We have partner requests for this. It comes from various places. Fills gap on older hardware that doesn't have smaller data type support. Very common in older D3D to get read write storage of f16, say, without needing full f16 type support. Widely used.
* DN: this lets you treat GPUBuffer memory as a big array of textures. Does not have coordinate size limitations. But use the texel format conversion hardware on the data path into/out of the shader.
* JB: Learning about this feature. Don’t express an opinion.
* AB: Certain use cases can be satisfied with f16. There are other use cases to help porting existing code. There’s also a reach aspect. E.g. D3D doesn’t have f16 until a later SM 6.x but prior APIs have texel buffers which is how you end up covering for it.
* JB: Sounds like some folks really need it but many don’t care.
* KG: Estimates on how much work this is to add?
* DN: Mostly just plumbing, so should be easy enough.
* JB: Ok! Mozilla is neutral. M1 is tolerable but not preferred.
* **->M1**


### add subgroups, portable if possible [#4306](https://github.com/gpuweb/gpuweb/issues/4306)



* AB: The M1 part we want is setting a basic direction.
* JB: Let’s talk about that int he second half of the meeting.


### textureQueryLod [#4180](https://github.com/gpuweb/gpuweb/issues/4180)



* AB: Think we just missed this. Should be easy.
* **→ M1**


### struct methods or namespaces [#4286](https://github.com/gpuweb/gpuweb/issues/4286)



* DN: I think this will be a lot of work. Also, GLSL got away without this :P 
* JB: Sounds right.
* MOD: 👍
* **->M3**


### mutable function parameters with the mut keyword [#4113](https://github.com/gpuweb/gpuweb/issues/4113)



* DN: We don’t need them, we shipped without it. It’s syntactic sugar, a var by another name. Not all that urgent.
* **->M3**


### floored or euclidean modulo [#3987](https://github.com/gpuweb/gpuweb/issues/3987)



* DN: Don’t need it. People complain, but there’s not compelling evidence from other languages, so urgency seems low. For me, in bucket like with tau and pi.
* JB: +1 to “there is no consensus [elsewhere]”
* **->M3**


### user-defined namespace [#777](https://github.com/gpuweb/gpuweb/issues/777)



* DN: Same discussion as #4286.
* JB: We prefer M3 also
* **->M3**

_(These next ones are currently untriaged, and have no milestone yet.)_


### Extensible accessors and function [#4337](https://github.com/gpuweb/gpuweb/issues/4337)



* AB: similar to me to struct accessors.
* MOD: +1
* **→ M3**


### matrix identity    [https://github.com/gpuweb/gpuweb/issues/4026](https://github.com/gpuweb/gpuweb/issues/4026) 



* KG: **M2** sounds right.


### image atomics    [https://github.com/gpuweb/gpuweb/issues/4329](https://github.com/gpuweb/gpuweb/issues/4329) 



* AB: getting some outside support on the issue.
* AB: I’ve heard it described as e.g. Nanite/Unreal5 use these to do software [parts?].
* KG: **M2 **then?
* JB: +1


### operator overloading    [https://github.com/gpuweb/gpuweb/issues/4357](https://github.com/gpuweb/gpuweb/issues/4357) 



* JB: **M3 **sounds great.


### vertex storage writes    [https://github.com/gpuweb/gpuweb/issues/4355](https://github.com/gpuweb/gpuweb/issues/4355) 



* 


### 8-bit integers (missing on Windows)    



* JB: I kinda wanted this recently. But I suppose An Interested Party should file an issue, since this doesn’t have one yet.


### 16-bit integers    



* (no issue yet)


### named swizzles    [https://github.com/gpuweb/gpuweb/issues/4333](https://github.com/gpuweb/gpuweb/issues/4333) 



* JB: I know Gregg writes a ton of shaders, so I give this request more weight than otherwise.
* AB: Yes, but this feels like operator overloading, in that it sounds useful, but probably tricky to choose.
* DN: I’m having some trouble reading from the examples what the proposed syntax would be like.
* BC: Having written lots of shaders, the thing I realized what I wanted was operator overloading. To declare structures with named fields but use them with vectors. We could ask Gregg whether the request falls into operator overloading. Think it’s a more general solution to the problem.
* KG: Depends how big that tent is.
* AB: Yeah if you can define it as [something], then it hits all the builtins as well.
* KG: **->M3, but more clarification from Gregg would be great.**
* JB: I think Gregg says that this is just him thinking out loud, so we shouldn’t over-rotate on this.
* DG: For the proposal, what’s the difference between this [and structs], since this should probably all be scalarized on the backend anyway. Is there an advantage to this? Maybe inheriting a struct from e.g. a vec3?
* AB: Think the advantage is that many builtins are now accessible as vec3f. And there’s packing rules for it.
* JB: It’s maybe a bit tricky, because there are builtins that take a T and return a T also.
* AB: I would be more comfortable to have a constructor where you’d name the output[?]. I don’t know how we’d write the spec for this, but we don’t have to anyway.
* JB: “higher-order constructor”? A la from e.g. Haskell?
* DG: How much value though from a new type of vector constructor, that is a different type?
* AB: I think by default we, scalars wouldn’t work by default, but… I do feel like operator overloading will solve much of this.


### relaxed conversions (want perf example feedback)    [https://github.com/gpuweb/gpuweb/issues/4280](https://github.com/gpuweb/gpuweb/issues/4280) 



* JB: It seems like relaxed conversions are in the same bucket as relaxed function operations, and that was M3+, so probably this too.
* **->M3**


### reliable support inf, nan, signed zeros, (maybe subnormals)



* JB: I’m surprised this is ranked so low.
* AB: We think it needs support/guarantees that we don’t realistically have now/yet.
* JB: But people use do Compute, right?
* DN: Likely e.g. “works on NV” attitude, OpenCL has this, but graphics APIs mostly don’t have great support, and users don’t care. Also e.g. LLMs need like… 3-bit math.
* JB: Sounds like we’re basically waiting along with anyone else who cares, if it ever does show up.
* **->M3**


---


# ⚖️ Discussions


### [Subgroups Proposal - Pull #4368](https://github.com/gpuweb/gpuweb/pull/4368)



* JB: Teo went through the proposal. Mozilla’s feeling is there’s no plausible alternative to this functionality.  This lets users take advantage of near-universal support of this in hardware. We think we should land this as a proposal doc.
* AB: I’ve landed some of the smaller of Teo’s changes, and will look up the device loss reports from the gpuinfo.org queries.
* AB: Very interested in the cascade of devices that get lost as you add features.  Had a question about the f16 one; doesn’t correspond to a Vulkan feature-bit.  Will need to followup. Some question about the report showing 4% loss if you couple subgroups-f16 with subgroups.  Want to follow up.
* AB: Overall would be happy to have fewer splits of the feature if the data bears it out.
* AB: Internally we discussed how to handle portability. E.g. possibly avoiding indeterminate value when you get data from an inactive invocation (e.g. for shuffles). Could look at polyfilling as the default, and then opt out if you want the speed but not the portability.
* JB: Definitely like the portable unless you ask for something else.
* MOD: Will some of these non-portable behaviors be security hazards? Or is it unlikely.
* AB: Think it is not a security hazard. All the invocations will be from the same end-user application (graphics context at the API level). 
* AB: Vulkan is weird in that it can pack multiple draws into a single subgroup, but don’t know how many devices actually do that.
* _(JB: What will hardware people think of next?? :P )_
* _(KG: Mesh shaders, yeah? :P )_
* JB: I was surprised that Mesh shaders didn’t come up from [Google] partner requests. Is there no asking here?
* AB: It’s large to spec. There’s a concern of whether the mesh features in underlying APIs are actually good across HW classes in the long term.
* JB: Yeah, I guess it’s a lot to spec. Lots of iteration vs earlier geom/tess approaches
* AB: for subgroups, I’ll add the diagnostic change. Would love to merge the proposal, and iterate from there, even without a firm ETA for merging into main spec.
* **KG: Sounds like consensus to merge proposal after some (but not perfectionist!) revisioning.**


---


# 📆 Next Meeting Agenda Requests



* Next meeting cancelled: (**Atlantic-timed**) Tuesday December 12, 2023, **11am-noon **Americas/Los_Angeles
* _(next pacific-timed, from JS)_ [HW Clip Planes support · Issue #390](https://github.com/gpuweb/gpuweb/issues/390) 
* AB: Probably not on the 19th. Lots of vacations started then.
* KG: **Let’s adjourn for the year, no more meetings. See you all in 2024!**