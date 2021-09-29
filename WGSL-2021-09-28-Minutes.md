# WGSL 2021-09-28 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** David Neto

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
    * Alan Baker
    * Antonio Maiorano
    * **Ben Clayton**
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
    * Jim Blandy
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
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)

Next week’s the time slot is moved to APAC-friendly time: 5PM America/Los_Angeles


### On Todos



* (JG: I will make a spreadsheet for thumbs-up/down/maybe for each todo)
* 


---


# ⏳ Timeboxes


### [wgsl: remove ignore, add phony-assignment #2127](https://github.com/gpuweb/gpuweb/pull/2127)



* **DN to reword PR as per previous meeting**
* MM: It does feel weird to me to do `_ = texture` to mark static use instead of `ignore(texture)`
* JG: IIRC there’s no special sauce in our builtin ignore, so it could be a user function? (though it would have to be e.g. `ignore_texture()`)
* BC: Go has _ =.  It’s a sink for values.
* DM: Can’t we do `let foo = texture`?
* ??: I think we require let-able types to be constructable
* MM: If I want my buffer to be static use, you’d want to say `_ = my_buffer`, which we can’t do today
* …
* JG: Could we not just also wave the magic wand and waive requirement for underscore assignment to be e.g. constructable, which would allow this.
* DN: We 
* JG: Maybe acceptable if not very satisfying? Least bad here?
* 
* MM: underscore has a problem because it’s unnatural to write  _ = texturething.
* DM: But ignore would need the same amount of magic (e.g. ignore(buffer)).  So may as well have the magic in one place: put it all in the _ rule.
* JG: Alright, **previous resolution stands then: Replace ignore with _ and make _ sufficiently magical.**


### [precise_math attribute on functions #2080](https://github.com/gpuweb/gpuweb/pull/2080)



* (Previously: MM: Let’s postpone.)
* DM: Offline, discussed MSL’s clang::optnone, which is on function scope.  Kai noted it’s a big hammer and may not do what’s wanted.
* MM: I thought we postponed this until after MVP.
* DM: New information came up.
* MM: Would like to re-propose postpone to MVP.  That attribute is not part of the Metal API. So you can’t rely on it. Don’t think it’s a good solution.
* DM: Other idea is to expose a function that shields optimizations across its argument vs. its result.  Think we can implement it well on all backends.  (NoContract on SPIR-V, or select trick as discussed on the issue.)
* MM: Not familiar with the technique, and I didn’t prepare; think it was a mistake to put this on the agenda.
* DN: Also think this can be postponed until after MVP. Have workaround, even if it’s in the “lore” category.
* JG: Will mark as milestone Post-V1.


### [ Request for compute: anyInvocation() and allInvocation() #2137 ](https://github.com/gpuweb/gpuweb/issues/2137)



* MM: These functions are no better or works than the other functions that we kicked out of MVP.  No reason to take them in.
* MOD: These also depend on the active mask. So same issues plague it as the whole subgroup feature discussed earlier.
* JG: Can **postpone**.
* GR: Is Invocation a wave or warp or quad?
* MM: A thread
* GR: +1 for postpone then
* MM: This is a good feature that we want, we just have more work to do here before we can ship it.

 


### [Сlarify rounding #2140](https://github.com/gpuweb/gpuweb/pull/2140)



* JG: Is this reasonable?
* DN: It’s wishing for something we don’t have. We can’t really narrow it down below two rounding modes.
* JG: Should we just list the two?
* DN: Plausible? Would like to see data. Those are the most common two, but I’m not 100% sure if that’s enough.
* **Needs Investigation**
* **DN to also review current PR**


---


# ⚖️ Discussions


### [wgsl: Add limits section, and "spurious" failure #1997 ](https://github.com/gpuweb/gpuweb/pull/1997) 



* DM: Helpful to think of concrete situations. E.g. auto-tuning compute shader.  What do they to today. They get out of memory, generally. They react and rewrite their shader until it succeeds.  How can the user do better by returning a “spurious” failure rather than “out of memory”.
* DM: should pass through the out of memory.  Don’t think our heuristics are going to add value.
* JG: isn’t “spurious” slightly more ergonomic? Why induct new users into the lore of “sometimes your shader doesn’t compile”.  It’s extraordinarily likely that it’s not really out of memory.  More likely just an issue we found.  I would like to pretend to do better.  This could save multiple people dealing with bug reports “hey it says out of memory but it’s not actually out of memory”. “Oh, it’s just a failure to compile”.  I’ve had to rescue things from investigation and triage for issues that were reported with out of memory.  I’d feel better saying “Generic error”.  It’s better to have *less* information. Saying “out of memory” is actually deceptive, even when the underlying implementation said out of memory.
* MM: 1. Think the motivation is that some limits are not surfaced well; you only know about them when you hit them. This occurs when the program satisfies all the browser’s checks, but the platform API fails to compile. In the Metal case this yields an NSError, with some string information.  So this spurious thing make sense in this case.  E.g. used too many registers, or function calls nested too deeply. For us, we’d yield “spurious” for this NSError case.  Should be rare, unless you hit one of these limits. So we like the functionality.
* MM: 2. For a case where you nested an expression too deeply, but “spurious” is misleading. As if trying again in a few seconds might succeed.  More like “implementation limit exceeded”. 
* GR: Agree name is bad.  Good case for driver bugs.  Another use case is when there are limits that can’t be well articulated.   “internal error” is common for compiler world.
* DM: We have a few categories.  User reacts differently.  E.g. rewrite your shader, or file a bug to the browser / platform.
* GR: Is there a debug layer that will help you get more info.  That would be nice.
* DM: Does the new kind of error carry extra detail in a message?
* DN: Sympathetic that a user can have two different actions: rewrite shader, or file a bug. But  it’s not clear to determine which action to take.
* DM: Ok, got an answer to my use case question.
* JG: Ok, let’s find a better name than “spurious”, which we all agree to replace.


### [Remove requirement that function / builtin return values are always consumed #2138 ](https://github.com/gpuweb/gpuweb/issues/2138) 



* DN: I wanted to add guardrails, but it seems to be causing heartache, so I’m fine dropping this requirement.
* RM: I also think it doesn’t need to be a requirement. Fine with not making it a rule so long as we can have a warning.
* DM: Let’s remove the rule.
* MM: Fine with me.  
* MM: WGSL/WebGPU should have a mechanism to yield warnings.
* DM: To do a nice job, it’s nontrivial analysis to determine if function has side effects, and only warn when the function has no side effects.
* MM: No, keep it simple:  function result not used.  As long as we have the _
* JG: The bar is lower for warnings.  If you don’t like the warnings, you have workaround with _ *and* can control warnings.
* DM: I don’t like false positives. Authors will turn them off, and they lose utility. Or complex control for turning off specific warnings.
* JG: They aren’t false positives (?)
* DM: They’re not false positives on atomics.
* JG: Leaves room to adjust warnings based on feedback. E.g. stop generating them for atomic functions.
* MM: Warnings are not standardized.  
* JG: I think I proposed a mechanism for warnings, 
* MM: Can write them to the console, as a last resort?
* JG: Shader module compilation information.  [https://www.w3.org/TR/webgpu/#shader-module-compilation-information](https://www.w3.org/TR/webgpu/#shader-module-compilation-information) 
* **Resolved: Remove requirement to consume return values**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-10-05 (APAC 5pm `America/Los_Angeles` time)
* PR [#2143](https://github.com/gpuweb/gpuweb/pull/2143) Allow syntactic navigation and styling