# WGSL 2022-04-26 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** KG, DS

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday 11am-noon Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-04-19 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/19BiwLfTepYLWzpk9HihUfIlHnKZGFFikofIeKQJVAo8) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * Robin Morisset
    * Daniel Glastonbury
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
    * Jaebaek Seo
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
    * Tex Riddell
* Mozilla
    * Dzmitry Malyshau
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Connecting Matrix
    * Muhammad Abeer
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* UC Santa Cruz
    * Tyler Sorensen
    * Reese Levine
* Dominic Cerisano
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
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


---


# ⏳ Timeboxes (until 11:30)


### [Consider making binding_array a reserved word #2794](https://github.com/gpuweb/gpuweb/issues/2794)



* **Yes, let’s reserve.**


### [Consider allowing creation-time expressions for attributes #2790](https://github.com/gpuweb/gpuweb/issues/2790)



* MM: **Probably good**
* KG: **Post-v1? (yes)**
* AB: Maybe not e.g. enums/interpolation attribs, but these are fine for now


### [Missing Abstract Int/Float preconditions for Conversion Expressions #2788](https://github.com/gpuweb/gpuweb/issues/2788)



* AB: I think this is already covered in the spec.
* **Let’s close.**


### [Clarify that evaluations to inf/nan may yield undef value #2777](https://github.com/gpuweb/gpuweb/pull/2777)



* AB: We should probably use the same language as e.g. OOB access.
* BC: Concern with “impl defined” implies a consistent behavior, and we don’t have that.
* MM: What is the granularity of this? E.g. can multiple runs have different results? Are results different within the same execution?
* MM: “The value of an undefined value would not change from one line to the next” such that we don’t get e.g. undefined branchin.
* JB: THis is something a user wants to know. Like “can I dupe this expr and have it have the same result”.
* AB: I agree that this sounds useful but I’m not sure how I (or MM?) would implement it?
* MM:
* AB: I know how I’d do it in llvm, but not sure about it other backends.
* MM: One solution: “For each expression that can produce UV, I will produce a value”, but if you want to rely on what spir-v has, I’m not sure what spir-v can guarantee w.r.t. UVs.
* AB: I wouldn’t want to have to insert e.g. clamping code after every UV application. I would like to distinguish between cases where we can guarantee it, vs cases where the underlying system has UV issues.
* MM: Less concerned about the portability than the ability for an author to get reasonable logical execution. E.g. skipping branches.
* BC: Not sure if it’s possible to do better.
* KG: Can we call it “arbitrary value”? Then we can rely on normal logical flow.
* MM: Yes that’s cool what I’m asking for.
* BC: Still not sure it’s implementable.
* KG: Can you give us examples where even branches wouldn’t be able to safely give us arbitrary values?
* BC: **Yes I’ll find them.**


### [Don't reserve buffer, texture, in, out, input, output #2774](https://github.com/gpuweb/gpuweb/pull/2774)



* AB:** Ship it**
* JB: Some examples I ran into. Others tripped over by internal teams. Take those criticism to heart as haven’t put too much thought into these reserved words
* MM: Reserved words are only useful if they can have ambiguity later. For things like buffer can’t see where we could have an ambiguity.
* GR: Spoken about buffer, what about in, out and inout?
* MM: Written often in HLSL, yea?
* GR: Right. Will there be a simillar concept? If not and we’re sure of that?
* JB: Oh, saying we’d like to reserve those as we may want to add those features?
* GR: No so absolute but don’t wan’t to foreclose
* MM: Solution for in/out is pointer
* GR: Want to consider independent of buffer and texture. If we’ve considered and are still good, will go along with that
* KG: Used to in/out from GLSL. In own code for utility use in/out as variable names. Out used a lot. Maybe because of c++ out var. Would like to not reserve and lean in that direction. Will we never have in/out …
* MM: For Metal concept of in/out is not sufficient to describe code needed to generate. Function with in/out and caller calls with thread var and acls again with device variable we have to specialize due to template or copy function. Would have opinions if we wanted to add in/out like GLSL and HLSL have.
* KB: Flush things out a bit, would more prefer to not reserve in/out but still reserve ???. Still have time to add back if we get spooked.
* JB: This PR not intended to engage future language features. Shouldn’t try to decide if we like in/out in language as part of this PR. Would be happy to drop those words from this PR. Already changed internal examples.
* MM: Another path, if we want to add in/out we spell them as @in and @out. 
* JR: Right. Same with special attributes on the pointer types.
* KG: Satisfied we’ve though about them to a sufficient degree.
* GR: Yup, just wanted to consider independently.

(5)


### [Clarify conversions: floating, and out-of-range AbstractInt #2773](https://github.com/gpuweb/gpuweb/pull/2773)



* KG: PR from DN. Needs review.** I’ll try to review **…


### [Potential parsing conflict with const literals and postfix expsesions. · Issue #2772](https://github.com/gpuweb/gpuweb/issues/2772) 



* DS: Just about moving it down in the grammar, though technically with greedy parsing it’s not a problem just yet. Also we can move it again later if we like.
* **Spec needed**


### [Add staticAssert #2759](https://github.com/gpuweb/gpuweb/pull/2759)



* **Just waiting on dneto, then ship it**


### [Change integer texture parameters to unsigned #2722](https://github.com/gpuweb/gpuweb/pull/2722)



* 


### [[wgsl] Split regex apart by segment. #2762](https://github.com/gpuweb/gpuweb/pull/2762)



* 

(10)


### [in certain contexts, an identifier does not resolve to its declared object · Issue #2486](https://github.com/gpuweb/gpuweb/issues/2486)



* 


### [wgsl: Missing specification on modf behavior for negatives. · Issue #2468](https://github.com/gpuweb/gpuweb/issues/2468)



* 


### [wgsl: limit layout size of a type · Issue #2118](https://github.com/gpuweb/gpuweb/issues/2118)



* 


### [WGSL: Allow global let-declarations initialized with constant expressions · Issue #1911](https://github.com/gpuweb/gpuweb/issues/1911)



* 


### [WGSL grammar needs update for var type inference #1805](https://github.com/gpuweb/gpuweb/issues/1805)



* 

(15)


### [Are padding bytes of structures intialized with zero value expressions? #1747](https://github.com/gpuweb/gpuweb/issues/1747)



* 


### [specify behaviour of floating point operations on NaNs #1711](https://github.com/gpuweb/gpuweb/issues/1711)



* 


### [Uniformity of Private-class global variables #1579](https://github.com/gpuweb/gpuweb/issues/1579)



* 


### [support volatile atomics · Issue #978](https://github.com/gpuweb/gpuweb/issues/978)



* 


### [Use brackets for array types · Issue #854](https://github.com/gpuweb/gpuweb/issues/854)



* 

(20)


### [shader programming model: use little-endian layouts for scalar data #234](https://github.com/gpuweb/gpuweb/issues/234) 



* 


### [WGSL pointer aliasing rules · Issue #1457 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/1457)



* 


### [Issues of pre/post matrix multiplication in wgsl #2481](https://github.com/gpuweb/gpuweb/issues/2481)



* 


### [wgsl: add 'array' overload to construct an array from components, infering type and count #2346](https://github.com/gpuweb/gpuweb/issues/2346) 



* 


### [Annotation for the uniformity analysis #2323](https://github.com/gpuweb/gpuweb/issues/2323)



* 

(25)


### [WGSL should forbid pointers to runtime-sized values as user-defined function parameters · Issue #2268](https://github.com/gpuweb/gpuweb/issues/2268)



* 


### [C-style casts · Issue #2220](https://github.com/gpuweb/gpuweb/issues/2220)



* 


### [☂️ Initial internal feedback #2213](https://github.com/gpuweb/gpuweb/issues/2213) 



* 


### [How should WGSL map to the availability/visibility parts of the Vulkan Memory Model? · Issue #2228](https://github.com/gpuweb/gpuweb/issues/2228)



* 


---


# ⚖️ Discussions


### [WGSL parsing assumes context-aware tokenization #2717](https://github.com/gpuweb/gpuweb/issues/2717)



* Previously
    * DN: Working on parser generator/analyzer. which can now determine LL1-ness, but WGSL is not. Not the intent to make it so.   Working on upgrading to LALR-table generation.
* Waiting on dneto.


### [Pointers in uniformity analysis · Issue #2741](https://github.com/gpuweb/gpuweb/issues/2741)



* Previously
    * JP: If we’re going with option 2, should have feedback this week.
* JP: Implemented and works as expected. Still has complexity increas with number of function parameters, but we have a limit on those too.
* **SPEC NEEDED**


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* JP: Essentially, the only way two invocations could get different values from a load from the same location, is from a data-race. So the only way it could be non-uniform is if it is a data race.
* MM
* JP: If E’m a shader compiler that’s presented with this case…
* JP: In either of the two cases, do users want workgroup broadcast? I worry that workgroup broadcast isn’t the right tool to tell the compiler that “I don’t have data races in my shader”.
* MM: So you’re imagining a program where an if statement has a non-uniform load. The programmer “knows in their brain” that the load is uniform, but needs to tell the compiler that “I know you can’t prove it, but trust me”, because otherwise it’s slower.
* JP: Yes but I think a user should prefer to load from a read-only storage.
* MM: Well there’s SIMD broadcast, and that’s useful even without uniformity analysis. I think that shows that this kind of broadcast would be useful even without UA. 
* KG: Is a “arbitrary value” concept a useful concept here?
* MM: After talking with the Metal team, `volatile` is the officially supported way to ensure that UV doesn’t cause UB for bounds checks. That is, when the index being bounds-checked is an undefined value, you can store it in a volatile variable, and then check the volatile variable in the bounds check. Making the array indexing operation conditional on that check ensures that the value produced is not undefined.
* JB: Is that written down?
* MM: No
* KG: Just in our minutes now. :)
* KG: It feels like WB is like volatile but for workgroups.
* MM: 
* JP: What I was proposing here is to say that loads from writable storage buffers are still uniform. …
* JP: In the spec, underlying APIs define things as UB. Where does WGSL talk about this?
* AB: We defer to Vulkan here.
* JP: I think what I’m hearing is that “practically, these are UV not UB”, and that maybe we can just spec that?
* AB: The UB comes from a complicated place. People think they can “ask for a value from memory”, but lots of things go on behind the scenes that make this much more complicated, and e.g. x != x. It’s because of assumptions the compiler has made, that cause these surprising things. 
* AB: If we get to the point where we want races to not be UB anymore, we might make a different decision on this topic.
* KG: Could we use volatile for that?
* MM: We’re sort of saying “data races gonna data race”, so long as we can be safe. Having every load be volatile would be maybe too expensive, but doing it for every array access would be doable. 
* KG: So you could get UB, but it’s “safe UB”. 
* …
* MM: Probably more than half of WGSL programs will have races somewhere, so it feels too pessimistic to say that these programs are all SOL, we we won’t try to help them.
* AB: Do you think it’s that many programs?
* MM: Yes
* …
* MM: So I’ve described safe bounds checks in Metal. How would you do it in spir-v?
* AB: I don’t know as our guarantee always happens in shader code. Might need hardware requirements.
* MM: Would you require RBAB for WGSL?
* AB: No, but we have other things we can maybe do.
* AB: Spir-v does have a volatile, but it’s not necessarily enough.
* MM: Is volatile required/core?
* AB: It’s core, you can always annotate with it.
* KG: But it might not give you what you need here?
* AB: Certain classes of issues go away with volatile (no rematerialization of loads) but it doesn’t make races go away


---


# 📆 Next Meeting Agenda



* Next week: **Non-APAC!** Tuesday, May 03, **11am-noon** (America/Los_Angeles)
    * There is a Chinese holiday next week, so Intel Shanghai would not be able to attend.
    * Let’s schedule for the following week, on **May 10 for our APAC this month**.
* 