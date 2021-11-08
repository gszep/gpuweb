# WGSL 2021-11-02 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribes:** JG and DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday ****5-6pm Americas/Los Angeles**** (APAC-friendly)

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
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
    * James Darpinian
    * James Price
    * Rahul Garg
    * Ryan Harrison
    * Sarah Mashayekhi
* Intel
    * **Jiajia Qin**
    * **Jiawei Shao**
    * **Shaobo Yan**
    * **Yang Gu**
    * **Zhaoming Jiang**
    * **Yunchao He**
    * Narifumi Iwamoto
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Dzmitry Malyshau
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


### Todo triage



* Document: [https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0) 


---


# ⏳ Timeboxes


### [Allow f suffix for float literals #2210](https://github.com/gpuweb/gpuweb/issues/2210)



* AB: This is included in the “literal unification” PR #2227.


### [Consider replacing elseif with else if #2212](https://github.com/gpuweb/gpuweb/issues/2212)



* RM: PR if we go that way: [https://github.com/gpuweb/gpuweb/pull/2239](https://github.com/gpuweb/gpuweb/pull/2239)
* AB: It’s in conflict with removing braces (which was proposed elsewhere).  Ok with this if we don’t remove the requirement for braces.
* GR: Does requiring braces make this harder?
* RM: It makes it easier to require, rather than if it were optional.
* MM: Do we have to pick just one of “required braces” or “else if”?
* JG: Yes, we pick one of the two. If you make both changes, you have the dangling else problem.
* KN: another solution is to only allow removing braces for some specific statements, not including if statements


### [Clarify the rule about discard in a continuing block #2219](https://github.com/gpuweb/gpuweb/issues/2219)



* DN: I think we got consensus offline.
* MM: If we do modules, does this need an extra annotation?
* RM: We need to know this for behavioral analysis anyway
* DN: And `discard` only valid in frag shaders, so we definitely need to know this anyway
* AB: Is RM fixing his PR?
* RM: I was waiting for decision, but I can either have this in the main PR or make a new PR just for this
* **RM to resolve this**


### [Feedback after porting ~80 shaders from GLSL to WGSL #2204](https://github.com/gpuweb/gpuweb/discussions/2204)



* MM: Really helpful list, thank you to the authors. Didn’t see anything that was super urgent, but together this is like a death by a thousand papercuts, so our feeling is that we want to fix *some* of these for v1. (presumably the easier ones)
* GR: Something that surprised me was the order of params in the `select` command. FWIW HLSL just got a `select` that is different: select(cond, true_val, false_val)
* …
* DN: MSL has (false, true, cond)
* JG: I would imagine that one reason to want cond last is because we want the instinct to be that authors expect true and false expressions to be both evaluated before choosing which one via cond.
* MM: In order for any of these issues to be fixed, we need filed Issues for them, so we should file Issues for them.
* GR: The `select` issue might actually only be from the blog post. There are slight differences between this Issue and the blog post
* 
* RM: Would rather have ‘if else’ as an expression.  Since they have braces, it would not have ambiguity.
* **GR to file `select` issue DONE: [Select operator arguments are in a non-intuitive order · Issue #2242 · gpuweb/gpuweb (github.com)](https://github.com/gpuweb/gpuweb/issues/2242)**
* **JG to file others**


### [support dot(vecN&lt;i32>, vecN&lt;i32>) in WGSL #2231 ](https://github.com/gpuweb/gpuweb/issues/2231) 



* SY: We have use for this in big-int operations.
* AB: Are you expecting hardware accel, or is emulated ok.
* SY: We hope for hardware, but we’re ok with emulated.  Doing the operation in floating point can lose precision.
* MM: This sounds fine. To us, the barrier to adding functions to the standard library is very low. The main requirement is that if there is hardware support, we need to be sure that we can match the behavior versus the polyfill.
* AB: Spir-v doesn’t really have wide support, and where there is support it’s mostly for smaller ints. So long as we’re fine without hwaccel, I think it’s easy enough to add for convenience. 
* **Resolved yes: Needs spec (Intel to write PR)**


### [Behaviours analysis #2216](https://github.com/gpuweb/gpuweb/pull/2216)



* AB: Looking good. Happy with where this is going. 
* RM: Thanks, any outstanding issues?
* AB: Just take out the discard, and DN’s issues.
* RM: I applied those, so I’ll just do the discard part, thanks.
* DN: I think we can land this and if I find an issue I’ll file an issue.
* **Resolved fix up and give DN a day, and then land**


### [Add a memory model mapping to Vulkan #2055](https://github.com/gpuweb/gpuweb/pull/2055)



* AB: I appreciate RM’s review. Some things I want to address, some things to follow on, RM filed an issue, but I think no course-correction is needed.
* RM: Yes, just editorial fixes, and one non-blocking issue.
* **Resolved: land when ready**


### [Should WGSL expose the concept of coherent buffers? · Issue #1621](https://github.com/gpuweb/gpuweb/issues/1621)



* Related to #2055


---


# ⚖️ Discussions


### [The order of top-level declarations should be irrelevant #875](https://github.com/gpuweb/gpuweb/issues/875)



* DN: We looked offline, some recommendations:
    * 1. Move enable directives to always-at-top
    * 2. When you make a decl, where is it visible? Top-level-decl visible across entire module.
    * Must watch out for “silly cycles”, like declarations that depend on how they’re defined
* RM: Happy with this, and we already have checks like this for recursion, so we don’t see this as an issue.
* JB: Naga also already handles the acyclic check separately from the parser
* **Resolved as above, Needs Spec**


### [MSL does not support address-of vector component #1931](https://github.com/gpuweb/gpuweb/issues/1931) 



* See also [PR #2225: Disallow taking the address of a vector component](https://github.com/gpuweb/gpuweb/pull/2225)
* DN: There’s a PR based on what we decided. It’s not type-based, but it does the thing we need. I think we can land.
* MM: +1
* **Resolved: Merge**


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* AB: Wanted to raise: doing calculations in an idealized-number space.  We assume we’ll get to doing constexpr, should constexpr also operate in an ideal space.
* RM: Have comments about the PR, in particular the concept of calculation in the ideal space. It can’t necessarily be done in the shader itself. E.g. 64 bit values (integer and double precision). Makes it impossible to do some kind of refactoring; or rather it can silently get the answer wrong when you refactor.
    * let y = (... + ...) - ...;
    * Vs.
    * let x = ... + ...;
    * let y = x + ...;
    * These would have different answers because .e.g the first is arithmetic over doubles for literals.  The second collapses x into f32 first.  So you get precision differences.
* RM: To handle 1 + 1 + 1u, have a simpler different way based on Myles’ older proposal.  Need to write it down.
* AB: The PR I first wrote on this didn’t do it in an ideal space. It’s easy enough to phrase it either way.   From an internal discussion, we should keep in mind that constexpr has similar considerations and may want to do 
* JG: Doesn’t golang have something like float256 types for literals?
* DN: Yeah, though that’s the minimum. Feedback is really positive because people can just ignore bit sizes as “large enough”. Our proposal we stuck to 64-bit to sort of split the difference without requiring anything more complicated in the implementation.
* MM: If you have a bunch of literals added together, the default type of the literals are f32, but the ops prefer f64 somehow, because it’s actually dealing with different rules for literals. (Even if the dev hasn’t enabled any f64 extension)
* RM: I was expecting a different solution here.  Expecting a solution where you can get exactly the same answer by adding suffixes.
* AB: One example that was annoying was minimum integer: Can’t write the minimum int as a literal, which is hard to solve.
* RM: Agreed that unary minus is an allowable exception, but it doesn’t bother me quite as much for some reason. It doesn’t change the precision of fp.  Can have unary negation as applying to “ideal space” but not other operations.
* MM: We’re asking for ~a week to write up a counter-proposal, and then we can come back and discuss them side-by-side.
* **Resolved: Wait on counter-proposal.**


### [Parallels between WGSL and SPIR-V don't justify `let` · Issue #2207](https://github.com/gpuweb/gpuweb/issues/2207)



* JB: So I think the consensus is that we should do nothing.
* MM: +1
* GR: but was a pain point for the guy who ported lots of shaders
* JB: The title needs to be read very literally: There may be justifications for `let`, but parallels between WGSL and SPIR-V don’t.
* **Resolved: “Yes”, Closed**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-11-09 11am Los Angeles time
* [https://github.com/gpuweb/gpuweb/pull/2251](https://github.com/gpuweb/gpuweb/pull/2251) wgsl: Require [[location]] for [[interpolate]]
* [Are double brackets necessary for attributes? #2243](https://github.com/gpuweb/gpuweb/issues/2243) 
* [[proposal] Changes to overridable constants · Issue #2238](https://github.com/gpuweb/gpuweb/issues/2238)
* [https://github.com/gpuweb/gpuweb/issues/2245](https://github.com/gpuweb/gpuweb/issues/2245)  should WGSL handle unicode bidirectional overrides specially? 
* [https://github.com/gpuweb/gpuweb/issues/2261](https://github.com/gpuweb/gpuweb/issues/2261) WGSL should specify the execution order
* [https://github.com/gpuweb/gpuweb/pull/2263](https://github.com/gpuweb/gpuweb/pull/2263) Go from attribute_list * to attribute_list ? in the grammar
* [https://github.com/gpuweb/gpuweb/pull/2265](https://github.com/gpuweb/gpuweb/pull/2265) Restrict the lhs of assignments to fix a grammar conflict
* [https://github.com/gpuweb/gpuweb/pull/2266](https://github.com/gpuweb/gpuweb/pull/2266) Remove obsolete attribute_list in variable_ident_decl
* [https://github.com/gpuweb/gpuweb/pull/2241](https://github.com/gpuweb/gpuweb/pull/2241) Add complex assignments (+=, -=, etc..)
* [https://github.com/gpuweb/gpuweb/pull/2239](https://github.com/gpuweb/gpuweb/pull/2239) Replace the "elseif" syntax by "else if"
    * (already discussed, maybe should wait until a discussion of the brace-removal question)
* [https://github.com/gpuweb/gpuweb/pull/2267](https://github.com/gpuweb/gpuweb/pull/2267) Update operator precedence rules
    * The content of the changes was already decided by the group one year ago, this is just updating the spec to catch up to our past decision
* 