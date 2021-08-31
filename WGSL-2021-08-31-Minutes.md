# WGSL 2021-08-31 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

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
    * **Antonio Maiorano**
    * **Ben Clayton**
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * **Kai Ninomiya**
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


# 📢 Announcements


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


### APAC-timezone’d meeting



* WebGPU group decided to hold each first meeting of the month at an APAC-friendlier time
* WebGPU will be meeting at 5pm (America/Los_Angeles time) on such Mondays
    * Next Monday’s meeting is cancelled, so the following Monday Sep 13 will meet at 5pm instead of noon.
* Is 5pm next Tuesday (Sep 7) workable for WGSL’s next meeting? (Yes)


---


# ⏳ Timeboxes


### [ Fix description of mix builtin; fix fma (works on vectors too) #2069 ](https://github.com/gpuweb/gpuweb/pull/2069)



* DN: Sarah found this, where we don’t treat the float argument here like elsewhere(?) or for underlying APIs(?). Trying in my addendum here to save the scalar overload for `mix`
* DM: We want the scalar case here but not elsewhere?
* DN: Yes
* DM: We probably don’t need to discuss this, if it’s editorial
* AB: This might be typos from when I refactored some tables
* (merged)


### [ Fix atan2() params #2058 ](https://github.com/gpuweb/gpuweb/pull/2058) 



* _merged_


---


# ⚖️ Discussions


### [Use brackets for array types #854](https://github.com/gpuweb/gpuweb/issues/854)



* RM: Last week suggested a solution at grammar level to forbid multiple dimensional brackets to keep things simple here.
* JG: I like `[]` as sugar, just another way to spell array&lt;>, and that this limit nicely constrains the complexity here. Most users should be happy with it.
* BC: Is there any precedent for writing the same thing in two ways.  No precedent yet.
* MM: also talking about things like vec4&lt;f32> having a short alias name.
* AB: There are type aliases the user can write.
* BC: This is unsettling. For a young language, multiple ways to declare the same thing.
* DM: Closest thing we have is the ‘for’ syntax, another way to write a loop.
* MM: Related  If they are [ ]  available, can they be used for a runtime-sized array.  Think we should make that work too.
* AB: That’s how GLSL does it.
* JG: Would you write   array&lt;foo,2>[3]
* MM: So can you mix the notations.  Parallel question.
    * array&lt;i32[3],3>
    * Goal is familiarity, and that’s not familiar in combination. So no opinion offered here.
* JG: As long as no square
* DN: Amplifying what BC said, where we’re adding a separate way to say the same thing in order to add familiarity for a slice/lineage of languages. Taking only some parts of C’s array but not all, so that weakens the familiarity argument. Other new languages (Zig, Go) put the element-counting before the element type.
* DN: Non-orthogonality is concerning. Too early in evolution of the language.
* KN: WGSL isn’t close enough to C / Java to justify taking this syntax.
* BC: No accommodation for stride. Will have stride forces you to use the generic syntax. So will have code for both. Then newcomers will have questions.
* MM: Why did we not think we could add a stride here?
* AB: We didn’t come up with a way to add it, and it’s hard for multi-dim
* AB: Also Google is discussing internally maybe making this not an attribute anymore, such as inside the template list
* MM: I was afraid the approach would be to say “use a bigger type to stride things”
* DN: GLSL has e.g. std140 to implicitly enforce padding
* MM: This isn’t a dealbreaker for anyone, sounds like. What would move the needle for anyone? The thing that would make me change my mind would be that squarebracket syntax would end up being so uncommon that no one uses them/knows them. That would make it feel like a bad idea to me.
* BC: I want one way to declare an array.
* KN: +1, if we replaced the current syntax entirely, that would be fine too. Not wanting to do that right now, but could be happy either way.
* MM: Is it worth coming up a wholesale replacement? Would that be a waste?
* DN: I like it now, so “no progress” is forward progress.
* AB: If you have a full proposal, maybe having that would be useful, and we could progress it outside the meeting.
* BC: We also have a merge conflict with the new stride compatiblity stuff, so we should sort that out sooner rather than later. Not waiting on a meeting, waiting on DN.
* JG: This sugaring proposal might be a great thing for one implementation to prototype and then solicit concrete feedback from devs, and see if they like it, or not, or need it.
* DM: This would make an exception in parsing. This would be the only place where, if that place expects a type, you have to continue lookahead to see if there’s an array-ing suffix to modify the type you just parsed. 
* MM: Don’t think that’s a strong argument.
* JB: Lots of places where lookahead by one token has not been a problem. 
* DM: Yeah I guess we have one other place where we do this, so it only makes the compiler slightly more complicated.
* Resolved: Needs action (prototyping and more feedback)


### [[wgsl] Support compile-time-constants (constexpr) · Issue #1272](https://github.com/gpuweb/gpuweb/issues/1272) 



* DN: Some new discussion internally. I think it was notable that the primary use cases were all integers, which makes thing easier because we don’t need to worry about float issues.
* DN: Some users wanted to be able to get the length of a fixed array, rather than repeating the length for the array and for the iteration over it.
* DN: We do have a dearth of CTS tests, and if I were to choose either CTS or working on this feature, I would like to work on CTS first before working on this new feature.
* MM: +1 to array length builtin
* DM: Where this would fail would be if you wanted multiple arrays of the same type.
* MM: I think it’s still useful and we should do that part.
* DN: I think I can propose that.
* MM: For this I think we want to support let or var, where implementations don’t do an actual function call.
* DN: We can make a carveout for this, to make it like sizeof, but I don’t know if we can forbid execution of the expression called to it.
* MM: E.g. ArrayLength(foo(5)), would call foo(5)
* DM: Also for static use, if I use ArrayLength(foo), I expect foo to be statically used
* DN: “static use” is one thing, and actually causing the accesses to occur (by requiring the evaluation of the argument)
* BC: ArrayLength has to be very special (even as a builtin) for dynamic sized arrays
* Resolved: Revisit post-mvp
* DN: I will also say that I want CTS first, but then constexpr rather than a workgroup size proposal that I don’t like. But picking two of those three, I would pick CTS+constexpr.
* BC: Constexpr can be done in stages.  The builtins will cause most of the work here. We could start with integer arithmetic and binary and unary operators on those.  Agree with David in that not necessarily needed right now.
* JG: Do we need constexpr OR workgroup-size for mvp? If so, we should keep working on this.
* DN: Internal users have said that our lack of support for good workgroup size (or constexpr) is really annoying, though tolerable for now. Would really want this for v1, but probably not needed for mvp.
* DM: What would you like to go back to for solving this? Overrideable constants?
* DN: Yeah, but keeping in mind what Apple wants wrt pipeline-overrideables
* MM: Could we talk again next week?
* DN: Probably not.
* MM: Ok, will ask the same question next week.
* DN: For next week, would rather present something on attributes-on-types.


### On Todos



* GR: We should probably open up the spec and check the todos.
* MM: Should we do this in-meeting?
* GR: Maybe? But we should touch on all of them.
* DN: We do have a bunch, and a bunch can probably go away. Would rather have rounds of PRs to prune bunches of them off at once.
* JG: I can maybe make a spreadsheet for thumbs-up/down/maybe for each todo
* DN: I like that that can be async


### On MVP vs Origin Trials



* DM: MM noted that at the webgpu meeting, KR stated (in summarizing a perceived consensus) that origin-trial *is* mvp. In the editors meeting, we found that many “post-mvp” issues were really “post-v1” and so we renamed the categories. If the WGSL group feels differently, we should probably revisit this.
* JG: I do think they are different, but that we definitely have a bunch of “revisit post-mvp” for which there may actually be an appetite for making them “post-v1”, but that there are ones that aren’t, so I think these are not the same set.
* DM: In my mind mvp is multiple different implementations implementing the same thing.
* MM: So what is the result here?
* DM: Doesn’t matter what we call it, but we want to get a bunch of feedback for something-like-mvp, in order to polish up something for v1
* JG: I’ll add more discussion on this to the WebGPU agenda. For now, continue to do your best to mark things, and if we need to fix anything up, we’ll do so.


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-09-07 (**APAC time! 5pm** instead of 11am!)
* 