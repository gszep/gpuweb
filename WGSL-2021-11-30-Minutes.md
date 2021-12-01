# WGSL 2021-11-30 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribes:** DN, JB

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
    * **Sarah Mashayekhi**
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


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


---


# ⏳ Timeboxes


### [wgsl: Add vector and matrix constructor overloads that infer component type #2305](https://github.com/gpuweb/gpuweb/pull/2305)



* DN: On a vector type constructor, one can infer the component type from the arguments, so, let’s do so. This reduces visual noise when writing literals. This PR adds the overloads. We could do the same for arrays, but that’s another PR.
* JG: matrices?
* DN: all elements must be the same T.
* JG: So you can have vec(1,1,0,1), but not vec(1,1,0, 0.5)?
* DN: Yes.
* MM: If this is just adding overloads, and not adding new syntax, then this is fine.
* MM: Doing this for arrays will be different from adding overloads, because the compiler will have to get involved.
* DM: We haven’t had functions before with a generic parameter, but generally I like the idea. But, this is not meant to cover all cases, right?
* DN: No, just vectors and matrices
* DM: Can I create a vec4 from two vec2s? Isn’t that a lot of overloads?
* DN: yes
* MM: So I can create two vec2s, and then combine them into a vec4, all without ever writing `f32`?
* DN: yes
    * vec4(vec2(1.0,1.0),vec2(2.0,3.0))
    * Is same as    vec4&lt;f32>(vec2&lt;f32>(1.0,2.0),vec2&lt;f32>(2.0,3.0))
* DN: It does not allow vec4(vec2(1,1),vec2(2.0,3.0))
* DN: This is independent of literal type inference.
* JG: It seems like this doesn’t help people not type .0 all the time, which is the main pain point
* MM: Should we defer this until next week?
* JG: I’d appreciate the time to think about this for another week
* **RESOLVED: defer until next week**


### [Add compound assignment (e.g. +=), updated](https://github.com/gpuweb/gpuweb/pull/2345)



* DN: Robin wrote a PR, but we decided not to pursue it. But then James had an idea to resuscitate it, but that actually did end up complicated. So this is a third approach which isn’t too bad. The reference is calculated *once*, and the spec has an example that illustrates this.
    * myArray[foo()] += 3;     // `foo()` called only once
* RM: It would be nice to have a “NOTE” explaining how it’s implemened
* DN: Okay, I’ll revise this and have RM review
* MM: Are we also going to have ++, since we have +=?
* DN: yes
* MM: And only one kind of ++?
* DN: Yes, I propose only suffix
* MM: And ++ is not an expression, so it doesn’t produce a value, so the prefix/suffix distinction doesn’t matter
* JG: I subscribe to the Python school of thought, that x += 1 isn’t really that much more typing. But if there’s consensus around having it, that’s okay
* DM: Doesn’t seem necessary
* MM: It’s common in other languages, so there’s a lot of utility in familiarity. And since we’ve got the conceptual overhead of += already, the incremental cost is low.
* JG: People get used to having to type +=. The fact that people use something doesn’t mean that they need it. But, we can discuss on the PR.
* RM: Would like a non-normative note to capture our previous discussion: how it translates to simpler WGSL for non-vector-componet, and the special case for vector component.
* DN: I’d like to take another crack at simplifying the writing, so let’s see how it looks
* **Resolved approved**


### [Add integer increment and decrement statements #2320](https://github.com/gpuweb/gpuweb/issues/2320)



* DN: There is a PR, but it’s poorly phrased. I’d like to rephrase it.
* RM: If the earlier PR is merged, then this can be specified as pure syntactic sugar for that
* DN: I’ll rewrite, pass it by RM
* MM: It’s not unreasonable to have ++ work on floats, if it’s easy to specify
* DN: It’s not easy to specify.
* MM: The ULP of some floats is greater than 1, so +=1 can have no effect, so therefore, we should probably not support ++ on floats, to make it clearer that people are adding a tiny number to a large number.
* DN: It’s a little more of a footgun, so let’s not have ++
* JG: There’s merit to pure syntactic sugar definitions, though.
* DN: It’s all PDP worship
* JG: It doesn’t seem necessary
* DM: iteration is the actual need
* JG: we can abbreviate for loops ad nauseum but it doesn’t establish necessity
* MM: People do use ++ all the time
* JG: but the frequency of something’s use is not evidence for its necessity. We should weigh the actual quality of life engendered by the feature. Many popular languages don’t have it
* BC: But every time I trip over its absence, I wonder why it was taken away from me
* JG: People get used to Python.
* MM: Swift doesn’t have ++, but the reason is that they didn’t like the prefix/postfix confusion. They do have += because it lacks that ambiguity.
* JG: We solved that by saying it’s only a statement. If in the future += becomes an expression, then at that point I would really object to ++ as an expression
* MM: I will never want ++ as an expression, no matter what we do with +=
* JG: Well, with my and DM’s weak objections noted, let’s look at the revised PR and then accept it
* **ACCEPTED pending revisions**


### [wgsl: for texture coordinates, allow array index be appended to the coordinate vector #2340](https://github.com/gpuweb/gpuweb/issues/2340)



* DN: This was an offshoot of investigating GPU assembly languages
* DN: Was looking into the origin of the requirement that the texture offsets be integer constants. D3D and Vulkan both allow floats there. Happy to close this with no change as just an investigation.
* TR: D3D does allow integer coordinates, right?
* DN: Yes. WGSL allows only normalized coordinates for sampling operations, and integer (non-normalized) coordinates for texel loads and stores
* **CLOSED: no change**


---


# ⚖️ Discussions


### [Remove [[block]] attribute #2104](https://github.com/gpuweb/gpuweb/pull/2104)



* DN: All the rules that [[block]] was created to enforce are now enforced in a different way. There is a “fixed footprint” rule: if its memory consumption is determined at pipeline creation time, then it can be used as an array element, and [[block]] is no longer needed. And it’s nice not to have struct required for globals.
* DM: How are these restrictions enforced now?
* DN: runtime-sized arrays can only appear in the acceptable places now. So you can’t put a runtime-sized array inside a runtime-sized array.
* MM: Yes, we should get rid of block. Thumbs up.
* MM: I don’t think this opens the door for not requiring these buffers to hold structs. We could just permit [[block]] on non-struct things.
* MM: The benefit we’re hoping for from this change would be how we’re going to get to bindless, sometime. What do we want the user to type to distinguish between a two-dimensional array versus a collection of one-dimensional arrays? I had some suggestions in the PR. We could discuss this after MVP.
* DN: Internally, when Google was discussing this, we were also concerned with preserving a path to bindless.
* MM: We don’t need to block this on that path. All the ways forward that I came up with involved removing [[block]], so we can certainly do that at this point.
* DM: Yes, the path seems pretty clear.
* **ACCEPTED**
* DN: And if it’s broken we’ll fix it!


### [Add textureGather, textureGatherComponent, textureGatherCompare. #2341](https://github.com/gpuweb/gpuweb/pull/2341)



* DN: The functionality is clear, the question is how to capture that functionality as functions. D3D has separate overloads depending on which component is being fetched; the current proposal is to specify the component with a constant expr argument.
* TR: Historically, there was only gather on a single component, because that’s all the hardware supported. But nowadays there’s no problem getting the other components, so there’s no need for the no-component-selector overload.
* DN: For depth textures, you’d specify 0?
* TR: Yes
* GR: It might be easier to document with the overload?
* JG: The huge number of overloads is a fact of life
* MM: What’s the problem with overloads? Developer confusion? Compile time?
* TR: Choosing the right overload with the different orderings can be hairy
* GR: It’s a pain to test all the combinations.
* MM: If you have a depth texture, but the metal shader doesn’t know it’s a depth texture, these non-compare gather functions are untested and unlikely to work. If we accept this, then we shouldn’t accept the decision to delete the depth texture type, because depth textures have a different set of components that one can reasonably request
* JG: Couldn’t people just apply a color sampler to a depth texture?
* DM: Can you do a gather without comparison on a Metal depth texture?
* MM: Not sure.
* JG: Let’s specify something, try to implement it, and then see if things break.
* MM: DM’s proposal to get rid of the WGSL depth texture type, but continue to have it in the WebGPU API, then this problem goes away
* MM: But I definitely think we should have gather functions, in some form. They’re not super-useful, but they’re useful.
* DN: How should I change the signatures of these things? Just always require the component, but in some cases it must be zero?
* JG: either having an overload or requiring the component would be fine. Wild idea: put component selector argument first
* TR, GR: That’s a nice idea
* GR: We shouldn’t just test things that the spec doesn’t cover
* JG: Yes, but we have influence over what Apple will add to the spec
* MM: Let’s accept this PR (modulo reformulations), discuss depth textures later, and then if the conclusions don’t jive, we can reopen this.
* DM: Checked the spec, it seems like Metal can support this
* JG: Do we have consensus on moving the component selector to the front?
* DM: Yes, but not for the depth textures
* **Revise and acceptd**


### [[proposal] Changes to overridable constants · Issue #2238](https://github.com/gpuweb/gpuweb/issues/2238)



* Previously:
    * JB: Can we defer until DM is back next week? (yes)
* **Revisit next week**


### [Literal unification, bottom-up #2227](https://github.com/gpuweb/gpuweb/pull/2227)



* **Revisit next week**


---


# 📆 Next Meeting Agenda



* [https://github.com/gpuweb/gpuweb/issues/2310](https://github.com/gpuweb/gpuweb/issues/2310) process, policy for extensions / enables.
* [wgsl: Add vector and matrix constructor overloads that infer component type #2305](https://github.com/gpuweb/gpuweb/pull/2305) deferred from last wee. (JG)