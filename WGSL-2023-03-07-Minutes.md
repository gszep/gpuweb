# WGSL 2023-03-07 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **4-5pm **Americas/Los_Angeles (APAC!)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-02-07 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1c19Tf_DP7BB0H6cZMjY8X-AR66pZEpkwu5TqisP1WBw) , [Agenda / Minutes for GPU Web F2F meeting 2023-02-16/17](https://docs.google.com/document/d/1bbwoAu7NZ5GAWAE12xUNZi08fRuSRDfYLzHxV6yFDF8/edit) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * Mike Wyrzykowski 
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
    * **Jiawei Shao**
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
    * **Tex Riddell**
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



* [#3859](https://github.com/gpuweb/gpuweb/pull/3859) types, type-generators are ordinary predeclared identifiers, no longer keywords or context-dependent names.
* [#3874](https://github.com/gpuweb/gpuweb/pull/3874) Define "software extensions", and the "requires" directive
* [#3862](https://github.com/gpuweb/gpuweb/pull/3862) Add @must_use attribute
* [#3853](https://github.com/gpuweb/gpuweb/pull/3853) [editorial] Move value constructors and bitcasts to built-in functions
* [#3851](https://github.com/gpuweb/gpuweb/pull/3851) [editorial] Reorganize memory sections 
* 


---


# ⏳ Timeboxes (until XX:25)


### [question: can barriers synchronize memory accesses across workgroups · Issue #3774](https://github.com/gpuweb/gpuweb/issues/3774)



* DN: Could have a bunch of examples of what works what doesn’t, but htat’s more like a blog post. If committee wants something like this, we can write it, but it’s not really spec material.
* AB: Sounds more like programmer’s guide rather than spec.
* KG: Over time, make it a “Discussion” for this kind of thing.  That’s where we’ll put things like this.
* **converted the issue to Discussion**.


### [Publish WGSL CI artifact to a repo for reusability #3778](https://github.com/gpuweb/gpuweb/issues/3778)  



* MOD: will do so.  Will fix the WASM build.  Syntax /grammar is stabilizing.   When it’s ready I’ll point the asking person to it.
* **MOD will close when ready.**


### [wgsl: enable directive should support a list of enable-extensions · Issue #3885](https://github.com/gpuweb/gpuweb/issues/3885)



* [Enable directive takes a list of extensions. #3886](https://github.com/gpuweb/gpuweb/pull/3886)
* KG: Mozilla is fine with this.
* **_Merge it._**
* 
* 
* 


### [Evaluation order of assignment and compound assignment are flipped #3928](https://github.com/gpuweb/gpuweb/issues/3928)



* DN: Glad someone looked up other languages, and saw JS does it this way..
* DN: Will affect uniformity analysis as well.
* **_Proposal: order assignments, and compound assignments left to right. RESOLVED._**


### [[WGSL] Consider removing limit for maximum workgroup array size · Issue #3917](https://github.com/gpuweb/gpuweb/issues/3917)



* AB: This is one of those limits you can do to avoid fuzzers annoying you.  We can say exactly matches the webgpu limit.  It plugs a small hole when you have it.
* **Table until James is on the line.**


### [wgsl: unrecognized diagnostics #3927](https://github.com/gpuweb/gpuweb/issues/3927)



* DN: Currently, unrecognized is ignored for diag names. The bad part is that if you fat-finger it and misspell, you don’t get the effect you wanted. And that sounds bad.
* DN: JB proposed making it an error, to be forward-compat and flexible, and also safer.  Don’t have to deal with the flexibility a future design discussion.
* JB: Yeah we don’t need to do anything more than make this an error for now. I have examples of how rust approaches this too, but we don’t need it for v1. It’s important to me to leave the way open. We want to start with strict and relax later.
* AB: We already have diagnostics [in code] and if you run it in e.g. Firefox it will fail, which is a bad experience. I also might recommend a diagnostic-diagnostic. 
* JB: We would rather deal with things by adding recognition of the diagnostic to Firefox, instead of trying to prevent it ahead of time.
* JB:  Whenever the rule is “ignore”, it’s a mess. E.g. variables in Makefiles.
* MM: Ignoring things you don’t understand is how things are done in CSS. The best-practice is to e.g. put the syntax that is ideal/expected at the top, and then amend that later with workarounds for partial implementations in browsers.
* MM: I also wanted to ask, I didn’t expect diagnostics to be Productions in the grammar, that e.g. authors type. I expected them to be spec-only things.
* AB: This is where we get our uniformity opt-out.
* MM: I don’t see a rule of the list of things that authors can type.
* AB: Two things, 1) severity, 2) any-string (the diagnostic name)
* DN: [https://www.w3.org/TR/WGSL/#filterable-triggering-rules](https://www.w3.org/TR/WGSL/#filterable-triggering-rules)   is the list of standard names. There is only one right now. ‘Derviative_uniformity’.
* MM: So this is if I want the “Myles” diagnostic to be an error, but no one recognizes that name for this.
* AB: Want to support use cases where people get creative with linter rules, and stuff their source code with it.  Don’t want other implementations to have to add every name in the world.
* JB: Rust’s solution here is to have a hierarchical name;  namespaced names would be ignored. Standard names that don’t have their namespace must be recognized.
* MM: It sounds like then that kind of namespaceing would be a requirement for this feature
* JB: I would love to write that proposal, but not for v1
* MM: Another way to do this would be to not use this same mechanism for offline tooling scenarios. Instead those should be controlled via command line arguments, or the like.
* KG: **want to revisit with more time**. Want to prioritize resolving for V1.


### ~~[Querying number of array layers for TextureCubeArray not supported in SM5.1 · Issue #3913](https://github.com/gpuweb/gpuweb/issues/3913)~~



* ~~[Remove textureNumLayers overloads for texture_cube_array texture_dept… by dneto0 · Pull Request #3915](https://github.com/gpuweb/gpuweb/pull/3915)~~
* (Resolved and closed offline)


### [wgsl: float to integer conversions should saturate in the destination type, not the source type. · Issue #3908](https://github.com/gpuweb/gpuweb/issues/3908)



* DN: E.g. there was no way to get uint::max into a float, so this is BC’s proposal for how to fix it.
* BC: This was discussed before, and criticized for requiring conditionals. My current proposal uses two selects instead of true conditionals. 
* MM: I am concerned about performance here, because adds and muls on float are expected to be mega-fast.
* DN: This is just for conversions, not for arithmetic.
* MM: What is it these examples?
* DN: Oh, not that part, look further down.

	`u32_alternative( a: f32) -> u32  { return select(0xffffffff, select(u32(a), 0, a >= 0), a > 0x1.fffffep+31); }`



* MM:: The common case of conversions should be indexing into an array, which we already check, so I’m not worried about this then.
* JB: Worried that people will care about perf here, and we’re giving that up for a feature that fewer people care about
* BC: &lt;quantization is surprising>
* JB: I think that we should consider whether people will hate wgsl because it’s hard, or because it’s slow.
* DN: too large float to int is undef behaviour in C++ and poison in llvm. This is probably a programming error in general.
* JB: is this improperly rounded? Shouldn’t a gpu give a well rounded result
* MM/DN: not in the apis.
* KG: Seems it’s something folks might be concerned about being too slow.  But it’s UB.
* MM: Motivated by the need to avoid UB, so already taking a performance hit.  Output a value that’s meaningful.
* KG: Caveat is that for some platforms it might be something other than UB, and we might be different.
* BC: Current spec rule does not result in UB.  It says clamp to largest quantized value of source type that fits in target type
* **_Resolved: take the new rule._**


### [wgsl: consider disabling out of bounds memory accesses to shared memories #3893](https://github.com/gpuweb/gpuweb/issues/3893)



* BC:  A partner was converting their shaders.  One of their shaders was rejected by FXC. FXC said it was a data race.  We investigated, and spotted that bounds clamping could produce a data race on the write.  FXC in this case could see it deterministically.  HLSL disables writes when they are out of bounds.
* BC: Our software robustness was taking something that was sound in HLSL terms and turning it into a data race. It turns out the partner had messed up, subtracting a giant number from all IDs. FXC saw that all indices were zero, and balked.  They fixed their problem and the problem went away.
* BC: It was enough of a concern that we wrote the code to do predication instead. I implemented this, and then found a bunch of driver bugs from this.  Fortunately many of those driver bugs have been fixed with more recent drivers. But others are still open, and predication is broken.  It’s very concerning. We will continue to clamp for v1 launch.  It’s easier to go from clamping to predication than the other way around. If Chrome ships with predication, then people will rely on that (Hyrum’s law), and then switching to clamping would break shaders.  We’re more comfortable with clamping first, then switching to predication later.
* TR: Yeah, FXC would only have that error if it could tell it was constant, and knew that all threads were writing to the same value. Or a uniform value.  FXC would be ok with it if it saw that the index might not hit the bounds.
* AB: If you do clamping in software, we say “you get one of these X number of results”.  But really it’s undefined behaviour in a very common case. It’s a bit of an awkward truth. We’re currently doing investigation in Vulkan about what’s reliable in terms of their robustness feature. It’s a different thing between the underlying HW doing something vs. a tool generating the conflicting case. Will come back with the Vulkan answer.
* TR: The no-writing OOB behaviour is limited to things with descriptors. (e.g. buffers, but not with things bound through root arguments in the root signature)  There’s no bounds checking on (ByteAddressBuffer, and StructuredBuffer as root arguments)  [Root sig 1.1](https://learn.microsoft.com/en-us/windows/win32/direct3d12/root-signature-version-1-1) has an option where a driver is allowed to move something from descriptor to root signature, and then lose the bounds checking. \
See: [D3D12_DESCRIPTOR_RANGE_FLAGS (d3d12.h) - Win32 apps | Microsoft Learn](https://learn.microsoft.com/en-us/windows/win32/api/d3d12/ne-d3d12-d3d12_descriptor_range_flags) \
For: D3D12_DESCRIPTOR_RANGE_FLAG_DESCRIPTORS_STATIC_KEEPING_BUFFER_BOUNDS_CHECKS = 0x10000
* AB: Ok, as long as it’s only on deliberate request.
* RC: Echoing what Tex said.   Through descriptor, know the size and it gets bounds.  Through root signature, it’s just a pointer without a size.  A later version of root signature there is an additional option even more that says “if you do move that to root signature, do also retain the size and the bounds checks.”
* RC: There’s no bounds checks on other kinds of arrays in your shader, e.g. groupshared memory.
* BC: Mapping that to WGSL terms, that’s workgroup, private, function. 
* AB: Similar in Vulkan you only get robustness supports on buffer resources.
* MM: About the original topic.  WebGPU will have data races. Presumably whoever writes an HLSL implementation will need to have some mitigations. Is the issue that FXC detects the data race and there’s no way to stop it from doing so?
* BC: At the start the concern was getting a bunch of shader compilation failures. But now we know there was some developer error involved here, shader compilation failure is not our concern.
* BC: Spec now has several options on OOB writes and reads.  
* AB: Clarifications: We say anything that’s in the buffer that’s attached.  If it’s local to the GPU its any memory inside the variable’s space.  The UB from data races is not just about the data you get back, but also the assumptions the compiler makes about the kinds of values that are read back; will affect downstream code.
* MM: We’ve known about data races for a long time now, so why is this new?
* AB: My issue is the spec is misleading right now.  It’s still incumbent on the implementations to make sure the actions of the machine are still secure. That’s a separate thing.
* AB: Myles is correct a user can introduce their own data race, and that will be UB on the underlying platform. There’s only so many mitigations.  My suggestion is to be more honest with readers of the spec.  By weakening it, it’s a signal to be more cautious.
* **(Tabled)**


---


# ⚖️ Discussions


### Publish the explainer as a companion Note to the specs?



* KG: This is about the explainer we wrote to give to the TAG.  Probably useful to redirect it to a more general audience. Sounds almost like “programming guide” which has been mentioned once tonight.   What work would be needed?
* AB: The explainer doesn’t have anything about WGSL. **Would have to be fleshed out.**


### More repos in the [github.com/webgpu](github.com/webgpu) org?



* KG: Proposal is in Corentin’s hands.  Let’s wait until the API meeting.
* MM: Sounds like we should watch consistency of information between gpuweb and webgpu.
* AB: Should we permanently transition everything to webgpu?  Could we mirror automatically? What’s the current thinking?
* KG: Nobody is championing the idea of migration from gpuweb to webgpu. Main idea is continue to use gpuweb indefinitely, and webgpu would be more community-targeted repos.  Possible distinction …
* MM: Need a better set of names?
* KG: **Let’s talk about this at the API meeting.**


### WebGL+WebGPU meetup at GDC



* MM: There was one before the pandemic, and it was useful to hear people’s thoughts. Good to hear corrective feedback.
* KG:  Think this in-person meeting would be valuable just like the recent F2F was valuable. Get a different kind of feedback in person.
* **(Sounds like a good idea)**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**non-APAC**) Tuesday March 14, 2023, **11am-noon **Americas/Los_Angeles
* 

FYI LANDED:



* PR [#3945](https://github.com/gpuweb/gpuweb/pull/3945) Weaken min,max,clamp on denorm inputs
    * Happens on Apple silicon (M1).
    * Issue [#3944](https://github.com/gpuweb/gpuweb/issues/3944) wgsl: max(denorm1, denorm2) may return either value, in MSL/Metal

 


### [[WGSL] Consider removing limit for maximum workgroup array size · Issue #3917](https://github.com/gpuweb/gpuweb/issues/3917)



* AB: This is one of those limits you can do to avoid fuzzers annoying you.  We can say exactly matches the webgpu limit.  It plugs a small hole when you have it.
* **Table until James is on the line.**


### [wgsl: unrecognized diagnostics #3927](https://github.com/gpuweb/gpuweb/issues/3927)
