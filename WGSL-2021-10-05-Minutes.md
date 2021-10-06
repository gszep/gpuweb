# WGSL 2021-10-05 Minutes

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** David Neto, Jeff Gilbert

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday ****5pm-6pm (APAC)****

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
    * Robin Morisset
* Google
    * Alan Baker
    * Antonio Maiorano
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * **David Neto**
    * Ekaterina Ignasheva
    * Kai Ninomiya
    * James Darpinian
    * James Price
    * Rahul Garg
    * Ryan Harrison
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


### Unscheduled News



* MM: Apple is ok removing depth types for samplers.
* DM: Under the assumption that users specify it in the binding layout? 
* DM: If the pipeline layout knows it’s depth, we can generate the right MSL when compiling at pipeline compilation time
* MM: We would be ok if shaders that used depth textures didn’t need to be distinguished from non-depth textures within the shader.
* **MM to take AI to write about this in its issue**
* MM: BTW: Metal argument encoders (objective C argument that is used to populate an argument buffer) don’t differentiate between depth and normal textures, fwiw. Just like passing a depth texture to a non-depth type de facto works, it should work just the same there.


### Office Hour



* **Wednesday 10am-10:50am**
* **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
* Everyone welcome
* Mass calendar invite will have been sent out
    * If you still need an invite, add your email here:
        * [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)


### On Todos



* (JG: I will make a spreadsheet for thumbs-up/down/maybe for each todo)
* 


---


# ⏳ Timeboxes


### [wgsl: editorial cleanup: memory vs. storage #2155](https://github.com/gpuweb/gpuweb/pull/2155)



* DN: Trying to use “memory” instead of “storage” in most places. Some other minor fixes.
* DM: I feel that “storage” etc in WGSL has been confusing from the start, and I like that we should fix it.
* DN: Would like to have that larger talk in another issue** (DM to follow up)**
* MM: PR seems good though.
* **JG will merge**


### [wgsl: remove ignore, add phony-assignment #2127](https://github.com/gpuweb/gpuweb/pull/2127)



* Since last time:
    * rebased against main
    * as requested, unify phony-assignment more closely with regular assignment
    * regular assignment is now called "updating assignment"
    * Also, for a phony assignment, the type of the right-hand-side is not restricted. So you can do nice things like `_ = buffer_with_runtime_sized_array` because that RHS is a reference, and the Load Rule does not kick in.
    * Still removes the ignore builtin.   (So speak up if you want to keep it)
* MM: I wonder how the wording is going to work with e.g. `let a,_ = foo()`, like would it be required to be constructable?
* DN: Well we don’t have multiple assignment yet.
* DM: Why not just require &tex, instead of having this extra complication in the specification we write.
* DN: I can put that back.
* MM: Either sounds fine.
* **DN to fix and merge after DM approval**


### [Allow syntactic navigation and styling #2143](https://github.com/gpuweb/gpuweb/pull/2143)



* MOD: Applied some straightforward feedback.  Can bikeshed over some details offline.  Makes it easier to read on mobile (wraps the formatting). Uses Bikeshed dfn to create each node.
* DN: Have you changed the flow to keep the grammar rules as before, but then generate to inser bikeshed definitions.? 
* MOD: That’s what I did originally, and then inserted the result.  But didn’t want to introduce an extra script in everyone else’s flow. 
* DM: Outcome is what we definitely want.  The boilerplate of writing bikeshed definitions will be painful to maintain. Will hit the editor most.
* DM: Do other groups have script-processed bikeshed?
* MM: CSS has a grammar, but bikeshed has special support for CSS because BS was originally made for CSS.
* MM: Good direction, but has a positive result we definitely want
* DN: Take 2 actions 1. Will reconsider how bad it really is to maintain the grammar in the proposed form (is it that big a burden), and 2. Reach out to Tab Atkins (Bikeshed maintainer) to see if there are grammar-friendly facilities in Bikeshed that we could use better, or perhaps add some.


### [Should hex exponents be optional? #2036](https://github.com/gpuweb/gpuweb/issues/2036)



* MOD: Making them optional can be simpler for authors, when writing original source.  Otherwise, can always add p0 exponent at the end.
* DN: Want to keep it the same as C99, which is where feature came from.
* …
* JG: Is it necessary to disambiguate? 
* DN:: No. Already have 0x and a mandatory ‘.’ that already disinguishes.
* JB: I’d like to write hex floats, and would like it to be optional. It’s more convenient
* **MOD to write PR**


### [Define a MIME type #1682](https://github.com/gpuweb/gpuweb/issues/1682)



* DN: I looked at the registry [https://www.iana.org/assignments/media-types/media-types.xhtml](https://www.iana.org/assignments/media-types/media-types.xhtml) and could not find GLSL, C, or many other languages. CLosest I could find was application/javascript.  That’s a close enough analog to suggest application/wgsl.  But I’m no expert.
* **JG: Let’s do application/wgsl until someone says we’re wrong.**


### [Clarify memory locations accessed when writing a vector component #2152](https://github.com/gpuweb/gpuweb/pull/2152)



* JG: This seems like a harsh reality that we have to accept
* MM: If you don’t like it, don’t use vectors
* **Accept PR**


---


# ⚖️ Discussions


### [wgsl: Add limits section, and "spurious" failure #1997 ](https://github.com/gpuweb/gpuweb/pull/1997) 



* **Let’s do it!**
* MM: For these limit values, where are they from?
* DN: SPIR-V
* MM: Can we lower after V1
* DN: Not really. Implementation could emit an “uncategorized error”, but that’s a bad experience.   We should test before V1 and change if we need to.


### [MSL does not support address-of vector component #1931](https://github.com/gpuweb/gpuweb/issues/1931) 



* MM: Haven’t heard anyone complain about MSL not supporting this. Would rather forbid in the language than try to emulate somehow.
* DN: One approach would be to translate vec4 as float[4], which would work in MTL. There could be a perf cliff, but that’s up to the user to run into and solve if needed. 
* MM: Seems scary. E.g. builtins would need to handle both? Go in and out?
* DN: Yeah, but that’s probably ok? I think we’re investigating it. I’m pretty aggressive about this one, and we’re not at-consensus within Google yet.
* MM: So you may have already paid the implementation cost, and you are evaluating the perf cost. How do you feel about my assertion about usefulness?
* DN: I feel like removing this adds an orthogonality to the language, which I would like to avoid.
* DM: Maybe could have reference to the element of vector, but not a pointer to the element of the vector?
* JB: So we’d have a carve-out to forbid converting certain refs into pointers?
* JB: Maybe we should ask MSL people, if they were to support this, how would they do it?
* MM: Can’t speak for them, but I expect them to say “drivers don’t have that functionality”.
* **Check back next meeting, but no promises**


---


# 📆 Next Meeting Agenda



* Next meeting: 2021-10-12 (non-APAC 11am time)
* 