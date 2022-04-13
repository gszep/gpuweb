# WGSL 2022-04-12 Minutes

**🪑 Chair:** Kelsey Gilbert

**⌨️ Scribes:** DN

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** Tuesday 11am-noon Americas/Los_Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSN TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

**Previous:** [2022-04-05 - WGSL - Agenda / Minutes (APAC)](https://docs.google.com/document/d/11YDLK4yzj6opeR5U1WlmsjcHlznbmmuguyT5IXZ46YY) 

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send dneto a Google Apps enabled address and he'll add you.


---


# 📋 Attendance

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



* Apple
    * **Myles C. Maxfield**
    * **Robin Morisset**
    * Daniel Glastonbury
* Google
    * **Alan Baker**
    * Antonio Maiorano
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **Dan Sinclair**
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
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
    * Rafael Cintron
    * Tex Riddell
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


### Daylight savings shifts are complete for now!



* **We use Americas/Los_Angeles, and the Google Calendar invite should show correctly for you.**
* Just in case: [https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220412T11&p1=137](https://www.timeanddate.com/worldclock/fixedtime.html?msg=WGSL+Meeting&iso=20220412T11&p1=137) 


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [kelsey.gilbert@mozilla.com](mailto:kelsey.gilbert@mozilla.com) (example)
        * 


---


# ⏳ Timeboxes


### [FP16 Extension against spec #2696](https://github.com/gpuweb/gpuweb/pull/2696)



* Offline:
    * David and Alan reviewed and approve
* Alan: API signed off already. Good to land.
* _Merge_


### [Use concise forms for entry point stage attributes. #2652](https://github.com/gpuweb/gpuweb/pull/2652)



* Alan: Looks good.
* _Merge_


### [WGSL: Add dot4U8Packed and dot4I8Packed as new built-in functions #2738](https://github.com/gpuweb/gpuweb/pull/2738)



* Alan: Agreed post-v1 on this.  So we haven’t reviewed.  Things to be worked out: do we want to force all implementations to emulate.  And whether to saturate.  Suggest set this aside for now
* _Reconsider post-mvp_
* MM: Goes into the extensions directory?
* DN: It’s already in mergeable form, can leave it in PR state.  


### [Make matrix notation more intuitive #2745](https://github.com/gpuweb/gpuweb/pull/2745)



* KG: Changing notation matCxR.
* DN: Yes, just editorial. And the edit found typos where we had swapped the M and N originally.
* _Merge when editor satisfied typos fixed._


### [WGSL: Explain parameterized type rules more explicitly. #2682](https://github.com/gpuweb/gpuweb/pull/2682)



* JB: Needs updates due to review.  Fine to make those changes. 
* 


### [[wgsl] Minor formatting and language changes. #2726](https://github.com/gpuweb/gpuweb/pull/2726)



* DN: This is editorial.
* DS: Had one fix to apply from David.  Applied it.
* _Merge when conflicts done._


### [Change integer texture parameters to unsigned #2722](https://github.com/gpuweb/gpuweb/pull/2722)



* KG: textureDimensions yield unsigned.
* AB: Didn’t go as far as we had proposed at the F2F.  This is the half-stage: if the value can’t be negative then change it to unsigned.   Should be a reasonable first step.
* MM: Think we should go farther. Using int’s is idiomatic in other shading languages.  And would be unfortunate to require casts.
* DN: Are we going to add the signed forms again?
* _Yes_
* DN: So let’s not remove them now.
* AB: Ok, so add the variants.  The whole cross product of cases.
* KG: consensus to have both overloads.
* KG: Suggest have all-signed forms, and all-unsigned forms. Rather than full cross product. Wait for feedback if people miss it.
* AB: Ok, will update the PR that way.


---


# ⚖️ Discussions


### [Add creation-time expressions #2668](https://github.com/gpuweb/gpuweb/pull/2668)



* Previously:
    * KG: I promise to review this week.
* KG**: **Think we should land this. Progress
* _AB to resolve conflicts, then David to land._


### [WGSL Internationalization Adjustment for Code Points #2734](https://github.com/gpuweb/gpuweb/pull/2734)



* MOD: Changes “character” to “code point” and explicitly lists the code point numbers.
* MOD: Feedback from Myles: use “ignorable” set.  But that conflicts with XID sets and is not guaranteed stable.
* MOD: Based the change on “pattern whitespace” set.  Think it should be mergeable.
* KG: Is there an official category for that?
* MOD: Yes, can mention it in the spec. But in this current one enumerated each one.  Can mention the 
* KG: I like having the category there as an intuition and indication of justification.
* MM: May have feedback. Doesn’t mention other bidi characters.  Only lists one quarter of the bidi characters.
* MOD: Similarly, there are characters here that are not in pattern_whitespace. Unicode had some considerations.
* KG: So decision is do we trust other people’s categories.
* MM: So if you did have a bidi code point there, would it be part of an id?
* MOD: no, because not part of XID.     Would yield invalid program.
* MM: So part of the bidi characters are blank space, and others are not (only usable in comments) because Unicode guides us this way.
* MOD: Right.
* MM: Ok, agree with KG generally that that spec should reference to character categories where possible.  Generally, stay away from listing individual characters.
* KG: Ok, so with addition of naming the referenced pattern category, we can land this.
* 


### [WGSL parsing assumes context-aware tokenization #2717](https://github.com/gpuweb/gpuweb/issues/2717)



* Previously:
    * Leave this for a week. Call to action: review your recursive descent parser to see 	
* DN: Landed the grammar refactoring separately, as [https://github.com/gpuweb/gpuweb/pull/2737](https://github.com/gpuweb/gpuweb/pull/2737) 
* DN: Still working on programmatically extracting the lookahaeds from the grammar. 
* MM:  Our concerns last week was number of tokens to lookahead to be bounded. 
* JB: Have not reviewed parser structure.
* _Check back next week._


---


# 📆 Next Meeting Agenda



* Next week: Tuesday, April 19, 11am-noon (America/Los_Angeles)
* [#2741](https://github.com/gpuweb/gpuweb/issues/2741) Pointers in uniformity analysis
* [https://github.com/gpuweb/gpuweb/issues/2632](https://github.com/gpuweb/gpuweb/issues/2632) Reserve fewer words.
    * DN: Already removed ‘box’ and words that are types in other languages
    * DN: Close this?