# WGSL 2022-08-02 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** ****APAC!**** Tuesday **5-6pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-07-26 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1P9dSwbzmm24UpiDu4rLDic9SKL4SRK7-lnU7nvb_Z5E) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Daniel Glastonbury**
    * **Myles C. Maxfield**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
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
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
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
* Dominic Cerisano
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


### Call for Editors



* Corentin sent out an email beginning this process, check your email!
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



* Uniformity analysis now assumes function return is a reconvergence point
    * [https://github.com/gpuweb/gpuweb/pull/3254](https://github.com/gpuweb/gpuweb/pull/3254) 
* 


---


# ⏳ Timeboxes (until 5:30)


### [wsgl: disallow repeated use of builtin inputs and builtin outputs #3236](https://github.com/gpuweb/gpuweb/issues/3236)



* MM: Think it’s more important to add this restriction.
* AB: GLSL doesn’t allow it. HLSL compilers don’t allow it.
* **_Consensus forbid it for now._**


### [Parsing of strange float cases #3218](https://github.com/gpuweb/gpuweb/issues/3218)



* DN: Gives background.
* MM: Let’s not solve it ourselves. Let’s defer to another existing well-defined spec.
* DN: I looked at C++17, C++11 and they both defer to strtod in C99, and that’s not very strictly specified.
* DG: JS is not so clear. Says convert to an abstract value, then a double.  Doesn’t put a constraint on number of significant digits.  Says take 20 significant digits, then round.
* JB: Rust isn’t very specific.  Naga defers to a library for hex floats, and I looked at its code and it handles overflow cases e.g. with checked arithmetic. For decimal floats, it uses the Rust standard library, which isn’t well-specified.
* MM: Next step is investigation into JS.  
* DN: Does JS have hex floats? (no)
* KG: Would be ok to have our hex floats be special if JS doesn’t have them.
* KG: Let’s not be better than JS.  File an issue against ECMAscript.
* MM: Reasonable language spec:  GLSL, JS. Let’s look at those
* DG: [https://tc39.es/ecma262/multipage/abstract-operations.html#sec-roundmvresult](https://tc39.es/ecma262/multipage/abstract-operations.html#sec-roundmvresult) 
* MM: Agree with KG: Don’t block on this if JS doesn’t have a good answer for us.
* JB: (ECMAScript doesn’t seem to have hex floats)
* KG: Can we **just say “let’s aim for JS” and come back to talk if we discover that it’s not good enough**.
* DN: Sounds good.
* MM: +1
* JB: (ECMA requires arbitrary digit lengths)
* DG: (Also what strtod in clang does)


### [wgsl: Add limits to avoid nuisance floating point literals #3255](https://github.com/gpuweb/gpuweb/issues/3255)



* KG: What do we want to do that is different from what we currently do?
* DN: **I want to wait and see where the non-hex-float stuff lands before deciding here.**


### [Clarification of loop identifier lifetime #3153](https://github.com/gpuweb/gpuweb/issues/3153)



* Now has PR: [https://github.com/gpuweb/gpuweb/pull/3267](https://github.com/gpuweb/gpuweb/pull/3267) 
* MM: Kinda two issues here:
    * 1. Does a declaration in the parens collide with a declaration within the curlies?
        * for (var i = 0; i &lt; 10; i = i + 1) {
        *     let i = 17; // **consensus: error “`i` is already declared”**
        * }
        * 
        * I think consensus is that within-curlies **do** conflict with decls from within parens
    * 2. Declaring in a subscope of a loop.
        * for (var i = 0; i &lt; 10; i++) {
        *  if (true) {
        *    var a = 4;
        *  }
        *  var b = a; // **consensus: error “no `a` in scope”**
        * }
    * 
    * 
    * 


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253)



* JB: Seems consensus on making the out-of-bounds case should be such an error.
* JB: When you have an inconsistency and one of the alternatives is “hard stop”, then that’s less of a portability problem because programmer sees it.
* JB: If there is no good usage of a feature then it’s ok to ban it. If a programmer can reasonably rely on a specified behaviour then it’s more problematic. E.g. JS  obj.x where the object does not have ‘x’ property.  JS programmers rely on that.  So we should look at it case by case. 
* MM: With you about splitting into two cases: out of bounds, vs math operations.  Agree out of bounds accesses are definitely wrong.  There are so many wildly different results that can occur, which are legal by our language.  So the philosophy of “are people doing this on purpose” is not the right test. Nobody writes “1/0” intentionally. It’s sensible to honor the “x/y” semantics from the main language.  We think the right test is “is the set of potential behaviours of this operation wildly large, or is it fairly narrowly scoped”.  And arcsin(2) (is expected to) have narrow behaviour.
* DG: Another test we also considered: If the program would stop, then at compile time, that should be an error.
* MM: Right now, we don’t let the program stop, but I have filed an issue to add that because I thought that was allowed.
* MM: I filed this issue: [https://github.com/gpuweb/gpuweb/issues/3272](https://github.com/gpuweb/gpuweb/issues/3272)
* KG: I do write 1/0 sometimes.
* AB: Think that line is arbitrary.  No other language defines “x/0” .We decided to do that “just because” so that’s not enough impetus on its own.  The mathematical domain questions are still more choices by us, if we make them.  The OOB access on buffers is from previous behaviour of APIs.  The cutline feels arbitrary from a previous arbitrary choice.
* MM: Depends what you mean by an “error”.  E.g. in WGSL indexing off the end of the array _is _ an error.  But in JS it’s not an error.  
* AB: JS decides to give it the behaviour.  Depends one which array it is, determines whether we give the array access the meaning of an error.  I’ve seen HLSL shaders in the wild relying on OOB indexing being 0-valued. 
* MM: Not special to constexpr.
* AB: We try to avoid undef behaviour in underlying APIs. That’s the driving reason, not the portability reason.
* MM: Doesn’t matter why, but folks are going to rely on it.
* AB:So we can choose which way to set it. Incentivize away from bad math being a useful result. That’s more useful that defining an arbitrary value.
* MM: Why discourage them from bad math, if it’s defined.
* JB: If we make it a compile time error, we can walk it back in the future.
* JB: Yes, we can give these things values, and folks might find ways to use them. But we should discourage them from using it because it’s bad code. The code will rely on a corner of the spec. It’s mean to code readers.
* TR: If there are cases where a tool is collapsing code, and ends up that because it’s able to consteval something that it wouldn’t before, then it will fail to compile, and fail to run the same thing that potentially could run with defined behaviour.  And indexing out of bounds is used in practice because of the defined rules.
* MM: This is a real issue. A code generator could hit on this case; e.g. do substitution.
* DN:  Agree with Jim’s argument.  But additionally array indexing is a “computer” concept, but trig math is hundreds of years old, so we should embed in that cultural practice. Which means to me arcsin(2) is nonsensical and the lang should not define it.  As for consistency of consteval vs. runtie evaluation, that’s an argument for reopening some of the choices for math functions at runtime too. 
* **KG: Let’s come back next week.**


### [wgsl: permit certain resource variables to overlap #3266](https://github.com/gpuweb/gpuweb/issues/3266)



* This is a duplicate of [https://github.com/gpuweb/gpuweb/issues/1842](https://github.com/gpuweb/gpuweb/issues/1842)


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* JB: I believe that most of the topics raised by MM are largely solved/handled/decided. The big thing left is whether aliasing is a static or dynamic error. Google likes this behavior because it is good to be permissive this way for supporting long translation toolchains that still work. Mozilla believes that we should take a stronger stance towards reliably-portable behavior, done at static time. DN pointed out that my proposal is a global analysis, and WG previously decided to avoid global analyses. I made a new proposal that I think might be more palatable, but might still count as “global analysis”.
* MM: I think it would be too strict to have strict static alias forbidding. I think there are valid reasonable programs that do this that we should support. Apple kind of agrees with Google there, that being permissive (somewhat) is valuable. The shower thought I had was, in languages that have real pointers, we can support copy-in-copy-out. (modulo more work) What if we had a language feature that that did copy-in-copy-out instead, since everyone could support that on all backends. So I’m proposing:
    * Adding `inout` keyword, and
    * Forbidding pointers as function params.
* AB: External person asked on this issue whynot inout params.
* AB: Future we see is that ptrs work like ptrs.  And inout is legacy. Don’t want to be carrying inout along as legacy language feature, i.e. foreverish.
* AB: Also ties into workgroup-shared inout not being reliable in HLSL. [https://github.com/gpuweb/gpuweb/issues/3276](https://github.com/gpuweb/gpuweb/issues/3276) 
* MM: inout potential feature is compatible with more powerful pointers that could be built on MSL’s pointers and SPIR-V’s variable-pointers. In that extension we could have more real pointers, and reopen the ability to use ptrs as arguments to functions.
* JB: The static error can be taken back in the future.  Wanting this to be a static error is about preventing nonportable behaviour.  We’re not against aliasing; we’re just against can’t-actually-implement-this.
* AB: Since you can do copy-in-copy-out yourself, then why add it to the language.
* MM: It’s a convenience thing. Painful to force the human to write the boilerplate code.
* KG: Want to pretend we don’t actually have inputs, and instead just skip ahead to pointers.   If the answer is indeed we can’t do it satisfactorily enough, then we should consider adding inout again.
* MM: Given authors can do their inout, then we can ban pointers as args. Logically consistent.
* JB: What’s the prospect for real pointers. Does DXIL support pointers? Do Vulkan SPIR-V get real-ish pointers.
* AB: DXIL requires it. And Vulkan requires “variable-pointers” at 1.x (for x >0 . .need to check).
* TR: Mostly true. Pointers for many things. Can’t have pointer to the middle of a resource.  Can have pointers passed between functions, where the functions are resolve (at compile time).  
* MM: Kind of buy the argument that if we wait that SPIR-V will get better.  Don’t buy that if we wait that D3D languages will get better. Because of shipping DXC is expensive.
* AB: What we can’t do is ship DXC.  Separate issue from having Chrome emit DXIL.  (bypass DXC).
* KG: e.g. fXC is 4MB, DXC is 14MB. And that’s expensive.
* KG: Peeking around corners, expect we may eventually emit DXIL directly.  Out of expedience, for now.
* KG: Maybe if we can’t get function params as ptrs in v1, do inout for now. 
* AB: Echo technically feasible to emit DXIL; better specfiied than HLSL, but question of engineering resources.  Almost would be happier to removing pointer parameters altogether.
* MM: **If we think the way forward is to remove pointer parameters altogether. And not add inout. Wait until “real” pointers land in all underlying platforms/languages**.
* KG: **Let’s talk more next week**.


### [wgsl: language evolution: managing rollout of core (essentially sugar) language features #3149](https://github.com/gpuweb/gpuweb/issues/3149)



* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, August 09, **11am-noon** (America/Los_Angeles)
* 