# WGSL 2021-12-07 (non-APAC 30m supplemental) Minutes

**🪑 Chair:** Kelsey (was Jeff) Gilbert

**⌨️ Scribes:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon Americas/Los Angeles

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

**Todos doc: **[WGSL TODOs](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0)

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
    * Ryan Harrison
    * Sarah Mashayekhi
* Intel
    * Jiajia Qin
    * Jiawei Shao
    * Shaobo Yan
    * Yang Gu
    * Zhaoming Jiang
    * Yunchao He
    * Narifumi Iwamoto
* Microsoft
    * Damyan Pepper
    * **Greg Roth**
    * Michael Dougherty
    * Rafael Cintron
    * **Tex Riddell**
* Mozilla
    * **Dzmitry Malyshau**
    * **Kelsey Gilbert**
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
* **Timo de Kort**
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


---


# ⏳ Timeboxes

**(none)**


---


# ⚖️ Discussions


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* (Also [Typing of literals #2306](https://github.com/gpuweb/gpuweb/pull/2306))
* Previously:
    * Revisit next week
* RM: Mostly one point of disagreement between the two: Should we have very high precision for literals, or have the same precision for literals and other vars. Question is probably “is it worth having one set of rules instead of two” for our authors
* DM: Programmers don’t think too deeply about precision. They try something and iterate until it works.
* RM: I think programmers have to be somewhat aware, aware that precision is finite, so they have to be somewhat aware of these things.
* MM: More important for shaders: using denormals or infs.  Shader authors think more about precision than non-shader authors.
* KG: I usually try to ignore precision, unless I know it’s going to be a problem. E.g. in Javascript do everything in doubles.  If I know I have to use int64 math I do the extra work to be careful about it.  So “default” mode is “probably works, don’t worry about it” vs. “edge cases for precision.”  If I have to do big bitmassks I’ll reach for 64bit bitmsasks and mess with the bother 
* BC: What you’ve described is what Go does.  Doesn’t have types for literals. Only cast to particular type at the end.
* MM: Can see conclusion Kelsey’s argument going either way. So which is it.
* KG: At the edges I don’t care that much.  Both cases can work. From C++ background I am used to reaching out for suffix annotation on literals, or casting if I need to. To me it’s not a huge value add to do everything in extreme precision space and then cast it down.  If I need to do 32-bit integer math, I’ll use an explicit marking.
* DN: Absent a feature in WGSL, the programmer will compute these literal expressions in JavaScript, which is going to be in high precision space. So there are fewer surprises in going into the WGSL form of it.
* BC: In the Apple proposal, what do you see with constexpr.  A goal is to put expressions into variable/let and you get the same result.  Q: When do you finalize a machine type: at the statement boundary or across declarations.
* RM: Don’t propagate across statements.
* BC: So if it infers a machine type there. Then not sure that putting into variables gets you the same value result.
* RM: …. Cases where you can get a type error. Won’t get a silent conversion (?)
* BC: In office hours, Google’s stance is very willing to reduce the “ideal” implementation precision to be same as double and 64-bit int.
* KG: Was a concern, more palatable to use 64bit types.
* RM: Not super comfortable having different semantics for different types.  Seems the group leans in agreement to allow it. Won’t block it.
* MM: Would be helpful: Comment Robin made at the beginning is: trying to be as helpful as possible to authors. Shall we ask authors.
* KG: Struggle with the difficulty of translating expressed developer desire into actual developer need.  To them it’s often “it’s free, give me the nice thing”. Best we can get is “how much certain things suck”. And then weigh that.
* DN: Always a question of how we sample developer populations.
* BC: Both proposals always allow you explicitly type things. We’re talking about the defaults.  If anybody runs into a problem in their case then they can explicitly coerce.
* **_Progress, let’s revisit._**


### [[proposal] Changes to overridable constants · Issue #2238](https://github.com/gpuweb/gpuweb/issues/2238)



* Previously:
    * JB: Can we defer until DM is back next week? (yes)
    * Revisit next week
* DN: Some things will have their values finalized at pipeline creation, so that’s the purpose. Some things will need ids for bindings, but others are fine without. “Hey, if it’s overridable, it’s overridable”
* MM: Can you clarify?
* DN: Key idea is to make analogy to default parameters in C++ function calls.
    * override&lt;0> blockSize: i32 = 0; // can be overridden in API with id 0
    * override foo : f32;   // must be overridden by name.
    * override blah = foo;   // Question.  Can I override in teh API side? Yes.
    * override blitz = 2 *  blah;  /// Can i override this.  Yes, in analogy to default parameters
* BC: Override declares a thing that can be overridden.  If it has an initializer, then the implementation can compute the value, at pipeline creation time.  
* MM: Like DM asked in comment, above, if blah is overridden via API, what should blitz be.
* DN: It would be 2*blah_from_api
* DM: What I don’t like is the angle-brackets for the IDs.
* MM: Ok to change to [[/]] attribute.
* JB: So we’ll need to reserve some calculations/evaluations til pipeline compilation.
* BC: Yeah, have the expression tree carried into pipeline creation. And evaluated at that time.
* JB: This is probably okay implementation-wise, because we’ll probably have enough of an interpreter to do constant evaluation available for other reasons.
* DM: Why does the code need to exist anyway?
* JB: For something like constant folding. [e.g. to compute blitz from blah] But, this is just a concern about implementation complexity
* KG: Each pipeline creation that does a different override will have to recompute.
* DN: What attrib name should we use? Id?
* MM: “location”!
* DM: Can we still override by name then?
* MM: Yes, can always override by name, and alternatively by location if given one
    * E.g. “override foo: i32;” would be valid, and would be identified by the ident “foo”
* DM: We’re introducing a new keyword so that we can have separate contexts here, but we haven’t introduced a new type to enforce this. It would be nice if the function signature could tell us if it’s an override enough
* MM: We already have keywords with different expressivities, such as let allowing reassignment, while var does not. I think for const exprs we do want something obvious on signatures so that people know what to expect.
* **Resolution: General approval, needs more specific issues for follow-ups probably. DN will fixup PR to match consensus.**
* DN: I’ll update the PR to match consensus, and think about constexpr encroach into type system,and what to do at formal parameter boundary.


---


# 📆 Next Meeting Agenda



* Next meeting: **TODAY 2021-12-06 5pm Los Angeles time (APAC)**
* Next week’s meeting: 2021-12-14 11am Los Angeles time
* 