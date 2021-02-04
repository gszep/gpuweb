# **WGSL 2021-02-02**

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
    *   **Alan Baker**
*   Intel
    *   Yunchao He
    *   Narifumi Iwamoto
*   Microsoft
    *   Damyan Pepper
    *   Greg Roth
    *   Michael Dougherty
    *   **Rafael Cintron**
    *   **Tex Riddell**
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
*   Mehmet Oguz Derin
*   **Pelle Johnsen**
*   **Timo de Kort**
*   Tyler Larson
*   Eduardo H.P. Souza



---



# **⚖️ Agenda/Minutes**


## Meta


### Office hours call



*   (working on finding a time, JG to email)
*   JG: Will do this week.
*   


### Make “Under Discussion == For Next Meeting”?



*   DM: WGSL workflow issue where after investigation is done, we go into ‘under discussion’. Not clear how to move from there to ‘For Next Meeting’. Main group has one tab. Propose merging them. Figure out what to discuss from this tab based on what’s on top. If you want to discuss, push up the list.
*   JG: Enjoy the separate ‘For Next Meeting’. A little awkward to know when under discussion and when ‘for next meeting’. It’s a judgement call that folks might not be comfortable making. Things we’re still hashing out and making proposals but maybe better captured by a different name. ‘Awaiting proposal’ or something. Something to indicate we can’t make more discussion right now.
*   DM: That’s ‘Needs Investigation’. Goes back to the earlier tab if a blocker is found.
*   JG: Thought that was for the original investigation phase, not for writing a proposal.
*   DN: Could have a broader needs action tab that gets used. ‘For next meeting’ is only useful if the list is fall. Would suggest more aggressively decaying items from ‘for next meeting’ back into under discussion.
*   MM: Is this proposal to merge the two columns to a single column? I’d disagree for the same reason as DN.
*   KN: Issue is under discussion means something different from WebGPU. One column for  essentially ‘needs action’ and one for ‘needs discussion’. If we made WGSL tracker the same as main, most would move from under discussion to main column and some would move into ‘for next meeting’ which has the same sorting order as main list for needs discussion Main project tracker has worked very well, just needs maintenance from time to time.
*   DM: Do what we suggested and move back to the previous tab on call if it needs action and rename the tab.
*   JG: Yea, or TODO. Captures vagueness. A catchall tab for an idea someone had that needs to be looked at. Have things that are in progress that are trending to a conclusion that hopefully someone is working on. There is always the possibility it falls through the cracks and we have to re-discussion. Will take a look and see what we can do.
*   DM: Maybe worth noting this is a real problem. File a lot of issues and don’t know what to do next. Wait for a response? Wait for a call? Don’t want to move all issues into waiting for a call.
*   JG: Instinct would be if the next forward progress is on call, put it in ‘for next meeting’. We’ve had things sitting in that column which we want to talk about but it hasn’t come up in priority.
*   DM: How do you decide if it’s ‘for meeting’ or not? To me, it needs offline or online discussion.
*   JG: Someone has to make that call, when everything is under discussion that call is whoever makes the agenda to go through and decide what’s on call. The ‘for next meeting’ is more collaborative as you can opt into the meeting.
*   DM: Optin by moving into the column in the main group
*   MM: One criteria is urgency. Maybe other criteria.
*   JG: Mostly merging and remaining. For next meeting -> Under discussion and Under discussion -> TODO. Like have more granularity here for where things are in progress For next meeting is things blocked on having folks talk on them and benefit from discussion. ‘Under discussion’ is ‘we’re chatting and throwing around ideas but not ready for meeting’. Judgement call.
*   MM: ‘Please comment’ column? Needs discussion but not for the group?
*   DM: Don’t think anyone would check for it.
*   JG: That’s under discussion. Surfacing we have a lot of things going on and a lot of work to do. Maybe ‘for next meeting’ -> ‘needs meeting’
*   DN: Some environments, initial state Needs Triage. 2nd Needs Action (investigation or proposal). 3rd Needs approval if needs feedback or approval. Could go back to ‘needs action’. 4th Approved. Approved maybe needs PR. Having the 4 states makes it clear what happens next. The buckets you look at for an agenda and if work needs to be done.
    *   Triage
    *   Needs Action
    *   Needs Approval
    *   Approved
*   DM: That’s what the main group does, tirage is implicit. 
*   JG: What I like about what DN said is there is a distinction between needs action and needs approval. Right now both fall under webgpu’s ‘under discussion’ column. 
*   KN: If we need action from a particular person we move back to the first column as it needs revision. Don’t always do that but if something is in discussion for a while we’ll notice and move back.
*   JG: Like this because needs approval refocuses on having a proposal we can talk about. Had requests for a stronger emphasis in the meeting talking about things without a proposal. Like that needs approval emphasises proposals to approve. Inclined to try DNs suggestions to see if they’re better. Closer to what WebGPU uses already and we can bridge the remaining gap.
*   KN: Think it will be close and good to see if we prefer 1 over the other. Small difference.
*   [https://github.com/orgs/gpuweb/projects/2](https://github.com/orgs/gpuweb/projects/2) ⇐ the cards



---



## Timebox


### [Add bool vector coercion #1362](https://github.com/gpuweb/gpuweb/pull/1362)



*   DN: _THIS WAS MERGED_


### [Define how .5 rounds when using the `round` method. · Issue #1381 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1381)



*    JG: General proposal to round even
*   RM: Looks reasonable. See that it requires a different OPCode in Metal, would like some perf testing; don’t expect any kind of issue just want to make sure there are no surprises
*   MM: Can we do that asynchronously.
*   JG: Propose we take the RoundEven functionality and we find later we revisit.
*   RM: Agree with that
*   MM: Metal has multiple rounding with different behaviour?
*   JG: Covered by top comment
*   DS: SPIR-V Round and RoundEvent, MSL rint and round, HLSL round (as round even)
*   MM: Could do it in software, but go with the simple option of RoundEven and can come back later if needed.
*   JG: Works for me. Needs spec.
*    
*   


### [Invariant qualifier #893](https://github.com/gpuweb/gpuweb/issues/893)



*    JG: This fell off the wagon. Had loose consensus. Not sure if MM had time to look.
*   MM: So idea is we have a feature that just doesn’t work sometimes on some hardware
*   JG: very rarely doesn't work and eventually the hardware goes away
*   MM: Seems unsatisfying.
*   DM: Is it hardware getting better or software in MSL case?
*   MM: I believe software
*   DM: And you’re aggressive at updating the software
*   MM: We don't’ ship safari to old iOS versions. We do ship webkit on old macs. I thought this was just an iOS problem, not mac.
*   JG: Think this is a metal issue.
*   KN: It’s a mac issue.
*   MM: Question is if the set of mac versions which have this keyword extend back further than webkit supports. Chrome and firefox may ship on older versions.
*   KN: It’s before 10.15 so not super old.
*   JG: Thought is if you don’t have an invariant commitment from metal, if you follow the invariant rules, and pretend even if not labeled. Suspect we’ll generally get invariant behaviour even on systems which don’t support it. In my mind, far enough down this could be an errata. Occasionally, on some OS’s there isn’t actually a commitment here. If this is just a Chrome and Firefox and Edge thing we just have to make sure they’re all happy to implement with this issue in it.
*   MM: From our point of view, this is great.
*   JG: Great, others?
*   DN: The alternative is you don’t get webgpu
*   MM: There is a middle option which is make it an extension
*   JG: Think that's unsatisfying.
*   DM: MM, don’t you allow shaders to be compiled (... missed …). If you could provide a portable compiler for us to include we could support on old systems. Then we don't’ depend on macos version when building shaders
*   KN: Not tied to SDK, pretty sure it’s tied to the OS.
*   MM: There is a frontend and backend compiler. The backend compiler is a daemon running on the machine. This is a feature both front and backend need to know about. Can ask the metal team but expect it to be impossible.
*   DM: I thought the backend was LLVM and LLVM knows how to do this and the frontend just needs to tell it.
*   MM: Webkit is one of the rare things that ships a new version of framework to old MacOS. Can ask.
*   JG: In the meantime, can we try to put `invariant` into the language and revisit if there are issues? 
*   RC: How to do on non-metal ios?
*   JG: Do your best. Invariant has to follow uniform control flow guarantees?
*   DN: You don’t fiddle with the order of operations and associated operations.
*   JG: Rules we have to follow, we’d be enforcing on the application and I believe if we enforce the rules, then not marking the code as precise, likely to just work. Otherwise, there are whole rendering systems that wouldn’t work. Think Metal decided they need stronger guarantees. So, we’d be falling back on the old way of not obviously doing anything that changes the order of ops. \
MM: I thought HLSL had a keyword?
*   JG: In HLSL yea.
*   TR: We have precise which isn’t the same but at least as good as. So we can be conformant. 
*   MM: GLSL has the same thing so SPIR-V does. Use the keyword and it works. Metal only some versions have the keyword. So Metal is only issue
*   KN: OPen question about fno-fast-math.
*   JG: DM had a question for MM about if f-no-fast-math makes vertex shader invariant.
*    MM: Think the answer is the trick would not work, but don't’ know for certain
*   JG: Agree trick is not guaranteed to work
*   KN: Agree.
*   MM: Resolve approved?
*   JG: Yes resolved needs spec.
*   


### [Remove the requirement to have entry points #1340](https://github.com/gpuweb/gpuweb/pull/1340) 



*    DN: Surprised at this, to have a useful WGSL program you need an entry point. So, no point to not have it.
*   DM: Incomplete rule that doesn’t help anyone
*   MM: We expect JS preprocessors to mangle the strings and if they want to call the function on a string without entry point they should be able to do it
*   DN: No semantics for what it means
*   MM: You can create a shader module but not pipeline
*   KN: Exactly
*   JG: Fail at creating a shader module or pipeline. Similar to if you have something and don’t use it should we forbid it. Sometimes applications end up with 0 lights and don’t want to special case that. Generally expect the rest to work and they don’t ever use it, and as long as they dont’ use it it’s fine even though there are no entry points, So similar to if we allow 0 sized buffers
*   MM: You can have a var and never use it. You can have a func and never use it. If the compiler sees there are no entry points and early outs, that’s fine.
*   DN: Maybe ties into 1241 which is to spell out when you reject a module. Maybe distinction if you can reject at pipeline creation or shader module time.
*   JG: Hoping we could close this out before doing that discussion.
*   DN: Could temper perspective if we were more clear on that. Have in mind there are 2 places we can reject. An f64 rejects at shader module creation as we don’t have that yet.
*   DM: Suppose we add f64 to the language, and .. (missed) … and the device does not have the feature enabled, when do we reject?
*   MM: Alternative approach, extensions are part of the create shader module input and if you want to compile and f64 shader you have to pass that in
*   KN: f64 tied to a device so passed in.
*   DM: But then you can’t use that device as it’s a WGSL rule. This is the interface and the interface should not be defined in this document.
*   KN: If f64 can’t be used on the device then create shader module fails
*   DM: Not what I thought. Number of ways a shader could be used and only use entry points for ways that it’s used.
*    
*   


### [does textureSampleCompare assume level-of-detail 0 in non-fragment shaders? · Issue #1319 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1319) 



*   (We said we would have more concrete proposals)
*    
*    
*   


### [spell out when an implementation must reject a shader module · Issue #1241 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1241) 



*    
*    
*    



---



## Discuss


### [Provide shader inputs as arguments to entry point functions · Issue #1315 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1315) 



*   DM: Don’t think there has been any good argument against and addresses the issue identified.
*   DN: Did a design about the translation and was less complicated than thought. Would generate code DS doesn’t like but it’s correct and readable. Much better than I thought.
    *   [https://dawn.googlesource.com/tint/+/afb7b30f606f9c2cc35bb44c87bff9bec1f28ee9/docs/spirv-reader-input-output-variables.md](https://dawn.googlesource.com/tint/+/afb7b30f606f9c2cc35bb44c87bff9bec1f28ee9/docs/spirv-reader-input-output-variables.md)
*   MM: So for SPIR-V you’d just promote to global variables
*   DM: Yes, in private address space and could see being inverted on the way back out.
*   MM: In src that uses these entry point arguments going to want to pass to helper functions, so helpers would now take extra params. In the spirv code would the helper determine if it’s passed and then skip the param and use the global?
*   DN: Enhancement you don’t need to get too. It’s ok to pass the ptr to inputs and outputs as function pointers and if that doesn’t work do inlining. You can detect code patterns that it’s another variable and short circuit in the helper. Find heuristically but doesn’t always work.
*   MM: Question is, you’re cool with params that get passed around but never used? Oh they are used?
*   DN: Yup. Did analysis for pipeline input and outputs. Resources are not cool as pointer semantics don’t work.
*   DM: Wanted to know if the thing with privates passed into the function is something we have to do in the frontends regardless. Today when calling HLSL you have to have in the arguments. You put that into privates or pass into functions. Do the same quirks as spir-v frontend. But, I don't have to do it for the HLSL backend anymore. So justifying if we do this for SPIR-V frontend, but not HLSL backend but shaders are nicer to write we should do the former.
*   DN: Don’t think anyone is arguing against this.
*   MM: Downside is asymmetry between resources and in/out. Benefits of parameters outweighs the asymmetry.
*   DM: Do we follow up by removing input/output storage classes?
*   DN: Let’s have a draft of the spec change and see where it goes?
*   DM: Should I do it?
*   MM: Does this come with the idea that the inputs/outputs can be in a struct and can be shared between vert and frag shaders?
*   DN: Yes, same thing. Don’t like a single level of struct.
*   MM: So we’re going to punch through arbitrarily nested structs? Did that for WHLSL and found it was a pain and had little benefit. If you think it’s valuable we can go for it but don’t think it’s worth it.
*   DN: It’s an orthogonally question. Wrapped up in counting of struct parameters. Why have this corner if we don't need to. Don’t think you need to but not sure where heading into.
*   DM: Can discuss when we have the basics. I’ll draft this.
*    
*   


### [Removing `ptr` and adding `ref` · Issue #1334 · gpuweb/gpuweb](https://github.com/gpuweb/gpuweb/issues/1334) 



*   (MM: Would like to discuss with RM)
*    MM: We discussed some concerns that when we add real pointers there will be 2 similar but distinct kinds of references and that’s distasteful. Mechanically the proposal would work, however. I assume that if we don’t take this proposal and we want ptr-to-ptr in the future, we would be using the same pointer type as today. Wouldn’t be new types; just rule relaxation.
*   DN: Would say yes. Current PR on how ptrs work has a rule that DM dislikes that if you mention a pointer and use it in a context that takes a non-pointer you do a load. With ptr-to-ptr that rule is no longer useful.
*   MM: We’d have to add a new operator or something
*   DM: LIke a deref operator which we could have now and solve the problem?
*   DN: Way other PR works is that there is no lvalue or rvalue but mentioning the var means you have a pointer. If you want fewer rules do what’s in the PR. With deref there is ambiguity if you’re using value storing or address. In c have & for address-of. Have to reach to outer level to determine if doing calcs or address calcs.
*   DM: We know types we do ops on. If a user is adding pointers we error out, is that an issue?
*   JG: Right now, pointers implicitly decay to refs. If you have a + 3 today if a is a pointer what that does is loads a and adds 3 and gives a result. The pointers we have today are refs that way which is weird. Would definitely prefer to have it explicit as it resolves ambiguity.
*   DM: I see it as we don’t have pointers today. The section is incomplete and no-one is using them. Thought it was a good idea in the future. Don’t need to base off something today
*   DN: Don’t understand that, currently exactly as SPIR-V and LLVM handle them.
*   DM: No spirv has explicit deref and you’re making it implicit.
*   KN: Would be nice to have deref for pointes but requiring deref on var because var is technically a pointer isn’t great
*   MM: Similar point, previously we started we don’t want explicit load/store and think that’s an approximation. We want code to be readable and having loads/stores would look bad; doesn’t mean there isn’t a middle ground.
*   DM: Leave var as is, not confusing, works like any language. Not super happy but it works. If we have real pointers which aren’t implicitly deref solves the problem
*   JG: Would be happy to write a proposal to have explicit load/store for pointers.
*   DN: PR from last week has all type rules for assignment and type rules for pointers. If you have a mirror to that of ‘this is what it looks like’ and ‘this is why the type rules aren’t wild’ and you haven’t introduced lvalue and rvalue.
*   DM: I think lvalues are already there. Pointers don’t change anything. When parsing `struct.member` you already have to know it’s an lvalue if on the left side of assignment. If you see deref on the left side you know it’s an lvalue. 
*   DN: For me, lvalue and rvalue is mysterious for people reading code. Can see a phrase in language and know the type. Don’t know if you have a variable which on the outside takes the address of, don’t know if calculating. Even sizeof in c, not actually doing work. As programmers we don’t put complicated expressions on the inside to get fooled. Would like to see JG's proposal.
*   MM: In JGs proposal, if we had a local variable (not let, var) and add 4 and assign to another local, no pointers involved, would that involve pointers and derefs and address of?
*   JG: No, would just work like for a normal variable. In some ways, var just says you handle the storage for this . Only time ptrs are involved is once you start using the reference of op to get the pointer to something. Now you have a pointer and there is no implicit decay. Really obvious when you have a pointer and it behaves differently.
*   DN: One thing to be clear. Later when we have a Memory Model, we have to look at knowing when memory accesses occur. Not an issue for private but in shaders we need to know when they occur and what statements. Probably Ok.
*   MM: Not sure it was stated, what the distaste of having lvalues and rvalues is?
*   DN: It’s hard to understand when you have an lvalue context vs rvalue context.
*   JG: Because you have to walk to top of expression tree
*   DN: Maybe that melts away as no pointer return from functions so anything on left is lvalue and anything on right is rvalue. If someone does that analysis and says it’s that simple I’d be happier. If the 5 node lattice in the c++ spec, trying to explain is a mess.
*   JG: Will move to the needs action column. Weird we have interdependent issues but don’t have meta issues. Weird that PRs are issues.
*    
*   



---



# 📆 Next Meeting Agenda



*   Meet 2021-02-09
    *   Atomics proposal? [https://github.com/gpuweb/gpuweb/issues/1360](https://github.com/gpuweb/gpuweb/issues/1360) 
    *   Barrier	proposal? [https://github.com/gpuweb/gpuweb/issues/1374](https://github.com/gpuweb/gpuweb/issues/1374) 
    *   1319 
    *   1241
    *   Struct layout stuff
    *   