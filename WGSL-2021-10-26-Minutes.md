# WGSL 2021-10-26 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Jeff Gilbert

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
    * David Neto
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
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
    * Rafael Cintron
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


### Meeting time availability recheck



* **Please fill out this survey**: [https://doodle.com/poll/z2wngnzttmnecpci](https://doodle.com/poll/z2wngnzttmnecpci)
* It is for both WebGPU and WGSL
* Poll will close around the end of the week!


### Todo triage



* Document: [https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/](https://docs.google.com/spreadsheets/d/1OnsotW_eWoWn4m9lW2-nRrOfR8gBSJ5Oqz9O-9hTDdA/edit#gid=0) 
* 


---


# ⏳ Timeboxes


### [wgsl: decimal point is optional in float literal having an exponent #2201](https://github.com/gpuweb/gpuweb/pull/2201)



* **Merge**


### [wgsl: detailed semantics of integer division and remainder #1830](https://github.com/gpuweb/gpuweb/pull/1830)



* MM: I think JS is well-defined there, so we should consider following that.
* JG:
* MM: Two aspects, implementability and consistency.
* JB: JS is IEEE, even for integers, so e.g. x/0=+inf, 0/0=NaN
* MM: WGSL doesn’t say that NaNs are handled
* JG: What about infinity?
* AB: Also not guaranteed
* MM: It’s more important that the spec says something, rather than nothing yet.
* JB: I see the spec says we have IEEE, and inf and nan. Is that wrong?
* AB: You can load them, but operations are allowed to assume that there aren’t inf and nan.
* JB: Can we then just leave it IEEE, and [let the wheels fall off naturally]?
* AB: Do we want to handle 0/0 different from x/0?
* MM: Should probably use the same result for INT_MIN/-1 (overflow) too?
* MM: For the rarer cases, we can probably change it later and people are unlikely to notice.
* **Up to DN for whether to change existing PR**
* **Resolved to try to pick results rather than call it an error.**


### [Support arrayLength for fixed-size arrays #2074](https://github.com/gpuweb/gpuweb/issues/2074)



* AB: We feel this isn’t as useful now, and think we should drop it.
* BC: We feel that using a mod-scope-let length i32 as an array length, that’s what should be done here.
* MM: I do think there’s value in symmetry here. You say this is a func that operates on the type, but I think we could say it operates on the value anyway, and that authors would be ok with that.
* AB: The runtime array one takes a pointer
* MM: Then we should have two, one that takes a pointer, and one that takes this
* BC: What about side-effects
* MM: If you do arraylength(helperFunc()), I don’t think that’s very common, as you’ve lost the contents of the array. 
* BC: In the pointer form, I think there can’t be side-effects.
* MM: Yes
* BC: So I think Google thinks it’s not important, at least for v1.
* MM: I think it provides weak value at weak cost
* JG: I like allowing the compiler to tell us things that the compiler knows, is good.
* **Defer until after resolution for e.g. dynamic indexing into arrays**


---


# ⚖️ Discussions


### [Literal unification #2200](https://github.com/gpuweb/gpuweb/issues/2200)



* AB: This came out of discussions internal to Google, but matches some of the feedback that MM gathered also.
* AB: It’s a layered proposal, such that we can choose how many layers to adopt.
* MM: Proposed something a year and a half ago: [https://github.com/gpuweb/gpuweb/issues/739#issuecomment-634874369](https://github.com/gpuweb/gpuweb/issues/739#issuecomment-634874369)
* MM: Had 6 goals, this proposal answers most of them
* MM: We would like more precision than this proposal, but like the general direction.
* AB: Sure, wanted to confirm direction before starting to write spec text
* MM: We believe that operators and functions should behave the same way
* MM: Not super sure that what’s written matches what we have in mind, but I think this is generally the right direction.
* AB: I think up through step 4 matches what you expect.
* Consensus: Don’t stop between steps 3 and 4
* JG: Do we have consensus for just going through 4?
* MM: I think we might come up with more examples/problem cases once we have something more spec-like.
* BC: So should we just spec 1-4 before speccing step 1?
* **Needs spec, can be spec 1 first, but spec 2-4 together. (or just 1-4 together)**


### [The order of top-level declarations should be irrelevant #875](https://github.com/gpuweb/gpuweb/issues/875)



* Previously:
    * BC: Would like to talk to DN about this.
    * DN: Next week is fine, though I’d like to see a proposal from MM
* BC: DN and I talked. I think having these at the top, or being global would be preferable to having a before-and-after.
* BC: So that’s Google’s preference.
* BC: AB also contends that out-of-order declarations should be punted after v1, as it’s backwards-compatible without it.
* RM: I think my preference is to allow enabling within blocks, and we should say what happens if you e.g. call a function that uses something that’s not enabled.
* JB: These are subjective, but in Rust, declaration order doesn’t matter, and I love it.
* JB: Also, “extensions” are so open-ended that it’s hard for us to be absolutely sure that we will be able to handle anything that might happen.
* MM: Even if you have a function that returns a f16 inside the block, and you call it from outside the block, it’s the *use* of the type that’s invalid, even if the author isn’t writing f16 there.
* AB: GLSL requires at-top decls, and that’s fine I think. I’m definitely ok from my C background.
* BC: I also like out-of-order, but I think it’s fine not to be v1.
* BC: One of the reasons for it is for code-stitching, but I don’t think it’s a killer feature.
* RM: To clarity, while I have a slight preference for blocks, what I most disliked was before/after, rather than at-top or out-of-order.
* RM: I have a preference for allowing declaring things in any order.
* MM: Worth noting that at-top decl requirement is effectively the same thing as requiring out-of-band.
* RM: Also we have a difference from C/C++: without headers, we cannot declare every function, and then define them in whatever order is most readable/convenient.
* **No consensus yet**


### [MSL does not support address-of vector component #1931](https://github.com/gpuweb/gpuweb/issues/1931) 



* **(out of time to discuss)**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-11-02 (***APAC*** **5pm time**)
* 