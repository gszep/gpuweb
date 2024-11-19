<!-- Copy and paste the converted output. -->

<!-----



Conversion time: 5.076 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β40
* Tue Nov 19 2024 11:59:15 GMT-0800 (PST)
* Source doc: 2024-11-19 WGSL - Agenda / Minutes

WARNING:
You have 5 H1 headings. You may want to use the "H1 -> H2" option to demote all headings by one level.

----->


<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 1; ALERTS: 0.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p>
<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>



# WGSL 2024-11-19 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [Untriaged](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial++-label%3Acopyediting+no%3Amilestone+), [M0](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+0%22+), [M1](https://github.com/gpuweb/gpuweb/issues?q=label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+-label%3Acopyediting+is%3Aopen+milestone%3A%22Milestone+1%22+)

**Todos doc:** [WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previously:** 



* [2024-09-17 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1ov0Ti3nzclj_gYJaLtUqIffwgGoQPq2CYckSRe5MhTc/edit)
* [Agenda / Minutes for GPU Web WG F2F meeting 2024-10-29/30 Mountain View F2F](https://docs.google.com/document/d/1FlVeiqRzx5t-9z03Ocx7_gw-lPpysUaw_83xofJyxQQ) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](http://meet.google.com) invitation and plan on participating, please send **dneto** a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
    * Myles C. Maxfield
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * **James Price**
    * Kai Ninomiya
    * Natalie Chouinard
    * Rahul Garg
    * **Ryan Harrison**
    * Stephen White
    * **Peter McNeely**
* Intel
    * Hao Li
    * Jia A Chen
    * Jiajia Qin
    * Jiawei Shao
    * Narifumi Iwamoto
    * Shaobo Yan
    * Yang Gu
    * Yunchao He
    * Zhaoming Jiang
* Kings Distributed Systems
    * Daniel Desjardins
    * Hamada Gasmallah
    * Wes Garland
* Microsoft
    * Damyan Pepper
    * Greg Roth
    * Michael Dougherty
    * **Rafael Cintron**
    * Tex Riddell
* Mozilla
    * Ashley Hale
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * **Teodor Tanasoaia**
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
* Dominic Cerisano
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* **Mehmet Oguz Derin**
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb


---


# 📢 Announcements/Meta


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


### FYIs and Notable Offline Merges



* **Milestone 0 is empty!**


---


# ⏳ Timeboxes (until XX:20)


### [bool has 4 byte size and alignment #4972](https://github.com/gpuweb/gpuweb/issues/4972)



* PR: [wgsl: bool has size and alignment of 4 bytes #4974](https://github.com/gpuweb/gpuweb/pull/4974)
* DN: For various reasons, we were revisiting this, and we found some differences. We would have to fake a bunch of stuff if you tried to fake [a one byte bool]. 
* JB: A question: Spec makes it clear that if you have a struct of two bools, that different invocations can access different fields without causing issues w data races. So that’s settled by the spec. Beyond that, it seems that the shape of a bool isn’t actually observable (DN: right) so this is kind of a no’op for the spec, right? Seems like this would be fixed by just a note that says this.
* JB: So it’s not observable to users, and kind of just as guidance to implementers.
* MW: If this doesn't really have impact on devs, why is this in the spec? Agree with JB that this can just be a note. If we allow for it in the future, we can add it then.
* DN: The place where this could show up is in a non-uniform/-storage struct, users can ask for @align(1) bool today, and this would forbid this.
* DN: If we add a 1-byte, we should call it byte or i8 anyway. Metal doesn’t have a packed bool either.
* AB: Our limits talk about bools being 4 bytes, so this 
* MW: I guess why even have bool then?
* DN: [reasons]. Distinguish type-checking cases: if condition is bool. Don’t automatically allow u32 in those places. Also don’t mandate a representation of 1 and 0 for true and false. (In vector cases true is the all-1-bits pattern for the component.)
* KG: So should we just take this?
* JB: I think we should take it, but just add a note that mentions that many GPUs can’t implement single-byte writes without introducing data races, so making bool 4 bytes is required to match our spec requirements.
* DN: Ok
* **Resolved: Agreed with JB’s note.**


### [Does WGSL intentionally permit duplicate global diagnostic filters? · Issue #4976](https://github.com/gpuweb/gpuweb/issues/4976)



* Milestone?
* JB : Happy to close this, since my q was answered. No action necessary.
* **Resolved: Closed**


### [Texel buffer proposal #4912](https://github.com/gpuweb/gpuweb/pull/4912)



* Milestone?
* KG: M1?
* DN: Maybe yes? Likely to implement 2025q1.
* MW,JB,RC: +1
* KG: It’s a proposal, should we land it?
* JB: +1
* **Resolved: Land as proposal**


### [WIP: Add subgroups feature #4963](https://github.com/gpuweb/gpuweb/pull/4963)



* (FYI)
* DN: AB’s been speccing this, dropped the subgroup_branching diagnostic.
* AB: We were looking at this diagnostic, and looking at the subtest. Any time you wanted to [very common patterns] you had to turn this off. Many shaders immediately had problems when this was enabled, so we think almost everyone would turn this off. So [looking at?] portability instead of this diagnostic.
* **(no resolution today)**


---


# ⚖️ Discussions


### [wgsl: @align(n) must divide required-align-of, for all structs #4978](https://github.com/gpuweb/gpuweb/pull/4978)



* Milestone?
* DN: Had reason to look at this again. Put this rule into tint, and compared to naga/webkit, and tint was doing something different: Only when instantiating in a variable, gets corner cases for looking at the first field of a struct. Confusing, we decided we wanted to just make the spec match what naga/webkit does. Looking at the history here, we made things complicated in order to leave options open for future, but now think that we should just change things if we need to in the future.
* JB: So concretely, if you ask for a not strict enough alignment, it becomes an error?
* DN: yes
* JB,MW: +1
* **Resolved: Accepted**.


### [Plainly state alignment requirement for scalars and vectors #4965](https://github.com/gpuweb/gpuweb/pull/4965)



* Milestone 1?
* DN: If we land #4978 then this is redundant.
* DN: Lets talk 4978 first.
* KG: Now with 4978 we can drop this?
* DN: Yes
* **Resolved: closed**


### [enhancement: uniform buffer can use standard layout · Issue #4973](https://github.com/gpuweb/gpuweb/issues/4973)



* DN: People complain that std140 is annoying, so this would drop that requirement. This tends to be the first thing people want for flexibility.
* DN: Further in the future, VK has “relaxed block layout”, and then further has “scalar” layout.
* JB: So at present, you have to specify the layouts, and currently we just validate strictly, and this allows us to relax this without hurting content.
* DN: Yes.
* DN: This was M1 to discuss it, but should be M2 or later for implementation. Just wanted to discuss early.
* **KG moved to M2**


### [smoothstep: please remove error check when edge0 > edge1 for compile-time constants · Issue #4900](https://github.com/gpuweb/gpuweb/issues/4900)



* KG: The rough shape of the request here is a domain expansion to make this symmetric for negatives. This op is only for floats, so no int concerns here. 
* DN: So judgment call: Do you pass it down to driver? Or do you polyfill it yourself? Are you afraid of new drivers?
* JB: I’m not really afraid. It seems to be longstanding as-is, and it doesn’t seem like there’s much room for a driver to try to take advantage of the restrictions today.
* ds: FXC does do a weird thing here, and return 0 for high &lt; low, even if 0 is out of range.
* JB: We feel like we should be moving away from FXC anyway.
* KG: So it’d be up to the implementers how to do this, but we could choose to either use OpSmoothStep if it works, or polyfill our version instead, based on the driver?
* DN: We are worried about the millions of devices, and maybe picking the wrong choice if e.g. OpSmoothStep is picky on one of them.
* KG: The rumor that there aren’t any dedicated ISA instructions for this makes us think that we can just generate the math ops ourselves, instead of e.g. OpSmoothStep. And that we can probably just do that universally, and maybe in the future someone will show up with a perf bug report and ask for for-reals OpSmoothStep, and then we’ll deal with that then. (but this sounds unlikely)
* DN: Well… If other people are happy enough to just always polyfill, I guess I’m ok with that.
* **Resolved: Expand the domain, both for runtime and for constant. (keeping const-time compile error for domain errors, e.g. low==high)**


### [Proposal: fully explicit, non-exclusive "auto" layouts in WGSL (without createPipelineLayout) · Issue #4957](https://github.com/gpuweb/gpuweb/issues/4957)



* KG: We didn’t have much time to go over this, so we need more time to give it more thought.
* JB: I do like the direction though, especially for explaining to new people.
* JB: It’s this thing that you’re always imagining in your head, but now you would get tooling support.
* AB: It seems like JSP is saying they don’t want to make this about static-use, like the proposal proposes.
* JB: I wonder if we could encourage the WESL people to experiment with this?
* KG: We can probably just @them on the issue? (or if you’re reading this: hi!)
* DN: So what’s the impetus to this?
* KG: It’s nice when you e.g. have the same vertex binding layouts between two shaders.
* ds: It sounds like JSP is asking for declaration in the @interface to count as static-use for the purposes of auto-layout. 
* **(no resolution today)**


### [Proposal: "auto" layout algorithm should be able to generate any layout · Issue #4956](https://github.com/gpuweb/gpuweb/issues/4956)



* KG: We didn’t have much time to go over this, so we need more time to give it more thought.
* ds: Some things we need, like unfilterable-floats, so if we give a way to say so, we wouldn’t have to guess in our implementation.
* **(no resolution today)**


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday December 03, 2024, **11a-noon** (America/Los_Angeles)
    * Can we talk about:
    * 
* But email Kelsey or the list if you want to meet sooner!
* 