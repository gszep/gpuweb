# WGSL 2022-10-04 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** **(APAC!)** Tuesday **5-6pm **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-09-20 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1FQX6vZ09p2u4jN9IZg0SusWqyPT9pII0mmsdJ56H6Dc) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Myles C. Maxfield**
* Google
    * Alan Baker
    * **Antonio Maiorano**
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
    * Erich Gubler
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



* WG charter update: [https://github.com/gpuweb/admin/pull/18](https://github.com/gpuweb/admin/pull/18) 
* [https://github.com/gpuweb/gpuweb/pull/3469](https://github.com/gpuweb/gpuweb/pull/3469)  Allow attribute params to be u32 or i32 when they were only one or the other, and already had bounded ranges.
* [https://github.com/gpuweb/gpuweb/issues/3253](https://github.com/gpuweb/gpuweb/issues/3253)
    * DN: there were 3 PRs landed for the compile-time side, and the runtime side was split off and not blocking.  So close this one?  


### CG Charter



* **WGSL Only: [https://github.com/gpuweb/admin/pull/19](https://github.com/gpuweb/admin/pull/19)**
* 


### More APAC meetings during Nov and Dec?



* MM will be out Nov and Dec, but DG can make APAC timeslots. Shall we have more APAC timeslots for these two months?
* Generally: yes, more APAC is fine, though we generally won’t have BC for APAC timeslots. (Current APAC slot is 1am for BC)
* DN: I think more-APAC-meetings will slow down BC’s great feedback for Nov+Dec, but that’s alright for the short term.
* DG: We might be able to move the slot an hour earlier (which I can still make) and make things slightly more reasonable for people further east. Though China is two hours off from me I think, so we should check with Intel Shanghai. 
* KG: I’ll talk with CW and we’ll check with our non-APAC members (who aren’t all here today) for more feedback.


---


# ⏳ Timeboxes (until 5:15)


### [wgsl: add way to get special values for a given numeric type (e.g. max, min, lowest) #3431](https://github.com/gpuweb/gpuweb/issues/3431) 



* DN: All options seem bad. Proper mathematical names confuse programmers. Let’s [just make a table?]. Maybe no change, but better docs would help. Unless someone has a better set of name ideas.
* DG: MM and I were kinda confused by `unit`, but maybe that’s coming from category theory or something?
* DN: Yeah maybe? [...] It still ends up being confusing if you don’t have a background in group theory.
* DG: Even though other options may not be the perfect option, it would be preferable to match what existing APIs do.
* DG: E.g. GLSL doesn’t define these
* DN: **I’m going to withdraw my proposal here, and I encourage someone else to make a different proposal.**
* MM: Could do what C does. [for example]
* DN: As you add new data types, it has to grow, and FLT_MAX is not good for that.
* DG: I agree that min and max are natural names for this, but we have this overloading with our builtins.
* KG
* MM: **If I came with with a proposal that was like three words long for each, would that be objectionable? (“no, make a proposal [please]”)**
* 


### [wgsl: Define trunc, ceil, floor for infinities #3494](https://github.com/gpuweb/gpuweb/pull/3494)



* (DN thinks this is slam dunk. Surely this is slam dunk)
* JB: +1
* MM: I thought we were in the world where infs cannot be relied upon. So is this testable?
* DN: Yes, that’s our world, and that’s kinda an issue that overshadows the whole spec. So strictly, probably not necessarily testable.
* JB: It can’t give you an inf unless you pass in an inf…
* MM: It’s more about that infs can disappear at any time, not so much a boolean “infs work” or “not”.
* TR: 
* DN: I”ll defer to when we removed isinf. We said, can’t rely on compiler not inserting stuff, because of transforms. 
* DG: Do we assume a fast-math env?
* DN: Yes
* MM: Legal def of isnan would be return false?
* DN: Yes, that’s part of why we removed them.
* DG: So to TR, wouldn’t it not be possible to check for nans from loads?
* MM: It might be possible to preserve.
* TR: There are those that want to test for e.g. nans before, while still in –fast-math. 
* MM: So even in HLSL with fast-math, people get mileage with isnan.
* KG:** So accept this? (Yes)**
* MM: Also MM proposes re-adding isnan, just with a big warning
* DN: I don’t want that
* KG: I think I’d be ok adding things to the shadowy area that DN described earlier
* BC: We had sec issues here
* TR: To be clear, we always had isnan, it just used to be rather useless, and people wanted it to be at least a little useful.
* MM: Maybe we 
* JB: Can’t nans not even reliably be detected, e.g. in Vulkan?
* KG: I believe the idea is to detect this load+isnan chain and desugar that into a u32 load and a bit-test.


### [Validating maxComputeWorkgroupStorageSize #3485](https://github.com/gpuweb/gpuweb/issues/3485)



* _Not discussed_


### [invalid expressions in const-expr and override-expr should be error (no later than pipeline creation time) #3253](https://github.com/gpuweb/gpuweb/issues/3253) 



* _Not discussed_


---


# ⚖️ Discussions

Ben Clayton would like to present some slides covering the existing pointer restrictions and proposals so far (10 mins)

>>>> **[Slides here](https://docs.google.com/presentation/d/1JH9F9U1Gc-XfrYGYcIcwtZx5JMvO-mLFwK3GNAquV74/edit?usp=sharing&resourcekey=0-Oc8BHDGlVef-aOybMsAT3w) &lt;<&lt;<**


### [wgsl: pointer-to-workgroup in user-defined functions · Issue #3276](https://github.com/gpuweb/gpuweb/issues/3276) 



* AKA “function-out-param language functionality”
* See also:
    * (pr) [wgsl: ban ptr-to-workgroup params to user-defined functions #3422](https://github.com/gpuweb/gpuweb/pull/3422)
    * (issue) [Tuple-like-returns proposal for function-out-params functionality #3501](https://github.com/gpuweb/gpuweb/issues/3501) 
* Ben presented slides.
    * (Will share once we figure out public access)
    * 
    * Tint patch for the pointer rewriting: [https://dawn-review.googlesource.com/c/dawn/+/103860](https://dawn-review.googlesource.com/c/dawn/+/103860) 
* GR: Want clarification. Multiple return proposal: is that _instead _of pointers?
* KG: Yes
* GR: Are we constrained by being able to translate SPIR-V to WGSL?
* BC: Want many basic things from SPIR-V, including that pointer-param feature.
* GR: When I asked “short duration” between v1.0 and 1.1, but still don’t have good sense of that gap.  We have ever slipping dates for v1.0.
* TR: Not sure I followed the transformation.  Not sure how it would handle all the cases for HLSL.
* BC: Deliberately skimmed over the details.  Can point you at code.
* TR: Is the gist that you generate variants of helper functions…
* BC: Gist is you pass the base pointer, and the dynamic indices. The rest of the chain is static and you bake it in.
* TR: For HLSL, need approach that gets to the base variable, not a pointer to it. Because the definition is copy copy out.  So
* DN: So BC’s transform keeps unwinding the access until it bottoms out in private and uniform. For function, pass in the entire variable, and then pass as param and the indices. His transform does bottom out for bottom-scope variables. 
* TR: So it doesn’t pass the actual value up through the chain?
* DN: Right. For vars that are declared at module scope (buffers, workgroup), you end up bottoming out at the module scope variable.  For function scope you pass in the whole function-scope variable as an extra parameter to the helper function; in addition to the dynamic indices used to index that variable.
* TR: 
* BC: It’s the number of unique shaped chains on the path from caller(s) through access
* BC: In HLSL that does mean that you *may* get copies, but it would technically work, but be a perf cliff
* TR: It reduces the number of variants to do it that way
* JB: One way to understand this, is for every call, check how much can you manage by passing in values, and how much can’t you. For anything that you can’t do dynamically, you could bail out to a verbose static alternative
* BC: We have end2end tests that check generated MSL HLSL SPIR-V and then validates those through the compiler.  And those work.
* JB: An idea of “proof” that it works is that if you fully inlined then you’d get an expanded version of what this does.
* TR: Right…. It’s specialized.
* KG: It’s nice it came together quick/well in Tint.  Not sure about Naga.
* JB: 
* MM: 3 things, #1: We’re generally positive, off the cuff here. #2, because we just heard this, we don’t wan tto have a super substantial discussion yet, and want to discuss it offline before e.g. deciding. #3: In the slides, you listed a bunch of options, and then listed a bunch of paths forward. Do you think you can enumerate the paths forward again?
* BC: I can re-present the slides.
    * Proposal: Keep spec as-is
    * Proposal: Keep spec as is, lift restrictions in V1.1
    * Proposal: Keep spec as-is, add extension
    * Proposal: Remove pointer restrictions on function parameters (but keep aliasing).
* DN: About a week ago, I talked about tradeoffs that we implicitly+explicitly listed what we’re deciding with. One was “no heroics”, to avoid complexity. I would say that BC’s thing is great, but counts as “heroics” to me. So it does do the thing to solve this issue, but it’s kinda compromising by allowing “some heroics”. It’s TBD what e.g. Naga ends up feeling the complexity here. Does thing go from “there’ a lot of work to do” to “there’s a crushing amount of work to do”?
* MM: Many props to BC here
* **Let’s think amongst ourselves and meet back next week.**


---


# 📆 Next Meeting Agenda Requests



* Next week: Tuesday, October 11, **11am-noon** (America/Los_Angeles)
    * non-APAC Oct11
    * APAC again Oct18
* 