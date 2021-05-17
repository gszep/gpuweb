# **WGSL 2021-05-11 Minutes**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** 

**🗺 Location:** [meet.google.com](http://meet.google.com)

**🌐 Timezone:** America/Los_Angeles

**⌚ Time:** Tuesday 11am-noon

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send Dan Sinclair a Google Apps enabled address and he'll add you.



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   Corentin Wallez
    *   **David Neto**
    *   **Kai Ninomiya**
    *   James Darpinian
    *   Brandon Jones
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
    *   **Ben Clayton**
    *   **Alan Baker**
    *   **James Price**
    *   Rahul Garg
    *   Ekaterina Ignasheva
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   Rafael Cintron
    *   **Tex Riddell**
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
    *   **Jim Blandy**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   Hamada Gasmallah
    *   Wes Garland
*   Dominic Cerisano
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   **Timo de Kort**
*   Tyler Larson
*   Eduardo H.P. Souza



---



# **⚖️ Agenda/Minutes**


## Meta


### Office Hour



*   **Wednesday 10am-10:50am**
*   **[https://meet.google.com/xrp-hpck-vmy](https://meet.google.com/xrp-hpck-vmy)**
*   Everyone welcome
*   Mass calendar invite will have been sent out
    *   If you still need an invite, add your email here:
        *   [jgilbert@mozilla.com](mailto:jgilbert@mozilla.com)
        *   


### VF2F



*   Survey soon!
*   


### Shortname [#1707 (comment)](https://github.com/gpuweb/gpuweb/pull/1707#discussion_r625333903)



*   “wgsl” or “WGSL”?


## 

---



## Timebox


### [Remove IN and OUT from storage class grammar #1718](https://github.com/gpuweb/gpuweb/pull/1718)



*   Merged


### [[proposal] Numerical compliance #1716](https://github.com/gpuweb/gpuweb/issues/1716) 



*   RM: Should be the least precise of the underlying platforms.  Question. How does fast math work, and is it independent of accuracy. Can fast-math reduce accuracy below.
*   AB: Vulkan doesn’t specify it as an option. It’s enabled-by-default.  E.g. in Vulkan for fast-math the accuracy says “must be no worse than the broken-down operations”. 
*   AB: fast-math can be explained by “these are the things you’re allowed to do to rearrange.”
*   MM: Metal fast-math is a distinct opt-in. Scientific computing needs stronger guarantees.
*   AB: My intent is fast-math is the default. Could have post-MVP option to add stronger guarantees.
*   DN: Vulkan SPIR-V has “NoContraction” decoration to apply stronger guarantees.
*   MM: Ok to have an extension for stronger guarantees.
*   AB: Vulkan will get some way there, but not all the way.
*   MM: Want to be open to updating the “fast math” definition in WGSL to match what the underlying APIs do natively and most quickly.
*   TR: HLSL, is not strict by default. It has “precise”, but that’s not the default.  Strictness as documented is strictness of the operations themselves when marked IEEE safe mode and “precise” and other assumptions like no infinities and NaNs.  By default it’s more like Vulkan’s behaviour.
*   AB: I’ll pick it up from here.


### [[wgsl] support readonly and writeonly on storage buffers · Issue #935 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/935) 



*   Closed as obsolete


### [wgsl: support extended integer arithmetic for add, subtract, multiply · Issue #1565 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1565)



*   AB: should be post-MVP
*   MM: not clear what HLSL offers for this
*   TR: mul_high is exposed in DXIL, but not in HLSL
*   MM: what is our rule for this: will all implementations that rely on HLSL go straight to DXIL, or do we want compatibility with HLSL the language.
*   DN: DXIL path is at least one year from now for us, so we want support in HLSL itself
*   GR: also not clear that DXIL-only operations are as widely tested.
*   MM: so we should only implement things which are supported by HLSL.


### [Equality comparisons on structs · Issue #1572 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1572) 



*   MM: can just close
*   RM: might be good later, but clearly post-MVP
*   JG: postponed


### [does textureSampleCompare assume level-of-detail 0 in non-fragment shaders? · Issue #1319 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1319)



*   DN: Thanks Myles for the summary. Comparison textures can be sampled in vert and frag, but they have different behaviors, which is worrisome when the builtin is spelled the same. Would be nice to have a different builtin for the different behaviors
*   TR: +1
*   JG: +1
*   DM: Final proposal is `textureCompareLevel` (uses level 0)
*   AB: Add restriction `textureSampleCompare` to be frag-only
*   TR: So then could we add a param later? (yes) So we support that style of overloading (yes, for builtins)
*   DN: I volunteered to write the PR


### [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 



*   MM: One Q: How does this work in VK? Can you use a uint to index into array? (yes) If the value is too large, does it wrap around? (yes, unless the buffer is big enough) If there’s an array of size 3B, and I use uint=2.9B, does that work?
*   DN: Hmm, in llvm address info is *signed*, so what I said above is probably wrong. 
*   AB: Well, could use i64 if supported, and unlikely to have a system support 3B addressable that doesn’t support i64, so this is probably a perf question.
*   MM: I understand that buffers are usually smaller than 2GB, but it would be nice if we didn’t have to pick between perf and capability up-front.
*   TR: Is this a backend/implementation question? (maybe)
*   AB: VK doesn’t usually care about signedness of ints, *but* addressing is defined in signed ints, which limits us.
*   MM: So my 2.9B might be interpreted as negative i32? (yes)
*   AB: What’s the current limit? (128MB?) So implementations could choose to offer &lt;=2GB and just not have this problem.
*   TR: Seems preferable not to decide that unsigned ints aren’t interpreted as signed for the purposes for addressing.
*   DM: So in the future, we could offer >2GB limits only if we support the i64 support we need. And since limits are opt-in, we could know when authors expect to need >2GB, and do the right thing.
*   DN: Is there a portability concern?
*   JG: Probably no? 
*   DM: We are free to not expose anything above the baseline limits.
*   DN: Do we still need uint and sint overloads for addressing?
*   MM: If I have a buffer with 3.9B indices, and I ask for -0.1B, would that work?
*   MM: So is it, if the author doesn’t claim a max buffer size, they get the 128MB limit. Only if they ask for >2B, would we have to emit the extra ops during shader translation.
*   DM: Why are we investigating negative indices? Don’t we not want them?
*   DN: I don’t know how much of a hit it would be to usability to offer i32 support here.
*   JB: Rust always uses uints for indices, but it has good enough ergonomics that supports this.
*   DN: Do we really need the full >2GB? 
*   JG: &lt;missing>
*   MM: In real-pointer languages, they really are signed, because you can move them up or down.
*   JG: But fundamentally i32 and u32 add are the same bit results
*   &lt;missing>
*   AB: Can we just limit the size?
*   MM: Yeah but what happens with out-of-bounds inputs?
*   


## 

---



## Discuss


### [consider pulling 'access' into the type of a pointer/reference #1604](https://github.com/gpuweb/gpuweb/issues/1604)



*   New proposal: [https://github.com/gpuweb/gpuweb/issues/1604#issuecomment-832937987](https://github.com/gpuweb/gpuweb/issues/1604#issuecomment-832937987) 
*   AB:: Seem close to consensus.  Related is passing textures/samplers by handle-value vs. pointer.
*   MM: Is it valid to take a const access to a non-const.
*   AB: Would like to be able to get a read-reference to a read-write buffer.
*   JG: Hear there are two schools of thought.
*   MM: Doing that genericity is an accidental feature. Think for MVP it’s ok to make a function not generic over const vs. non-const
*   AB: Fine with that. Not big enough usability gain.
*   ~~Consensus: access must match (ie no read_write -> read decay)~~
*   MM: Had been thinking about allowing a function to be    ptr with ‘*’ for read, write, or read_write.  That’s what we think we shouldn’t tackle now.
*   **Consensus: Must declare access qualifiers (no templating/generic functions (yet))**
*   MM: So now, can we make a ptr&lt;read> from a ptr&lt;read_write>
*   **Consensus: no read_write -> read decay for now**
*   MM: So some background: One thing to consider here is that the contents behind the read-only might be changed elsewhere.
*   JB: Probably less bad so long as we don’t call it ‘const’ :)
*   MM: Yeah, difference between “I cannot change the object” vs “The object cannot be changed”
*   AB: Proposal: Forbid (for now) passing e.g. textures by value
*   MM: Tentative agreement, but want more time to think.
*   DN: So is my proposal the direction? (modulo minor things?) (yes)


## 

---



# 📆 Next Meeting Agenda



*   Next meeting: 2021-05-18 (like normal)
*   [https://github.com/gpuweb/gpuweb/pull/1720](https://github.com/gpuweb/gpuweb/pull/1720) limit override-id to 0-65535, to match the valid range in MSL function constants.
*   [https://github.com/gpuweb/gpuweb/issues/1345](https://github.com/gpuweb/gpuweb/issues/1345) textureDimensions() returns vec3 for cube textures 
*   [https://github.com/gpuweb/gpuweb/issues/1550](https://github.com/gpuweb/gpuweb/issues/1550) var: infer type from initializer 
    *   Popular on the priorities spreadsheet.
    *   Only difference of opinion is whether to allow it at module scope. (would be only private variables, as they are the only module-scope vars that can have an initializer)
*   [https://github.com/gpuweb/gpuweb/pull/1722](https://github.com/gpuweb/gpuweb/pull/1722) - out of bounds for non-memory values
*   RM [last week]: Uniformity analysis update will be ready definitely [next week], but maybe not by [this week].
*   [Buffer indices should be unsigned · Issue #1135 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1135) 