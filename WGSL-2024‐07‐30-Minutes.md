# WGSL 2024-07-30 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-06-25 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1D1m0tpdN--1rJfzjJv0H8FQDWTf65SJtQPvdrluU5P8) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
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
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **James Price**
    * Kai Ninomiya
    * Natalie Chouinard
    * Rahul Garg
    * **Ryan Harrison**
    * **Stephen White**
    * **Peter McNeely**
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
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * Ashley Hale
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



* Approved draft fix for [https://github.com/gpuweb/gpuweb/pull/4801](https://github.com/gpuweb/gpuweb/pull/4801) Clarify const-expression eval and type checking
* 
* AB: Triaged M0 things, down to 1 page of issues. Vast majority are “domain of floating point functions”.


---


# ⏳ Timeboxes (until XX:25)

None!


---


# ⚖️ Discussions


### [Should invalid combinations of storage texture formats and access generate a WGSL error · Issue #4728](https://github.com/gpuweb/gpuweb/issues/4728) 



* KG: Want to reopen the discussion about when errors happen. When does the shader make contact with the API; that’s pipeline creation time.
* 
* AB: There are 3 categories: enables, formats, compat mode. Formats were creation errors, which is appropriate, because a lot of things move between stacks, there’s no reason to do it, there is a clean boundary. For enables, there’s no consensus internally, there are arguments on both sides. One argument is it would be nice to write a module that enables f16 but only some entry points use it, and should be able to compile. But the counter argument: Have to know how to parse F16. That’s a problem for future features. There are some future implications.
* AB: Enables are a little odd because API validations that don’t get validated at validation time - it’s an error off to the side. Usually WGSL mentions creation errors in WGSL, so there’s some things to clean up in the spec.
* MW: I’d be fine making this a validation error at pipeline creation.
* JB: Mike and I would enthusiastic about making this a shader creation error.
* KG: We did a great job of figuring out how strongly we feel about this. It saved a lot of time here
* **Consensus: Retain the previous behavior (which is not in the spec): They are pipeline errors. That includes compat: WGSL would understand them, and compat violations should be handled at pipeline creation time.**
* DN: 3 categories:
    * 1. Enables -> doesn’t get moved
    * 2. Formats -> gets moved
    * 3. Compat/Access validation -> gets moved
* DN: There’s a benefit of having shipped 1 shader blob which includes both path. This morning we had a discussion: that makes us feel queasy. Maybe there’s a better feature that can support that without being wishy-washy in the spec. We could do @enable f16 on functions and declarations. It works like diagnostics. If you parse the whole thing, it says “these are the bits that i’m going to use” and it’s fine-grained and it’s more user-consistent. That would also solve the forward compat and fallback path.
* JB: The effect of this attribute would be used if F16 is not enabled?
* The function is enabling F16. If you’re a compiler that doesn't understand F16, skip the entire block
* MM: That means we can’t add a feature that has unmatched brackets, but that’s probably good
* JB:  Rust has a balance-brace(?) requirement also.
* JB: It’s like a conditional compilation thing, sort of.
* DS: Yes. could also put it more finegrain, like if-statement.
* MM:If you did @enable(foobar) then that can act as a giant block comment?
* &lt;all> yes
* KG: What milestone are you targeting this for?
* DN: M3+.  Don’t rush into inventing this.
* JB: We aught to be setting up for - we wanna be co-evolving with people who are using stuff. We don’t want to be inventing little features that we think will help people in the large. WGPU should maybe partner with [someone] to ask them for requests


### [array_index out of bounds behavior is not specified for texture builtins · Issue #4782](https://github.com/gpuweb/gpuweb/issues/4782)



* PR: [Clarify array layer OOB for texture sampling #4792](https://github.com/gpuweb/gpuweb/pull/4792)
* AB: Our Metal backend will clamp at top-end. Array index will be clamped to 0..numLayers-1
    * For signed case, Tint will need to add the clamp to get to the lower end, rather than passing signed int to the Metal builtin which only takes uint, and hence falls back to usual arithmetic expression.
* JB: Does that permit median-of-three clamping? 
* AB: negative-to-zero, and anything larger to the last layer.
* JB: good to performantly require a single behaviour.
* AB: Given you’re about to touch memory, a max operation doesn’t seem expensive. (Metal is already clamping at the top end, so the only operation a WebGPU implementation would need to add is a max(0, …) to handle negative values.)
* **_CONSENSUS: clamp_**
* 


### [Clarify textureSampleBias bias param #4771](https://github.com/gpuweb/gpuweb/pull/4771)



* Should it be an early evaluation error on the range?
* JB: Trend in the spec is to generate shader creation errors when you can.  This is one such case. Is it more consistent to treat it shader-creation error for this.
* KG: Worry about the weird range, particularly on the positive end of the bias.  (+15.99) We can reasonably extrapolate that many devs will set a bias of 16, and then you get a nitpicky compiler that says it’s wrong.  My proposal is why don’t we just clamp this.
* MM: That makes sense to me because general case of falling between two adjacent floats at compile time, we fix it for them.  This is another case of a weird range, we should just fix it for them, rather than complain at them.
* AB: If you clamp then you get portable runtime behaviour.
* KG: If you clamp at runtime, then clamp at compile-time, and so it doesn’t error out.
* **_Consensus. Clamp it._**


### Meta Conclusion



* KG: This was everything though M1 in WGSL that wasn’t marked as copyediting or resolved. We need to clear out M0s for CR, great work has been happening. Thanks. Please keep at it. We’ll get there!
* AB: We spent a bunch of time triaging remaining M0 things this morning. We have less than a page of issues remaining. Many of them are “what’s the domain of this floating point function”
* KG: If you want to divide up the work, I’m happy to write spec text and make PRs, but I don’t know what the numbers should say
* DN: IEEE is where to start. atan2 has 12 rules or something. Pow has a lot of detailed work
* KG: I’ll take some stabs at things. Please tell me if I get them wrong
* DN: I’ll be out-of-office for a week, but if we can be patient, then just give me time to tackle the issues assigned to me. We’re down to ~15 issues, which should be less than a month of work.
* 


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday August 26 2023, **11a-noon** (America/Los_Angeles)
* But email Kelsey or the list if you want to meet sooner!
* 


