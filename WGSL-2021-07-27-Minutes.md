# **WGSL 2021-07-27 Minutes**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send Dan Sinclair a Google Apps enabled address and he'll add you.



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting** (probably incomplete this week, I couldn’t catch everyone - JB)**:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * **Antonio Maiorano**
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
    * Sarah Mashayekhi
* Intel
    * Narifumi Iwamoto
    * Yunchao He
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
* Mozilla
    * **Dzmitry Malyshau**
    * **Jeff Gilbert**
    * **Jim Blandy**
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Dominic Cerisano
* Eduardo H.P. Souza
* Joshua Groves
* Kris & Paul Leathers
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Pelle Johnsen
* Timo de Kort
* Tyler Larson



---



# **⚖️ Agenda/Minutes**


## Meta


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
        * 


## 

---



## Timebox


### [Matrix initialization #1964](https://github.com/gpuweb/gpuweb/issues/1964) 



* JG: A user of the API said this code is ugly. They don’t want to construct a matrix out of the columns. And they have to initialize them with vec4s. So making mat4(vec4(), vec4(), vec4(), vec4()) is ugly
* JG: But this code is machine generated.
* JG: The reporter did research. Everyone has constructor overloads for these. You can do it by columns or by element. Seems like we should just allow overloads by elements.
* AB: Should this be a library function or a type constructor?
* JG: Type constructor
* JG: Resolved.
* DM: HLSL has row-major order, not column major order. This is unfortunate.
* DM: If people use their code straight from HLSL, they will be surprised
* AB: Does that mean we don’t want to help non-HLSL people?
* JG: HLSL is the most common language.
* JG: One idea that would fix this is to have an explicit builtin. mat4x4_colmajor(). It’s clear what’s going on. No one is ever confused.
* JG: But people already have to remember that mat4(vec4, vec4, vec4, vec4) is column major
* GR: HLSL accepts a qualifier to change the majorness of a matrix
* DM: Is the qualifier on the type or on the field?
* GR: It’s on the type.
* DM: Is it attached to the type? I can have 2 different float4x4s, one is column-major, and one is row-major
* MM: Isn’t it the same in GLSL too?
* JG: No. Those are binding-point differences, not shading language differences in GLSL
* GR: You can initialize with vectors
* DM: How does one initialize a column-major matrix in HLSL?
* ACTION: GR write something that does this ^^^ 
* RESOLVED: This is not resolved.
* GR (Addendum): I was mistaken about the effect of column_major. However, transpose() can be used to initialize matrices in column major order: float4x4 mat = transpose(float4x4(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16));  However, whenever you apply a single index to a matrix, you will get the row, not the column unless you apply a transform to it: transpose(mat)[0] == float4(1,2,3,4)


### [Add convenience type aliases [fiub]vecN and fmatNxM. #1976](https://github.com/gpuweb/gpuweb/pull/1976)



* JG: We discussed this a long time ago. This is because vec4&lt;f32> is too hard to type on the keyboard. One idea coming out of that is to make common overloads like fvec4 or ivec4, just as sugar. Not necessarily as a Type Alias, but just to say these types mean the same thing.
* MM: Can we make them typealiases?
* DN: We can make them such that they are predeclared before the beginning of the program. A predeclared typealias’s scope ends at the end of the program, meaning you can’t declare another identifier of that name at global scope, but you can inside a function, because they have different scopes. This uses shadowing.
* DN: So the person who doesn’t want to pay attention to this doesn’t have to.
* JG: So can rephrase this as for real typealiases.
* AB: Typealias should be able to let you use fvec4()
* KN: This isn’t the only solution to using a shorter constructor. If we inferred the type argument to the constructor, then it would be short.
* JB: Many languages have type constructors that are just brackets, and it figures out the type.
* JB: The reason we wanted to infer types for globals from the initializer is because you end up writing the type twice: Foo f = Foo(...). That redundancy would go away if we inferred the type of constructors. If we had a typeless constructor syntax
* MM: Trying to understand that direction.  Have type inference.  And what  you’re discussing is a type-inferring constructor. How does the computer know which type I’m trying to declare
* JG: var x : fvec4 = {1, 2, 3, 4} is how it would work. When you do the type checking, if one doesn’t have the type, you can infer it to the other one, going each way. Now we always go from inside out.
* KN: I don’t want outside-in type inference. That doesn’t gain us anything. I don’t want to say const x : fvec32 = {1.0, 1.0}. I do want vec2(1.0, 1.0). This is unambiguously typed, and doesn’t require outside-in inference. It saves all the typing, and is even 1 character better than fvec2
* JG: Something that could be useful here is when you are declaring a matrix. mat4x4f32([a, b, c, d], [e, f, g, h], …). Whenever you call into a function, the function type has to match.
* MM: Doing that would make it more difficult in future to allow overloading of user functions, which I think is really important.
* JB: What is the fvec2 in that final expression saying, that we don’t already know?
* JG: We’re not committed right now to the idea that literals will be unambiguously typed
* KN: The current proposal is to say “fvec2 means vec&lt;f32>”. If we added something that was _only_ for vectors and matrices, then this would be unambiguously typed
* JB: So we could use square brackets or something
* KN: Yeah, something like that
* JG: So there’s some disagreement about whether we want to go _further_. As an intermediary, how do people feel about these type aliases? They are also useful for function declarations
* KN: It’s great if these type aliases exist
* JG: Existing comment: Adding short prefixes doesn’t scale well to different sizes of scalars. We are implicitly reifying the 32-bit types. We want to pave the most common paths. If you want to go on another path, you can still do that. But if those also become super useful, we can add more type aliases in the future. This is worth it. These types are extraordinarily common
* DM: That makes sense. Do we really want to bless 32-bit types? It’s fine if we want to, if we’re conscious.
* RESOLVED: Accept this PR, with the modification to use real typealiases


### [[wgsl] Limit number of function parameters to 255 #1959](https://github.com/gpuweb/gpuweb/issues/1959) 



* DN: We have fuzzers which hit this limit. There are going to be cases that are easy to spec a rule that says XYZ is definitely invalid for tooling reasons. We want this to be a common limit and put it in the spec. There may be other limits (number of structure members, etc.). We could put the limit in the spec, or not
* RM: We should put it in the spec, if it’s required by the backends. For portability. It’s a common thing, like for web specs
* MM: Two classes.
    * Imagine we put a limit in the spec. Could live in a world where most implementations will succeed if the user program satisfies the limit requirement. Authors will want to know the boundary.
    * Second class.  Limit where almost every implementation will have a de facto limit which is smaller than the limit in the spec. E.g. instead of 255, then have 4B. E.g. every Vulkan device will fail in that case.  So putting that number in the spec is misleading to what will be generally expected to work.
* AB: For the ones David listed, can you review to see what strikes you as reasonable.
* MM: nesting level, switch cases, members per struct; these seem reasonable. Not sure about # function-scope variables (per function); want to consult with the Metal team on that one.
* AB: Another that may map well, is the number of entry points.  First step is to collect candidates.
* JG: Limits are the combo of “thou shalt not write a program with more than this” but are we guaranteeing that if you 255 WGSL function arguments, that we will always be able to compile that?
* AB: No. There can be other issues with your shader.
* MM: It is an indicate of roughly what authors should expect to be able to use
* DN: The half-million limit is wobbly enough that authors won’t get useful information out of the spec. Given that compilation can fail for weird reasons. I’d rather just withdraw that particular limit.
* JG: Resolved: specification needed:
    * 255 nesting level on composites. (This would cover the "structure nesting" and also the access-chain indices).
    * 16383 distinct switch case values/targets per switch.
    * 16383 members per struct.
    * 255 function parameters
* 


### [[wgsl]: Add fmod(), frem() builtins #1913](https://github.com/gpuweb/gpuweb/pull/1913)



*  [postponed]


### [wgsl: Change modf and frexp to return a structure #1973](https://github.com/gpuweb/gpuweb/pull/1973)



*  [postponed]


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



*  [postponed]
* (Rebased, and updated to incorporate #1135 (allow array element count to be unsigned), and a bunch of cleanups)
* Further discussion: allow this only the case where:
    * The array type is the store type for a workgroup variable. (only the outer dimension. (Then can be implemented in MSL as a pointer-to-local entry point param.)
* 


### [Issue 1534's array types differ on stride #1943](https://github.com/gpuweb/gpuweb/pull/1943)



*  [postponed]


### [wgsl: can hex floats explicitly represent infinity and NaN as float literals? #1769](https://github.com/gpuweb/gpuweb/issues/1769#issuecomment-879990010)



*  [postponed]


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135](https://github.com/gpuweb/gpuweb/issues/1135)



* [https://github.com/gpuweb/gpuweb/pull/1942](https://github.com/gpuweb/gpuweb/pull/1942) 
* [https://github.com/gpuweb/gpuweb/pull/1792](https://github.com/gpuweb/gpuweb/pull/1792) 
* JG: We reached agreement on accepting both signed and unsigned. We are confident that we can implement these in a way that works for all of our platforms.
* JG: I wrote up 1942 so grammatically you could use a uint instead of an int. 1792 includes this and more. Maybe that’s what we actually want.
* DN: 1792 didn’t reach consensus. I want to land yours instead.
* DN: For 1792, there was a discussion of a more constrained feature
* JG: Is there more to talk about here?
* DN: No. We all agree here.
* DN: I like 1942 that just updates the grammar. That’s the only change to WGSL that’s required.
* AB: I want to get back to 1792 later.
* RESOLVED: Accept 1942


### [when does applying an attribute to a type result in a different type #1534](https://github.com/gpuweb/gpuweb/issues/1534)



* AB: Me and DN discussed this offline. We should leave the tooling cost of dealing with complexity, when you try to assign an array with one stride to an array with another stride. DN has a nice way of phrasing it, so if the load rule is invoked, you’re stripping the array stride. You’re loading it into registers, and that has no stride. If you’re doing an assignment, if that assigned thing ends up having a stride, then you convert the stride …???
* AB: So, the attribute doesn’t change the type. The user can write a helper function of an array of 16 uints that can be passed to 
* MM: If an author writes a function taking an array, as you described.  And call it twice, with two different strided arrays, doesn’t the function have to know the stride (to codgen)  and therefore you couldn’t write it generically over strides.
* AB: Values vs. memory views.  It matters only when the type is a pointer or ref.  
* AB: Within a function you could take the pointer to something, and access an element. But the compiler will know what the stride is
* AB: the implementation decides how to lay out workgroup memory. I have seen implementations that can support other features because there is no specified layout. So workgroup layout doesn’t have, the implementation will choose to stride things to make perf increase.
* DM: I thought we have a limit for WG storage size, and the user has to fit
* AB: But the compiler can fail anyway
* DM: Fun.
* AB: This came up in the discussion of pipeline creation spuriously fail
* JG: We might want to actually call this out in the spec. People will actually hit this.
* JG: An author will look at their max size, craft their shader so they’re within that limit, and assume that because they’re under the limit, it will work.
* AB: The API already has a limit on workgroup storage used, right?
* JG: Ok.
* DM: I’m fine with this. IIRC right now, the tooling doesn’t need work for this. In the future, we’ll carry around the stride as data inside the pointer.
* AB: Right now the tooling needs to know whether it can do an element-wise pointer. Say, in SPIR-V, I could OpCopyLogical between two different arrays of different strides. Or I could do it element-wise. In other APIs, I could use memcpy() or something like that.
* DN: This is a pretty tough discussion to have. I would probably want to write a speculative PR. In our discussion offline, AB reversed my leaning. It’s subtle
* AB: DN came up with good spec language
* MM: It seems like a layering violation for a pointer to hold the stride instead of an array
* JG: We’re not even there yet. You can’t even hit this problem in the first place just yet.
* AB: SPIR-V does it this way because it wants to support getElementPtr. The first index in getelementptr acts like an array index, but you don’t have the array in hand.  But the address calculation needs to know the stride to multiply that index by to get the address offset.  It’s purely a representational issue.
* KN: My understanding was that array strides would be attached to the type, and they would only apply if you used them in certain ways. Sure you could have a pointer to an array, but what if you have a typedef of an array, or a struct containing an array? Is this disallowed?
* AB: You can put it on an array element. It only applies in a small subset of places.
* DN: I’d like to stop this conversation and have a written thing before we go further. What Kai just raised and what Myles raised will be written down.
* JG: Okay.
* ACTION: DN to write proposed spec text.


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792) 



* AB: What we want to turn this into, to support this on Metal, is to allow a single outermost level of array in workgroup storage that can be sized by a specialization constant. This can be compiled to SPIR-V and Metal. They’re pretty much the exact same.
* AB: The tooling shouldn’t assign a number under the hood. We need to determine what is associated with what global. If we 
* MM: That approach isn’t unreasonable.  Think structural features should be imposed structurally.  Unfortunate to have this limited by a bunch of prose.  Think this feature is sufficiently distinct from spec constants.  Would like a different grammar or type to represent this. Just because it seems kind of silly to have something in this spec that can only be used in a corner case that is surprising and non-obvious.
* AB: Sympathetic to that argument, but don’t want a lot of different array types. There’s a tension there.
* MM: Can write down some ideas.
* AB: Important thing here is that we should have a feature that functions this way (non-statically sized workgroup storage array).  How it’s spelled is a second step. 
* MM:  +1 to that. Want one language feature that maps to both Vulkan and Metal. How does an HLSL author write code that exercises this feature? Is it possible?
* GR: Don’t know.
* MM: If it’s possible in D3D, then the WGSL feature should map to that too.
* AB: As I recall, it’s an #ifdef that is then late-compiled.
* MM: I plan to talk to Greg about this offline, and come back with examples/proposals.
* GR: Heard back: there is no such D3D feature.
* MM: Ok, then will limit viable targets to match Vulkan and Metal.


## 

---



# 📆 Next Meeting Agenda



* Next meeting: 2021-07-27 (like normal)
* 