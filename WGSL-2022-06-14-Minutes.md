# WGSL 2022-06-14 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon** Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-06-07 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1EnjkYkp3ce4A78npEV8-iyAOiXNN8YzvVyt2AHHm4oI) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Daniel Glastonbury
    * **Myles C. Maxfield**
* Google
    * Alan Baker
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
* Intel
    * Hao Li
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * **Tex Riddell**
* Mozilla
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* **Dominic Cerisano**
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson


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



* [https://github.com/gpuweb/gpuweb/pull/2754](https://github.com/gpuweb/gpuweb/pull/2754) Add a grammar analyzer
    * See “make lalr” target in wgsl directory
    * Python script generates LALR(1) parse table in ~7s on David’s laptop.
    * The table dump is in an ad hoc text format.  I don’t want to post that to GitHub pages. I’d rather design a deliberate JSON format for machine consumption.
* [https://github.com/gpuweb/gpuweb/pull/3037](https://github.com/gpuweb/gpuweb/pull/3037) adds the strict LALR(1) check to CI
* 


---


# ⏳ Timeboxes (until 11:30)


### [uniformity analysis + basic tail-branching #3007](https://github.com/gpuweb/gpuweb/issues/3007) 



* DN: Computers don’t work the way you think they do. If you believe the world is inlined always, then this simplifies down, but I’m not sure we can rely on it.
* DN: If you introduce subgroups into this, I’ve demonstrated that you *don’t* get subgroup reconvergence at end of function. We know it’s annoying, but it’s the rule.
* DN: We have spec’d what we can. 
* MM: When we discussed whether returns reconverge, we said “no”, and said we’d file an issue. It’s likely that programmers’ expectations don’t reflect what we do today. We should try to match programmer expectations, unless that’s too hard.
* DN: I agree that there’s a mismatch.
* DS: What do we do now? While we can rewrite e.g. with local variables, I worry that we might cause a performance issue in doing that rewrite.
* Options:
    * 1. We do what we have now. All burden is on users to adapt their shader.
    * 2. Add annotation like @reconverge to ask the compiler to rewrite it to ensure a single return.
        * 2b.  Inverse:  annotate the “please don’t restructure this, at risk of triggering uniformity error”.
    * 3. Compiler detects #2 but does the rewrite automatically. Potentially risk performance pit because you use extra variables and control flow.
* MM: So the rewriting would rewrite return V into storing V into a return function local var, and break back out?
* DS: Yes
* MM: Does any compiler do this?
* DS: Tint no
* KG: Naga no
* TR: DXC has this
* DN: Why?
* TR: It was useful to someone in some cases. It’s called “structurize return”
* (trying to find it) `dxc -T ps_6_0 -opt-enable structurize-returns`
* Other links:
    * [https://github.com/microsoft/DirectXShaderCompiler/blob/main/lib/Transforms/Scalar/StructurizeCFG.cpp](https://github.com/microsoft/DirectXShaderCompiler/blob/main/lib/Transforms/Scalar/StructurizeCFG.cpp)
    * [https://github.com/KhronosGroup/SPIRV-Tools/blob/master/source/opt/merge_return_pass.h](https://github.com/KhronosGroup/SPIRV-Tools/blob/master/source/opt/merge_return_pass.h)
    * [https://github.com/microsoft/DirectXShaderCompiler/blob/main/tools/clang/test/HLSLFileCheck/hlsl/control_flow/loops/structurize_multiexit_crash_do.hlsl](https://github.com/microsoft/DirectXShaderCompiler/blob/main/tools/clang/test/HLSLFileCheck/hlsl/control_flow/loops/structurize_multiexit_crash_do.hlsl)
* KG: Do we need this in v1? People have a workaround.
* MM: I think programmers will get caught by this a lot, so we should probably do this in v1.
* DN: I want another week, I want AB to weigh in.
* MM: Is this a whole-program problem?
* DN: Yes.
* TR: So you’re saying the control flow uniformity is leaking? So e.g. the instruction pointer might desync.
* int foo() { if (tid % 2 == 0) { return 4; } else { return 5; } int bar() { foo(); barrier(); }
    * The barrier() is legal in HLSL
* TR: HLSL does reconvergence; as if predicated execution across the entire wave.
* DS: So it’s just SPIR-V that doesn’t reconverge coming out of functions?
* DN: Yes
* DN: So on the table is that we give the programmer this tool, and UAs will polyfill this as needed.
* ??: We also need to decide implicit or explicit
* **Come back next week**


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Talked internally. Our general position is unchanged. We do understand that in HLSL, for in-out params, it’s not generally possible to support aliasing, to behave as-if params are aliased, is to specialize each (aliasable) param. That means 2^N specializations, which would not be good.
* MM: What I’m sort of asking for what our philosophy is. 
* DN: So this is exactly and only about function params?
* MM: Is it? You can have a function that [...]
* MM: The problem is larger than just function params.
* [...]
* DS: I filed an issue about local aliases
* [...]
* MM: If the problem can be decomposed into function parameters which may or may not alias, as distinct from aliasing that occurs within the body of the function. I think there’s a path forward. I think the second one can be legal, but that we can’t support the first one.
* DN: Agreed.
* [,,,]
* MM: Does HLSL have mutable global variables?
* TR: Yes, `static`
* TR: so what about this example? [https://github.com/gpuweb/gpuweb/issues/1457#issuecomment-788202781](https://github.com/gpuweb/gpuweb/issues/1457#issuecomment-788202781) 
* DN: I would need to look at the table in the SPIR-V spec.
* MM: It  would be better than saying “this is bad, don’t do it”, it would be better if we had defined behavior here; in the worst case it would be what HLSL does, which may be:  Either there is no aliasing, or at worst copy-in copy-out for those inout params.
* DN: I want to think about if weirder things can happen. **I want to take this offline to think about this.  Can you get more confusing results than HLSL’s worst case.**
* MM: Can we agree on generalities, even if not specifics?
* KG: I don’t understand this well enough to talk in generalities.
* MM: What can we do next?
* DN: We can clarify our language for “root identifiers”
* DS: “If you have a module-scoped thing, can you alias that?”
* MM: I propose “try as hard as we can, but don’t promise consistent behavior”
* MM: Aside: Internally we call “programmer-expected behavior” “javascript behavior”, and JS has well-defined aliasing.
* [...]
* DN: I don’t want to tie us to inlining, especially because of potential combinatorial explosions
* 


### [Investigation: kill vs demoteToHelper behavior in native APIs · Issue #921](https://github.com/gpuweb/gpuweb/issues/921)



* DN: We now think we should just use Demote. If we had both, people will end up just using Demote.
* DN: We do need to figure out what termination guarantees are, or at least a way to explain when we don’t have them.
* MM: It would be cool to pick a good name, even if we add terminate later on.
* KG: I think we should call it `discard`, because even when people use `discard` in GLSL, they expect Demote behavior (generally) and not Terminate.
* MM: I would like it if someone else can corroborate “what glsl authors expect”
* DN: With demote, I think we can remove Discard state from uniformity analysis.
* KG: I was talking with JB about this, and we think the subset of devices that might actually need a demote–on-terminate polyfill is narrow. Someone also said that on drivers that pre-date the OpDemoteToHelper extension, often OpDiscard gives OpDemoteToHelper behavior, even though the (current) behavior of OpDiscard is OpTerminate. (i.e it does not continue to contribute to derivative calculation)
* KG: Further, since newer drivers do support OpDemoteToHelper (and old ones might support it accidentally via old-OpDiscard) that we could probably ship v1 without doing the demote–on-terminate polyfill, and simply not run webgpu on those drivers that need it yet. We could add a demote–on-terminate polyfill in a later roll-out of support for these drivers.
* **Resolved: `discard` means demote-to-helper**


### [should WGSL allow shadowing of builtins at module scope #2941](https://github.com/gpuweb/gpuweb/issues/2941)



* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, June 21, **11am-noon** (America/Los_Angeles)
* [wgsl: function param that is pointer-to-workgroup is problematic if the store type contains an atomic #3067](https://github.com/gpuweb/gpuweb/issues/3067)
*  