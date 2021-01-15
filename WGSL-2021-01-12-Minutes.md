# **WGSL 2021-01-12**


<table>
  <tr>
   <td><strong>🪑 Chair</strong>
   </td>
   <td><p dir="rtl">
<strong>:</strong></p>

   </td>
   <td>Jeff Gilbert + Dean Jackson
   </td>
  </tr>
  <tr>
   <td><strong>⌨️ Scribe</strong>
   </td>
   <td><p dir="rtl">
 <strong>:</strong></p>

   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>🗺 Location</strong>
   </td>
   <td><p dir="rtl">
<strong>:</strong></p>

   </td>
   <td>Google Meet
   </td>
  </tr>
  <tr>
   <td><strong>Specification</strong>
   </td>
   <td><p dir="rtl">
<strong>:</strong></p>

   </td>
   <td><a href="https://webgpu.dev/wgsl">https://webgpu.dev/wgsl</a>
   </td>
  </tr>
  <tr>
   <td><strong>Open Issues</strong>
   </td>
   <td><p dir="rtl">
<strong>:</strong></p>

   </td>
   <td><a href="https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl">WGSL Issues</a>
   </td>
  </tr>
  <tr>
   <td><strong>Meeting Issues</strong>
   </td>
   <td><p dir="rtl">
<strong>:</strong></p>

   </td>
   <td><a href="https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3A%22for+meeting%22+label%3Awgsl">Marked Issues</a>
   </td>
  </tr>
</table>


Note these are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).


## Agenda


### Discussion - what should we do with these old open PRs?

We don't need to discuss each of them today, but it would be nice to decide (as a group or as the PR authors) if they should remain open.



*   Add FAQ for direction on shader language ([#562](https://github.com/gpuweb/gpuweb/pull/562))
*   Introduce Subgroup Operations Extension ([#954](https://github.com/gpuweb/gpuweb/pull/954))
*   Update Goals section of spec ([#588](https://github.com/gpuweb/gpuweb/pull/588))
*   Update goals section ([#599](https://github.com/gpuweb/gpuweb/pull/599))
*   WGSL FAQ ([#687](https://github.com/gpuweb/gpuweb/pull/687))
*   Similarly, what about the older open issues?


### Open PRs



*   Change `offset` to `span`. ([#1339](https://github.com/gpuweb/gpuweb/pull/1339))
    *   Replace `[[offset]]` with `[[span]]`? ([#1303](https://github.com/gpuweb/gpuweb/issues/1303))
*   Remove outerProduct ([#1290](https://github.com/gpuweb/gpuweb/pull/1290))
    *   Support for outerProduct() ([#1286](https://github.com/gpuweb/gpuweb/issues/1286))
*   abs only works on signed integer scalar or vector ([#1282](https://github.com/gpuweb/gpuweb/pull/1282))
*   Bools are IO-shareable, but only for builtins ([#1246](https://github.com/gpuweb/gpuweb/pull/1246))


### Issues



*   Removing `ptr` and adding `ref` ([#1301](https://github.com/gpuweb/gpuweb/issues/1334))
*   arrayLength acts on pointer-to-runtime-sized-array ([#1330](https://github.com/gpuweb/gpuweb/pull/1330))
*   WGSL: add sample_index, sample_mask_in, sample_mask_out ([#1318](https://github.com/gpuweb/gpuweb/pull/1318))
*   Consider using multiple scalars to construct matrix. ([#1342](https://github.com/gpuweb/gpuweb/issues/1342))
*   arrayLength should operate on pointer-to-runtime-sized-array instead of runtime-array ([#1329](https://github.com/gpuweb/gpuweb/issues/1329))
*   Support compile-time-constants (constexpr) ([#1272](https://github.com/gpuweb/gpuweb/issues/1272))
*   update IO-shareable to include bool ([#1244](https://github.com/gpuweb/gpuweb/issues/1244))
*   texture builtins: For most texture builtins, Vulkan only allows compile-time-constant offset, and with with bounded range ([#1235](https://github.com/gpuweb/gpuweb/issues/1235))
*   Make all textureLoad functions on sampled textures require the level ([#1301](https://github.com/gpuweb/gpuweb/pull/1301))
*   Proposal to enhance defining input/output variables. ([#1155](https://github.com/gpuweb/gpuweb/issues/1155))
*   Add workgroup size and ID builtins ([#1295](https://github.com/gpuweb/gpuweb/pull/1295))
*   spell out when an implementation must reject a shader module ([#1241](https://github.com/gpuweb/gpuweb/issues/1241))
*   conflict between mat2x2 layout in uniform buffer and storage buffer ([#1258](https://github.com/gpuweb/gpuweb/issues/1258))
*   wsgl: Add rule that offsets must be compile time constant ([#1238](https://github.com/gpuweb/gpuweb/pull/1238))



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   Dean Jackson
    *   Fil Pizlo
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   Corentin Wallez
    *   **Dan Sinclair**
    *   **David Neto**
    *   **Kai Ninomiya**
    *   James Darpinian
    *   Brandon Jones
    *   **Ryan Harrison**
    *   Sarah Mashayekhi
    *   **Ben Clayton**
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   Rafael Cintron
    *   Tex Riddell
*   Mozilla
    *   **Dzmitry Malyshau**
    *   **Jeff Gilbert**
*   Kings Distributed Systems
    *   Daniel Desjardins
    *   **Hamada Gasmallah**
    *   Wes Garland
*   Dominic Cerisano
*   Joshua Groves
*   Kris & Paul Leathers
*   Lukasz Pasek
*   Matijs Toonen
*   **Mehmet Oguz Derin**
*   Pelle Johnsen
*   Timo de Kort
*   Tyler Larson
*   Eduardo H.P. Souza



---



# **⚖️ Discussion**


# Discussion - what should we do with these old open PRs?

We don't need to discuss each of them today, but it would be nice to decide (as a group or as the PR authors) if they should remain open.



*   Add FAQ for direction on shader language ([#562](https://github.com/gpuweb/gpuweb/pull/562))
*   Introduce Subgroup Operations Extension ([#954](https://github.com/gpuweb/gpuweb/pull/954))
*   Update Goals section of spec ([#588](https://github.com/gpuweb/gpuweb/pull/588))
*   Update goals section ([#599](https://github.com/gpuweb/gpuweb/pull/599))
*   WGSL FAQ ([#687](https://github.com/gpuweb/gpuweb/pull/687))
*   Similarly, what about the older open issues?
*   jG: Would be nice to finish some of thse off like the FAQ
*   RM: There was a proposal that was mostly agreeable to the group except for a few things. Will try to fix those things and ask for a re-review. Hopefully avoid needing to revisit
*   DN: Will close #562 in favor of #687
*   JG: Can add this to be discussed next meeting to discuss #687, sounds like almost there just need to push over the line.
*   MM: RM deserves commendation for trying to thread the needle here.
*   JG: Definitely, thank you.
*   JG: Two open PRs on the goals section
*   MM: Apple thinks WGSL should first focus on the FAQ and once that’s done we can deal with the goals.
*   DN: Sounds good to me. 
*   DS: Can we just close these and we’ll re-open a new one when we get there?
*   MM: Sounds good
*   RM: Fine with that
*   JG: Can you do that DS?
*   DS: Yes.
*   JG: Other PR from Oguz is #954. It sounded more complicated and we had bigger fish to fry so this one has stalled due to prioritization.
*   MM: One open question, asked DN what GPU
*   DN: Quatro P1000
*   DN: Would be OK with what JG said. Not an MVP thing, it’s a bunch of work and should come after first release.
*   MM: Probably fair.
*   JG: Can set the post MVP milestone. Don’t want to say we aren’t going to work on it, but we have more pressing work. In the category of things that if it seemed likely there were important questions answers we could timebox time to make real work forward, but don’t want to commit to a 30 minute discussion
*   MM: Added benefit the issues we have are the same as other industry users so it may just get solved for us.
*   JG: That’s my favorite solution
*   MOD: Would appreciate if people who wanted more sections comment on the PR with their concerns so that we can keep working towards addressing those.
*   JG: Please feel free to collaborate on it as interest and time allows. For the purposes of fixed time meetings, not a priority.
*   


# **Open PRs**


# Change `offset` to `span`. ([#1339](https://github.com/gpuweb/gpuweb/pull/1339))



*   DS: Was discussed at the last meeting, this is the PR to implement. Offset has spooky action at a distance.
*   JG: Was what we discussed
*   MM: Is span optional? \
DS:In the case where the memory span is different then the type size
*   MM: If we accept this, are all types aligned on their natural boundaries?
*   DS: Not discussed yet
*   DN: We’ve landed a change which satisfies what MM is asking about. There was a translation from offset to span based thing that preserves alignment. So, yes, because those are the rules we already had.
*   MM: Right, I think answers my next question, write vec3&lt;f32> would require a span so the span would be 16. Could i write span(12).
*   DN: Good question, I think what that means is the next field is placed on the 12th byte afterwards which breaks the rule. So, would not work.
*   JG: So not at this time, it sounds like
*   MM: I see, so I think since we have one layout authors can’t move padding bytes around.
*   JG: Right, if you want padding stuff that isnt’ required by the alignments then you need to create placeholder variables.
*   MM: I wonder if this is the strategy if this would be better represented by the type system. Rather then saying span(16) vec3&lt;f32> we have a type which means that.
*   DN: Let me backtrack, I think it should be possible to have span(32) on a vec3 which would fill the gap
*   DS: That’s not allowed by the current spec
*   MM: Would like that better
*   DS: We’d need to be careful as HLSL has certain sizes you could have
*   DN: Taken account of by the other alignment size rules
*   MM: So you can have what you want as long as the compiler can inject the right sizes
*   JG: DOn’t like that as much
*   DS: What’s the benefit?
*   DN: Don’t have the noise if you have padding you don’t have to enter it
*   JG: It isn't’ noise, if i have a vec4 and skip the 20 bytes after due to other stuff. Seems overflexible to me
*   KN: My opinion is it isn’t strictly necessary right now and I don’t think span is the most natural way to do that. I think offset in addition to span is the natural way to do this.
*   MM: I agree with that, wrote a comment later. If the author wants to say it’s a specific location we should be receptive.
*   JG: To move this forward, is the strict version palatable?
*   MM: Yes
*   DN: Yes
*   KN: Yes. One question, MM mentioned alignment but so the size of vec2 is tightly packed, so does a vec4 have to align to 16 bytes. If you have vec2 vec4 do you have to span the vec2? I’d say yes probably as in that case the member is taking up more space. Also, that makes the requirements context dependant so the info has to be part of the struct member.
*   DS: That’s not in the PR, KN can you add a comment so it gets in before landing?
*   KN: Yes
*   MM: I think it’s important for webgpu and WGSL that types be on their natural alignment boundaries. Because, when building in JS probably have a uint8 type’d array. If you want to write a float one of the JS ways is to make a f32 array with the same backing but only works if floats are aligned on float boundaries. So, for our purpose it’s important we have natural alignments
*   JG: Also a requirement for easy implementation.
*   KN: I’m pretty sure STD140 does that
*   DN: STD140 does, vulkan i think only requires vec component size alignment. But, happy to not have that as it’s conceptually simpler to have the alignment be a multiple of the vec size.
*   JG: One of the nice things about span is that it was idempotent from the surrounding context, so could say span 16 vec3 and inject where you want without shader context. With our handling for vec4 &lt;space> int if we allow that in span then span isn’t part of context,it’s dependent on variable which follows it.
*   DN: It’s limited to the one field, as opposed to all fields. Major win over offset.
*   JG: Not as convinced anymore. I think that’s a benefit but having ctx depend on what follows is weirder. Opposite direction i”d want but don’t have a good solution. Most elegant solution would be to say span is just ot make sure you know how big something is and you can only have the min size it could be a span of and if you want to skip, have a placeholder variable. Would recommend that in code I write. If we want to open it up I’d recommend against it but could be convinced.
*   MM: I think it’s valuable but not valuable enough to stall the proposal. The reason I like this, is the argument i’ve made before, if we’re going to force typing there shouldn’t be one right answer, there should be flexibility. If there is only one right answer, we shouldn’t force the author to type.
*   JG: See point, but don’t necessarily agree.
*   DM: Can we clarify, it was aligned we should have the vecs aligned the whole vec size. If I have [float, vec] then I have to span the float? Is it required?
*   DN: Either that or you need 3 placeholder scalar fields.
*   DM: I see hte interpretation of the PR now.
*   MM: Sounds like we should accept and 3 followup issues.
    *   1. [float, vec] does the float need span
    *   2. Does the compiler inject anonymous variables if the stride is multiples
    *   3. If you have vec4 &lt;gap> float can you say the vec4 is of span 20
    *   4. When do the rules about packing a float after a vec3 come into play. Are there cases you can have a span 12 vec3 followed by a float?
        *   DN: Answered by tables in the spec, could guess but not sure.
        *   (Answered below)
    *   5. Can we make these more obvious by using the type system. If you want a vec3 of size 16 is that an rgbx type?
*   MM: Would like to clarify, don’t think there should be one right answer, but think there should be a few right answers. One is too few, but 5 is probably ok. Infinity is OK.
*   JG: Sounds like we accept this PR and then will talk about the followup issues we’ve identified
*   DN: Looked it up, to answer KN the field after a vec3 has to has to be the allocation extent of the vec3 afterwards, and a vec3 has allocation extent 16 not 12. So, no tucking in the end.
*   MM: For using the type system, in Metal there are packed types. If you use a float3 it’s 16 bytes big. There is another type called packed_float3 which is 12 bytes big. That’s how metal gets around it, by having different types.
*   JG: Interesting tyes
*   KN: Metal is always scalar layout, so no weird layout rules.
*   DM: In light of span on float we discussed, type system wouldn't’ work well. Don’t want a float type that takes 16 bytes and 12 bytes and 8 bytes.
*   MM: One other thing, don’t necessarily think WGSL should take the MSL solution but do want to mention the packed types are things the metal team thinks is important as folks are trying to make their structs as small as possible. Tucking the float after a vec3 is important, don’t need it now but good to have a story.
*   DN: That’s why I want the span to take that case into account as in the future I could see an option to allow it tucked and not require the span. I agree we need this that we need that in the future
*   DS: span makes it obvious that’s what happening as well.
*   JG: Makse cloud shoes more obvious
*   BC: If you now you want your vec3 to have an x forth component you use span to say it’s bigger.
*   JG: 2 things, implementation overhead and things that aren’t necessarily supported by underlying apis. Tolerable but worth nothing. Other thing, one of the desires and reasons to require these is when it’s something that’s surprising. For instance, whats the alignment of mat2x2 of floats. On some APIs the alignment is 32 bytes but we spec’d it as 16. Worth considering, if we’re doing the mat2x2 transform, how bad would it be if everything was inline and we just did magic
*   BC: Simplest rule is everything is tightly packed. Many cases were people use vec3’s you typically expand to a float and it’s explicit
*   DS: Can’t just move things around because has to match JS data
*   JG: But we just polyfill everything as tightly packed, but we might have to do a lot of perf overhead to take unaligned mat4’s and extract the values.
*   MM: Want to make the same point, it turns a single load into 5 or 6 loads which is a pretty big perf cliff that’s invisible.
*   BC: In case of vec3, float if you say vec3 has alignment of a single scalar then it’s permitted. If you say mat4 is always 16 byte aligned then you don’t have that issue.
*   KN: Going to ask a stupid question, why do we allow vec3 at all?
*   JG: I like that question?
*   MM: Didn’t we have an example of how mat2 has this issue?
*   DS: We spec’d mat2 as tightly packed but mat2x3 and mat3x2?
*   JG: One of those is Ok?
*   KN: If we removed vec3 we’d still have [vec2, vec4] case. But, vec3 feels like it doesn’t mean anything.
*   BC: If the rule is you tightly pack and larger types must be aligned then it forces the smaller type to be aligned to align the larger ones
*   KN: Closer to comment I made on the issue. Types have natural alignments. What if we added an align annotation that you had to specify anytime the natural alignment is violated. That way, autogen code with a list of fields where you don’t know the fields that will show up you always put in teh alignment. The alignment annotation puts in teh padding for you. The rule is you require align anytime it would insert padding but it would be allowed anytime. Then you’d not use span for alignment. No a matter of adding the spans to determine the offset. Add span + align to next element. Only slightly more complicated bit of math homework. Only slightly further then struct offsets?
*   MM: In addition or instead of span?
*   KN: In addition.
*   JG: Could supplant if wanted
*   MM: Having both feels like 2 tools to do the same thing. 
*   KN: Not sure if they’re the same, with the thing in mind, span doesn’t inject padding. Like the vec2 vec4 case. Not sure if span is necessary except for vec3. Maybe it does supplant span.
*   JG: Want to wrap this up, so last call for questions
*   MM: If this is either or we shouldn't resolve on one of them.
*   KN: Would like to see what align looks like will try to work it out.
*   MM: One closing comment, what I like most about span is that it’s optional. A counter proposal should be optional as well.
*   KN: Definitely, optional in most cases.
*   DS: So no merge, talk about next week
*   JG: Yes.
*   


# Remove outerProduct ([#1290](https://github.com/gpuweb/gpuweb/pull/1290))



*   JG: Think this is what we agreed to in 1286. Anyone attached too it? Wanted to ask DN? \
DN: In my spirv-reader haven’t implemented and no-one has asked for it.
*   JG: approved, merged
*   KN: Never once seen it used.
*   
*   
*   


# abs only works on signed integer scalar or vector ([#1282](https://github.com/gpuweb/gpuweb/pull/1282))



*   
*   
*   
*   


# Bools are IO-shareable, but only for builtins ([#1246](https://github.com/gpuweb/gpuweb/pull/1246))



*   
*   
*   


# Discussion about Mat2x2 cl (1341)



*   MM: Don’t like how STD140 shows up in the spec
*   DN: We define what’s in there
*   MM: Could we call it something else?
*   JG: Prefer not too, for folks who are familiar they will know about it, for those that aren’t it’s in the spec and they can look at it. As long as we present it, it’s fine. If we call it something else. I’d prefer, at the least, we continue to say it’s basically STD140.
*   DN: The only 3 times STD140 appear are in a comparison to OpenGL paragraph. We’re saying if you already know this in an intent, non-normaltive section.
*   MM: Does it say it’s non-normative?
*   DN: Yes.
*   JG: If DM is happy, I’m happy. If no other review comments we can probably jsut merge?
*   MM: Yup, merge.
*   JG: Merge it.
*   


# **Issues**


# Removing `ptr` and adding `ref` ([#1301](https://github.com/gpuweb/gpuweb/issues/1334))



*   
*   
*   


# arrayLength acts on pointer-to-runtime-sized-array ([#1330](https://github.com/gpuweb/gpuweb/pull/1330))



*   
*   
*   
*   


# WGSL: add sample_index, sample_mask_in, sample_mask_out ([#1318](https://github.com/gpuweb/gpuweb/pull/1318))



*   DS: SampleMask, if we have one then we can associate based on the storage class.
*   DM: Having separate builtins would allow making the storage class omittable, but if you allow this, then we close that door. So, maybe worth discussion if it makes sense.
*   DN: So that’s an argument for keeping the separate names.
*   DM: Right. That’s what the PR does.
*   DS: Makes sense to me
*   JG: Probably sounds good, but we should revisit.
*   MM: Right now they’re same?
*   DS: No, we don’t have one at all right now.
*   DN: Right, sample mask in is fragmenter stuff and out is killing off stuff coming out.
*   MM: 3rd proposal, have one inout var without storage.
*   JG: Related, if you have in/out thing and we determine the sample masked based on the type of the thing?
*   DM: So we’d have to detect if the value changes?
*   MM: Yea at the beginning of the shader you’d populate it and at the end you’d write and you’d have to do that for all the exit points which kinda sucks.
*   JG: Easiest way is that in the preamble when you have this is copy the in builtin to the out builtin and every time you read or write into the builtin you have it operate on the out builtin.
*   DN: Very wary after looking at metal semantics. Alpha to coverage only kicks in when you write it. Doing things in the shader breaks other possibilities.
*   MM: Are there other native APIs without distinct names?
*   DN: I think SPIR-V has a builtin enum distinguished by storage class
*   MM: GLSL has distinct names.
*   DS: To move forward, sounds like the _in and _out variants are the way to go
*   JG: SOunds like move forward with _in and _out and before committing to backwards compat we need to decide if we want inout.
*   MM: Sounds good, distinct names for now.
*   JG: WIth that, PR good? I think yes.
*   MM: Merge it.
*   JG: Approved.
*   DN: Will fixup merge conflicts and get it merged.
*   
*   


# Consider using multiple scalars to construct matrix. ([#1342](https://github.com/gpuweb/gpuweb/issues/1342))



*   
*   
*   
*   


# arrayLength should operate on pointer-to-runtime-sized-array instead of runtime-array ([#1329](https://github.com/gpuweb/gpuweb/issues/1329))



*   
*   
*   
*   


# Support compile-time-constants (constexpr) ([#1272](https://github.com/gpuweb/gpuweb/issues/1272))



*   
*   
*   
*   


# update IO-shareable to include bool ([#1244](https://github.com/gpuweb/gpuweb/issues/1244))



*   
*   
*   
*   


# texture builtins: For most texture builtins, Vulkan only allows compile-time-constant offset, and with with bounded range ([#1235](https://github.com/gpuweb/gpuweb/issues/1235))



*   
*   
*   
*   


# Make all textureLoad functions on sampled textures require the level ([#1301](https://github.com/gpuweb/gpuweb/pull/1301))



*   
*   
*   
*   


# Proposal to enhance defining input/output variables. ([#1155](https://github.com/gpuweb/gpuweb/issues/1155))



*   
*   
*   
*   


# Add workgroup size and ID builtins ([#1295](https://github.com/gpuweb/gpuweb/pull/1295))



*   
*   
*   
*   


# spell out when an implementation must reject a shader module ([#1241](https://github.com/gpuweb/gpuweb/issues/1241))



*   
*   
*   
*   


# conflict between mat2x2 layout in uniform buffer and storage buffer ([#1258](https://github.com/gpuweb/gpuweb/issues/1258))



*   
*   
*   
*   

Next Meeting Agenda:



*   WGSL Prioritization?
    *   MM: Probably not ready to talk about next week but the week after?
    *   JG: So, come up with lists, but not talk about it yet. Will clone the API document on prioritization and send out to see if it works.
*   Ref vs ptr #1334 
*   FAQ
*   WGSL FAQ ([#687](https://github.com/gpuweb/gpuweb/pull/687))
*   Depth compare / lod 1320
*   arrayLength
*   abs only works on signed integer scalar or vector ([#1282](https://github.com/gpuweb/gpuweb/pull/1282))
*   Bool as io shareable.
*   [https://github.com/gpuweb/gpuweb/pull/705](https://github.com/gpuweb/gpuweb/pull/705) 