# WGSL 2021-10-19 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Myles, Jeff

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


### AB: We have some UC Santa Cruz researcher who are working on memory-model testing, and would like to present to the group, if we want to replace our meeting with that, in about a month’s time.

MM: Yes please, sounds cool

MM: One of our internal teams have been using WGSL, and they have feedback. 

MM: Have an internal group working with the WGSL language. Gathering feedback and will file issues.

MM: I have started filling backlog of issues.  About ⅓ of the way through.


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
* Poll will close around the end of the week!


### Todo triage



* **New document: [https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0) **
* 


---


# ⏳ Timeboxes


### [wgsl: more restrictions on control flow interruption #2096](https://github.com/gpuweb/gpuweb/pull/2096)



* Previously:
    * RM: I will try to extract the “Behaviors” analysis from the uniformity PR because it’s saying the exact same things. I keep seeing more in this PR, they are things which have already been solved by that. So let’s avoid repeating the work.
    * MM: Let’s block this PR on Robin’s work.
* **RM to handle soon**


### ~~[Add round_up_size attribute #1909](https://github.com/gpuweb/gpuweb/pull/1909)~~



* ~~Previously:~~
    * ~~(Google needs to talk amongst ourselves)~~
* ~~Closed.~~


### [wgsl: Rename smoothStep to smoothstep #2175](https://github.com/gpuweb/gpuweb/pull/2175) 



* DM: Why the exception
* JG: Prior art.  Ease of use for existing programmers is not a non-goal.
* DM: E.g. inverseSqrt in WGSL, but rsqrt (etc.)  Would like the rule written down.
* MM: Endorse Dmitry’s desire for a written rule. Even if it has a bunch of if statements.
* JG: It’s qualitative design.
* MM: Ok, philosophy.
* JG: Ok, I’ll take that action.
* DM: Ok, draft a philosophy, and see how/whether it applies to all the names we currently have.


### [wgsl: Function call section accommodates built-in functions #2179](https://github.com/gpuweb/gpuweb/pull/2179)



* DN: Fixed the phrasing as desired.
* MM: Haven’t reviewed, but don’t block on us.
* AB: Approved.
* _Landed_


### [Storage variables shouldn't be required to be structs #2188](https://github.com/gpuweb/gpuweb/issues/2188)



* AB: should be tied to ‘block’ removal.
* MM: Think we can discuss at the same time, but think it can be treated as separate.
* AB: Bound up with making clear  how to differentiate descriptor-of-array vs. array-of-descriptors.  Already in a draft PR queued up for discussion.
* JG: ok, will defer.


### [wgsl: Add scalar overloads of any() and all() #2174](https://github.com/gpuweb/gpuweb/pull/2174)



* _Merged._


### [wgsl: OpXMod -> OpXRem for integers #2187](https://github.com/gpuweb/gpuweb/pull/2187)



* MM: Can we discuss in terms of semantics rather than SPIR-V opcodes.
* DM: Closest thing to [previous agreement in #1830](https://github.com/gpuweb/gpuweb/pull/1830/files#diff-709b5c343d59ae10cbabe69f9df1eee9e2dbb6e9d848632fa94357083d6e0a34R4004) 
* BC: Agree we should discuss direct semantics.
* **Resolved that we should include the algorithm instead of just the spir-v opcode**
* AB: I think one place this is getting stuck is how to deal with negatives, since spir-v and hlsl have restrictions on what can or can’t be negative. We have to decide how that works.
* MM: Should probably just polyfill
* DN: I think there was another issue that got held up on figuring out how “dynamic error” should be defined
* DN to split things into separate sections
* MM: Our position is that these functions should work with signed negative numbers.
* MM: Making behavior match between ints and floats also seems good
* JG: +1


---


# ⚖️ Discussions


### [The order of top-level declarations should be irrelevant #875](https://github.com/gpuweb/gpuweb/issues/875)



* DN: Nesting of scopes makes this more palatable. However, still tbd: How does `enable` directives work? Do they affect text that came before them
* MM:
    * 1.  ‘Enable’ can be in the middle and only applies after (what we have today)
    * 2. Require `enable` at the top
    * 3. ‘Enable’ can be anywhere but applies globally.
    * 4. `enable` direction to be out of band
* RM: One of goals was to allow stitching of shaders by concatenation, which pushes us towards not requiring enable-at-top and not out-of-band
* RM: If we really want to support stitching of shaders by concatenation, maybe we want a scope for `enable`s
* MM: Maybe use more squiggly braces
* BC: Maybe this makes it tricky, e.g. if `enable` lets you return something new, does the caller need to have it `enable`d also?
* AB: `f16` is a good example, where a new type is introduced. Maybe we could decouple the keyword and identifier namespaces?
* BC: I think at-top is good. It’s unlikely that stitching will work great
* MM: I think that’s reasonable, even if you have a big body of source files, you may still be able to say “these all need X,Y,Z”
* DN: I like the idea of having enabling first. Doing more than that feels like more pain than profit. Worried about having to carry the lexical position all the way through parsing/validation/etc. Could also run into an issue where two extensions e.g. collided on a name
* MM: Question for RM, how do you feel about parsing the source twice.
* RM: Not super happy, probably not a huge deal though. Seems like a big hack, prefer to avoid.
* DM: Can we postpone this feature?
* DM: Changing the parser to 2 pass is a big change for us. (We do name resolution during parsing.)
* …
* DM: Weren’t you going to propose something like module linking to satisfy this MM?
* MM: Why not both? I think we do want both, but also not for mvp.
* MM: Also, using this from Metal requires access to the filesystem today. 
* DM: Maybe this is something that can be fixed before we get to it? :)
* MM: We do have a radar about it
* JP: About postponing, since we already have enable, does it prevent us changing this in the future?
* MM: We might need an `enable` for this :)
* MM: I was thinking of something like array syntax, where if we enable it, we might need to parse *differently*. 
* **BC: Would like to talk to DN about this.**
* MM: Would we like to postpone for a week, or for longer?
* **DN: Next week is fine, though I’d like to see a proposal from MM**


### [MSL does not support address-of vector component #1931](https://github.com/gpuweb/gpuweb/issues/1931) 



* 


### [wgsl: replace stride attribute with array type parameter #2103](https://github.com/gpuweb/gpuweb/pull/2103)



* Previously:
    * MM to ensure existence of an issue for not having stride
    * MM: Yeah I can put up that issue so we can talk about that first.
* Updates:
    * Still:
        *  spell runtime-sized array as ‘dynamic_array’.
    * New: 
        * Introduced ‘modifier word’ kind of token, which has special meaning only in certain contexts and is otherwise interpreted as an identifier. (Should make ‘access mode’ and image formats into these, rather than keywords.)
        * Use ‘,stride=N’ syntax instead of ‘,N’ to specify explicit stride.
        * Updated grammar to match the prose.
* **Block on MM’s TBD issues**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-10-26 (non-APAC 11am time)
* [https://github.com/gpuweb/gpuweb/pull/1830](https://github.com/gpuweb/gpuweb/pull/1830)
* 