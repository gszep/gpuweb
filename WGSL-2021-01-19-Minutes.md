# **WGSL 2021-01-19**

**🪑 Chair:** Jeff Gilbert

**⌨️ Scribe:** Dan Sinclair

**🗺 Location:** [meet.google.com](http://meet.google.com)

**⌚ Time:** 11am PST

**Specification:** [https://webgpu.dev/wgsl](https://webgpu.dev/wgsl)

**Meeting Issues:** [Marked Issues](https://github.com/orgs/gpuweb/projects/2#column-8898490)

**Open Issues:** [WGSL Issues](https://github.com/gpuweb/gpuweb/issues?q=is%3Aissue+is%3Aopen+label%3Awgsl)

Note: These are the minutes taken in real-time. The official minutes can be found on [the WebGPU wiki](https://github.com/gpuweb/gpuweb/wiki).

If you didn't receive a [meet.google.com](meet.google.com) invitation and plan on participating, please send Dan Sinclair a Google Apps enabled address and he'll add you.



---



# **📋 Attendance**

WIP, the list of all the people invited to the meeting. **In bold, the people that have been seen in the meeting:**



*   Apple
    *   **Dean Jackson**
    *   Fil Pizlo
    *   **Myles C. Maxfield**
    *   **Robin Morisset**
*   Google
    *   Corentin Wallez
    *   **Dan Sinclair**
    *   **David Neto**
    *   Kai Ninomiya
    *   James Darpinian
    *   Brandon Jones
    *   Ryan Harrison
    *   Sarah Mashayekhi
    *   **Ben Clayton**
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   **Greg Roth**
    *   Michael Dougherty
    *   **Rafael Cintron**
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



# **⚖️ Agenda/Minutes**


## Meta


### WGSL subgroup chair



*   JG: Dean has exciting things, asked JG to take over chair of the WGSL meetings moving forward and they have agreed. If you have concerns, please reach out to JG, DJ or CW.
*   DJ: Thanks Jeff
*   DN: Sounds good to me, any procedure stuff to do?
*   JG: Don’t think so?
*   MM: Editors may have some official powers, if interested could pursue that.
*   JG: DM is an editor on the Mozilla side already. If useful we can figure it out later, but applies to everyone here.
*   


### Prioritization and path to MVP/v0



*   Add your blockers to prioritization spreadsheet: [https://docs.google.com/spreadsheets/d/19jeGI3uLwVQryUmW12rxaWJe4FTbFBwVi-uEGdwhLQ8/edit](https://docs.google.com/spreadsheets/d/19jeGI3uLwVQryUmW12rxaWJe4FTbFBwVi-uEGdwhLQ8/edit)
*   JG: Been constrained and want to keep an eye to prioritization and focus discussions. CW made this doc with a bunch of WGSL stuff in it. Plan is to reference that doc when deciding what to discuss and what to focus/work on. There are a bunch of new additions, which is great. If you have a chance, go back through and give org estimate. Especially looking for P0 items which need to be discussed ASAP.
*   MM: Apple has added our prioritization for API but not for WGSL yet. Still in the process of making.
*   JG: Collaborate on the doc and we’ll use it to focus our efforts and time. Any comments or ideas ….
*   

Meeting time



*   JG: Crushed on time, some desires to [missing]
*   DM: We haven’t been reaching the end of the agenda for a while and lots of basic things to resolve like how we pass builtins and how we do inferred types. Need to either meet more often or longer. I think that would be productive.
*   DN: For example, an agenda of 15 items but not everyone was prepared. Lots of learning on the spot which takes time. Rather than more meeting time, better to have shorter/more focused agenda. 6 things we get through 4 rather than 15 we get through parts of. This is better as timeboxing knocks off lots of things. Would prefer suggestions at end of meeting on what to focus on and focus more. Otherwise we’ll just fill up more meeting time.
*   MM: Agree.
*   JG: More valuable to flip timebox and discussion and bound discussion? Or should we have more meetings?
*   DN: Prefer timeboxed first so we have momentum and accomplishments. Hard to cut off discussion.
*   MM: Agree with that.
*   GR: Been in a lot of groups and is a common refrain and never works out. Will always have people learning on the fly.
*   DN: Degrees matter and agenda control matters
*   MM: Prioritization will help. Not getting through the agenda is not a signal of failure, many groups don’t get through the agenda.
*   DM: So, the idea is by strict time boxing we incentivize members to do more work ahead of time.
*   JG: Timebox is stemming the bleeding. Feeling of what can we do to push this along in the call instead of getting bogged down.
*   KN: Think the most important is keeping the total size of the agenda down so it isn’t daunting to prepare. Reserve time before the API meetings to look at the agenda and remember the topic for each one. The WGSL is big and makes hard
*   JG: Will cut down the number of timebox items on the top to keep it manageable and give folks time to prepare. Main goal is to prepare for the discussions and timebox is what we get through. Will make discussion topics more prominent so folks know what to prepare for. If nothing else, prepare for discussion topics. If we finish the timebox early, we start discussion early.
*   MM: DM mentioned not getting to some items you’d like to discuss. Prioritization would be a tool to handle that. Would the spreadsheet work or another tool?
*   DM: Don’t know if there is another tool
*   JG: Trying to keep focused on where we do need priority based on feedback and the document. That’s also why the ondeck section, we won’t get to them, but are the things we’re looking forward to discussing in future meetings. Things we know are priorities and we will be talking about them. Will try to not have too much discussion on things which are less of a priority.
*   DM: Example in mind was 1315 which was from the middle of Dec “for next meeting”. It’s now the middle of January but we haven’t talked about it and no homework. At least hoping if we touch on it we’ll make a decision if it’s a path or not.
*   JG: So, we have 2 systems, the milestones and the labels. Didn’t see this because it wasn’t labeled. Was following a link in the top of the doc.
*   DM: Thought we dropped the label a while ago.
*   JG: Will remove the label.
*   JG: Will add 1315 to next week's agenda.



---



## Timebox


### abs only works on signed integer scalar or vector ([#1282](https://github.com/gpuweb/gpuweb/pull/1282))



*   JG: abs() only works on signed int scalar or vector. Can decide either way, people have weak opinions. Mini investigation from DM.
*   GR: HLSL does not require signed, abs can take pretty much anything. Weak preference regardless. There is a fabs isn't’ there?
*   DN: There will be.
*   MM: Can we make it an overload, the f is because c doesn't overload.
*   GR: Good idea.
*   JG: If users want to be strict they can write their own code which is only specified for signed and get the same static guarantees. Flipside is maybe it’s desirable we’re clear when an op is needed and don’t want to do no-ops. Generally appetite we give the uint back if you pass a uint.
*   DN: PR out for a while, can MM file an issue for float issue
*   JG: PR as written, accepts unsigned?
*   DN: Thumbs up. 
*   JG: Any objections to landing … Merging.
*   DN: Just check, abs float exists for scalar and vector.
*   
*   


### Bools are IO-shareable, but only for builtins ([#1246](https://github.com/gpuweb/gpuweb/pull/1246))



*   update IO-shareable to include bool ([#1244](https://github.com/gpuweb/gpuweb/issues/1244))
*   DN: There is a front facing builtin for frag shader which we need to allow as bool. The Vulkan spec requires non-builtins for i/o never be boolean. This was to allow the case for front-facing but not for user spec’d. Could just do 0 is false, but would require spec’ing bit patterns on the boundaries which we haven’t done and I don’t think we should do. 
*   MM: +1 to the general idea. Don’t understand what the relation between builtin variables and io-sharable in Vulkan.
*   DN: Builtin variables for frag shader are input variables, (in input storage class) so want to say front facing is  bool variable but no other variables will be so
*   MM: Is that true in WGSL also?
*   DN: Yes. The front facing is expressed as a builtin attribute on an input variable.
*   KN: Agree with MM’s thinking. We have concept of io-sharable but we don’t care about memory layouts in cases like this. Doesn’t io-shareable imply memory layout which isn't’ relevant?
*   DN: Io-shareable is things which come in as input or go out as outputs
*   KN: Does it imply a concrete layout?
*   DN: Input assembly stage has to produce IO-shareable values.
*   MM: Maybe editorial comment, data flowing through the rasterizers is a conceptual different thing then the variables. Vert out, frag in. The fact we use the same term for both of these is confusing and a little misleading
*   DN: What do you mean by the same term? IO-shareable?
*   MM: Yea. Feel like we should use different terms for these two different things.
*   DN: Hearing, IO-shareable is a bad name, suggestions?
*   MM: Happy to take to a bug, not talking about naming but the fact that one name applies to both data flowing out of vert and into frag and also to builtin variables.
*   JG: I think they’re in the same category because under the hood they are. We can choose to talk about them differently to users and it’s a detail for us
*   KN: Don’t see the issue with io-shareable for both. In both cases a piece of data coming from somewhere regardless if it’s from vert into frag shader. Or, not from vert shader, from the interpolator.
*   JG: We have more to think about for naming and categorization but we can take this PR as-is. Do we think we can land now, or should we do another round of review?
*   DM: Haven’t heard answer to question about allowing bools in varying. If they aren’t host shareable and it’s trivial to polyfill on Vulkan.
*   MM: If you have a triangle where one of the verts outputs true/false
*   DM: Different problem, that’s interpolation. If verts have different values you have to interpolate.
*   DN: So, vulkan should polyfill into an integer?
*   DM: Not pipeline specific, at shader module creation convert to an int
*   DN: Signedness?
*   DM: Up to you.
*   JG: Move on for now….
*   
*   


### Consider using multiple scalars to construct matrix. ([#1342](https://github.com/gpuweb/gpuweb/issues/1342))



*   JG: Do we think this is a good idea? So mat2x2(vec2&lt;f32>(2., 2.)  ..) or mat2x2(2., 2….). General idea is still to be column major layout. Any concerns here? Sounds like we just need a PR.
*   DM: Minor concern that if you aren’t using host shareable matrix then you don’t care about which majorness. Specifying inline can be confusing.
*   MM: Constructor taking vectors also has a majorness so already there.
*   GR: Don’t think it’s more confusing with scalars [than vectors].
*   JG: Resolve, needs spec.
*    
*   


### [Scalar -> Bool casts · Issue #753 · gpuweb/gpuweb · GitHub](https://github.com/gpuweb/gpuweb/issues/753) 



*   MM: We should have them
*   DN: Consensus for scalar discussion about vector.
*   MM: Vector ones are bad as unclear what you want all or any. Not seen in other shading languages either.
*   JG: Not any/all builtins?
*   DN: Are, but take vec of bools
*   JG: So, narrowing of bool vectors to bools but not non-bool. Two part cast?
*   MM: Nest them, easy
*   DN: in there is vec&lt;bool>(vec&lt;T>), would like that discussed. Split out to separate issue.
*   MM: Will move into a new issue.
*   JG: Resolved needs spec.
*    
*   


### WGSL: add sample_index, sample_mask_in, sample_mask_out ([#1318](https://github.com/gpuweb/gpuweb/pull/1318))



*   _DN: That was merged last week._
*    
*   


### wsgl: Add rule that offsets must be compile time constant ([#1238](https://github.com/gpuweb/gpuweb/pull/1238))



*   texture builtins: For most texture builtins, Vulkan only allows compile-time-constant offset, and with with bounded range ([#1235](https://github.com/gpuweb/gpuweb/issues/1235))
*   Also related (discuss later): [wgsl] Support compile-time-constants (constexpr) [#1272](https://github.com/gpuweb/gpuweb/issues/1272)
*   
*   JG: I think there is consensus for this.
*   BC: Further discussion about constexpr which is in the doc for later. This just classifies they must be constant which is good to land as stage 1.
*   JG: Any objections to landing this? Which just means constexpr is the parse time construct so you can use vector constructors to make a vec literal.
*   DN: Thumbs up
*   MM: +1 to a single definition of constexpr.
*   JG: Ready to merge if approved?
*   BC: May have collisions.
*   JG: Merging …..
*   


### Make all textureLoad functions on sampled textures require the level ([#1301](https://github.com/gpuweb/gpuweb/pull/1301))



*   JG: Texture loads have no implicitness, always explicit which is different from the sample compare. Question is do we require …
*   DM: LOD level
*   JG: If we made the level optional and we don’t specify it we assume it’s 0 and assume the 0 level, or just have the user write `,0`.
*   DM: Having more overloads for 3 symbols is a waste
*   MM: Sampling is different from loading, for loading should just make author list all params
*   BC: First objection is certain formats don’t have MIP levels, having to give a mip level is weird (like 1d textures). If we’re always ensuring these are clamped should be ok
*   GR: Why not 1d mip level?
*   BC: Not in spec. If we anticipate this in the future, good to have the param there. If always expect a level and use robustness to fit into bounds so special case falls away
*   MM: Aren’t 1d textures a distinct type. So, if they don’t take mip levels we can make the overloads. If the type doesn’t have mip levels we can make the overload for that type not take the level
*   BC: That’s what’s in the spec. Those that don't’ have one, don’t take one. Ones that do have mip levels have 2 overloads, one for with and one for without. PR is to unify and just have one path.
*   MM: So, idea is if 1d gets mip level we don’t want to change the signature?
*   BC: Taking this further, asking to reconsider if we can provide the level suffix for things that take a level as we may want another integer in that spot. More verbose and tedious as these methods are getting longer. Paranoid about future compatibility with overloads.
*   JG: Would prefer to just always require. Then if you don’t have a level say 0. Line between “mips not supported” and “i don’t have mipmaps” is thin. Treat it as it doesn’t have a miplap then use level 0. Fixes signature where we don't’ have level as there is always level.
*   MM: Is it likely we’ll support 1d with levels?
*   JG: Not super likely. Like the symmetry where you always have it so you don’t have to remember 1d doesn’t have the overload. The coords are different as they take a scalar instead of a vec2. That’s my pref. Don't’ expect folks to want mipmaps for 1d textures.
*   MM: Is this something vulkan doesn’t support? And if so, is there an extension?
*   DN: Don’t know.
*   KN: According to comments MacOS doesn’t support mip’d 1d textures. Trying to look it up but haven’t found a resource on it. DOn’t know if that restriction is lifted in metal.
*   JG: Sounds like more discussion?
*   MM: We should resolve, forward progress is more important than issues.
*   BC: My concerns are gone. Would like level included in name but not a blocker.
*   JG: Ok.
*   MM: To understand, you’re imagining parallel functions where some say level and some don't?
*   BC: Happy with providing level
*   MM: YOu want to add a word?
*   BC: Kind of, provides symmetry between words and param order. Nitpicky so dont’ care too much.
*   JG: Think we should move forward.
*    
*   


### [does textureSampleCompare assume level-of-detail 0 in non-fragment shaders? · Issue #1319 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1319) 



*   
*    
*   


### spell out when an implementation must reject a shader module ([#1241](https://github.com/gpuweb/gpuweb/issues/1241))



*   
*    
*   


### conflict between mat2x2 layout in uniform buffer and storage buffer ([#1258](https://github.com/gpuweb/gpuweb/issues/1258))



*   
*    
*   


### Add workgroup size and ID builtins ([#1295](https://github.com/gpuweb/gpuweb/pull/1295))



*   
*    
*   



---



## Discuss


### Change `offset` to `span`. ([#1339](https://github.com/gpuweb/gpuweb/pull/1339))



*   Replace [[offset]] with [[span]]? ([#1303](https://github.com/gpuweb/gpuweb/issues/1303))
*   Related: Remove offset ([#1349](https://github.com/gpuweb/gpuweb/issues/1349))
*    
*   KN: Intended to try having a new proposal and spent some time on it but wasn’t able to reach a point I was happy and could send it out. Discussed internally but didn't come to a good conclusion. Happy to continue discussing this.
*   JG: More to discuss? Or talk through things.
*   DS: I made follow on (1349):  remove offset. Specify exact memory size.  Question still around storage buffer vs. uniform buffer.  Much more like C layout. And rely on tooling to reflect the data out.
*   MM: This is what we wanted.
*   KN: We have discussed this before, don’t think it’s the right direction. Don’t think it’s possible to specify a single layout. Have to satisfy uniform buffer rules.  Std140 is just not good enough for other purposes.
*   KN: It’s impossibly complicated.
*   DN: This stuff always trips people up which is why it's a benefit to have it explicit. Does come with costs. Happy to keep on iterating.
*   BC: You say 140 isn't’ good enough, but it works. Would get us to MVP even though it isn't’ optimally packed.
*   KN: Not opposed to going MVP with 140, but needs to be explicitly specified. Shouldn’t say all structs are 140 and then having some way to say that. You put 140 on the struct and you know it’s 140 and then you put something else on later.
*   MM: This is exactly the opposite of what I wanted last week where the user has at least one degree of freedom.
*   JG: Don’t agree as it’s forward looking
*   KN: No freedom now, but we will add it in the future so concern doesn’t apply here.
*   JG: Good to think about, don’t want to force useless code. Forcing devs to give offsets and spans, even if no freedom is them demonstrating they’re making the right choice. Confirmation from the developer that they’re picking the right degree of freedom instead of what they assume the freedom is.
*   KN: Analogy, many languages allow type inference all the time but type annotations make the code easier to read.
*   MM: Agree. Having the annotations be optional is a thumbs up. Argued for that in one of the comments, in the span said we should allow the optional thing in case the author wants to tell us.
*   KN: Only works if we have a default layout. No opposed to a default if we come up with something satisfactory.
*   BC: Like a default layout and being able to customize as we see fit later for packing and padding.
*   MM: Think we could have a default and could allow switching the default on a per struct or per shader level and that doesn’t conflict with allowing authors to tweak size and alignment of members. Going with default doesn’t constrain the future.
*   DN: Discussed with DS offline where something like 430 is default but allowing [[packed]] but regardless of what you think it’s byte by byte. Simple so you can be in full control. Don’t want a proliferation of these.
*   MM: Is that implementable on vulkan?
*   DS: That's’ what 1349 says, but 140 not 430
*   DN: Don’t think that’s a good idea.
*   MM: What’s the next step …. 
*   JG: Mental exercise, need to coalesce and cross off ideas. 
*   KN: Useful to have folks continue to think about these ideas. E.g. shipping MVP with only std140 - needs percolation.
*   DN: Have an idea that evolves 1349 and maybe flushes out packed.
*   


### Removing `ptr` and adding `ref` ([#1344](https://github.com/gpuweb/gpuweb/issues/1334))



*   DS: Confusion of what pointer means in WGSL.  Also confusion of ‘var’. Want to use ‘ref’ instead of ptr.  Idea: use ‘ref’ instead of ‘var’.  Some opposition due to inconsistency.
*    MM: Thought this was in addition to var and const. There’s now a third one.  Thought you said “ref” was an operator on an l-value to produce reference to  
*   MM: Does that make all function params pass by values
*   DN: No, it’s ptrs changed to type.
*   DN: A variable is a pointer to storage. So, a variable is actually  a pointer to its base.
*   DM: This isn’t obvious
*   DN: I know. 
*   DM: That’s what SPIR-V does, that isn't’ what WGSL says
*   DN: No, WGSL says a var is a ref to storage
*   DM: But you use it as a var like any other programming languages
*   DN: We haven’t written down those rules yet. We get to write down the rules. This is a model where there is no consistent (missed).
*   DM: At call site you don’t know if it’s ref or value?
*   DN: Function signature tells you
*   DM: Right, at call site you don’t know
*   DN: At call site you have the function prototype already and the prototype tells you where to go
*   DM: This is the problem of c++ refs. You don’t know at the call site what it’s going to do even though you can look up the function. Disappoints me we have const, var, ref and ptr in future and why do we need 4 things when we can get by with const and ptr which replaces everything else.
*   DN: That was the original proposal. Maybe we’re coming too early as we don’t have enough spec written out. Tried to avoid no address-of and no conversion to l-value. End up with a name which is the value or pointer to the value.
*   JG: Thought, don't’ know if it should be a goal to not have address-of. If you want a pointer to the variable use &var and you get a pointer and that’s the type that comes out. It’s obvious what you’re doing and it’s obvious it’s different. Can then take full advantage of type inference and you take the pointer to it and you don’t type ptr. Also a thing that at least in our style guide is to desire passing by ptr if you expect to get it back. The reason is so you know at the call site it’s different then passing by ref or copy.
*   MM: This proposal params to functions would take ref keyword or not, would those instead take const or var?
*   DN: It’s always const. We agreed before it’s a const value. You don't say const
*   MM: So nothing or ref and nothing means const.
*   DN: Yes.
*   MM: Familiarity argument in favor of leaving how it is because c++. Our rules match c++ refs and that’s valuable.
*   DM: Our rules? We don’t have refs now?
*   MM: What we call ptr holds the same rules as c++ refs
*   JG: Thing I don’t love is it implicitly coerced to a ptr from a var. Would be nice if it was an explicit conversion op
*   MM: Not for refs
*   JG: True, but don’t like that.
*   MM: Familiarity is an argument as we match c++ refs.
*   DN: Hearing that address-of is advantageous for readability.
*   GR: One of the things I hate most about c++. Where do we land on having something else also called ptr and how our approach impacts?
*   JG: Still exploring
*   GR: Crucial in our decision. Follow familiarity but not c++
*   MM: In a world of ptr -> ptr, it’s ref -> ref. It collapses down. All predicated on can’t ref + 4. In that world, all refs collapse down as in c++.
*   GR: Would refrain from saying never.
*   JG: Good point to hit pause.
*   MM: Would be interested in that world
*   GR: No details, just saying never is a dangerous world
*   JG: There are some vulkan extensions for variable pointers where you have a pointer into something bounded by the size of the storage buffer.


### arrayLength should operate on pointer-to-runtime-sized-array instead of runtime-array ([#1329](https://github.com/gpuweb/gpuweb/issues/1329))



*   arrayLength acts on pointer-to-runtime-sized-array ([#1330](https://github.com/gpuweb/gpuweb/pull/1330))
*   
*    
*   
*   



---



## On Deck


### Proposal to enhance defining input/output variables. ([#1155](https://github.com/gpuweb/gpuweb/issues/1155))



*   
*    
*   


### Support compile-time-constants (constexpr) ([#1272](https://github.com/gpuweb/gpuweb/issues/1272))



*   
*    
*   


# 

---



# 📆 Next Meeting Agenda



*   Meet 2021-01-26
*   #1315
*   Unresolved timeboxed bits from above.