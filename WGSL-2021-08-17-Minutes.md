# WGSL 2021-08-17 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Alan Baker

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.



---



# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * David Neto
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
    * **Damyan Pepper**
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
* **Mehmet Oguz Derin**
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
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
        * 


## 

---



# ⏳ Timeboxes


### [ Add new matrix constructors #2001](https://github.com/gpuweb/gpuweb/pull/2001)



* Ship it


### [ Clarify out-of-bounds RMW atomics #2032](https://github.com/gpuweb/gpuweb/pull/2032)



* MM: Looks like what we decided on, yes.
* Ship it


### [ Use brackets for array types #854](https://github.com/gpuweb/gpuweb/issues/854)



* JG: Do we want to talk about this before MVP?
* MM: Important to talk about, because it’s hard to feature-detect, and also people will definitely assume it works everywhere if it works anywhere.
* AB: Talking about arrays internally, one thing we’re considering is pulling stride into the type. (removing it as an attribute)
* MM: Is this a functional change?
* AB: Benefit is that it’s less ambiguous when types are compatible, or when attribs are required. 
* MM: I see three directions:
    * 1. `[[stride=12]] i32[5]`
    * 2. `i32[(stride=12), 5]` as long as the () parts are optional (or something like that)
    * 3. `i32[5]`, and if you need stride, use angle-brackets
* MM: Should we postpone discussion until Google has its discussions?
* AB: Not ready to agree to a final syntax today, but not opposed to it either.
* DM: I don’t think C’s syntax is a good one to follow, and I think other languages end up with because of their C heritage. I agree with DN’s points about why this syntax can be complicated/confusing.
* MM: Yeah, so like `i32[4][5]` is confusing because it’s not clear what it is? (yeah)
* AB: I feel like we’re there already with our accessors, so if you have to access them you’re already decoding it mentally somehow.
* JG: That would be a (i32[4])[5]
* AB: But that’s backwards from how the accessor works
* DM: We have conflicting guidelines: It would be nice to have the access and declaration order be the same, but also we should match the C style(?)
* JB: Comparisons to C are worrisome, because we’ve already decided to swap the order of the ident and the type for declarations. I think if we’re trying to be recognizable, it might be too late to cleave towards what C does.
* [...]
* [https://godbolt.org/z/MKKW4nMT7](https://godbolt.org/z/MKKW4nMT7) 
* void (*myFunctionPointer)(int[3][4])
* (We should come back to this in a week after more thought)


## 

---



# **⚖️ **Discussions


### [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 



* MM:
    * What should be able to be constexpr
    * Should it be an attrib? New keyword
    * Can you take a pointer to them?
* AB: + Must the compiler evaluate them?
* MM: Let’s start with, what can flow into a constexpr?
* JG: Just literals and constexpr?
* MM: What about math? (we want math)
* JG: A couple different tiers:
    * literals and constexpr
    * Operators
    * Builtins
    * Constexpr functions
* MM: Can’t have sampling from a constexpr, but could do sqrt. Like mathy vs not-mathy
* JG: I want to avoid defining what’s required for a function/builtin to be a constexpr, but maybe we can just mark a bunch of builtins as constexpr
* MM: Eventually we want users to be able to do this, even if it’s illegal today
* DM: If we have constexpr, is there still use for non-overridable constants
* MM: I would view them as the same feature
* DM: Same syntax then? If not overridable, then it’s constexpr.
* AB: Though if you want to forbid sampling, it would look different
* MM: At the global scope, just `let` would be constexpr.
* AB: Yes, but not outside global scope
* MM: We probably want a way for an author to commit that something is constexpr (and have it validated that it is)
* JG: OK I think we know what we want to be constexpr-able
* […]
* MM: Imagine a program
    * Let x = myTexture.sample(...);
    * Let y = 5;
    * myTexture.sample(..., y);
* MM: From an author’s perspective, we want authors to know that y can flow into other constexpr, but x cannot.
* AB: We could have it either be implicit or explicit
* MM: There are places where constexpr are required, so ideally authors can assert (early) what’s constexpr or not.
* AB: Technically optional
* MM: It’d be spooky action at a distance, where somewhere up the dependency tree breaks something downstream.
* JG: What about `constexpr y = 5;`, like instead of `let`
* DM: Do we need these at local scope?
* MM: I now think we do, since it’s nice to have declarations near their use site.
* [...]
* MM: Could have a rule that, for everything you would use `let` for at the global scope today, instead you have to use `constexpr`, and make it illegal to use `let` at the global scope.
* MM: So potentially, at the global scope, `constexpr x = 5` is legal, but `let x = 5` isn’t.
* AB: Well, this gets into “when does the compiler need to evaluate these at compile time”
* MM: Specs are as-if, so I think we don’t need to make strong commitments here
* AB: In floating point, should everyone produce the same bits? E.g. for a program that (implicitly?) relies on rounding errors?
* MM: We don’t commit to ieee754 so I think it’s fine to leave unspecified
* AB: Does constexpr in control-flow change static use rules?
* MM: I don’t think so, and I think we’ve previously said no. In particular dead code elimination is best-effort. (not required)
* MM: Sort of a separate thing, we should have a reflection API where we would not require identical reflection data to be returned on different APIs or OSes. This reflection data would return information about the shader _after_ the OS-level optimizers have run. It would be part of the contract this _this_ specific API would be intentionally non-portable. The reason is from some advice that Microsoft gave maybe 2 years ago about how authors often specialize huge shaders, and rely on post-optimization reflection data to determine which inputs they actually need to hook up to the shader


## 

---



# 📆 Next Meeting Agenda



* Next meeting: 2021-08-24 (like normal)
* Do we have enough to discuss? Yes
* No WebGPU meeting take-over, just normal tuesday meeting