# WGSL 2022-05-03 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** DS

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon** Americas/Los_Angeles **(not APAC!)**

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+is%3Aissue+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-04-26 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1WRPkm7Y9BK82rFouwcwZLjPzWxRZioWeAbGeTyTIWDY) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Myles C. Maxfield
    * Robin Morisset
    * Daniel Glastonbury
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
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
    * **Ryan Harrison**
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
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * **Tex Riddell**
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
* **Dominic Cerisano**
* Eduardo H.P. Souza
* **Jeremy Sachs**
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


### [Change integer texture parameters to unsigned #2722](https://github.com/gpuweb/gpuweb/pull/2722)



* KG: Approved by me.
* AB: Added extra set of unsigned overloads. 2 versions, signed and unsigned.
* KG: anyone opposed? More time to think
* AB: All queries are also unsigned returns. Dimensions, num layers, etc
* MM: Adding option for signed as well?
* KG: Will accept signed but queries are unsigned results. Asking about ability to accept signed or via extension to request signed?
* MM: Using for loop with signed loop counter to iterate over a load command
* AB: Adds new overloads in terms of params
* MM: Sounds fine
* KG: Does change for (i32 0 ; to max;) doesn’t typecheck now. Saved a bit because no-longer int. Don’t have int vs unsigned int verbosity. If have to write type will write i32 which is same number of types as u32. Gets better with ranged for loops as well. This will case previously valid for loops to no longer type check.
* MM: Think that’s bad. Try to match two things together. Adding overloads shouldn’t break.
* AB: New overloads of param types. But things that return can only change to unsigned
* MM: RIght so, signed in 1->7 no problem; signed 1->TexSize may not work. Ok with that
* BC: New overloads for texture load takes i32 for cords and level new ones take u32 for coords and level. Two variants for i32 and u32 but no way to mix u32 and i32.
* AB: Agreed to that in previous meeting
* BC: Thought we had different template per param
* KG: Considered it but ended up just trying this way with more overloads later if needed
* MM: You can also complain and suggest it.
* KG: Will to add later, for now try to not add all the things.
* KG: We’ll take this for now.
* 


### [[wgsl] Split regex apart by segment. #2762](https://github.com/gpuweb/gpuweb/pull/2762)



* DS: Splitting regexes to make them more readable.
* MM: Doesn’t really need to bring up to the group
* MOD (in chat): I agree with Myles. This should have merged much earlier
* KG: Could have merged it earlier. Mark it editorial and can just merge it if editorially it’s a good choice. If not editorial then that’s a bug and we can revert.


### [in certain contexts, an identifier does not resolve to its declared object · Issue #2486](https://github.com/gpuweb/gpuweb/issues/2486)



* AB: Still needs work
* 


### [wgsl: Missing specification on modf behavior for negatives. · Issue #2468](https://github.com/gpuweb/gpuweb/issues/2468)



* KG: Thought we figured this out
* MM: Feel like we should do what we did for integrals to floating point values
* BC: Spent time trying to agree on what flavour of modf we’re doing. Hadn’t resolved mismatch between int and float. Not sure if that’s what this refers too
* AB: Spec states implementation of floating point modf. Not sure what more is needed. Separate comment on issue about some people preferring GLSL behaviour which does not match HLSL or MSL. We ended up with Spirv remainder vs mod as it’s more consistent. In past talked about buitlin function to provide both behaviour but backed away previously
* KG: Leave comment we deferred to SPIR-V spec and therefore is well defined?
* AB: SPIR-V mappings were taken out of spec. Now for fp type we give the formula in the spec directly. So, I think it’s properly defined and matches, but not written exact same way, what is in the int section
* MM: **Should link to section of spec, close issue and say if behaviour is not desired to open a new issue.**


### [wgsl: limit layout size of a type · Issue #2118](https://github.com/gpuweb/gpuweb/issues/2118)



* KG: One question, do we know what we want to do?
* BC: Sensible thing for V1 is to limit to less then 1&lt;<32. This is Tint implementation for some time. Fuzzer found serious issues if going beyond that. Sensible limit.
* MM: Similar topic came up few months ago, made point the real defacto limit is &lt; 4billion and having spec say value is &lt; 2billion and someone will think they can use 1billion but real limit is megabytes. Shouldn’t say 2billion but instead say something like if this value is too big your shader can fail to compile. If wanted to go beyond could add device specific limit but calling out 2billion when limit is not there seems like a mistake
* AB: In my mind we have 2 specs. WGSL says theoretical upper bound and API spec says with real device there is a limit you have to check. If you compile WGSL in vacuum without this limit you can have that size. 
* JB:  Agree with AB. Would be nice if compiler folks don’t have to deal with arbitrarily large numbers that are useless. Pointless to use bignums for arbitrary precision numbers if spec lets off hook
* MM: Already allowed to fail if value is > 2billion
* JB: People use compiler offline. Compiler is not fused to GPU
* MM: In offline compiler, if value > 2billion, fail compile
* JB: Not if WGSL doesn’t say we can’t. When there is no GPU want to be able to say WGSL compliant and so want language to say so
* MM: Then flip and rather then spec have max, have a min. Every compiler must be able to handle buffers of a certain length and in order to be compliant must handle of that length but not necessarily any larger
* JB: That makes sense.
* KG: Is this implementation complexity? \
JB: Yes. Don’t want to use bignums
* KG: Need bignum or checked int?
* JB: Cant’ throw error. Picky stuff and probably won’t impact users. To be picky want to be conformant to letter of wgsl spec. Then if don’t have minimum above which allowed to throw error then in order to be picky must use bignums. There pointless and no-one needs those numbers.
* JP: Some hard limits by underline limitations. Private array case. … chokes on array > 64k elements. Static injected. So probably in that sort of range.
* MM: The argument is already true on webplatform since inception. Document node limit already.
* KG: Not standardized. Have limits on how big strings can be in JS. FF changing how big data urls can be. Not standardized and the world goes on. See JBs point to want to satisfy the letter of the law.
* JB: Picky point. Don’t think anyone would be upset. Feels nicer to work against something to point ot limitation and have check and explain why
* MM: Dom node number; length of text nodes limited. Shouldn’t aspire to past that.
* KG: Proposal, say we have a limit of 4billion. Shouldn’t bother anyone too much. Don’t want someone to think they can use a 3billion size. So have to phrase properly, hard limit of 4billion but in all likely hood won’t compile with more then millions. Satisfy MM with language choice and JB/AB with having the limit.
* MM: Whats the downside of flipping around? All arrays smaller then x must work
* AB: Don’t mind phrasing that way
* DS: Could also test that in the CTS
* BC: When does that fail, shader creation?
* MM: Right
* JP: What’s the number? 
* JB: 2 billion
* JP: Can’t, fxc chokes on 64k
* MM: Then 64k?
* AB: Maybe multiple limits. Sizes of certain types and byte sizes of structures.Could have giant structure and maybe worth having a bunch
* KG: Fair, but saying up to 64k will definitely work; above that … maybe?
* AB: Phrasing as a byte limit or .? What is this 64k?
* MM: Don’t think answer is important don’t have to be precise. Whichever you think is best.
* KG: If satisfied everything up to limit will compile then whatever limit is satisfactory as long as > 4. Usefully large number.
* BC: The thing we hit is about bytes. The sizeo f array * array element size. Limit has to account for if the multiplication exceeds value we can not compile
* AB: Want to phrase as overall size. Also hard limit on number of array limits.
* KG: Pick a thing and move on.
* MM: Not in webgpu spec but a resolution to add max buffer size and there is a max binding size. One other solution is the max array size in bytes has to be &lt;= the limit from webgpu (binding size limit)
* KG: Sure. Anyone not currently satisfied, make a proposal and we’ll figure out how to satisfy.
* AB: I’ll take it.

(5)


### [WGSL: Allow global let-declarations initialized with constant expressions · Issue #1911](https://github.com/gpuweb/gpuweb/issues/1911)



* KG: Post v1 thing, maybe?
* MOD: No global let
* AB: Have const instead of global let. Closed as worked out a different way


### [WGSL grammar needs update for var type inference #1805](https://github.com/gpuweb/gpuweb/issues/1805)



* KG: Similar, closing.
* JP: Just check grammar now allows type inference at module scope. Prose doesn’t say how. Think we later changed our minds?
* MM: If we do inference at function scope, we should do at global scope.
* AB: Pretty sure with const we did it.** Should say if it doesn’t. Can clarify**.


### [Are padding bytes of structures intialized with zero value expressions? #1747](https://github.com/gpuweb/gpuweb/issues/1747)



* MM: Can SPIRV do this? Thought there was no way to access padding bytes
* AB: Can copy memory to do it. Shouldn’t be able to observe it. Don’t think the spec needs to define it unless allow arbitrary type casting. Don’t think have to say anything
* MM: If struct in buffer and populate struct in buffer with type generated in shader then could observe it.
* KG: Like populate a vec3? \
MM: Like float then vec4 and construct in single line then write into buffer. Involves 2 ops, construction and copy. Could spec it either way.
* AB: Cannot observe in shader but maybe in API. But seems it doesn’t belong in spec.
* KG: Overlap thing.
* MM: API spec doesn’t have concept of WGSL spec. Doesn’t belong there.
* AB: So, when make a store don’t access other memory locations. How are you planning on accessing those? Don’t think it needs to be spec’d
* KG: Rules as written think we’re safe. If write doesn’t write padding, only values, then results should be untouched
* JB: Sometimes people expand small writes to larger writes and that, depending on actual bus size, can be an optimization. Basically totally observable in system as whole. Put thing in storage buffer, call shader, and look at bytes. Say it’s unspecified what happens to padding bytes when shader writes to buffer.
* KG: Instinct also.
* BC: Fairly sure for a couple backends we trample over padding bytes
* JB: Very common
* KG: Satisfied as a deviation from portability guarantees?
* MM: Satisfied, not UB in classic sense. Better then some of the non-portabilty we have already
* TR: Satisfied that writing to a struct with a buffer the impl can write to those padding bytes and making that invisible?
* MM: As long as padding bytes don’t come from different origin. To observe would write sentinel values then call shader with buffer. Shader casts to struct with float, float4 then shader writes on top of buffer and then shader finishes and on api side read out bytes on buffer and see if padding bytes got touched. Claim making, there are no guarantees on the values of those bytes.
* TR: Could always read bytes back through a shader as well by mapping to a new type
* KG: Aliasing rules for that. Could do in a different shader
* TR: Still seems odd to allow writing of padding. Doesn’t seem necessary
* AB: Allow in other places is a memcpy type operation. If copying whole struct don’t want to do multiple ops. Vulkan says you can’t touch padding at end but can touch in between.
* BC: What we do for the MSL backend. Have a struct with align/padding and populate struct and do assignment.
* TR: Seems 0 init of padding would be safer.
* MM: Safer for security or portability? \
TR: Portability and reproducibility. Keeping behavior defined.
* AB: Comes in to shader local storage. If have padding can’t instantiate variable in underlying language. Have to do futzing in shader to make those bytes available if underlying language doesn’t allow. Have to make new type just to set to 0.
* MM: Anything done, any struct with members it is legal to write a spriv implementation with a struct with the same members. Shouldn’t have to deconstruct WGSL to series of scalars
* TR: For HLSL will write padding because we break down structure. Opaque what struct is in HLSL you’re writing the member layout for the structure. Buffer layout could be different from register layout
* JB: For storage buffer do RWByteBuffer
* TR: No case in hlsl where this is  memcpy writing padding
* AB: … doesn’t get zero initialization. Done outside api. Function doesn't’ have a layout in spirv, probably doesn't’ exist. If write a struct, do i have to write the zeros? Seems weird to have to enforce.
* TR: Trying to say actually writing random registers out by doing memcpy seems odd
* AB: Agree. Feel making 2 issues. 1. Can a write write padding 2. Is padding initialized. Trying to focus on 2. If not disentangle you can’t.
* TR: Don’t have ot init padding if it doesn’t exist and doesn’t get written out anywhere
* MM: They’re entangled
* JB: TR is talking about case for workgroup which is not init for buffer and can only be accessed using type and wont’ be seen as another type so not observable
* AB: Copy struct with padding into buffer. Someone could observe
* JB: Load as type and store as type
* TR: Doesn’t store in HLSL
* JB: Will behave normally for htat storage buffer
* AB: Vulkan says you can write the copy so you can do a memcpy.
* JB: That’s a permitted implementation.
* MM: Question for TR, can construct a program where most of the struct is padding and can construct where local variable gets spilled into memory. In this program take big hunk of data and write to buffer, imagine a memcpy would be faster then scattered read/write. Encountered a program like this or saw scattered read/write was slower?
* TR: Don’t have data to compare. 1. People tend to avoid having data structures that are largely padding by rearranging so they aren’t. Waste of space and memory bandwidth. Don’t have data and wouldn't have as we don’t do memcpy vs individual access. Just do individual access.
* MM: Just looking for expertise
* TR: Don’t have evidence
* JB: Still at point saying we need to give implementations the leeway to have writes to storage buffers effect padding bytes and should be unspecified what ends up there as long as not from another security domain.
* MM: In metal it might be hard to make that guarantee. So in metal may have to make the scalar writes instead of memcpy
* TR: That’s what spec may say, only implementat can guarantee that what’s in padding can come from another security domain so either don’t write padding or write 0’s for padding. 
* MM: …? Makes guarantees about security origin as blanket statements
* KG: So need to write down that the padding is not defined?
* JB: Don’t say undefined
* AB: Right now it’s unspecified and can’t do better than that. Can add clarification that memory ops, specifically write accesses to struct, may change internal padding but not padding at end.
* KG: Unspecified values
* AB: Literally not specified now, just doesn’t appear.
* KG: Should write down it’s unspecified.
* JB: Good to confess we have variation.
* MM: How strongly does Google feel their operations that can clobber the memcpy are required by the compiler for perf reasons?
* AB: Can rephrase? What does clobber mean? \
MM: Believe Tex was proposing that spec bytes are not written when writes to buffers occur.
* TR: Getting at fact that’s what happens in HLSL but don’t known other languages and on’t know perf implications. That’s the alternative, padding can not be observed. Does not write/read.
* MM: Question, I’m a little worried about situation where we start writing padding bytes to buffers where we don’t know they came from. Is google convinced writing padding bytes is OK and the spec has to have facilities? How strong is opinion that padding should be undefined?
* AB: In spir-v would be an illegal transform to change copies to memcpy so can’t guarantee underlying implementation would do that. So if say we can’t touch padding bytes don’t know how to do that on vulkan
* KG: Would have to full struct manually
* AB: Don't’ know the underlying won’t change that back into cpy
* MM: There is a copy in spirv, don’t do that do a whole bunch of ops
* JB: AB is saying impl may gather those ops and make a single write and write other bytes. That’s allowed by vulkan.
* KG: What about 2 vec3’s in a row and upgrading to vec4 then control the padding as there is no padding
* JB: That seems bad
* AB: Can see overall type in spirv so know overall struct and can see all members set then why not upgrade? In vulkan I can write to that padding.
* TR: Does seem odd to me but don’t know spirv well enough that you can change several scalara writes to a larger write
* KG: one way allowed unaligned write
* TR: Writing to memory not in original program
* AB: That’s in program
* KG: Setting up layout saying it has shape and then writing to something and because you’ve said it has this layout and it has this padding, then the compiler can look and see that it’s writing to that struct and it’s only padding between so it _can_ merge those
* TR: Seem wrong. Just because scalar store comes from structure?
* KG: …
* TR: Have to check spec it says you can do that. Don’t wait on me.
* KG: GOod place to say wait on it a bit.
* MM: Means in spirv it isn’t point to int but it’s a thing in a struct and that’s wild. Type system in vulkan is way more complicated then what’s in spec
* KG: Like c++ spec which says you can’t have pointer to thing without value. You can represent it but baked in is if you have address of float you can’t assign that to type of int pointer. You can try but it’s UB. Top down meta promise of I won’t do these things or weird things happen.


### [specify behaviour of floating point operations on NaNs #1711](https://github.com/gpuweb/gpuweb/issues/1711)



* KG: Recommendation from a year ago was to say NaNs and denorms may not be maintained
* AB: Have that in spec
* KG: Then done i think.


### [Uniformity of Private-class global variables #1579](https://github.com/gpuweb/gpuweb/issues/1579)



* KG: Think also probably done?
* AB: Didn’t end up addressing specific concern. Right now non-uniform but this is not a primary input vector that is being designed around. SPIRV->WGSL. So, don’t know if we want to solve. That said, this ties into workgroupBroadcast and if things can be uniform as a global
* MM: Point I was about to say, if buffers count as non-uniform then private class globals have to be non-uniform. May make them both uniform but having different is weird
* KG: Aren’t globals uniform if they're uniform?
* AB: Because they’re globals, the analysis can’t keep track of globals
* KG: Conceptually wouldn’t it be intuitive that is the truest form of behaviour? Fine analysis doesn’t handle it yet. Might be where we might aim. Fine leaving them fully non-uniform for V1?
* AB: A DM issue, so JB have you run into this?
* JB: Have not run into where we needed it but just because haven’t run into it
* KG: Mark post-v1.

(10)


### [support volatile atomics · Issue #978](https://github.com/gpuweb/gpuweb/issues/978)



* 


### [Use brackets for array types · Issue #854](https://github.com/gpuweb/gpuweb/issues/854)



* 


### [shader programming model: use little-endian layouts for scalar data #234](https://github.com/gpuweb/gpuweb/issues/234) 



* 


### [WGSL pointer aliasing rules · Issue #1457 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/1457)



* 


### [Issues of pre/post matrix multiplication in wgsl #2481](https://github.com/gpuweb/gpuweb/issues/2481)



* 

(15)


### [wgsl: add 'array' overload to construct an array from components, infering type and count #2346](https://github.com/gpuweb/gpuweb/issues/2346) 



* 


### [Annotation for the uniformity analysis #2323](https://github.com/gpuweb/gpuweb/issues/2323)



* 


### [WGSL should forbid pointers to runtime-sized values as user-defined function parameters · Issue #2268](https://github.com/gpuweb/gpuweb/issues/2268)



* 


### [C-style casts · Issue #2220](https://github.com/gpuweb/gpuweb/issues/2220)



* 


### [☂️ Initial internal feedback #2213](https://github.com/gpuweb/gpuweb/issues/2213) 



* 

(20)


### [How should WGSL map to the availability/visibility parts of the Vulkan Memory Model? · Issue #2228](https://github.com/gpuweb/gpuweb/issues/2228)



* 


---


# ⚖️ Discussions


### [Workgroup Broadcast · Issue #2586](https://github.com/gpuweb/gpuweb/issues/2586) 



* 


---


# 📆 Next Meeting Agenda



* Next week: **APAC!** Tuesday, May 10, **5-6pm** (America/Los_Angeles)
* 