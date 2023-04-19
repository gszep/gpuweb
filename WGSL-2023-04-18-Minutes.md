# WGSL 2023-04-18 Minutes

**🪑 Chair:** KG

**⌨️🙏 Scribes:** DN, AB, KG

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday **11am-noon **Americas/Los_Angeles (**Atlantic-timed**)

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl), [Outstanding V1 Issues+PRs](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+milestone%3AV1.0), [Untriaged WGSL issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aopen+label%3Awgsl+-label%3A%22wgsl+resolved%22+-label%3Aeditorial+no%3Amilestone)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2023-04-11 WGSL - Agenda / Minutes](https://docs.google.com/document/d/1y4RWErleDCetzZwqGCZKWY0Ilu_rcdn8t_kYzS96Wtw) 

 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

 WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * Dan Glastonbury
    * **Mike Wyrzykowski **
    * **Myles C. Maxfield**
* Cocos
    * Huabin Ling
    * Zeqiang Li
    * Zhenglong Zhou
* Connecting Matrix
    * Muhammad Abeer
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **dan sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * **James Price**
    * Rahul Garg
    * **Ryan Harrison**
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
    * Erich Gubler
    * **Jim Blandy**
    * **Kelsey Gilbert**
    * Teodor Tanasoaia
* UC Santa Cruz
    * Reese Levine
    * Tyler Sorensen
* Unity
    * Brendan Duncan
* **Dominic Cerisano**
* Dzmitry Malyshau
* Eduardo H.P. Souza
* Jeremy Sachs
* Joshua Groves
* Lukasz Pasek
* Matijs Toonen
* Mehmet Oguz Derin
* Michael Shannon
* Pelle Johnsen
* Robin Morisset
* Timo de Kort
* Tyler Larson
* Jason Erb


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


# ⏳ Timeboxes (until XX:20)


### [Support for gl_PrimitiveID / SV_PrimitiveID -like construct? #1786](https://github.com/gpuweb/gpuweb/issues/1786)



* KG: Milestone 2.  Needs concrete PR.
* AB: We think it needs an enable.  It’s not a software feature.
* DN: In MSL it seemed tied to barycentric coordinates. Which is maybe narrower than what the original poster asked for.  Suggest needs investigation.
* KG: I think I’ll place this in “post-v1”, and someone can investigate/make a pr.
* 


### [Add component-wise conversion constructors? · Issue #4021](https://github.com/gpuweb/gpuweb/issues/4021)



* KG: Tolerable place to stop making things easier.
* JB: Beneficial to make authors write out the extra verbiage. I see Ben and Alan seeing it was annoying. What did you do to respond. Did you change a type, or choose another place to do the conversion, and did it improve your code.
* BC: The most common fix is to cast the arguments. It’s not pretty.  
* JB: In the examples, why aren’t x and y f32 in the first place.
* AB: They were invocation IDs, or for loop iterators. That kind of thing.
* BC: The thing that annoyed me is there was no nice solution that still looked clean. Just wanted the thing to behave the way I thought it would.
* MM: Texture sampling functions have a bunch of overloads.  What’s the rationale for picking one set to add, and the other not to.
* KG: The rationale is API design judgment call. How useful is this thing vs. the bugs you find.
* DN: These seem like int to float conversions, but the textures are signed vs. unsigned.
* BC: Difference is this case is all the arguments to the constructor should be consistent types.  Already baked in.  Whereas texture args are different kinds of things like level and offset.
* KG: Minimizing inconsistency is not the goal.  Goal is to provide the right value to authors in writing specific things.  Think this is a good place to stop in the slope of adding more implicit conversions. Think it’s tolerable.
* AB: I don’t view it as “implicit” because I’ve _named_ what I’m converting to.  Not a dangerous place where conversions silently happen.
* KG: when I’m scanning the code, and I see the line vec2f(x,y), when I see that line, it’s not clear from that line that there is going to be an implicit conversion there. You may know because you have intent and context, but at the actual call site, it’s not clear what the inner types are.  If you require conversion, then you see f32 casts, and you see there’s a reason / actual cast there. Or you see vec2f(vec2i(x,y)) in which case you also know it’s clear.
* AB: You can use vec2(x,y) as the constructor, but doesn’t give info.  Can buy if all components are individually spelled out. But vec2f(vec2(x,y)) isn’t clear.
* KG: That does show you that something actually is going on there. Reader asks: why is there something extra there.  The reason is, there is a conversion occurring there.  It’s a valuable signal.
* BC:  Seeing vec2f is a strong signal that you’re doing more than if you had just spelled vec2(x,y).  Think vec2(x,y) will be the normal thing to see.
* JB: T**hink this can be post-v1.  Let’s put it off.**


### [investigate ULP bounds on unpack2x16unorm · Issue #4038](https://github.com/gpuweb/gpuweb/issues/4038)



* DN:  **Needs investigation**.  Suspect need to weaken from “correctly rounded” but need more info.


---


# ⚖️ Discussions


### [Shader extensions are redundant · Issue #3961](https://github.com/gpuweb/gpuweb/issues/3961)



* MM: **David replied just before the meeting so need time to review**. Think it’s slightly different argument from Kelsey’s, so still **would like to see Kelsey’s writeup**.

Milestones:



* DN: Currently have v1.0, post-v1, Milestone 2, Polish post-v1.  Would like rationalization of those.
* KG: Will propose a triage.
* DN: Useful to have two future buckets: near and far.  To help us focus.

Dan: [https://google.github.io/tour-of-wgsl/](https://google.github.io/tour-of-wgsl/)  open for feedback, PTAL


---


# 📆 Next Meeting Agenda Requests



* Next meeting: (**Atlantic-timed**) Tuesday April 25, 2023, **11am-noon **Americas/Los_Angeles
* 

 
