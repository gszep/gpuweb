# WGSL 2022-01-25 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

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
    * **Dzmitry Malyshau**
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


### Virtual F2F



* Virtual F2F week of (Monday) February 7th
    * (See email “WGSL VF2F February 2022”)
    * Focus: remaining work to finalize V1
    * Proposed schedule:
        * Day 1:
            * State of the spec (dneto)
            * Literal unification
            * Constexpr
        * Day 2:
            * Uniformity analysis (and related issues)
            * Atomics
            * Smaller data types investigation (native vs emulation)
        * Day 3:
            * Air time for Google, Apple, Mozilla and anyone else to present features or changes they want for V1
                * Ideally each company could have some slides/materials prepared if there are still changes they wish to make to WGSL


---


# ⏳ Timeboxes


### [wgsl: detailed semantics of integer division and remainder #1830](https://github.com/gpuweb/gpuweb/pull/1830) 



* DN: two exceptional cases: MIN_INT/-1 and div-by-0. Spec’d both in favor of portability. Polyfill would use two selects
* MM: how would the MIN_INT case work without 64-bit ints. Can see how you’d pack into 64-bit, what if you don’t have it?
* DN: two equalities and a boolean and as the condition of the select.
* MM: just change denominator to 1? Would like to name the specific result you get. Happy with any branch-less polyfill
* DN: think it names the value. This PR also changes modulus in consistent manner.
* DM: is this worth it? INT_MIN case seems extra exceptional. Would hate this.
* RM: would be ok if value was undefined, but not ok with UB.
* KG: is part of the intuition that integer division is already expensive?
* MM: yes, also rare. So making it worse isn’t a bad outcome for more well-defined language. Worthwhile to have integers fully defined.
* DM: is it real UB?
* DN: yes in C++
* AB: and SPIR-V
* DM: is it a security problem?
* RM: think since it is based on C++, in metal it could be
* MM: optimizations could exploit this and elide most of the functionality dependent on it.
* JP: MSL spec says unspecified value. Doesn’t cause an exception. \
MSL spec says:
* "Division on integer types that result in a value that lies outside of the range bounded by the maximum and minimum representable values of the integer type, such as TYPE_MIN/-1 for signed integer types or division by zero, does not cause an exception but results in an unspecified value."
* GR: could we make the polyfill obvious to change per platform? Know how I’d do it in DXIL
* DM: can we make WGSL say undefined value?
* MM: still think portability is important, even if security is not a problem.
* DM: think it’s about tradeoffs
* DN: ok with undefined value
* KG: ok because it’s integer division
* MM: could see this in a fast math extension
* JB: read most denominators are constant. Doubt this is expensive
* _Consensus leave as fully defined_
* KG: Would be nice to select the divisors in the same way.


### [wgsl: support extended integer arithmetic for add, subtract, multiply · Issue #1565](https://github.com/gpuweb/gpuweb/issues/1565)



* DN: Google happy post-V1
* RM: ok either way
* _Post V1_


### [Rename storage class into memory class #2524](https://github.com/gpuweb/gpuweb/pull/2524)



* DM: lots of terms use storage. Too many IMO. I picked changing storage class to memory class. Don’t think David’s reasoning is that strong
* DM: “memory class” is fine with me as standalone, detached from past cultural reference.
* AB: This is a new term of art.  Would like reuse a name like “address space” used elsewhere. “Memory class” doesn’t conjure something specific to me.
* AB: HLSL’s “constant” buffer  for UBO
* JB: MSL uses “address space”.
* AB: OpenCL uses address space, lots of compilers use it.
* MM: In Metal it is a different address space.
* JB: We segregate these because they have different implementations.
* _Consensus: rename “storage class” to “address space”.  DM to pick it up._


### [Add override declarations #2404](https://github.com/gpuweb/gpuweb/pull/2404)



* AB: Remaining issue is renaming “override” to something else, but without other suggestions.
* KG: Let’s take this, and rename later if someone makes a better suggestion.
* _Agree to land._


### [Uniformity analysis #1571](https://github.com/gpuweb/gpuweb/pull/1571)



    * Robin, Alan, David met offline and came to a common understanding of what needs to be done before landing vs. after landing.  Expect a landable PR by the end of week of .  Expect to review and land next week.
* Since then Robin updated, and Alan reviewed and approved. Have a list of followups to do in separate PRs (see note in the issue).
* AB: Would like others to review, but also accelerate landing so we can get to implementation.
* DM: We agreed conceptually on what it is, so I expect to approve and land by next week.
* _One week to review, expect to land next meeting or earlier._


---


# ⚖️ Discussions


### [Should unreachable-code be an error or warning? #2378](https://github.com/gpuweb/gpuweb/issues/2378)



* RM: Already agreed to allow unreachable code, with a warning.
    * But what is the behaviour analysis of that code.
    * What should be checked inside that code.
    * 
* RM: I think we should not treat these questions differently. Don’t think it’s wise to allow dead code to be silently bad.
* KG: Should type check dead code no matter what.  For behaviour or uniformity, I’m more fuzzy.  Not sure of the philosophical tying them together; whether that’s the driving factor. Should decide on their own merits, not the tying principle.
* MM: What is the motivation for discussing this.  Design choice or codegen example.
* DM: We like dead code when we hack on stuff. Also codegen examples show us this is wanted.
* MM: If we know of codegens that exercise the weird case, then we can try to be as strict as possible that accommodates those cases.
* DN:  Like the change: For functions with a return type "all control flow paths that don't discard need to return a value".
* BC: Tint implementation does not accumulate behaviours for unreachable code.  Argument is it’s control flow analysis: what are the kinds of execution paths through your code. You have a return, then content underneath. That’s definitely unreachable. Behaviour analysis right now feeds on the inputs and paths into that block.  So that code after the return doesn’t have an input from anywhere. Your behaviour analysis is definitely going to draw invalid conclusions about that code (there are no inputs).  Gets worse with uniformity.  Example you could imagine:  a uniformity bug post-return, but then it goes away when the terminating return goes away; or vice versa.  Value-type analysis is much less context-dependent that way.
* RM: Behaviour analysis is always conservative. Can’t be 100% precise.  We still run it even over `if (false) { .. } `  So don’t see why we run it on one kind of dead code, but not another.   Not clear why it’s different from type checking, but I see I’m in the minority in considering it that way.
* RM: We can make rules that are more strict (reject more programs), then relax later.
* DM: We have a framework in analyzing what dead code is.  Let’s use it. If the framework new that “if (false) { … } “ is really dead then we would take advantage of it.
* RM: The soundness of the analysis cannot depend on not running the dead code. It’s always safe to be more conservative.
* DN: When I insert a return in the middle of development, I deliberately want to cut off analysis of tha tlater code when I work on another  part.  Value type-checking I still want because of programming culture: it arrived much earlier than flow control checking.
* KG: I view value-type checking in dead code as a feature.  In Mozilla code I’ve moved away from #ifdef and toward constexpr conditions, so that static type checking is enforced on any platform I’m working on.
* MM: Maybe we want a new keyword to mean “cut off analysis here”, rather than trying to do it with “return …;”.
* KG: Want to see examples where it breaks down.


### [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 



* Where are we? What do we need?
* 


---


# 📆 Next Meeting Agenda



* Next week’s meeting: 2022-02-01 11am Los Angeles time (non-APAC)
* Finalize F2F Agenda
* 