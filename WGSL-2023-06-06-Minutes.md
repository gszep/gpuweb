# WGSL 2023-06-06 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (**Pacific-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-05-30 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1HDGzKVB8BsXf3MIuEZvokBRKCzY4islkOV31Il24re0) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
    * **Myles C. Maxfield**
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
    * Hao Li
    * Jia A Chen
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * **Shaobo Yan**
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
    * **Rafael Cintron**
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
* Dominic Cerisano
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb


---


# 📢 Announcements


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* [https://github.com/gpuweb/gpuweb/issues/4145](https://github.com/gpuweb/gpuweb/issues/4145) Reorganized sections


---


# ⏳ Timeboxes (until XX:15)


### Process: staging milestone 2 features



* Example: We’ve agreed to do [dp4](https://github.com/gpuweb/gpuweb/pull/2738), when should it be released?
* If features are frozen for some time, editors will come up with a process for staging features
* AB: Key question is are holding off landing things?
* KG: **Third option is to not talk about things for a while, and not have a staging branch**. Don’t want to specify things too far out in front of ourselves. Don’t have implementation and get feedback.  It’s takes time away from current engineering effort. Think we should triage into buckets.  Think we should hold off new development.
* AB: Want to avoid having PRs languish and bit rot over time. Example is dp4 that has a PR.
* KG: More important to do due diligence than to land things.  Don’t think we’ll have enough reason to spin up a staging branch.
* JB: Agree with Kelsey. Let’s focus on implementation and reduce activity.
* JB: In the specific case of dp4, is that something we agreed to merge already?  If we already agreed and did a PR.
* AB: We agreed to land it “post-v1”. 
* KG: In this case someone has done the work out in front of the committee. I don’t want to optimize for this scenario.  Think we should leave it as-is. Folks want to do fixups at the same time; and that unfortunately works against PRs that are already in flight.


### [WGSL: Add dot4U8Packed and dot4I8Packed as new built-in functions](https://github.com/gpuweb/gpuweb/pull/2738)



* Updated to include pack and unpack functions
* KG: **This falls into future work. Defer for now**.


### [Clarify behavior of (norm) packing built-in functions · Issue #4159](https://github.com/gpuweb/gpuweb/issues/4159)



* DN: As with all rounding for FP, it can round up or down. **Will look at making that optionality clearer in the spec. And CTS is written to allow either behaviour**.


### [Add built-in struct types for indirect calls · Issue #4158](https://github.com/gpuweb/gpuweb/issues/4158)



* KG: An option for convenience. Let’s give it to them.
* AB: When?
* JB: This seems easy and quick. I’m for landing it.
* AB: Think this is cruft.  Don’t think every API you add should get a named struct. Might want to have it in a buffer, and then it’s awkward.
* JB: Improves readability too.
* BC: 1. This would be the first time we have a built-in structure type that you can spell. 2. Last structure there has workgroup count as 3 scalars. Seems should be vector.
* _Discussion: think it should be scalars._
* KG: Don’t like the member names for workgroup one.  (3rd one).
* KG: Do want to use the other ones.
* MM: Hoist name of “WorkgroupCount” to the structure name.
* KG: **Think we can agree on the first two**. **Bikeshed naming of the last one in separate issue/pr.**.


### [Recursion in WGSL, like in CUDA and GLSL (Vulkan extension)? #4167](https://github.com/gpuweb/gpuweb/issues/4167)



* KG: Corentin notes this is not supported in some required targeted languages HLSL and SPIR-V.
* MM: MSL allows it with a hardware opt in.
* AB: There is no Vulkan extension for this.
* MM: Ok, so **can’t do it. [for now]**


### [Support for device pointers #4168](https://github.com/gpuweb/gpuweb/issues/4168)



* KG: The request is actually something like Cuda malloc.  Ability to “allocate” memory and access via pointer in shader. I can imagine ways to do this.
* AB: Sounds like “Buffer device address” in Vulkan.  You do a buffer allocation and can get a 64bit address for it, and pass that into the shader.  It’s somewhat supported and is popular among programmers.
* MM: Thought CUDA malloc was different. The malloc is an allocation and return a pointer.
* AB: Vulkan breaks it into two parts. Do the alloc, and then get the device address for this.  
* AB: Not exist in all 3 APIs yet.
* MM: **Person doesn’t list their use case yet, but maybe we could address the underlying use case if we had more info**.


---


# ⚖️ Discussions


### [Add dereference operator · Issue #4114](https://github.com/gpuweb/gpuweb/issues/4114)



* AB: Still like David’s old proposal.
* DN: That has perfect duality between references and pointers w.r.t.    .member and [index].
* AB: Think would be more useful conversation for when we have more functionality associated with pointers.
* KG: Think there is utility now.
* AB: Saving 2 chars is not enough of a win.
* MM: Could have two options.    ptr->member yields a reference, and  also take David’s thing  where ptr.member yields pointer.
* KG: Think I don’t like on ptr.member yielding pointers; sounds cool on the surface. But other languages don’t do this.  This is “novel”; don’t want to spend novelty points.
* JB: Think it would be very surprising if . yields a pointer.  Other languages don’t do that.
* MM: In C,  could overload + on pointer, so   ptr +  fieldname yields the pointer to the field
* JB: What about +.    => you could do ptr +. fieldname and that would be a pointer to the field
* DN: Please don’t bring back historical mistakes into WGSL
* KG: Personally, think we should follow other languages.  .member should yield a reference.
* MM: Intentionally not like C
* KG: It’s like Go and Rust.
* MM: Why not spell it as ->
* KG: Newer languages  -> is longer and more clunky.  Just use dot. It’s the thing you most often want.
* KG: If you have pointer-to-pointer of thing (in future),  -> only dereferences once.   
* _Discussion about what to write to dereference via a stored pointer._
* BC: Recall reading in early days of Go, they didn’t want to add another token to the language. There’s nothing you can do to pointers aside from dereferencing them.  The spec does say it’s syntactic sugar for (*p).member, just one level.  I stated on the issue re: not using ->, my feeling is why -> when you have exactly the same behaviour as with a pointer.  With var “A”,  A.b.c are all references; if you have a pointer at the start, it yields also a reference    p.b.c yields a reference.
* MM: We should match some existing language. Let’s not mix different pieces from different languages.
* JB: **Would like to see text to look at**.  As experienced programmers we would have some visceral reactions that may guide the conversation. David added an example already.  Kelsey had something with deeply nested structs.


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* KG: I’m ¾ of my way writing my comment.  Propose we **defer this to next week**.


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday June 13, 2023, **11am-noon **Americas/Los_Angeles
* 

 
