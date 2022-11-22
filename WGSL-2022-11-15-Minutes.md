# WGSL 2022-11-15 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** JB, DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** (non-APAC) Tuesday **11am-noon **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-11-08 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1mL2sKEWcM2PL-ooHUFSXmR13ohxq8NOCEfKcY8PGIOY) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * Alan Baker
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
* Intel
    * Hao Li
    * Jia A Chen
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
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
    * **Erich Gubler**
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * **Teodor Tanasoaia**
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Dominic Cerisano
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
* **Jason Erb**


---


# 📢 Announcements


### Double-check your timezones!



* Meeting is still anchored to America/Los_Angeles, but America/Los_Angeles switched from its Daylight Time to its Standard Time two weekends ago.
* Please refer to the times listed here: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20221115T11&p1=137&ah=1](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20221115T11&p1=137&ah=1) 
* 


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


# ⏳ Timeboxes (until 11:15)


### [wgsl: add way to get special values for a given numeric type (e.g. max, min, lowest) · Issue #3431](https://github.com/gpuweb/gpuweb/issues/3431)



* DN: Against proposal that adds new keywords. So angle bracket thing.
* DN: Solution shape is likely i32.lowest i32.highest.  
* BC: Against using underscored-names.
* BC: James internally wondered if   array-type.length might be another kind of thing that fits under this.  Would be happier with type.member rather than underscores.
* KG: Can do dots.  
* DN: Think highest and lowest, not smallest subnormal smallest normal.
* KG: Arg is to make people see the contrast. Prefer min and max
* BC: Prefer lowest and highest to avoid ambiguities across different languages.
* KG: Don’t like to type long names like lowest and highest.  And we have min and max functions. Don’t want to do better than rust here.
* **_Table it for now._**


### (NEW) [Consider limiting the set of override declarations in shader stage interface #3555](https://github.com/gpuweb/gpuweb/issues/3555)



* JP: currently every single pipeline override is in the shader interface for any shader I make. So when I create the pipeline, I have to supply override values for too many  values that are not relevant to the pipeline.
* JP: Need wordsmithign to get the transitivie closure of override decls statically used, and attributes, and array types.
* JB: Consistent with decision to be able to put lots of things in the same module.
* **_Consensus to do this.  (Recognizing Microsoft and Apple are not present but usually are)._**


---


# ⚖️ Discussions


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* PR: [Add workgroup broadcast and uniform load built-ins #3586](https://github.com/gpuweb/gpuweb/pull/3586) 
* DN: Google think workgroupUniformLoad is the nicer solution. No magic about allocating behind the scenes that you would get with workgroupBroadcast.  
* JB: Think it’s fine, Alan is the most pessimistic about implementation, and Raph is most demanding of features, so if they both like it, that’s good
* KG: What caused us to drop the ability to load from other address spaces? Why is it only workgroup now?
* DN: If you have multiple workgroups in flight, you can’t guarantee that they won’t race on other address spaces. Metal doesn’t have any way to prevent that. Restricting it to workgroup ensures that that won’t happen. Synchronization picture is much clearer.
* KG: Main concern was that this solves the issue for compute shaders with workgroup memory, but not other use cases that need the storage address class. But apparently it’s harder to get guarantees about that from the underlying implementation.
* JP: If you really need other address classes, you could pass it through workgroup memory
* KG: But I’m interested in fragment shaders.
* DN: You should use atomics for this. You’re trying to communicate something globally across all the invocations in a shader. The mechanism for that is to use an atomic in a storage buffer. Oh, but you’re saying it’s still not uniform.
* KG: The analogy between workgroup uniformity and quad uniformity.
* DN: If you want to communicate across a quad, you can use two derivatives. But there’s a chicken-and-egg problem, because you know it’s not uniform, but the compiler can’t tell
* KG: Right, the purpose here is something the compiler does know is uniform.
* KG: I like workgroupUniformLoad
* KG: It seems like Google’s feelings have evolved, so the PR needs to be updated?
* DN: Yes, and I’ll make sure to amend the uniformity rules
* KG: It should also be explicit that workgroupUniformLoad requires uniform control flow, like barriers do
* DN: This will need to be added to the collective operations list. The uniformity chapter lists the things that require uniformity


### [Designing a uniformity opt-out #3554](https://github.com/gpuweb/gpuweb/issues/3554)



* JB: Personally attribute syntax could be a perfectly fine way to use in a lot of places, without needing a lot of innovation.  Think it’s very similar to what I had in mind to the block; to me it’s a spelling difference.
* DN: Tex’s comment says, he likes a well-named attribute, especially one that could be checked in a debug mode - an assertion that could be checked dynamically. He also liked the very coarse-grained opt-out, to support Unity. Taking that, I can agree with a very coarse-grained opt-out. But we want to avoid blog posts where people just copy code without thinking about it. An option on createShaderModule would get the global opt-out someplace people wouldn’t copy-paste blindly.
* DN: It would also be useful to have an attribute you can put on a variable, “I promise this is uniform”. JP is concerned about the details: can it be explained clearly, in a way that people can absorb? I’d like to pursue this path and make a concrete behavior, so that we can all be on the same page.
* KG: Coarse-grained opt-out: I would hope that we could just make that a non-standard implementation-specific thing. If we do standardize it, we should be clear that it’s going away. I would like to prevent people from building on this.
* DN: We implemented it as a warning first, then errors. The feedback has been completely one-sided: people want it to go away. Raph has been the only one mildly positive about this. We should take that into account.
* BC: One-sided for fragment shaders. There are no security issues entailed, so people would only be opting into non-portable behavior.
* KG: The people who are happy with a feature aren’t going to complain, though. When it works, people are happy and we never hear about it.
* BC: The global opt-out would be an explicit action that people have to take.
* KG: The global opt-out would be too easy to reach for as a cure-all. I’d be more inclined to just give them a built-in which has “_nonuniform” in the name, so that they know what they’re getting into, and it documents their willingness to accept the non-portable behavior. Let’s just solve that pain point. Would a “_nonuniform” sampler builtin be acceptable to these users?
* BC: We’re not keen on “_nonuniform”. You’ll be teaching people to use the non-uniform examples always. And it affects the generated code.
* KG: I don’t agree with that. 
* BC: For SPIR-V -> WGSL, Tint would take SPIR-V, emit WGSL, warn if there were violations. The WGSL would not compile unless you passed the flag in the browser. So you’re always erring on the side of preventing problems. But you’re supporting the case of people with an existing corpus of problems. People copying WGSL from the web will have to change the API. We have industry experts asking us for this opt-out.
* KG: We’re saying try it out. We also have industry experts who have told us about bugs and flakiness in derivative-based code that they’ve been able to eliminate when they fixed uniformity issues.
* JP: That was a problem with barriers in compute shaders.
* KG: If the opt-out is local, then it doesn’t blindly apply to other areas of the code that were working, or to new code that you introduce. A shader-wide opt-out covers up too much, and people lose the benefit of the analysis when they’re not expecting to.
* DN: I’d like to pursue both options.
* KG: I’d rather have name mangling and detailed opt-outs than a global opt-out.
* DN: The SPIR-V -> WGSL conversion can just spew the local opt-outs everywhere, it’s not pretty but the output isn’t beautiful anyway.
* JB: Let’s agree that we want something local in v1, and that the global opt-out, if it ever happens, will only happen after we also have something more local, and can decide if we care about a global opt-out at all
* KG likes scary names
* DN: SPIR-V has Uniform and UniformId decorations on a value already, with semantics very close to what we want. For UniformId you provide a scope to indicate what area of uniformity you want
* EG: The SPIR-V decorations seem analogous to the aliasing annotations in LLVM IR generated from Rust.
* KG: So you’d be able to round-trip WGSL through SPIR-V and maintain the uniformity?
* DN: Yes, I think that would be workable.
* DN: For the Unity case, you could slap the attribute on every expression that influences control flow. Even if you load something from memory, you could sanitize it at the point that it affects control flow.
* BC: Would that affect function calls as well?
* DN: In the limit, you could put “if @uniform true” around your texture operation.


---


# 📆 Next Meeting Agenda Requests



* Next week: (**APAC**!) Tuesday, November 22, **4-5pm** (America/Los_Angeles)
* **Write any PRs you’ve promised!**
    * There are no other v1.0 issues outstanding (other than the ones on this agenda!) that are not blocked awaiting proposals.
* WGSL schedule:
    * November 22: APAC
    * (US Thanksgiving Nov24, Thursday)
    * November 29: APAC
    * December 06: Non-APAC
    * December 13: APAC
    * December 20: Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 27: No Meeting_
    * January 03: APAC
        * DN: Suggest we cancel
    * Note: Based on our current issues remaining for v1 though, we are very likely to cancel meetings due to lack of discussion topics, so we can likely be liberal with skipping meetings.
* 