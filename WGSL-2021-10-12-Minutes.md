# WGSL 2021-10-12 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Myles

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
    * **Rafael Cintron**
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


### Meeting time availability recheck



* **Please fill out this survey**: [https://doodle.com/poll/z2wngnzttmnecpci](https://doodle.com/poll/z2wngnzttmnecpci)
* It is for both WebGPU and WGSL
* 


### On Todos



* (JG: I will make a spreadsheet for thumbs-up/down/maybe for each todo)
* 


---


# ⏳ Timeboxes


### [Allow syntactic navigation and styling #2143](https://github.com/gpuweb/gpuweb/pull/2143)



* DN: I think it also includes cleanups, which is a good thing.
* DN: This is converting the grammar to Bikeshed. It’s great for reading the HTML in a browser. It’s not great for editing the grammar. But that has slowed down, and if that’s a problem, we can use scripts.
* DN: We might want a script to convert this new thing back to the old thing, and then forward to the new thing again. If it’s necessary.
* MOD: Syntax has been stable. It was well-authored at the beginning.
* MOD: After this PR lands, if you want to experiment with styling of the rules, we can.
* JG: This is ready to land.
* MOD: We can land. There’s nothing left to do.
* DN: I’ll land it now
* MOD: Thanks.
* **Merged**


### [Implicit flat interpolation on integers is confusing #2061](https://github.com/gpuweb/gpuweb/issues/2061) 



* JG: We had more discussion historically? Where was that?
* DN: Google is against this. Interpolation on integers isn’t going to happen. Authors shouldn't have to write extra text.
* DM: It’s still an issue in the same way it was filed. I’m curious about the Google position. In some ways, we follow the principle that if something is redundant, we don’t specify it (like handles). But other places, we prefer explicitness. In this case, we could argue that explicity has values. It’s action at a distance.
* DM: I’d prefer to have it explicit in V1, and consider making it more ergonomic later. But not a strong requirement.
* RM: What do the other languages do? Do they assume that int always has the same interpolation type?
* JG: My guess is it’s undefined.
* DM: GLSL requires you to spell out “flat”. SPIR-V also requires it. The other two (HLSL, MSL) default to flat. They don’t warn you.
* DN: It’s a tiebreak argument that we can relax this in the future.
* DN: Our view is that flipping the type from float to integer is not going to be an important enough use case. That’s a judgement call.
* DM: This issue is a judgement call.
* MM: We don’t have strong opinions. We understand both sides.
* GR: This is not something that we anticipate becoming an issue in the foreseeable future.
* JG: Would it be valuable for us to force documentation of people’s code, such that when you read something, something being flat is flat iff it says flat.
* JG: I don’t expect ints to be filterable in the future, but even still, I would love to be able to enforce this, so i can tell if it’s flat or not.
* MM: We’re talking about defaults. You can specify flat if you want
* GR: A non-aesthetic argument is if someone is using integers, and wants to get perspective behavior, that would be bad. If we require them to specify that it’s flat, we make sure there is no confusion. They won’t get mad at us.
* JG: If we allow implicit flat, Firefox will generate a warning probably. We can say we care about it without making it an error.
* MM: Aesthetically, I prefer defaults
* JG: These things can be typedef’ed.
* MM: But the declaration is right next to the type, though, so it’s obvious
* JG: You could say something like `Number` which isn’t clear if it’s integral or not
* DN: Tiebreaking says we should make it required.
* DN: Right now, the spec says “it must not be specified”. Another option is make it optional and have defaults, and a third option is “make it required to be spelled out”
* JG: I would prefer if it was optional. THe problem with that is the user has no indication that the default is flat. Unless they write “flat” and they’ll get a warning that it was already flat
* DN: What the spec says right now is pushing back on the person who does the right thing. Right now, the author *can’t* say flat on integral things
* JG: DM, how do you feel about just emitting a warning?
* DM: I’m okay with it but it’s not great. I think flat interpolation is a feature that you opt into. People need to know what they’re doing.
* JG: Pedagogically, the path is, you start with floats, and they are interpolated, then you jump to ints, and then …. When I was a “wee lad” I thought ints were okay to use in varyings. So i’d like to have it to nudge people in the right direction, and also to help authors. It’s less redundant to put it on int, but it’s 5 characters is okay. Defaults add cognitive overhead.
* MM: My opinions on this are weak. I cede.
* JG: RESOLVED: Let’s make `flat` required on all interpolated integral types.


### [wgsl: 0x123p0 is a hex float #2170](https://github.com/gpuweb/gpuweb/pull/2170)



* JG: This is ready. Can we merge this? MOD had one comment.
* MOD: The merged style chart includes the fixed version of this. It was a bug and it caught something about Tree Sitter. I asked the upstream devs if they knew about it, and they said they would look at it
* **Up to DN**


### [wgsl: more restrictions on control flow interruption #2096](https://github.com/gpuweb/gpuweb/pull/2096)



* (Alan gave feedback; needs to be addressed by David)
* DN: I have to revisit the text. Alan found something a few weeks ago.
* RM: **I will try to extract the “Behaviors” analysis from the uniformity PR** because it’s saying the exact same things. I keep seeing more in this PR, they are things which have already been solved by that. So let’s avoid repeating the work.
* MM: Let’s block this PR on Robin’s work.


### [wgsl: describe sequenced execution #2097](https://github.com/gpuweb/gpuweb/pull/2097)



* (dependent on 2096)


### [Add round_up_size attribute #1909](https://github.com/gpuweb/gpuweb/pull/1909)



* (Google needs to talk amongst ourselves)
* DN: I forgot this existed. Internal discussions haven’t included this. We’ll need more internal discussion time.
* DEFERRED UNTIL NEXT WEEK


---


# ⚖️ Discussions


### [MSL does not support address-of vector component #1931](https://github.com/gpuweb/gpuweb/issues/1931) 



* MM: MSL doesn’t support this, but SPIR-V does. Do we forbid it for ourselves, or do we allow it and emulate on MSL?
* DN: We may want to make vectors more complicated later, so might not want to sign up to handle those extra complications in the future.
* MM: With this restriction, we could convert WGSL pointers directly into MSL pointers.
* MM: Feedback:  
* 1. “There’s a reason that MSL doesn’t support these things, because LLVM doesn’t support (or will later be unsupported).” SPIR-V is based on LLVM, so how does it work there?
        * https://llvm.org/docs/GetElementPtr.html#can-gep-index-into-vector-elements
        * `Can GEP index into vector elements?`
        * `This hasn't always been forcefully disallowed, though it's not recommended. It leads to awkward special cases in the optimizers, and fundamental inconsistency in the IR. In the future, it will probably be outright disallowed.`
    * DN: They have commonalities, but they aren’t the same. Each side has their own changes, experts, and goals/targets. I think that the issue you’re raising is sort of the least of their concerns vis a vis gpu compiling. 
    * GR: Are there any spir-v compilers that generate this?
    * DN: Yes
* 2. “If we want to represent the pointer to a part of the vector in a different way than a pointer to a scalar, we would need to have 2^N overloads for e.g. builtins
    * DN: That’s forbidden today in WGSL due to SPIR-V restrictions for passing pointers to functions.
* 3. “If we want to represent vectors as arrays instead, it would probably have performance overhead, which would be unfortunate”
* RM: If we forbid this for now, we could add it later. I also wonder how useful this is, since MSL devs don’t seem to use it.
* DN: Sort of selection/survivorship bias, where people aren’t as likely to complain about something that’s just missing/forbidden in MSL.
* MM: Seems pretty restrictive
* **Let’s think about it more**


### [wgsl: replace stride attribute with array type parameter #2103](https://github.com/gpuweb/gpuweb/pull/2103)



* DN:  Part of overall attempt to remove attributes from types.  Make the stride part of the array template type. But then spell fixed-size and runtime-sized array differently.  Looking for high level approval of whether this is a good direction. Can bikeshed details later.
* **MM to ensure existence of an issue for not having stride**
* DM: I think we should probably not move stride around, if we might remove it, and so we might want to block on it.
* MM: I think that’s perfect-as-enemy-of-good, and we should just take this and make other changes later.
* DM: I still don’t like the `array&lt;i32,5,4>` number soup syntax
* DN: We could do the python-like thing for a named argument for stride.
* DN: Do we like having a named arg?
* MM: My knee-jerk reaction is 
* DM: I think naming with named args is great, but that it’s a larger grammar change that I’d like to avoid. Basically a new language feature
* DN: If I were to start from scratch within the param list, I would probably not split spelling a dynamic-vs-fixed `array`. Would that be better than what’s on the table right now?
* DM: Yes
* MM: If allowing a named stride, would be cool to have a named length too.
* JG: Aren’t there non-grammatical differences between fixed-vs-dynamic? I like spelling things different if they behave differently.
* DN: Ok yeah they do have differences, I see your point there.
* DM: Since this problem might go away if MM gets us to get rid of strides, which lets us ship with fewer changes. Shouldn’t we block on that?
* MM: **Yeah I can put up that issue so we can talk about that first.**


### [The order of top-level declarations should be irrelevant #875](https://github.com/gpuweb/gpuweb/issues/875)



* **Will talk offline and next week**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-10-17 (non-APAC 11am time)
* 