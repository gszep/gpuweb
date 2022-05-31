# WGSL 2022-05-31 Minutes

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



*  


---


# ⏳ Timeboxes (until 11:30)


### [AbstractFloat should not be convertible to infinity or NaN in concrete float type #2953](https://github.com/gpuweb/gpuweb/issues/2953#)



* DN: PR now posted: [https://github.com/gpuweb/gpuweb/pull/2984](https://github.com/gpuweb/gpuweb/pull/2984) 
* DN: When abstract numeric is too big for concrete type should be shader creation error. More in line with big float with `f` being shader creation error. Just makes it consistent.
* KG: Is this normal for a language or a new thing?
* DN: Normal thing I think.
* KG: Often warning or error?
* DN: Literal it’s an error. Looked at c++14, says if converting from double -> float (narrowing the type) if the value doesn’t fit in the finite range then it’s undefined behaviour. Don’t want implementation to do the check as it’s a hazard, so lets avoid.
* KG: Works for me. **Let’s take it.**


### [Describe WGSL's context-aware tokenization #2735](https://github.com/gpuweb/gpuweb/pull/2735)



    * David’s LALR(1) analyzer verifies that WGSL’s grammar is LALR(1)
    * PR updated with refined wording.
* DN: At point where have grammar analyzer which verifies WGSL is LALR(1). Can dump from every parser state what tokens to expect otherwise it’s a parse error. All that’s working and confident and can say rule in PR, tokenization is interleaved, gives rule saying token candidate says anything starting from here that is any valid WGSL token and you take the longest one valid at that parser state. Give term and filter by what exists. For recursive descent, take longest token, see if works, otherwise split to shorter and try. Continue until find, if fail all that then failure.
* KG: cool. Provides proof that our system should work. Is this something for CI?
* DN: Yes, current 2min on macbook, lots of stringing and hashing. Now switching to IDs and things to make fast. Will make part of CI (hoping for &lt; 2s). 568 states, 400 reductions. Not that big of a language.
* JB: Moz produced table driven parser for JS at one point which required lots of tokenization hacks. Was in python and was fast.
* DN: Wrinkles are nested comment blocks. Fact identifiers and keywords has a bit of precedence. (If looks like keyword is keyword).
* KG: Any actionalable things?
* DN: Land PR as it describes interleaving. Can then add &lt;< and >> back and then verify still LALR(1) (which I believe we will be)
* KG: When removing &lt;< and >> we added buildings, do we keep them?
* DN: Tink we should remove
* MM: No content using them.
* BC: Tint never implemented. Not aware of any implementation which actually did this.
* KG: Ask as someone may want to use builtin instead of &lt;< or >>. Can request if they want.
* MM: Can’t have ternary inside second part of thing between &lt;> for a vector type or array type. (e.g. array&lt;i32, 3 > 4 ? 5 : 6>) If wanted to add that then probably want the rust thing and require {} in that situation. If not doing anything special (array&lt;f32, 17>) then don’t need {}’s.
* DN: Right now, no general expression in that slot of the template. Special grammar rule called ??? expression which jumps far enough into hierarchy. Would have to put it in the right spot to make it available.
* JB: Rust only resorts to {} because () have meaning at the type level. So WGSL can use ().
* KG: **Resolve to do all the things. Merge PR, revert operator removal. Remove the builtins**.


### [vec4&lt;f32> is too hard to type on the keyboard · Issue #736](https://github.com/gpuweb/gpuweb/issues/736)



* Previously
    * DN: Would like a week to discuss internally.
* DN: Said we’d talk internally, none of us particularly happy with this. Don’t think great to have same way to write same thing. See strong desire to add this. Don’t want there to be a generic rush to add for every scalar types (i.e. i8). Do see utility in matching to suffixes in language so far. Don’t want to add more suffixes. What we have seen is vec4f, vec4u, vec4i in game engines. If we get these, think it should be spelt that way. So, under process but in spirit of agreement think that spelling would be livable.
* MM: Sounds great. Right now, listed 3 of 4, what about ‘h’, does it extend to h?
* DN: Yes.
* MM: Happy to agree with that.
* KG: Cool. Names?
* MM: Ones he said are fine?
* DN:**vec3i, vec3u, vec3f, vec3h**
* KG: **Let it be so**.
* MM: **Not vec1f** right, just vec2f, vec3, vec4f
* DN: Correct.
* KG :Matrices?
* MM: **Not worried about matrices**.


### [[wgsl] Encoding for shader text needs to be specified · Issue #565](https://github.com/gpuweb/gpuweb/issues/565)



* KG: Said I’d write something. Write up that we’re UTF-8
* DN: Anything to discuss, or just do?
* BC: Was against, been convinced general direction languages are moving. Think just **doing UTF-8 and reject other encodings is fine**
* MOD: I agree and **we should ignore BOM**.
* KG: Anyone to write?
* DN: **I can do it.**


### [Clarify behavior of float division/remainder by 0 · Issue #2798](https://github.com/gpuweb/gpuweb/issues/2798)



* KG: Planned on discussion of what it should be
* DN: Unlike int division, ieee has defined 1/0. That’s why different. Think this is a sub-answer for 2777 because the problem is some implementations don’t have inf or nan. If you have calculation which has one, what do we say? Want to push on stack until we solve 2777
* KG: Agree it should be generic. Basically just defined as ieee and additionally if yo umake inf or nan and the impl doesn’t support then extra step kicks in. SPec wise, you got inf, but system doesn’t do so you get something else. If we really wanted to support (like div/0) could spec it a particular way and that’s where we would say it.
* MM: Think that's reasonable. Biggest concern is branch on this value is _not_ UB.
* KG: Concern being requirement?
* MM: Yes. Whatever we do, branching on value should not be UB. Don’t know how to implementat in SPIR-V.
* KG: Tolerable if unspecified value and impl can have different ones?
* MM: Yes, ideally not but don’t see way around in this case. Trade off is if we want to spec how they work then we’d be leaving a lot of android devices out. Past that we want to make sure that (e.g. branch on if/else then one of the blocks, not both, not neither is taken).
* KG: Ties into what nouns we use for undefined values and behaviours. Different pr and issue. That’s waiting on PR from KG. Will finish doing that and we’ll have better nouns. Sounds like we’re on the same page. Satisfied for now. 


### [Clarify that evaluations to inf/nan may yield undef value #2777](https://github.com/gpuweb/gpuweb/pull/2777)



* KG: Wanted to discuss undef value first, but don’t care anymore. Lets land if that’s tolerable and fixup afterwards. Can either land what we mean and then fix, or land better language and use here. Either order is fine with me.
* DN: Either is fine with me. This is a net improvement.
* KG: **Let’s take this**.


### [Uniformity analysis: find a way to load from a storage buffer uniformly. · Issue #2321](https://github.com/gpuweb/gpuweb/issues/2321)



* KG: Context 2586 workgroup broadcast pushed post-v1. Would say, when we pushed that, then probably this is post v1.
* DN: JP feedback from implementation is that none of the feedback so far is from this case. So want to **push past V1**.
* KG: **No action for v1 unless further feedback**.


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Couple thoughts, homework was to read spec and figure out what spec said. Little interested in the statement that says it is a dynamic error if .&lt;some stuff>. Hoping someone could explain what an implementor is expected to do. How much should we be trying to detect these things.
* AB: Up to implementor. How much effort you want to put in. Didn’t statically detect because if the write is conditional and it didn’t occur you want to leave room for the user to do the right thing. It depends on your compiler, if compiler thinks it’s aliased by default then do alias analysis and should do the right thing. Might lead to perf issue but more predictable outcome.
* DN: Simplest implementation is to ignore this
* MM: Idea is if we imagine compile -> HLSL you either inline the function or you use inout and get 1 of 2 behaviours as you’re breaking HLSL rules as inout can’t alias and the spec says if the author does that it’s the authors fault.
* AB: Something safe will happen but the result probably won’t be correct
* MM: Little surprising because of the strength of pointers. They’re such that the compiler can see where they come from. Leaving up to programmer seems surprising as we know if they alias or if the program will produce different things on difrfernet compilers. Could tell them, warning or error
* AB: Free to make diagnostic. Making error rules out cases where dynamically it would never be error. If conditional control flow and know there is no alias write I’ve disallowed those programs from being written. If compiler says it’s an issue might be useful to users.
* DN: As concrete example, disallowing doubling buffering pattern with array len(2) and odd -> even, even -> odd. Both same variable and want to allow that pattern
* MM: Usually double buffer at draw call level instead of inside shader.
* DN,KG: …..
* KG: Makes sense to me. Would hope we’d emit warnings. Would then want way to annotate that it’s not an issue. Could mark double buffer function to say you think it’s aliased but it’s fine but other cases in program would not be ignored and could maintain clean warning logs. But, don’t have that yet and don’t think we need for v1. Could do diffing if you want. Makes sense to me.
* MM: Example at top of issue , this program would be legal and would be programmers responsibility to not write this.
* KG: Their responsibility to not write function cc?
* MM: Not super keen. **Would like to talk internally**.
* KG: Ok. **We’ll come back to this**.


### [Are padding bytes of structures intialized with zero value expressions? #1747](https://github.com/gpuweb/gpuweb/issues/1747)



* MM: No comments since last week.
* KG: Any thoughts? From notes last time, one of ideas from JB was forbid structs with padding and just say if you want padding you have to put in the fields (for v1). 
* Ab: Does that mean any vec3 has to be followed by 32bit scalar? (or 16 bit if half)
* KG: Yup, or call it vec4.
* MM: What happens if you have array&lt;vec3&lt;f32>>, is that illegal?
* JB: Would forbid that.
* KG: Type check as vec3 and implement as vec4
* JB: Talking about forbidding in wgsl, that suggest is to accommodate in compiler.
* BC: Pretty late in day to be making this breaking change. Not a lot of shaders, but this would break a lot of them.
* JB: Drastic move. The array of vec3 makes me uncomfortable.
* KG: One other proposal was to say that padding is all zeros all the time and make sure to read/write zero when we have to and then have optimization later for a type that says don’t care. That’s optimization opportunity and if user doesn’t care, they annotate. Some type of of vec3x type to make arrays like this but fairly tolerable. Or have struct that contains vec3 and  don't care scalar and you have array of that instead of array of vec3. Little clunky but add authoring complexity to squeeze out perf. Gradual complexity for performance.
* MM: Solves this by having different family of types like packedVec3 which is vec3 with stride 12 instead of 16. Think proposal just made would be acceptable. More concerned about defaults.
* JB: Setting aside optimization for don’t care type, the implementation technique that would work for the 0’s is that the wgsl translator always translates wgsl structs to fully packed structs in backend language and would expand initializers or other expressions  that create those to types which provide zeros. Is that correct?
* KG: Yes
* MM: Have to handle case where you do read and then store back over thing you just read. In this world that would have side effect of filling padding bytes with zeros.
* KG: Little satisfying as the default c version is just let padding bytes be trampled and if you wanted zeros put in a real type. We want to try harder to have things accidentally work on different systems with different behaviours. Accidental portability.
* AB: If we say you have to 0 init and use same representation on host and if it’s copy or trampled it’s 0 initialized. Free to do that optimization. Breaks down if on host initialize with other data. If you choose to do that, why do we care if we write zeros? Let us trample us or not, if doing in nice way we can guarantee 0’s on GPU side and host is up to person writing wgsl program.
* MM: Struggling to understand point. One response is the way data gets into the padding spots isn’t just filled in on host, run 2 shaders which treat buffer data as different types. Imagine sandwich of shaders type A, type B, type A. 
* KG: Or vec4, then vec3, then vec4.
* MM: Right. If the middle shader clobbers or not then the last part of sandwich can easily see it.
* AB: Why scared about that? Don’t see that as a problem when free to reinterpret data
* MM: Scared about portability
* KG: Concern is the problem of the instinct of people using smaller types. If reading vec3 is faster then vec4 but if they want to make sure no trample we would be saying have to read as vec4. Part is dealing with programmer intuition. Can spec it as will trample on data (read as vec3 and write as vec4) might have changed it on some systems, others we wont. Worry about it being accidentally breaking and if making different choice would always work
* aB: Vulkan says trample padding between struct members but not at end. If just vec3 then not trampling 4 bytes.So, if vec3, vec3 then can trample 4 bytes between the two. Distinction between end and in between bytes.
* MM: Either way, real important bytes are precious. Can’t just let get trampled willy nilly.
* AB: Disagree with that statement.
* TR: One issue haven’t heard discussed, struct side should be aligned by the largest alignment member. 64 bit value can effect ending and size of struct. If we trample would depend on if we have expanded or cut down versions with same prefixed elements. Could know stride of struct and reads from all locations that way. Can do this in structured or byte address buffer in hlsl. If you write subset of structure could be trampling what looks like padding but is part of larger structure. Could be a problem too.
* KG: SOunds like what vulkan is trying to solve. Intra padding is fair, outside struct is sacred. Good to hear why that’s the case.
* BC: For better or worse, tint doesn't’ use HLSL structures for buffers. Use big array of vec4’s and take structures and manually pack and unpack. Doing the 0’ing of the padding is more work and produces more code then just doing the loads/stores of the bits. Don’t know perf hit on compiler or ISV instructions but would be trivial to do loading /storign of fields of structure/array elements based on how we’re doing. That would be the easiest implementation
* JB: Promising to preserve padding
* BC: Only load and store what is declared in that structure
* MM: Ok with me.
* JB: So, if I load a struct from a buffer and the struct has holes and then write struct to another buffer then the bytes in the dest that are padding are left unchanged.
* BC: Correct
* JB: So, this forbids write widening. Even with 64 bit stores and the structure fits in 64 bits the optimizer is not permitted to widen to a 64 bit store has to do individual pieces
* BC: Based on our impl, yes, we do field assignments using 32 bit scalars and vectors. That’s how it would work. If wanted to get clever with a contiguous aligned range could use wider range. Have received comments output shaders are unreadable but removes a lot of complexity to make sure layouts match WGSL rules. The dxbc is identical to what FXC was producing. 
* KG: So output code was the same
* BC: For FXC, DXC might have difference
* JB: Feels like rule that writes don’t touch padding is easier on optimizer then writing 0’s
* AB: I think zeros are worse.
* KG: Could get other behavior if you wanted. Satisfied with that. Can manually widden types to carry along. Could tightly pack struct to keep bytes but you don’t have to. One of the concerns was complexity but tint already does this.
* TR: Like this approach. HLSL does this, doesn’t write padding for structure. Just disallow vulkan behaviour of widening interior of struct to write padding. Only allow that if there is no padding.
* MM: Possible to emit spirv program that the later program that consume buffer doesn’t touch padding
* JB: Thinking what SPIRV would look like. IR says store struct. Then spirv says, can’t use SPIR-V store anymore in order to protect padding. SO, break store into access chain to get value out and store pairs.
* BC: No, if to do for get element pointer then optimizer could go and reformat that thing. The thing that guarantees the optimizer does not remerge is the final reads and writes to storage buffer are not using compound type. It’s decayed down to a vector.
* JB: Each store pair is basically the struct store decomposed to stores of primitive types.
* AB: The important part is you can’t represent the type the same as WGSL. Have to pack in spirv.
* BC: Storage buffer in Tint is never a struct type as declared in wgsl. It’s an array of vec4 and we generate read/write function stubs which do packing to and from the array. Load and store function with offset param. Load given structure pulls apart and writes those things.
* JB: So, can’t do pointer arith in the dialect of spirv restricted to, so created spirv struct types that are fully packed with no holes and assigning to certina values?
* MM: 2 ways, WGSL struct as struct in SPIR-V and cover all holes. Or, WGSL struct never appears in SPIRV ever.
* BC: We do either. Don’t do this for spirv but would be trivial. Storage buffer becomes array&lt;uint32> and then structures from WGSL are handled around when writing functions but when load/store storage buffer. Instead of loading/storing type we call helper which pulls apart that struct and does vecto reads/writes.
* JB: K, so as far as psirv is concerned, in memory it’s arrays of vectors. Would like to discuss more in the office hours.
* MM: With saying struct never appears, is probably possible but having it for register types makes sense. For memory types doesn’t have to be there
* KG: **So consensus is to spec so we never touch padding**.
* 


### [Uniformity analysis of local variables #2859](https://github.com/gpuweb/gpuweb/issues/2859)



* MM: Bunch of examples here. We discussed examples on team but not larger principles. Could go through examples and talk about each one?
* KG: Think so, leads towards solution.
* MM: For first, RM made uniformity every var either runiform or non. Scope is entire variable. If ever write uniform or non-uniform then the variable takes that attribute. Discussed months ago splitting the lifetime for when it is and isn't' uniform. Reason didn’t do that was simplicity. Didn’t think better or worse, could split if we need to do that. Question of where you split. If you split at every write the graph gets bigger, maybe ok maybe not. For first example ambivalence from us if real authors think split lifetime is needed we can do that but don’t have opinions.
* KG: Because they can just use a new variable?
* MM: In trivial cases, yes. Haven’t made proof you can always make new var. If write is inside condition then new var has to be hoisted. Not confident for every case, but maybe true.
* DN: Our experience, JP said could do precise thing and impl was fast. No worse then what already have to do to understand dataflow. We think it’s a net benefit for users and is better to handle for them. We want to do the more precise analysis.
* MM: Split lifetime at each write?
* DN: Yes at every write and any time you exit control flow you get values together (PHis in SSA). If assign in condition you re-analyze outside the condition.
* MM: So if the condition is uniform and inside the then you write and the value is uniform and the value was uniform before then you’re phi is between uniforms then you’re uniform but if any of those was non-uniform you’re non-uniform
* DN: That’s my understanding
* MM: Works for me if we’re ok of the graph getting bigger
* KG: just graph size. What about composites? If I”m write tint stops short of re-unifying composites
* DN: Poison one component the whole thing is poisoned
* AB: That was due to ???
* MM: Good initial position, if authors want more we can talk about it. Good story if you want parts uniform and parts non-uniform in struct then either split the struct or make 2 structs and only pay attention to half.
* KG: **Think we have loose and cautious consensus to try it this way. Resolved**.


---


# 📆 Next Meeting Agenda



* Next week: **APAC!** Tuesday, June 07, **5-6pm** (America/Los_Angeles)
* 