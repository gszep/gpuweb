# WGSL 2022-05-10 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DS

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **5-6pm** Americas/Los_Angeles **(APAC!)**

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-05-03 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1ydLXqpn6gs0ZsPhWfL5QToY7RiA5X78YFUoX3P6G09g) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * Robin Morisset
    * **Daniel Glastonbury**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
    * Jaebaek Seo
* Intel
    * **Hao Li**
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
* Jeremy Sachs
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

FYI:  Landed [https://github.com/gpuweb/gpuweb/pull/2848](https://github.com/gpuweb/gpuweb/pull/2848)   



* clarification/fix for Conversion Rank table:  abstract vector-to-vector and matrix-to-matrix conversions inherit conversion ranks from their component type.
* Vector constructors work over abstract numeric types  e.g.   vec3(1,2,3) is  valid.


---


# ⏳ Timeboxes (until 5:30)


### [Clarify workgroup variable initialization value description #2816](https://github.com/gpuweb/gpuweb/pull/2816) 



* DN: Talked about constructing with 0 but atomics aren’t constructable so feed it with the constructable internal type
* KG: Had recommendation up for review. Feels either very carefully worded or accidentally worded. Having in parens seems better but couldn't edit it.
* AB: Will make change now.
* KG: Merge when ready


### [Adopt n11n note from i18n specdev #2845](https://github.com/gpuweb/gpuweb/pull/2845) 



* Landed.  :-)


### [Referencing the Unicode Standard · Issue #2829](https://github.com/gpuweb/gpuweb/issues/2829) 



* (David recommends no change)
* DN: i18n reviewer recommended referencing the unicode standard, we specified 14.0.0. We should keep it to be exactly what we’re testing and can expand in future if desired.
* MOD: Agree
* KG: No preference. Sounds like our desire is relatively weak. Out of an abundance of caution. Sounds like they’re saying because there isn't really a reason why married to specific version, could choose to make generic and have as of today no loss of accuracy. Only if unicode changes in 15 would it matter for us. Doesn’t mean we have to change, just is that an accurate description
* MM: Relevant question, if implementer and want to implement wgsl and system has unicode 15 what do you do? Just use 15? If that’s the case, I agree with original request and don’t write 14 in spec.
* DN: If you do use that version and it passes conformance then you’re fine.
* KG: True also. Unless we have stronger opinions, close for now and we can change later. Say we’re satisfied for now.
* MOD (in chat): +1, tables are small, good for tests and other concerns


### [Clarify behavior of float division/remainder by 0 · Issue #2798](https://github.com/gpuweb/gpuweb/issues/2798) 



* AB: Float div by zero is well defined. We don’t really support infinities so ends up being undefined value. Referencing spirv it’s undefined value vs scary undefined behaviour. Dont’ think we can do better then undefined value.
* KG: Arbitrary value? Afraid of calling undefined values, if you branch based on undefined value you get undefined behaviour. Any word that means undefined without saying undefined we’re in a better position. Floating arbitrary value instead
* AB: **Separate issue. Should lump them all together and figure out what we want to say about undefined value. If text already in spec that’s already undefined in other languages (extracting from composite). We should make decision on whole.**
* KG: Sounds great
* MM: Performance vs portability trade off?
* KG: Infinities aren’t always extant on the platform. Ideally the implementation gives you something. Decides i don’t do infinity so gives max value or 0 or maybe 3. Ideally, restriction trying to draw this isnt’ undefined value over branches causing UB. Would it be less scary if saying this is what we’re calling an undefined value which can not cause ub? \
MM: Less scary. Less worried about portability. Should investigate div by zero polyfil for integral values to see cost
* DN: Division by epsilon is also overflow. Problems occurring at 0 also occur close to zero. So no useful polyfil without being conservitive and making div not div anymore
* KG: If investing if polyfil will work, need idea of what polyfill would be. What would you want it to be
* MM: Don’t know …
* KG: Can I task you with that?
* MM: Probability low. Not going to stand in way, just stating values.
* KG: Leaves with something where we don’t technically need to do anything except for
* AB:  &lt;chat message>
    * Alan Baker8:13 PM
    * what about (roughly):
    * x / y = round(y) == 0 ? 0 : x/y
    * 
    * If you preround the value potentially, think it might cover other cases. Woudln’t be happy about making floating point slow but potentially something like this could work. Don’t know how many devices we’d need to test on.
* KG: Would polyfill and normalize just this case, but infinity on load would still get undefined values. Having our cake and eating it to in that maybe we have infinity
* AB: If have infinity the frem case maybe bad but hte ?? case i don’t know. The ?? If  supporting infinities don’t know what you’d get out of that
* KG: If you put x/y in polyfil and compared with infinity from load/store then not equal unless platform doesn’t have infinities and normalizes into zeros.
* AB: Just picked 0 because we picked for integer division
* KG: Part of exercise is figuring out where boundaries of portability vs allowing infinities is. 1/0 has well defined value if platform has infinity. Let’s think more outside time box.


### [support volatile atomics · Issue #978](https://github.com/gpuweb/gpuweb/issues/978)



* DN: Looked again today, in discussion for atomics agreed that the atomic ops should act as though they’re volatile but decinded ot not add volatile. Don’t see that intent in text in spec but don’t know what to add to spec to codify that in a satisfying way without detailing execution model. Want to take KG suggestion and post-v1. Have understanding when doing translation that if underlying thing can have volatile that’s what we mean
* AB: In my opinion implemetnation detail.
* KG: As long as no-one defects on implementation. MM opinion is no sane compile would defect?
* AB: Think talk was opposite conclusion and they would optimize atomics.
* DN: At best add conformance test with while loop and atomic load and say that thing will terminate in 2 threads
* AB: Worry about requiring forward progress. Don’t know if you can loop and trust result and separate thing to get into that model
* DN: Push post-v1 at best or close.
* KG: Anyone want to do more here? Or are we satisfied. We can always re-open later.
* DN: Other thing, have no idea hwo we’d enforce this in HLSL for example. Would have to look, but guess is not necessarily stable. Don’t want to promise in spec and then test and can’t follow through.
* KG: Would be OK, then would realize we’d fallen short and can adjust language. Don’t have to skip straight to right answer. Would like to record intent, even if no text in spec, intent is behaves as if volatile and we have forward progress. Having intent and can lookup issue later and we fall afoul we can lookup and have on record.
* 

(5!)


### [Clarify conversions: floating, and out-of-range AbstractInt #2773](https://github.com/gpuweb/gpuweb/pull/2773)



* Waiting on KG review.  (others?)
* KG: Was supposed to review this … slight concerns when going over, will get back on this. In regards to feels like algorithm doesnt’ feel like returns a value in the normal case. Say set x to original value and x is fp with more size then ??? then can truncate and if x on the extreme is result otherwise it’s infinity with same type of x. If strict on algorithm either get max/min or infinity. Never return x normally.
* DN: I think that’s in previous case, lets take offline. Added 1 word today, the previous clause if between 2 values then must be 1 of two. But meant 1 of 2 finite values.


### [Use brackets for array types · Issue #854](https://github.com/gpuweb/gpuweb/issues/854)



* KG: Instead of array&lt;f32,4> would be array&lt;f32>[4]
* AB: Push past v1 at least.
* KG: No desire to keep in v1
* MM: Probability can carry forward is non-existant.
* KG: So, post-v1? Something we might want to do?
* MM: Yes.


### [shader programming model: use little-endian layouts for scalar data #234](https://github.com/gpuweb/gpuweb/issues/234) 



* Closed.


### [WGSL pointer aliasing rules · Issue #1457 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/1457)



* AB: Pushed some clarifications around this section
* KG: Sounds like need info from MM
* 


### [Issues of pre/post matrix multiplication in wgsl #2481](https://github.com/gpuweb/gpuweb/issues/2481)



* DN: BC interacted and impression was person hadn’t understood what was done. Haven’t read myself.
* KG: Proposal to add row/column major. Case where not really sure it will matter. The abstract operations do count differently but in silicon may not be faster either way. Feel like we want to say doesn’t matter and to come back if find out it does matter.
* MM: Sounds like a post-v1 feature.
* KG: Can through in that box.
* MM: **Retitle also to actual request instead of `issues`**

(10!)


### [wgsl: add 'array' overload to construct an array from components, infering type and count #2346](https://github.com/gpuweb/gpuweb/issues/2346) 



* Closed. Withdrew the proposal


### [Annotation for the uniformity analysis #2323](https://github.com/gpuweb/gpuweb/issues/2323)



* KG: Seems like bigger topic. Probably not time boxy. Need to look and decide if still  applies.
* DN: Innovating by adding uniformity analysis. Know folks will hit bumps, donig for reasons. Want to implement, get feedback and if we need it, revisit. Propose pusing past v1 instead of pre-anticipating problems too far along.
* KG: Another way, push past v1 unless someone finds bigger issue and we need to tackle.
* DN: Right.
* 


### [WGSL should forbid pointers to runtime-sized values as user-defined function parameters · Issue #2268](https://github.com/gpuweb/gpuweb/issues/2268)



* JB: Just looking to see spec has changed. Looks, as far as I can tell, that there is no disagreement you can’t actually have a pointer workgroup const argument. Pointer to unsigned struct in workgroup address space. But, there isnt’ any language htat prevents writing such a type and no language which prevents user defined function from have a type. While Tint rejects, thats what I’m aiming for and want Naga to do the same, think Tint is overstepping and the spec doesnt’ support the rejection. If missed something, let me know.
* DN: Came to same conclusion this afternoon. Just a matter of finding good way to say it.
* JB: All agree on what we want, just finding right spec language.
* KG: Marking as; resolved, needs spec.


### [C-style casts · Issue #2220](https://github.com/gpuweb/gpuweb/issues/2220)



* KG: Notably, might be better then it used to be, don’t remember timeframe operating under. 
* DN: Commented in october we shouldn't do it and got 10 thumbs up. Don’t think we should do this.
* KG: Sounds like consensus. Closing and saying no.


### [☂️ Initial internal feedback #2213](https://github.com/gpuweb/gpuweb/issues/2213) 



* 

(15!)


### [How should WGSL map to the availability/visibility parts of the Vulkan Memory Model? · Issue #2228](https://github.com/gpuweb/gpuweb/issues/2228)



* 


---


# ⚖️ Discussions


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* MM: Tried to comment to move forward. Even if we can’t agree function is necessary for uniformity, there are still reasons for it to exist. Reasons to exist were stated before. Mirrors similar but not exactly other functions.
* KG: Feels like consensus to have this thing just do we count it as uniform or not
* MM: What is it
* KG: Workgroup broadcast
* MM: Output?
* KG: Yea, broadcast read
* MM: Didn’t realize it was suggest the output would not be uniform
* AB: Should we just make refs to global vars uniform and not add this vs adding this function and leaving globals as non-uniform
* MM: Place we start having strong opinions. What does it mean analysis declared non-uniform can we rely on it or just htat if the program has no data races then it’s uniform? Hoping we can solve that issue
* KG: Would be valuable to me given MM point that underlying platforms have bultins for this to have this and it’s an easy thing to mark as uniform i think. The other thing we can either figure out another approach or leave as is. Can do it separately. Inclined to having workgroup broadcast
* MM: One path formward that might help, can we separate having workgroup broadcast vs reads of read-write buffers being uniform. If separate can get resolution on this issue and discuss harder later.
* AB: Suggested earlier due to just uniformity. Do you have use cases?
* MM: Don’t have use cases, just symmetry argument
* KG: Because underlying impls have it?
* MM: Don’t have it, have something similar
* AB: Better left to an extension where we have something more close ot underlying?
* MM: This woudlnt’ be part of subgroup extension as there are no subgroups in this. Don’t know if this should be in an extension or not
* DN: Do you mean workgroup or subtgroup broadcast?
* MM: All subgroup have to be in extension as not in v1. Proposal we split issue and try to get resolution on this issue. Discuss reads from rw buffers separately. 2nd issue has to be in v1, this doesn’t have to be v1.
* DN: Not convinced. Leaning against adding workgroup broadcast as it doesn’t exist in underlying platforms. Synthesizing out of parts and feels like we’re going a bit further then we should. I would want to consider this post-v1 in any case. Regardless of resolution of other bit. In part portability aspect, if synthensizing high likelihood different platforms have different performances and that’s not good for ecosystem.
* KG: Think splitting out issue from this and saying must finish other for v1. Think that’s accurate and happy to do that. Generally in favour of leaving workgroupBroadcast to post-v1. 
* MM: Sort of what i was going to mention. Community group will get uniformity analysis feedback and that may drive it.
* AB: I don’t know that solving uniformity question needs to be pre-v1 as we analyze the global variable as non-uniform and tighten it to uniform it later.  Doing so would only allow more programs, not invalidate existing working programs. \
KG: If we say it’s uniform
* AB: If we say it’s non-uniform which is what spec says
* MM: So, latter if we change from non-uniform to uniform then you’re allowing a strictly larger set of programs.
* AB: Yes.
* MM: Sounds legit.
* KG: Does that mean don’t have to discuss other issue in v1? \
AB: Not a requirement. HOpefully uniformity feedback will inform if we need something extra. Need more feedback, don’t want to spend tonnes of time without more feedback
* KG: Sound good MM?
* MM: Sounds reasonable. Think this would be reasonable to have but ???
* KG: In marking uniform vs non-uniform satisfied as leaving not uniform?
* MM: Would have been my position, so satisfied with that.
* KG: Think we know enough to move forward.


### [Are padding bytes of structures intialized with zero value expressions? #1747](https://github.com/gpuweb/gpuweb/issues/1747)



* MM: Think we left off with trying to understand SPIRv behavior where if you have an attempt to ?? a progra.m Have a struct type, and buffer with struct type and use op access chain to get pointer to member of struct and do that twice. 2 pointers to 2 members with padding. Have spirv program that stores to first then second, are the bytes between the 2 pointers guaranteed to be untouched
* AB: Vulkan says you can touch the padding between but not at the end.
* KG: What is context for between and end?
* AB: Know layout of struct if there are 2 members with padding between vs this is padding after last member. 
* KG: This is the compiler knows because vulkan is
* AG: Strongly typed. You always have provenance. You know where the pointer came from and you have the layout of the structure.
* KG: Right. Deterministily pointer. If we had ??? pointers this wouldn’t work.
* AB: Don’t know what you mean by free base
* KG: Pointers you can do arithmetic on
* AB: converting integers to pointers (which is in OpenCL, for example, but not Vulkan shader languages without extension)
* AB: That being said, not know how well conformance tested vulkan is. Could test using physical storage buffers where you can generate pointers without true knowledge of where they are in the buffer.
* MM: Believe claim is vulkan doesn’t specify if stores to padding bytes or not. But there are no tests, it’s impossible to test
* AB: One way in vulkan to produce pointers to integers. (vk_khr_buffer_device_address which enables SPV_KHR_physical_storage_buffer) Just giving them a type. Don’t know if implementation would respect padding or not. Don’t know if that’s helpful just expanding vulkan functionality.
* MM: If padding bytes were defined to not be stored to then we can’t do an extension in webgpu. So it’s immaterial.
* AB: Sort of. Gets into implementation details.
* KG: Puts us in position of choosing how portable we want to be. Leave room in spec for implemetnations to trample on padding between members?
* KG Sounds like yes.
* MM: Would convince if there was an existence proof of a SPirv program which stored to padding bytes when not explicitly told too. Then would be convinced we should not define values of padding bytes.
* KG: Something where we couldn’t commit to a more reliable behaviour and then fall short, that would be a breaking change?
* AB: As we mentioned, if we wanted to could force implemtatnion to do type construction/reconstruction so there is no padding
* MM: Think that would be too hard. Don’t think that’s reasonable to expect.
* KG: Intent would be, don’t trample on padding bytes but if in order to do that had to do type deconstruction to make sure it doesn’t happen then should say we trample on padding bytes
* MM: Can
* KG: Can
* MM: Think that’s reasonable
* KG: Can we ask for an investigation. Someone take a try and do the basic thing and report on if that works?
* MM: Could give program to test, but can’t test vulkan. Program would test would be try to materialze struct with padding, make so big it spills ot memory then write to buffer and see if it turns into a memcpy or not.
* KG: No volunteer makes it hard, can we do somethign in the mean time?
* AB: Worth mentioning one concern is workgroup memory and how to initialize. Wether there is a security issue and we have to type mangle workgroup memory. Could create struct where second element is hugly aligned to create lots of padding and then if run other sharder can you somehow snoop what’s on device
* MM: Right, why i think on metal we have to do scalar store rather then storing ???/. Possibility of device doing memcpy is not acceptable due to reason gave. Why entertaining idea padding bytes should be untouched. Not sure impl could legally be allowed to touch and uphold security model.
* AB: Get what saying to maintain security is different from padding generally. Could choose impl way to do something that may lead to that but isnt’ as strong as never touched. Impl could make cases safe without spec saying. 
* MM: Logic had was if gunna have to, if impl strategy to uphold security is portable neesd to be in spec. Can you describe another way to uphold guarantee using another mechanism?
* AB: Not same on storage buffer as workgroup memory. Piece of memory you can’t have parts of, won’t need to do on storage but maybe no workgroup. Globally saying padding can’t be touched is different from saying on workgroup memory. Or making impl not touch it on workgroup memory.
* KG: One thign we could do si continue to say impl may touch padding between members. Can ratchet that down later if post-v1 impl come to feeling agree on what we all ended up doing. Can then maek change. Proposal if we specify and say don’t count on not being touched. Impl may change padding.  Leave issue for post-v1 to determine if we needed to touch padding
* MM: Reasonable if 2 things
    * 1. Agreement to not change padding bytes
    * 2. This issue is then taken up in 3 different backend api discussino bodies to ask for ways to make guarnentees we want. Only possible way in the future we’d discover we all conform.
* KG: Potential in a year we come back and say we did scalar deconstruction anyway. So, if we did it anyway can standardize better behaviour. Same path as far as what we care about today. Would agree to this agreement. Try to do the better thing and would even be ok with having non-normatlive language saying “implmentation should attempt to preserve padding bytes”. 
* MM: Doesn’t need to be in spec, could be doc in design folder.
* KG: OK.
* MM: Audience is us, so doesn’t belong in spec.
* KG: Consensus sounds like …
* DN: In interest of forward progress, say yes. Might right code which goes into vulkan driver where our code doesn’t touch padding but vulkan changes behaviour under us. So intent is good and can live with proposal. Will not go out of my way to poison the gap.
* MM: That’s why the 2nd idea of talking to people implementing backend APIs
* DN: What do C and C++ say? Imagine if taking to IHVs will scratch head and see what LLVM does. That will probably be guided by C and C++. Layers and layers with long turn around and signal loss.
* KG: 60% sure C++ says touch padding is UB. Don’t rely on them. If copy struct and then memcmp not guaranteed to be the same. Suppose what we’re saying, all agree intent wise all want but may fall short. Can commit to going to ask and instead of expecting them to say no, the tell us no.
* MM: That would be convincing along with an existence proof.
* KG: Let’s do that. Volunteers on quest to ask
* MM: Can ask Metal.
* GR: Can look into that for D3D
* AB: Can raise it, but feel like I know the answer. May need an extension
* KG: Asking about extension possibility
* AB: Could be written but not rolled out for v1
* MM: That’s fine, looking at long term future. In 10 year, now we can make guarantee.
* AB: Will raise.
* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, May 17, **11am-noon** (America/Los_Angeles)
* 