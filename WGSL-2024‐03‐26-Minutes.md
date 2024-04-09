# WGSL 2024-03-26 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+0%22), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+is%3Aopen+milestone%3A%22Milestone+1%22)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2024-03-12 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1g09Q3khvi1rK-h1Oc4E4Ks7Z3vmYlq9MbKKFsOBg9xU) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * Mike Wyrzykowski 
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
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
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
    * Erich Gubler
    * **Jim Blandy**
    * Kelsey Gilbert
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



* Discussion offline about working group vs. community group.
* Request for feedback from Apple:
    * [Are "Fine" derivatives guaranteed to actually be fine?](https://github.com/gpuweb/gpuweb/issues/4325#issuecomment-1984060405)
    * TL;DR: macOS + Intel is producing coarse derivatives. Is this a driver bug, or does Metal not guarantee that derivatives are fine?
* 


---


# ⏳ Timeboxes (until XX:20)


### [Function arguments cannot be modified? · Issue #4531](https://github.com/gpuweb/gpuweb/issues/4531)



* (untriaged)
* (Closed as duplicate of [#4113](https://github.com/gpuweb/gpuweb/issues/4113))


### [Accuracy for AbstractFloat `fract` give unintuitive/unexpected results · Issue #4523](https://github.com/gpuweb/gpuweb/issues/4523)



* (untriaged)
* DN: Gets swept up with the general rule that AF accuracy should be no worse than f32. But in this case that gets weird /bad answers, while doing the perfect thing is easy enough to implement.
* RH: Most times translating to f32 is intuitive enough. In this case you get a drastically different value; there’s a bad discontinuity you fall into.  Should be easy/clear to ..
* KG: Seems works-as-intended. 
* RH: That overload for fract is expected to return between 0.0 and 0.75 (if in f32 domain) in that case but it yields a number outside that range. 0.877 is the right answer.
* KG: Discontinuous functions can’t have error bars. 
* RH: Weirdness comes in because we round to something with a fraction of 0.
* JB: So the proposal is to have an exception in 14.6.2.
* RH: Say fract is calculated in f64 correctly rounded.  Depends on floor and subtraction which are both correctly rounded already.
* KG:** Please propose resolution. ** To my thinking discontinuities will always be a problem.
    * PR [https://github.com/gpuweb/gpuweb/pull/4541](https://github.com/gpuweb/gpuweb/pull/4541) 

DN: rule for abstract float 

	The accuracy of an [AbstractFloat](https://gpuweb.github.io/gpuweb/wgsl/#abstractfloat) operation is as follows:



* A correct result is required when the corresponding [f32](https://gpuweb.github.io/gpuweb/wgsl/#f32) operation requires a correct result.

 


### [WGSL built in functions are very under specified · Issue #4527](https://github.com/gpuweb/gpuweb/issues/4527) 



* (untriaged)
* 


### [Compatibility mode: provoking vertex differs between OpenGL and WebGPU · Issue #4489](https://github.com/gpuweb/gpuweb/issues/4489)



* (untriaged)
* (this will mostly be bikeshedding)
* 


---


# ⚖️ Discussions


### [Feature request: textureQueryLod equivalent · Issue #4180](https://github.com/gpuweb/gpuweb/issues/4180) 



* (DN asked for extra time last time)
* DN: Didn’t get enough time. 
    * Trying to translate to lambda and _d_ in the Vulkan spec. ([https://registry.khronos.org/vulkan/specs/1.3-extensions/html/vkspec.html#textures-lod-and-scale-factor](https://registry.khronos.org/vulkan/specs/1.3-extensions/html/vkspec.html#textures-lod-and-scale-factor) ) 
        * [https://registry.khronos.org/vulkan/specs/1.3-extensions/html/vkspec.html#_lod_query](https://registry.khronos.org/vulkan/specs/1.3-extensions/html/vkspec.html#_lod_query) 
        * Otherwise, the steps described in this chapter are performed as if for `OpImageSampleImplicitLod`, up to [Scale Factor Operation, LOD Operation and Image Level(s) Selection](https://registry.khronos.org/vulkan/specs/1.3-extensions/html/vkspec.html#textures-lod-and-scale-factor). The return value is the vector (λ', dl). These values **may** be subject to implementation-specific maxima and minima for very large, out-of-range values.
    * And should the array level return type be integer instead of fp?


### [Use enum tokens when they are the only possible grammar usage. · Issue #4505](https://github.com/gpuweb/gpuweb/issues/4505) 



* (KG: I’ve split this into four narrower topics below)
* 


### [[high-level] Should predeclared enumerant values (e.g. position) should be shadowable by user variables? (e.g. let position =) #4539](https://github.com/gpuweb/gpuweb/issues/4539)



* KG: Think they should not be.
* MOD: I can make `let textureSample = …  `and then have that take precedence. We also have a proposals for wgsl namespace.  Being shadowable is more internally consistent with the rest of the language.
* DS: Being internally consistent is less important to me than user feedback about hitting this problem with @builtin (position)  miscompiling.  Want these token locations not participate in name lookup.
* AB: I can see a future where enumerant values can be written in shaders in different ways, e.g. aliasing enums at the top. Everyone running into this only happens for module-scope declarations. The surface area of where you hit this problem is not that big a deal.
* KG: This is an unexpected thing to come out of the language. While it’s consistent within the grammar  / in the abstract, I think it’s much less consistent with what people expect. People think about attributes and variable names coming out of different pools.  I don’t think them being the same kind of token in the grammar should mean they are coming from the same pool for the purpose of name resolution.  If aliasing is something we want, that’s the next issue.
* MOD: The user feedback on these collisions, is that something we can trace online.  Are there filed issues?
* DS: It’s come up on the matrix chat once or twice, and its come up through internal feedback.
* KG: Even if it didn’t come up, it’s better to keep them separate. If they do shadow things, they should shadow things in their own pools.
* DS: There’s many things we would never have in these slots.
* KG: e.g. C++ attribute(likely) would never stop working if you declared likely elsewhere.
* MOD: Agree things in different pools can/should have different name resolution.
* JB: The language which takes single namespace the furthest is the language Scheme. In Scheme macros there is special provision for keywords, that don’t get name lookup.  That signal tells me, even when people are extremely concerned with generality and overridability, even in the most extreme case they have wanted to have carveouts for words whose meaning are determined by their usage context. Reference to internal consistency to me are not persuasive. All languages need this, and take advantage of this.  Pretty obvious application of that argument.
* KG: Can be extra clear here if we are explicit about what examples we would or would not want retained.  There are two examples:  builtin(position), textureSample. If you wanted to retain the behaviour we have cross-pool shadowing of position and args to attributes, that that is the disagreement here.
* MOD: I’m not supporting “predeclared enumerants should be shadowable”, to be clear. I just wanted to point out there is this perspective, but didn’t support at all. The earlier we distinguish kinds, the better.
* DS: “context-dependent names” should not be shadowable.
* JB: Seems _we have consensus that position, and the interpolation parameters should not be shadowable._
* DS: Point of order: Apple is not here to voice an opinion to contribute to consensus.


### [[high-level] Should be support aliasing predeclared enumerant values? (e.g. user-defined @location(pos) mapping to @location(position)) #4540](https://github.com/gpuweb/gpuweb/issues/4540)



* KG: Seems kind of nice we can make an alias for, but M3+. Seems niche.
* DN: M3+ or later.
* AB: The discussion isn’t about when, but rather if the design features will fit together well.
* KG: Think it has limited use.


### [Generalize attribute grammar #3911](https://github.com/gpuweb/gpuweb/issues/3911)



* (nominally editorial)
* DN: My preference is to encode more in the grammar; it’s “in your face” and mechanically encode. That makes it more robust and easy to read.
* KG: You often want to generalize further, don’t hardcode the specific names, because you probably want to emit a better diagnostic.
* MOD: I would support this distinguishing factor to be not in grammar, but to happen in static-lexical scoping (semantics), in the Declaration and Scope section.
* JB: Preference to leave this to editor’s discretion.  I see Kelsey’s point that a direct translation of grammar into code, you get “unexpected token” rather than “you got the wrong name for an attribute”.  In my experience every effort to provide good diagnostics entails stepping out of the grammar.  If you care about good diagnostics you’re already playing this game in every other spot.  Grammar shape won’t have an effect on quality of the implementation.
* DS: From Tint’s view, having things in the grammar prevents having to jump all over the spec. I just got tripped up to pointer-to-texture vs. address of texture. Two rules in different parts of the spec that you have to join. Having it in the grammar keeps it in the same spot. How it’s implemented is independent of the grammar. Makes it easier to understand what you put where.
* DN: Even so we have the LALR(1) normative one, and the mechanically translated recursive-descent-friendly one.
* KG: The further you get from what’s in the spec vs. what’s in the implementation: that’s where bugs creep in.
* DS: Which grammar; the recursive descent one is what we all work from.
* KG: It’s all downstream.
* DS: With error recovery we end up diverging even further.
* JB: There’s always huge gaps between specs and the actual parsers. It’s hard to find deployed projects that use parser generator grammars. None of the JS implementations.
* DS: Glslang GLSL parsed from bison.
* JB: Given the beauty, they are more rare than you’d expect.
* KG: I want more lay people to be able to read the spec and understand. I think grammars don’t help there.
* JB: I find it more legible to have it in the grammar.
* KG: I think it’s unreadable to most devs.
* KG: In my head you have attributes with @ then name then parens around names.  Then I go look up in the table of attributes.
* KG: The editors serve the group by putting decisions onto paper. Go through the editors.


### [Attribute syntax should not specialize to built-in language attributes · Issue #3520 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/3520) 



* (nominally editorial)
* 


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday April 09 2023, **11am-noon** (America/Los_Angeles)
* [[high-level] Should be support aliasing predeclared enumerant values? (e.g. user-defined @location(pos) mapping to @location(position)) #4540](https://github.com/gpuweb/gpuweb/issues/4540)
    * AB/DN: This is actually coupled to #4539:  Deciding that those attributes are not shadowable, means we can’t really make aliases for these things. That’s because defining and using an alias requires identifier resolution to be operating, and that means we get back into the world where we’re going to accidentally shadow `position` again.
* [clarify when const-exprs subexpressions are evaluated](https://github.com/gpuweb/gpuweb/issues/4558) #4558
* [Clarify whether type checking occurs for short-circuited RHS expressions](https://github.com/gpuweb/gpuweb/issues/4551#top)#4551
* 