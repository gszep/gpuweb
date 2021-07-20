# **WGSL 2021-07-20 Minutes**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Jim Blandy

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
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * **James Price**
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
* Intel
    * Narifumi Iwamoto
    * Yunchao He
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
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
* **Mehmet Oguz Derin**
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


### [wgsl: Change operator%(f32,32) to use OpFRem #1945](https://github.com/gpuweb/gpuweb/pull/1945)



* (Change from `(x - y * floor(x/y))` to `(x - y * trunc(x/y))`)
* BC: tried various forms tried OpFRem, then tried operator approach, DN isn’t happy with not having the builtins, decided to go with both
* DM: Generally we decide to expose a builtin function if it maps to most of the backend APIs, or at least gives a performance advantage. OPFMod is not in MSL or HLSL, and it’s easy to polyfill/implement for the user.
* AB: DN did a deep dive, and found that all the variants seem to have uses in different languages, only two out of three agree. Our preference is to have both built-ins, and for Google the operator is optional
* MM: If it’s easy to polyfill, and the perf isn’t wildly different, then there’s not a strong argument to not have it
* DM: having fewer things is better - less to test, less to ship.
* AB: We have so many polyfills already, why is this a special case? Frexp, modf, struct return
* BC: Between SPIR-V and WGSL, SPIR-V has two instructions. Once upon a time, we cared about having a bidirectional translation
* MM: But bidirectional translations can be achieved with polyfills
* JG: I’m hearing a lot of minor concerns, not watershed points - can we just pick some solution?
* BC: I don’t know what DN’s feelings are, but I have a few bugs I’d like to close, and having the operator will make things easier for that
* DM: Can we have some guidelines about when we expose builtins? I thought we’d had an agreement, but now I’m not so sure
* MM: I don’t think the group has guidelines - and that would be philosophical debate
* JG: What we put in the library is just whatever we can get consensus on. Design by committee is pretty effective
* MM: It would be nice to have an issue listing polyfills and their definitions, so that we could look over and decide which ones we actually want
* BC: But, can we land 1945?
* JG: Seems so. [consensus]


### [[wgsl]: Add fmod(), frem() builtins #1913](https://github.com/gpuweb/gpuweb/pull/1913)



*  Skip for now, related to above


### [[wgsl] Proposal: Remove pointer out parameters from modf, frexp · Issue #1480](https://github.com/gpuweb/gpuweb/issues/1480)



*  MM: There’s a lot here to talk about
* JG: defer to discussion


### [Pointers to handles #1832](https://github.com/gpuweb/gpuweb/issues/1832)



* (Proposal: Ban address-of on reference-to-handle)
* MM: The issue wasn’t clear about the motivation. What would this stop authors from writing?
* AB: Currently function restrictions prevent you from creating a pointer to the handle storage class. Maybe in the future when we have arrays of samplers, right now these have no real purpose. No builtins take them. So let’s keep things simple and forbid them. We want handles to be passed by value to functions, so taking away the option to pass them by pointers makes sense. Since all the builtins work this way, it makes user functions more consistent with the builtins
* MM: So I can still say var foo: texture = my_texture?
* AB: If you were to do let foo = my_texture, that’s just a local name for that. But you wouldn’t be able to apply the & operator to my_texture
* AB: This would be permitted:

        Var x : sampler


        Let y = x;


        Let y : sampler  = x;

* AB: This is forbidden:

        Let z = &x;

* AB: You can’t pass z, none of the builtins accept it, all you can do is dereference it.
* MM: Can you provide an explicit type for y, with a colon?
* AB: Yes. We should always work with handles consistently as values
* MM: I think this makes sense. DN raised the point that *&*& works in C.
* JG: It just feels like a handle is a pointer by another name. And we forbid taking pointers to pointers…
* AB: Right, at the hardware level, the handle is really a pointer to some stuff in memory with metadata
* JG: And traditionally, var creates some memory and gives you a pointer to it.
* MM: It’s probably fine to disallow the *&*& case for now. If people ask for it, we can add it back in
* AB: The time to reexamine this is when we re-introduce bindless. It’s good to encourage people to work with resources in a consistent way, it’s a benefit to readers. Rather than some people using pointers and others using values, everyone will just use values.
* JG: Yeah, vars are pointers with sugar on top
* MM: Is anyone arguing against this
* JG: I am. It’s not onerous to have it in the language, and it’s consistent, so why not just let it work.
* DM: The handle storage class is different.
* JG: It’s weird to say, you can get a pointer from anything if it’s a var, because it’s a var, but pointer handles are special.
* AB: We may have gotten the var/let split wrong, but I don’t want to revisit it. Ideally, handles would not be permitted in vars.
* MM: I don’t have a position on this. But one argument that might be relevant: If we want to have a larger language feature like bindless, at the point that we want to specify that, we might want to change how pointers to resources work. Forbidding them now gives us the flexibility to set the rules later freely when we need them
* BC: The rules around what we can pass in parameters are already difficult enough to validate
* JG: Isn’t that orthogonal to this change?
* BC: Validation is complicated by chasing derefs and refs
* JG: Doesn’t the function’s signature make that clear?
* BC: If you’ve got a let which is the address of a texture, and then you pass that variable to a texture, you need to chase that down. It was an implementation difficulty.
* JP: It complicated the identification of originating variables.
* MM: But you do have the chain to back through.
* AB: Yes, but there are fewer cases to handle
* MM: I’m surprised one is easier to implement.
* DM: Do people take pointers to textures in MSL? HLSL doesn’t allow it. Let’s just forbid it and see what we need later
* JG: We already have restrictions on passing pointers to textures to functions. This is a different way to enforce that, at the expense of consistency in what you can do with vars. “This is a var, but because it’s a texture there are additional restrictions”
* MM: sympathetic to that. Adding unexpected restrictions is irritating to developers.
* AB: I don’t think it’s valuable to have two ways to do the same thing. Handles are already pointers. There’s precedent for treating handles this way. It would be fine if we used pointers everywhere when you work with handles, but we’ve already gone with the implicit handle approach, and we should stick with it
* JG: I just don’t like the thing we did with vars for handles
* AB: Please file an issue - I’m happy to have the discussion, I just thought it wouldn’t be palatable
* MM: David had strong opinions about what an allocation is, what an rvalue is. Let’s revisit that when he’s here
* Resolution: needs specification


### [Allow unsigned wgsl array sizes. #1942](https://github.com/gpuweb/gpuweb/pull/1942)



* MM: This was part of the larger issue later on the agenda, let’s talk about that first


### [wgsl: array size may be module-scope constant (possibly overridable) #1792](https://github.com/gpuweb/gpuweb/pull/1792)



* (Rebased, and updated to incorporate #1135 (allow array element count to be unsigned), and a bunch of cleanups)


### [Issue 1534's array types differ on stride #1943 ](https://github.com/gpuweb/gpuweb/pull/1943)



* (not enough time)


### [wgsl: can hex floats explicitly represent infinity and NaN as float literals? #1769 ](https://github.com/gpuweb/gpuweb/issues/1769#issuecomment-879990010) 



* (Updated proposal based on feedback)
* AB: We like DN’s proposal
* JG: Do we have NaNs?
* AB: not currently for hex floats
* AB: C++ has macros for generating nans and infinities
* JG: I think I’d like more time to look at this PR
* RM: Question: when a hex literal overflows the target type, it maps to infinity. But if a developer is going to bother to use the hex representation of a float, don’t we want to assume that they know what they’re doing and should use the right size?
* AB: Do you think that should be an error?
* RM: If the programmer cares enough to write in hex, then it seems like it would be safer to make it an error instead of mapping to infinity.
* AB: We don’t have a strong opinion about this. Conversion seems slightly more usable.
* RM: I put a comment on the issue, we can review when DN is here


### [Reading from MSAA depth textures #1924](https://github.com/gpuweb/gpuweb/issues/1924) 



* JG: Request from API group was that we need to have a MS depth texture type.
* AB: I don’t like that we have a section on multi-sampled textures, and a section on depth textures, and it’s not clear where it ends up. The spec organization isn’t clear
* JG: You could make ‘multisampled depth’ be a third category
* DM: Is redundancy bad? Why not list it in both places?
* MM: There should be one source of truth - duplicated information risks divergence
* DM: We can refactor the spec later, I just want the type
* MM: +1 on the new type
* AB: I think the PR is fine
* Resolved: PR needs review, then can land


## 

---



## Discuss


### [Buffer indices should be unsigned · Issue #1135](https://github.com/gpuweb/gpuweb/issues/1135)



* (tabled until dneto is back)


### [2nd, 3rd, and 4th components of sampled depth/stencil textures are undefined in Metal · Issue #1198](https://github.com/gpuweb/gpuweb/issues/1198)



* (previously) MM: (listed 3 main directions)
    * 1. Allow deviant behaviour for those components.
        * 1b.  Allow two distinct behaviours (the ones that are observable today)
    * 2. Make a stencil texture type in shader language.
    * 3. Don’t allow binding stencil textures like this (as a shader resource)
* MM: Didn’t we resolve this in yesterday’s WebGPU meeting? We agreed to leave this unspecified.
* AB: We’re okay with that.
* DM: Arguably it’s not a shading language question
* MM: Right, because the shading language has no concept of the format inside the texture, so it would be an API issue, instead of a shading language issue
* JG: Prior discussion had been in this meeting, so I’m surprised it didn’t stay in this meeting


### [[wgsl] Proposal: Remove pointer out parameters from modf, frexp · Issue #1480](https://github.com/gpuweb/gpuweb/issues/1480)



* MM: Can authors use struct returns as well?
* AB: Initially limited
* RM: What’s the advantage of this approach to begin with?
* BC: A named structure would entail declaring a bunch of structures in global type scope. But this locks us into that; we can’t change the builtins to return anything different, even when they’re overloaded. The benefit of anonymous structures is that they give you names for the things, it avoids pointers, and anonymous structures may work for the cases we’ve discussed for multiple return values. But the goal was to not lock us into a different horribly-named global struct for each overload variant.
* JG: Does it lock us in? Can’t we just forbid them from naming the type?
* BC: That’s an anonymous struct.
* JG: But it’s not so hard to specify. If we think it’s easy enough to add anonymous structs, and it’s not too hard to implement, we should just do that
* MM: So you’d like to have structs named ModF_f32_f32, but authors can’t name those structs?
* JG: yes
* AB: Maybe for MVP, we can leave this as pointers. We spent a lot of time on this, and all the other languages take out parameters
* MM: We could split these functions into two functions, each of which returns one component
* RM: But wouldn’t that be inefficient?
* MM: In HLSL, some of these have to be polyfilled anyway. So it wouldn’t necessarily be a performance loss to make them separate functions
* AB: [mentions a case where there would be performance loss]
* DM: Calling the reserved names out in the spec now is useless. Even if they’re reserved words, new features/extensions would have to add more reserved words, breaking older code.
* JG: We would reserve a prefix, so additions won’t break existing code.
* BC: We already have a reserved prefix in other languages, lile “__xxx”
* DM: (nods)
* AB: There’s a lot of prior art for reserved prefixes.
* JG: even outvars aren’t the end of the world, we can add struct-returning overloads later
* JG: Couldn’t struct returns be post-MVP?
* MM: Having named structs, reserved names or not, is the expedient thing to do at this point.
* RM: Keeping it as-is would be fine for me. It doesn’t require adding anything, it’s ugly but it’s similar to other languages, so it’s not a barrier to adoption
* MM: I surveyed a lot of non-shader languages, and all but one of them use outparams [JB: Did I get that right?]
* MM: These functions are rare, it’s probably not worth having all this discussion.
* AB: Do we want to restrict what storage classes we can use with these outparam pointers?
* DM: In other cases, we’ve just exposed what the back end languages do, but we agreed that we should try to clean things up when the back end languages made mistakes. Now we even have the reserved prefix approach, so why aren’t we making the improvement?
* AB: Because we don’t agree, we have a workable solution for the MVP, and we have a way to fix it post-MVP.
* DM: Does anyone object to returning named structs with a reserved prefix? It seems like everyone liked it
* JG: fatigue?
* RM: I’m happy with that (reserved prefix)
* JG: Okay, let’s do it
* BC: I’ll draft a PR that uses a leading underscore for the struct name
* MM: When people use the word ‘reserved’, that can mean two things:
    * One says you can’t make a new type in that class
    * Another says you can’t even mention things in that class
* BC: We want the second case, so that we can change the names later. I’ll make sure we’re getting the second case when I draft the PR
* consensus \o/


## 

---



# 📆 Next Meeting Agenda



* Next meeting: 2021-07-27 (like normal)
* [Buffer indices should be unsigned · Issue #1135](https://github.com/gpuweb/gpuweb/issues/1135)
* [when does applying an attribute to a type result in a different type #1534](https://github.com/gpuweb/gpuweb/issues/1534)