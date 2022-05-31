# WGSL 2022-05-24 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DS

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon** Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-05-17 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1n8oWCouG76-swv2jaLKcVGH-TzsbFQIQcfGDwQCFKzk) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Daniel Glastonbury
    * **Myles C. Maxfield**
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
    * James Price
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
    * Greg Roth
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



*  TAG review is complete, and considered satisfied.
    * [https://github.com/w3ctag/design-reviews/issues/626](https://github.com/w3ctag/design-reviews/issues/626) 


---


# ⏳ Timeboxes (until 11:30)


### [consider namespaces for WGSL · Issue #777](https://github.com/gpuweb/gpuweb/issues/777)



* Previously:
    * KG: Had a lot of desire from Apple, let’s hold off
* MM: **Post-v1**
* KG: SGTM


### [☂️ Initial internal feedback #2213](https://github.com/gpuweb/gpuweb/issues/2213) 



* Previously:
    * Google: Thinks all items have been addressed.
    * KG: Is it satisfied, probably but need to ask Apple.
* MM: **Closed**


### [Special case uniformity of arrayLength #2846](https://github.com/gpuweb/gpuweb/pull/2846#)



* AB: From internal implementation. arrayLength won’t change depending on input value. In future where we allow pointer value to be non-uniform then it could be. But pointer value for array will always be uniform. So, easy to say uniform
* KG: Has approvals, let’s land.


### [Allow control flow reconvergence for short circuit expressions#2912](https://github.com/gpuweb/gpuweb/pull/2912#)



* AB: Talked about in office hours. Want to behave the same as ifs. If behaviour was next have same control flow as coming in.
* KG: Tried to read change, don’t understand way the things work here. Will probably ask questions offline/in office hours to make sure can read
* AB: Easier to read in rendered form. Merged table cells are hard to read. What it says is if have next behaviour control flow coming out is same coming in. If don’t have next then behaviour is evaluated after (e2) expression. 
* KG: Not confident I have the right read, will catch up later. You’re describing the thing I want, so thumbs up
* DN: Will merge.


### [Describe WGSL's context-aware tokenization #2735](https://github.com/gpuweb/gpuweb/pull/2735)



* DN: Have gotten far enough with LALR(1) to generate some conflicts. First is actually valid in way grammar structured. Module scoped decl has attr &lt;var> and we have attr &lt;function> but in 2 separate rules. In top level mod decl see @ don’t know if var or func. Refactor of rules should make that go away. So, finding things that are diagnosed as not right. Another batch to look at still. Close to confident the language is well behaved and will post on bug.
* KG: That’s exciting
* DN: Got compile from 1h to 20seconds.
* KG: Will let continue to find/ fix issues. Is the meta idea that we burn through and narrow down what is context aware tokenization?
* DN: Yes, PR up to describe context away which says ask for token which is valid and parse in an LALR derivation. There is an obvious way to implement in top-down, ask next if valid yaa, else cut down to next shortest token. Should line up with the same resolution. Once script is done, but into CI.

(5! = 120)


### [Confusing languages in technical overview around conversions. #2910](https://github.com/gpuweb/gpuweb/issues/2910)



* KG: Not pure editorial? Due to question 1?
* DN: Yea, needs to be qualified for that case. Overview needs to be updated for abstract
* KG: Is it editorial?
* DN: Yes.


### [context-aware tokenization: Sometimes >> is better parsed as two copies of > (similar for ]] ) · Issue #2092](https://github.com/gpuweb/gpuweb/issues/2092)



* Dup?
* DN: We’d removed the >> ops, think we’re getting to the point where we should revert and put back in. Problem already present in >= so we’re in a not-fully justified state. So, revert now and hope get to good spot
* KG: Was in place we said was weird and we’d change and back and hadn’t changed back until we were sure.


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Think there is group consensus to restrict some kinds of aliasing, but how far do we go? As I understand, SPIR-V says no-aliasing anywhere ever. Metal says anything can alias except params to entry points. Entry point with 2 params and the only thing entry point does is call function with same 2 params then codegen in function would be different from entrypoint which is wild. Don’t know what GLSL does.
* DN: You can do an access chain down into an object and have those two things alias and be visible at same point. Things folks sweep away mentally. High level spec does not have phrase as quoted as was re-written. Trying to find part where we talk about aliasing
* AB: Section a.4.1. [https://gpuweb.github.io/gpuweb/wgsl/#function-aliasing](https://gpuweb.github.io/gpuweb/wgsl/#function-aliasing)  Much expanded from previous. 
* MM: Question we have is are you going to be able to have a local variable (var a = 5) and then after (var b = *a); and (var c = *a); we think yes, people are used to pointers and it’s a common thing. However we think function parameters should not be able to alias. Reason is if backend is language where above is illegal don’t want to have to do global analysis to figure out what is legal. If have local aliasing less perf impact. For helper functions if we guarantee they don’t alias then better code gen. Asking for something metal doesn’t provide but could be beneficial for potential code gen. In metal there is no restrict keyword. No way to tell metal 2 params won’t alias. Still want rule to be that params to function’s can’t alias so it would be possible to emit that codegen.
* AB: Diff between description and spec says one must not write. Aliasing reads we don’t care. But if writing to either (and spec refers to it as finding all the way back to where you get it) either var or param and those have to be unique.
* MM: Didn’t quite follow
* AB: Suggest reviewing spec and see if matches expectation.
* DN: Pretty close to what you said. In my mind only diff is if we care about aliasing when there are no writes in that context. Worth re-reading what’s there.
* KG: Link?
* MM: So in program above a = 5, b and c pointer if both b and c are read only then that’s ok.  If one writes then not?
* AB: If both derived from same memory view. If b from pointer param and c from variable that’s not ok. If they all come from pointer param that’s ok. If both from variable that’s OK.
* DN: So var a; b = addr a; c = add a;. All have same root identifier so can alias
* KG: what if last is reference of the variable, a new reference?
* DN: That’s fine as it goes back to the same initial decl (root variable). Root is either variable decl or parameter to function. So static trace back through function body to hit var or parameter.
* KG: 2 pointers to same root function passed as readonly to …
* AB: If no write, dont’ care. Only matters when you do a write.
* MM: **Think this topic should be on agenda for next week**.
* KG: Sounds good.


### [Should the spec mention location aliasing? · Issue #956](https://github.com/gpuweb/gpuweb/issues/956)



* AB: We forbid all such things. It’s a static error to have overlapping locations. Don’t think you can do this. 
* KG: **Looks like we resolved**
* DN: This dates back to when we declared module scoped variables. Since all entry point param and return now it’s all localized.


### [vec4&lt;f32> is too hard to type on the keyboard · Issue #736](https://github.com/gpuweb/gpuweb/issues/736)



* MM: Still feel is valuable and don’t want to close. Internal discussion and we think that vec4&lt;f32> is too hard to type. Last time made a compromise proposal to add short names to some of the types but not others. Would still be acceptable but feel now if that’s actually a compromise or just a better solution. So, if the group thinks it’s reasonable to name sone and not others that’s fine. But leaning towards naming all of them and we like uniformity.
* BC: Since originally filed we have literal unification adn type inference for vec constructors so the only place to type vec4&lt;f32> is on a member of a struct or as a parameter. Every other place can be informed
* MM: Or local var?
* BC: No, can do `let a = vec4(1., 2, 3, 4)`
* MM: That has vec4
* KG: No &lt;>’s
* MM: Oh, hte f32 is inferred
* AB: vec3(1.0,2,3)    gives you a vec3&lt;f32>(1.0f, 2.0f, 3.0f)
* KG: Above gives float definition?
* AB: Yea, can’t convert a float into an integer so it prefers float
* BC: Only time need to type brackets in two places, so we feel it’s unnecessary.
* MM: Little scared of line of code. Function param isn’t written same way. Function param and local variable to be spelt the same way have the long way. Either have different or have the same but names are long.
* KG: Always felt we should have basic fvec, ivec, bvec and not go two wild about having all of them. Think it’s fine for gradual complexity if you want more complicated things. Feel wanting to name things vec4f is common enough that it would be nice if you coudl do it as a shorthand. Happy to say we do for a few things but not all.
* MM: Would be fine with that too. Have no opinions on names. Just don't’ want &lt;>’s in names
* DN: When you say all of them, not clear on what that is. Would like to know what that means exactly. F16 on horizon, could have i16.
* MM: So, all of them would be cartesian product of float, half, int, uint with 2, 3, 4. If group prefers some subset would be ok.
* AB: Intentionally leaving out boolean?
* MM: Boolean too.
* BC: Vocal against typedefs but lost battle on suffexts. Have alphabet soup with ‘h’ suffix. Talked about holding back. Have ‘i’, ‘u’ , ‘f’, ‘h’. If doing should make consistent so if adding would be vec3f, vec3h. Would be unfortunate if we didnt’ do all
* MM: Sounds alright
* KG: If we do this, don’t have to do this for all things in the future. Might not add the ease of use alias.
* DS: Couldn’t these be type aliases
* KG: Would have to do that in all programs. Do you want standard shorthand?
* MM: Important that code snippets that we all agree on how these things are named. Think HLSL is good example where they have both names but shorter ones are common
* BC: Assume not keen on extension in order ot not pollute namespace unless opt-in
* KG: This is a value add.
* MM: Because we reserve float4, agree with KG that this is the same pollution problem 
* DN: Stopped reserving words that are types in other shader languages
* KG: But we did it for weeks
* DN: We did but decided not to as it becomes impossible to do aliases. Doesn’t change direction
* MM: Point still stands. Also every other shading language has short / consiste global namespace names.
* KG: So, go back to making PR to add and just pick one?
* DN: **Would like a week to discuss internally**.
* KG: Ok.
* MM: One of the interesting questions to discuss is would you prefer to have the common names named or all of them named. 
    * Any names
    * All the names
    * Common names
* MM: And could define what common means

(10!)


### [[WGSL] WGSL needs a plan for expansion · Issue #600 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/600)



* MM: Can be closed?
* KG: Think we’re satisfied with this, we have a plan?
* DN: Yes, was to prod discussion and i think we’re far enough along with what we have until we have a concrete thing that’s different. So, close.


### [[wgsl] Encoding for shader text needs to be specified · Issue #565](https://github.com/gpuweb/gpuweb/issues/565)



* 


---


# ⚖️ Discussions


### [Are padding bytes of structures intialized with zero value expressions? #1747](https://github.com/gpuweb/gpuweb/issues/1747)



* Previously
    * MM: After the [2202-05-10] call today I consulted with my team, and we came to a different conclusion: [https://github.com/gpuweb/gpuweb/issues/1747#issuecomment-1123121829](https://github.com/gpuweb/gpuweb/issues/1747#issuecomment-1123121829)
* KG: Feedback from AB last week is that SPIR-V said do it yourself (functionally what is said). To not write on padding make padding variables and force spir-v to not write on padding by not having padding
* MM: Just give it name. Same thing we’d do in metal.
* JB: So translators must adjust the way they output spir-v types for structs?
* AB: No consensus on language requirements. Second paragraph of if padding can be touch is a separate decision from safety. Could emit types differently depending on address space. Safety wise have to make sure global memory can’t be seen which is different from not touch padding
* MM: 2 goals. 1. Security guarantees 2. Make portable and there is a portability/perf balance. For portability goal either of 2 ways listed would be OK. Either behavior would also implement the security requirements. So, our requirement to what’s written is anything said is fine with us.
* TR: Leaving them untouched, can’t see how reliably use data in padding so if using data in padding it could lead to portability problems between impls even if leaving untouched just because don’t control read/write order. More portable version is padding is written as 0’s. Probably can’t use reliably and if needed to make part of struct
* MM: Just to understand, 2 different shader programs run at same time on same device accessing same buffer in 1 program struct A and other struct B? \
TR: Subsequent reads of same buffer, not at same time but padding isn’t in same instances of struts output from run to run due to ordering. If append/consume pattern won’t necessarily get same ordering of data to write plus padding as is.
* MM: Still struggling, could you describe program?
* TR: Appending to a structured buffer and that’s done in some order depending on execution and have units of data to read in other shader. Other shader reads data nd it might read padding and do something based on that. Can’t imagine program written that way to rely on padding. Seems silly. Relying on padding wasn’t written, was left over from what was in buffer. To make programs have less potential bugs based on this kind of dep, can’t imagine someone caring what’s in padding but could read and depend on it .If we write 0 then more reliable then struct contents more reliable when you write as opposed to having garbage in padding.
* MM: Let’s imagine a program that fills buffer with a sensible &lt;???> . then run shader which treats buffer as struct with padding. Shader fills buffer with structure. If define that the padding bytes don’t get touched then the thing after the shader the buffer would have padding bytes with stuff existing. If someone has 2nd shader to observe contents of buffer would be silly to write shader which relies on the stuff still being there?
* TR: In that scenario could say trying to read a sentinel and is debugging situation. If re-using buffer and not clearing them and it’s possible the padding could be written with other data then could have garbage. Not simple scenario with clear buffer and run shader and read those. I’m thinking re-use of buffer scenario.
* MM: Ah, I understand
* TR: Paging buffer of some kind. Allocate paging buffer assign and use and could be used for whatever. Can’t rely on bytes to be specific re-usable value. THink it would be more repeatable to write zeros.
* MM: Making think of intel ASM language where write to low 32 bits of 64 bit register then top 32 bits get cleared.
* AB: So, still perf considerations for example if wanted to emit the same struct in workgroup memory (worried about workgroup memory being safe) now have padding and going to 0 all of that and when copy in global memory to emit struct would copy padding bits. Easier than having to do member wise copy if know going to emit all the same way. Could copy each field else have to do copy as in wgsl and then transform types or choose to not emit code for underlying language. From impl view reading padding gbytes is fine. What we have to do is zero for security purposes. Could leave at that for optimization reasons. Writing zeros complicates implementation. If buffer filled on API side and copy into workgroup memory shouldn’t have to do something to copy padding bytes. More opportunity for types to look the same way and not emit certain transforms. Don’t have to track which are real and which are placeholder fields.
* TR: Not quite sure if got that.
* KG: One of the issues is that we have 3 cases.
    * Just zero the padding bytes and they’re always 0, don't worry
    * Promise we won’t change what’s in padding bytes. Carefully work around and preserve data
    * Possibly write over bytes and forbid any correct program to access padding bytes and expect anything. Won’t have well defined values.
* KG: Is it valuable performance wise to say please don’t rely on the values in the padding and say we're going to optimize and if you look at padding, don’t but we can’t stop you vs actually trying to make well defined (read 0 or don’t touch bytes) but don’t think anyone wants to do preservation of bytes.
* AB: Yea. We can make it safe so you don’t observe outside sandbox but if padding bytes get written by other safe data then not relevant as not read in shader and on API doesn’t matter as don’t have structured data there anyway
* MM: Very much unified on that being important. Not important in shader vs in API is line which doesn’t exist. Saying only observable in JS doesn’t satisfy
* AB: Not if it’s visible but not important to maintain. Don’t think portability is a benefit. Not structured data. Everyone doing same thing with no real right answer doesn't’ give benefit.
* MM: Wed defined that as there is a right answer and it is structured. In shader don’t see structure but in API see bytes. In shader only some bytes are addressable but at end of day all are observable.
* KG: So disagreement in judgement between does it matter for portability and potentially is it an optimization target to not care about the padding. Sounds like 2 philosophies which provide different weightings for these two things.
* AB: That’s fair.
* KG: So, what do we do … 
* TR: Outlined three options above: don’t touch; may or may not be written so could be overwritten or non-zero; and 3rd when structs written explicit padding is set to 0. The middle one I understand where coming from but seems like odd on because you’re writing something (a struct) and you expect to write the members of the struct or …. Seems kind of odd to say anything could be here or overwrite random byes if not outside security context.
* AB: Not random. More efficient to do memory ops if (...) then it is to do multiple ops. A 64 byte memory operation over 3 32 byte operations. Don't’ think should cut off that operation for the sake of maintaining data in buffer
* MM: Argument for add 0 all the time so there are no padding bytes. All 0 all exist and all emitted because every struct is full then the instructions on GPU can be natural word sizes
* AB: In practical terms mostly works. Ok with going as far as saying when initializing memory it will have zero in the padding bytes. But don’t control if initialized on the API side. WGSL doesn’t control view of API side data. So, if copy in data may not copying in zero. Want to cut off to say we’ll initialize but if copy from API then won’t be zeros.
* KG: Strawperson proposal. Say padding is all zeros all the time but if we want opt-in optimization could have a type which is `don’t care` of one byte. If you want these things and just discard padding and do fast thing and opt-in to chaos that would be a way to do it while also generally preserving portability and author intuition that thigns have well defined values. If run in same place would be minimally surprising and also provides optimization possibility.
* MM: So, entertaining idea, wouldn't be type, would be extension? Wouldn't want to mix and match, presumably. 
* KG: Don’t really want it whole program, but maybe? Basically analysis can then realize have vec3 and then don’t care and could optimize that based on word size and store vec4. Would be something we’d re-recognize in analysis as a don’t care padding.
* MM: Guess the thesis on  our time is portability is a ???. One of the AB points is fill padding for buffer reads or not optimize buffer write. From us, either of those trade offs is worth it for portability.
* JB: Strawperson proposal. Forbid structs with padding. Struct must be packed. If gap it’s an error. Then relax in the future.
* KG: Don't hate it
* TR: Don’t hate it either. Struct padding can be major source of confusion for people. Don’t realize gaps are there due to members in a specific way.
* JB: Imagine folks don’t want padding
* TR: Yea, usually don’t want.
* MM: Dislike this but dislike less then chaos approach
* KG: Good ideas …. Should think about and revisit next week.
* MM: Reason I dislike this idea is because you see this in buffers it’s pretty common to have a array of structs where the struct is big and has a gazillion things inside but for vertex buffer only care about position field so have stride. Useful data in padding but have hole for vertex buffer. Could see that kind of thing for a uniform buffer where one shader just cares about one field in struct and potential issue there.
* TR: Argument for different type of struct mapping vs declaring thing that looks like a struct but everything has explicit offsets. Not really padding just only accessing given locations relative to beginning of struct. Good arg for not touching padding as they’re valid things other bits might look at
* MM: Good point
* AB: Or represent by just putting fields there
* TR: Right and not reading them.
* 


### [Uniformity analysis of local variables #2859](https://github.com/gpuweb/gpuweb/issues/2859)



* 


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, May 31, **11am-noon** (America/Los_Angeles)
*  [AbstractFloat should not be convertible to infinity or NaN in concrete float type](https://github.com/gpuweb/gpuweb/issues/2953#)#2953
* [Describe WGSL's context-aware tokenization #2735](https://github.com/gpuweb/gpuweb/pull/2735)
    * David’s LALR(1) analyzer verifies that WGSL’s grammar is LALR(1)
    * PR updated with refined wording.