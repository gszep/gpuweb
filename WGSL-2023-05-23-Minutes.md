# WGSL 2023-05-23 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-04-18 WGSL - Agenda / Minutes](https://docs.google.com/document/d/11J4bZ7aMvT9j-VC3IU71eEdXK0LCUHknG2Wt93Vl1DI) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


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
    * **Erich Gubler**
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



* 


---


# ⏳ Timeboxes (until XX:30)


### [Clarify workgroup size restrictions #4073](https://github.com/gpuweb/gpuweb/pull/4073)



* _Merged_


### [Consider matching either ECMAScript or HTML for whitespace and line break definitions · Issue #4068](https://github.com/gpuweb/gpuweb/issues/4068)



* KG: Yep, we matched unicode. Suspect other web specs were defined before Unicode, and would, if defined today, do what we did.  Any noticed warts would be considered warts on HTML and ECMAscript.
* MOD: The spec explicitly states the code points; it’s easy to follow.
* **_Will say: we know, it’s on purpose. Works as intended._**


### [[wgsl] workgroup_size does not list negative values as an error #4071](https://github.com/gpuweb/gpuweb/issues/4071)



* See #4073


### [Missing intrinsic functions like rcp and rsqrt that are useful in other shading languages · Issue #4092](https://github.com/gpuweb/gpuweb/issues/4092)



* KG: Unless rcp is opting into a reduced-accuracy, then auto-detect from 1/x.  If it is a faster approximation, then think about adding a builtin for it.  Maybe post-v1?  At some point v1 has to stop so we can catch up.
* BC: They’re asking for a reduced-precision instruction.  DXBC doc says it’s “fast” intrinsic for reduced precision
* Alan: It’s 2**(-21) absolute error.  For regular divide, we say 2.5ULP in a certain range.  If this is maximum error, then it’s more widely applicable.
* MM: Can we make bounds on division such that we could use this?
* Alan: They’re not subsuming one vs. the other.   Could be documentation fuzziness.
* KG: Could ask for clarity. Sounds like weak consensus is if there is a useful use case, then add the builtin, but maybe not in v1.
* MM: If people want to write rcp, we should let them. The error bounds are less important.
* Alan: The author wants it for speed, not because they like writing those letters.
* KG: **Proposal welcome for builtin if there is underlying platform support.  Not v1**.
* DAvid: What proposal would we allow?  If platform doesn’t have rcp can we polyfill with 1/x?  But as I understand 
* AB: Also requested reciprocal sqrt.
* KG: Request to record the accuracy investigation.  Don’t want authors to have to think hard if they want the most accurate answer.
* MM: **Don’t think these functions are worth an optional feature** based on availability of underlying hardware intrinsic.
* KG: **Suggest e.g. fast_reciprocal which is “the fastest one available”, with chosen implementation that is fastest**.  It’s author signal they just want the fastest one.


### [Feature request: a way of loading row matrices · Issue #4094](https://github.com/gpuweb/gpuweb/issues/4094)



* KG: two issues.  Rowm vs. colm memory access, and getting the last column to inject the 0,0,0,1.  
* KG: Consider using ‘transpose’ for converting.  Second half is the 0,0,0,1.   For that, consider an automatic expansion that fills those values in.
* AB: Seems easy in the spec, and well-defined.  No opinion on how generally useful it is.  Seems similar to vector construction that allows you to give mix of scalars and vectors to make a wider thing.
* KG: The ambiguity here is …
* JB: Seems would be nice to have mat4x4 overloads that added the 0,0,0,1
* **KG: What about getting a 3x4 from a 4x4.  **
* **JB: That loses information. Let’s not do that.**
* DN: Would be different function; a projection.
* JB: A differently named thing: homogenous value from affine?
* AB: Function to construct mat4x4 by providing 4x3 with an extra vector.
* KG: Clunkiness comes from hard to make a (0,0,0,1)
* DS: If you create a default matrix it’s all zeros, but now it extends with not-zeros.
* MM: Better to add builtin functions than to add row-major attributes to data…..
* KG: **No on attributes for row-major.**  Should we reserve row_major?
* MM: Seems we got it wrong. **Should have reserved row_major.**
* MM: Sounds like direction: no attribute.  But **proposals welcome for new std library functions**.
* DN: Yes.

(Followup.  Reviewing history, I found that row_major was dropped as a reserved word when we stopped reserving type names.  See [https://github.com/gpuweb/gpuweb/pull/3167](https://github.com/gpuweb/gpuweb/pull/3167)  

**Maybe column_major should have been removed also**?)


### [Allow modf() for scalar-vector and vector-scalar · Issue #4097 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/4097)



* KG: Works with % but not a builtin.  **Suggest post-v1.**
* BC: Similar vein to the next thing (4111).  Larger theme is implicit scalar to vector promotion. To match other shader languages.
* JB: **Slightly worried that implicit conversions have history of hiding bugs**. If lots of shader authors want this then it’s just a nice helper.  Weakly concerned but willing to go along.  Seems post-v1.  This is not minimum viable product territory.
* MM: I’m more worried about implicit conversions that lose data. E.g. float to int and backward.  These don’t do that.  Not worried about that.
* JB: concern comes from experience with C++ and Rust where things get too magical.
* BC: If we allow scalar to vector generally, does this apply elsewhere. E.g. `var a: vec3i = 1;`  Is that supposed to work?
* MM: Would take guidance from other shading languages.
* KG: Are you asking it to do more than you wanted.
* KG: Can get different meaning when vec3 to vec4 homogeneous coordinates and back.
* MM: Person asking scalar to vector not small vector ot big vectors.
* KG: Yes. I’m providing a landmark/ point of concern for saying generally promoting to vectors. E.g. if you ever have a type error, promoting t to t,n  does an implicit conversion you don’t want.  
* JB: Homogenous coordinates are tricky to work with.  Can’t just add them.  Have to be cautious about them.
* KG: Rays vs points.
* MM: To Jim’s points, it’s meaningless to add two points anyway.
* KG: Am interested in general direction of automatically expanding scalars to vectors, but have to do more investigation.  We can continue to approximate it by adding one by one, until we have bandwidth to do the general. To this point and next issue, would like to expand automatically.
* MM: Do we want to make more one-offs in the short term? Or pursue the larger feature instead.
* KG: Can do both in parallel, but post-v1.  Think we should focus on the general issue.
* JB: ECMAScript adopted principle that proposals for medium-major changes to the languages (this would be medium-scale), they have to evaluate it by actually building it, releasing to population of users that are informed, and get use experience with it.  They caught many bad choices with this process.  This committee doesn’t have an established process for this kind of experiment process.
* MM: [https://tc39.es/process-document/](https://tc39.es/process-document/)  is that stage4?  New proposals ship at stage3.
* JB: I need to look at that again.


### [Allow min()/max()/clamp()/smoothstep() for scalar-vector and vector-scalar · Issue #4111](https://github.com/gpuweb/gpuweb/issues/4111)

 \
(* See discussion above *)


### [Should there be a way to prevent loop unrolling? · Issue #4110](https://github.com/gpuweb/gpuweb/issues/4110)



* KG: Two underlying backends that can consume this. Makes me inclined to pass it along.
* JB: Like the criteria Ben suggested.  Don’t specify the meaning.  Doesn’t change the semantics.
* BC: This can be something to massage the questionable behavior of FXC. But most of the time it doesn’t do anything. Might be more helpful on compilers that behave better with the additional control. The original issue filed isn’t a compelling argument for us. Trying to run away from FXC as quickly as we can.  Hopefully won’t affect our users in future.  Haven’t checked DXC’s response to this control. Definitely consumable in SPIR-V.
* JB: If this is just a platform issue with minute long compiles. If us middleware can avoid we should just do that, rather than exposing controls to users.
* MM: Compare to ‘register’ keyword in C. Compiler has a lot of insight into what should be unrolled.  Among implementors in this group, do our implementations unroll at the WGSL level? Do loops always pass loops.
* BC: Tint does not unroll.
* JB: Naga does not unroll.
* MM: An attribute that might not do anything in any backend (in at least some cases, but maybe many cases), seems like a “not-right-now” and maybe even “never.”
* BC: Tint might in future do some.
* KG: We keep loops intact and could pass this down.
* AB: Generally not in favor of adding hints. I regard this as a hint.  Don’t think there’s a ton of value to them.  Generally your compiler should do “the right thing”.  Odd use pragmas is not the right way to deal with those problems.
* KG: context is this problem exists for a decade, at least. Standard solution is to trick the compiler into not being able to unroll the loop.  SPIR-V seems the strongest argument for this.  In practice the compiler might not be able to know how long it will take to unroll.  Is a bit of a hack. Not sure a compiler can “just fix this”. People are getting use out of them.
* MM: Avenue for compromise: wait until no implementations run on FXC, then see if there is still utility.
* KG: would rather do the opposite.
* JB: The more I listen, the more I think it’s *our* job to fix. The user doesn’t know the minute long compile is caused by loop unrolling.  Who is in a better position to investigate and understand when fix needs to be applied.  Think we should take it out of their hands. Don’t offer little switches.
* DS: We’ll run into with CTS: there’s a set of loops that we unroll on metal but force unroll on FXC.  One is slow in one and vice versa the other. Will we need to expose a feature that behaves differently based on underlying platform?
* KG: Ideally we would fix it for the user. In practiced, the whole field has not solved the problem in all the time we’ve had so far.  Think it’s too ambitious.  Tolerable amount of cruft to have the feature; we can deprecate later with console logs.  Feel relatively strongly we should have these annotations.
* AB: Feel we’re in a different position now.  Because we’re a layer removed.  Don’t see why it has to bubble all the way up.
* KG: My concern is we can’t tell which loops will blow up.
* MM: Disagree with Kelsey but your point is valid. Underlying I think the argument is: how would we know at our layer whether to emit the platform unroll hint.
* KG: Want us to be conscious that devs run into this problem for a very long time. If it were easy to fix by compilers, then compilers would have fixed it. That it’s still a problem today by multiple compilers, then it’s still hard.
* JB: It’s not that other compilers blowup. It’s a pessimization on MSL.  It’s really only FXC that we have a critical problem with.
* KG: Have heard: we want to switch of FXC.  Long term goal we might get to eventually.  We think but can’t prove that you won’t need this in a post-FXC world. But post-FXC world is years away. On behalf of developers, help those developers now.
* BC: When compilers respect this hint, it’s very useful in games development. Very useful in console games. Regardless of compiler bugs, if this is respected, it’s very useful to have.
* MM: Counterargument to consoles is you’re only targeting one device.  Same question to author: how does the author know whether to insert the hint.
* MM: Ask Kelsey are you making a value judgment which is better:
    * 1. having two years of allowing authors to get perf workaround in FXC, but then making them no-ops
    * 2. Can’t write the hint, and have implementations fix it.
* KG: Probably yes it’s worth it to have 2 years of workarounds.  (But also have doubts we will fix it perfectly.)
* MM: That’s a reasonable position to have.  Coherent.  I disagree with it tho.
* KG: Along the risk spectrum, it still may be useful in 2years anyway.   Think it’s a very light burden to hold.
* JB: The cruft argument doesn’t bother me at all.  My concern is it’s effective some platforms and wrong in other platforms. My guess is we could do a better job trying to handle it ourselves. It’s different than the last 10 years because we’re a new layer in a position to fix things.
* MM: Think that makes sense. Compilers already have sophisticated logic on how /when to unroll.  Making those complex decisions more sophisticated by adding compilation time as an input to the decision seems reasonable.
* **(no resolution)**


### [Allow mutable function parameters with the mut keyword. · Issue #4113](https://github.com/gpuweb/gpuweb/issues/4113)



* 


### [Add dereference operator · Issue #4114](https://github.com/gpuweb/gpuweb/issues/4114)



* 


---


# ⚖️ Discussions


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* KG: Haven’t done my homework.
* MM: No new comments since a month ago.
* **_Discussion deferred_**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday May 30, 2023, **11am-noon **Americas/Los_Angeles
* 

 
