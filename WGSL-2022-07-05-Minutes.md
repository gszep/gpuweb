# WGSL 2022-07-05 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️🙏 Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** ****APAC!**** Tuesday **5pm-6 **Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+milestone%3AV1.0+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+)**<span style="text-decoration:underline;"> </span>**

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-06-28 - WGSL - Agenda / Minutes](https://docs.google.com/document/d/1a5fj6wqQ5i8KleiT-R9CV-KHGJQm9IzdbmS3KWIqphk) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Daniel Glastonbury**
    * Myles C. Maxfield
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * dan sinclair
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Price
    * Rahul Garg
    * Ryan Harrison
* Intel
    * Hao Li
    * **Jia A Chen**
    * Jiajia Qin
    * **Jiawei Shao**
    * Narifumi Iwamoto
    * **Shaobo Yan**
    * **Yang Gu**
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * Rafael Cintron
    * Tex Riddell
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



* [https://github.com/gpuweb/gpuweb/issues/3113](https://github.com/gpuweb/gpuweb/issues/3113) Identifiers never resolve in certain contexts
    * Fixes: [https://github.com/gpuweb/gpuweb/pull/3117](https://github.com/gpuweb/gpuweb/pull/3117) 


---


# ⏳ Timeboxes (until 5:30)


### [wgsl: should length builtin have AbstractFloat and f16 overloads? #3111](https://github.com/gpuweb/gpuweb/issues/3111)



* **Resolved: YES, both**


### [Escape Mechanisms in Identifiers #2810](https://github.com/gpuweb/gpuweb/issues/2810)



* Offline: DN: Recommend closing this. Don’t keep it in PostV1.  (But only discuss if Oguz is available)
* Offline: MOD: Label was post-v1 (there the lowest priority likely to never make to the front of queue), don’t get why this gets aired. Was an extract from good expert inquiry.
* _CLOSED_
* KG: Procedurally, would prefer to keep post-v1 things open, but just in the post-v1 category, which would not cause them to show up in our v1 issue burndowns/triage. Ok?
* MOD: Sounds good.
* **KG: I will reopen and leave this as post-v1**


### [Why have literal suffixes at all? #2664](https://github.com/gpuweb/gpuweb/issues/2664)



* Offline: DN: All that remains is the idea of local context where you could say “abstract floats resolve to f16 before f32”. I would like to push that conversation to post-v1.
* MM: One of the options in this area is to have a language feature to request a different “concretization” of e.g. AbstractFloat to f16 instead of f32.
* MM: But also this got assigned to me, but I’m happy with what we have.
* DN: We can revisit post-v1.
* **Resolved: Status quo, proposal for concretization toggle welcome post-v1.**


### [wgsl: Is/should initializer of module-scope variables limited to creation-time expressions? #3081](https://github.com/gpuweb/gpuweb/issues/3081)



* Offline: DN: Google proposal: Background is at: [https://github.com/gpuweb/gpuweb/issues/3081#issuecomment-1170472250](https://github.com/gpuweb/gpuweb/issues/3081#issuecomment-1170472250) but the summary is:
    * const expressions can be struct. (We already agreed to allow array, in [Include arrays in creation-time constant expressions #3056](https://github.com/gpuweb/gpuweb/issues/3056))
    * override expressions can include structure-construction expressions, and member-access expressions
    * initializer to var&lt;private> is any override expression.
*  DN: We used to just have vars in module scope, could only be init’d by particular grammar construct. Now we have const-expr. Now discussion, when things are changeable, e.g. with specialization constants.
* MM: When we talk about adding creation time expr, that’s in addition to what we have now, right?
    * struct Foo { x: i32, y : i32 }
    * var myles: Foo = Foo(3, 4)
* DN: Yes. Point is to make this strictly more expressive.
* KG: So this is generating @const ctors for user structs.
* KG: Nested too?
* JB: Yes
* [...]
* DN: There are also overrides to consider
* MM: So could make `override X = Foo(1); var&lt;private> bar = X;` I remember arguing against this back in the day, because people should be aware when they’re Overriding.
* DN: What this does is allows `bar` to be among the overrides in functionality within the shader, but without exposing the interface to the ShaderModule by having a new named `override`. 
* MM: So if someone does the above, and we shadow-add the second var as a new override in the backends? Author can do this themselves, yes?
* AB: a var&lt;private> is mutable, and can be reassigned. Once you start executing, the override is an immutable value.  Override has to be assignable to something else to get into the computation flow somewhere.
* JB: It moves the point at which a var&lt;private> initializer to one point later than what I had pictured.
* AB: In thinking about phases, think creation-time expr first, pipeline-creation second, and var&lt;private> third.   No reason to associate the phase with address space so directly.
* **_Resolved to go with Google proposal._**
* MM: For HLSL, both var&lt;private> and overridable maps to same thing.  For MSL understand the translation.  For SPIR-V what’s the story.
* AB: If a var can have an initializer at module scope (Private), then it can be constant or specializable constant. So matches.


---


# ⚖️ Discussions


### [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457)



* MM: Posted examples
* AB: The first one: the compiler has to see the aliasing of y and z to x.  So myBuffer[0] gets 4 and myBuffer[1] gets 6.  Guarantee by the spec as written (we believe).
* AB: Second example: is permitted. But since there is aliasing, can’t guaranteed the values in myBuffer[0] or [1]; can’t be portable due to limitation of HLSL.
* AB: Avoid whole-program analysis. First case only requires an analysis within a function body.
* KG: Hopefully user agents would provide useful warnings about this situation.  Also could see people asking for feature flag. 
* JB: Within a function body we have to track the originating variable.
* AB: Don’t have to track the originating variable within a function body. 
* **JB: Let’s talk, I want time to think on this. Can we come back next week? (yes)**
* DN: TO emphasize, none of the compilers that we write will need to do the in-function analysis. So we shouldn’t have to do anything, because the native compilers should find any errors for us naturally.
* MM: What about HLSL? \
DN: We would do a transformation to the function body that causes the right thing to happen for free.
* JB: So you’re getting errors from the platform, and propagating those?
* DN: There aren’t errors. The first example is fine and would be well defined. The second example would have [implementation-defined behavior], so no promises there.
* MM: Eventually when we have real pointers, this will have to be fixed. That future extension should probably make the second example well-defined.
* AB: Maybe? Would have to be guarded by an `enable`
* MM: Yes, guarded by `enable`, and probably not implementable on HLSL?
* AB: Spirv still requires strict guarantees, so we’d have to look into it.
* MM: Sounds like later we’ll have to discuss this again, but in the future [after shipping]. It would be cool to not be held back by HLSL at some point in the future.
* 

Any APAC topics?



* ZJ: On #3081, it seems we don’t have a way to mandate override expressions for non-scalar types? Because we say that override must have scalar types, but we could have a vector of another override. Maybe I will leave a comment, and we can discuss if needed.
* DN: So the proposal would be that "override expressions can include structure-construction expressions, and member-access expressions"
* ZJ: So what about e.g. `override x = 1; vec4&lt;i32>(x,x,x,x);`
* AB: Think it could be supported, but would need more implementation work.
* MM: If we mark something as override, then it must be settable from the API side
* ZJ: That’s for an override declaration.  But expression can be more types.
* MM: If you give a name to this thing vec4&lt;i32>(x,x,x,x); then it should be something that should be overriable from the API side, but it is not possible right now.  So if we add this feature, then it should be overridable from the API side.
* AB: Right, that must be part of the solution. Will take more design work.  Suggest filing this as a separate issue.  Think it’s orthogonal to the resolution.
* MM: Question for ZJ: which milestone: v1 or post-v1?
* **ZJ: I will file another issue to track this.**

Alan:  Especially to Jim: SPIR-V 1.6 Rev2 was released, with much clearer structured control flow rules. And the SPIRV-Tools validator has been updated to match.


### [should WGSL allow shadowing of builtins at module scope #2941](https://github.com/gpuweb/gpuweb/issues/2941)



* DN: Read Kelsey’s comment from last time, and seems reasonable to me (independent of others at Google).  Would be compatible with allowing shadowing at module scope.
* AB: Concern we’d have to specify all the namespaces for that idea.  Independently there are many at Google internally who like the idea of the idea of the issue:  allowing user-declared module-scope declarations to shadow predeclared language things.
* MM: Thought we were going to talk about language evolution. Happy to accept allowing shadowing.
* **_Resolve: Allow user-declarations at module scope to shadow predeclared things._**
* KG: Think that shadowing thing is forward compatible with namespaces.
* DN: Where do we stop: reserved words, types, var, etc.? 
* JB: Scheme is super flexible. But draw a line in the sand.
* AB: Think we can stop reserving types.
* MM: Types and functions are reasonable to allow, because users can make their own. But they can’t make their own var, while, etc.
* JB: We already have problems at parse time because we need to forward-resolve function names and types.
* DN: **So I’ll post PRs 1. Allow the module-scope shadowing, and 2. Pares down the reserved word list, removing items that look like types and functions.**   Keep only things that look like what would be a keyword (control flow, decl specifier, etc.) in WGSL.


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, July 12, **11am-noon** (America/Los_Angeles)
* [WGSL pointer aliasing rules · Issue #1457](https://github.com/gpuweb/gpuweb/issues/1457) 
* 