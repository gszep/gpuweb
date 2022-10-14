# GPU Web Meeting 2022-10-12/13 APAC-timed

Chair: Kelsey

Scribe: Ken

Location: Google Meet


## Tentative agenda



* Administrivia
* CTS Update
* “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)
* createPipelineAsync() doesn't reject its promise, even if the created pipeline is invalid [#3296](https://github.com/gpuweb/gpuweb/issues/3296)
* mapAsync(WRITE) zero-fills the relevant region of the buffer [#2926](https://github.com/gpuweb/gpuweb/issues/2926)
* Canvas texture expiry timing is unpredictable [#3295](https://github.com/gpuweb/gpuweb/issues/3295)
* (stretch) Passive Fingerprinting Surface [#3101](https://github.com/gpuweb/gpuweb/issues/3101)
* (stretch) Permissions Policy for WebGPU and for unmaskHints [#3483](https://github.com/gpuweb/gpuweb/issues/3483)
* Agenda for next meeting


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Myles C. Maxfield**
* Google
    * **Austin Eng**
    * Ben Clayton
    * Brandon Jones
    * Corentin Wallez
    * Dan Sinclair
    * David Neto
    * Gregg Tavares
    * **Kai Ninomiya**
    * **Ken Russell**
    * Loko Kung
    * Shannon Woods
    * Shrek Shao
    * Stephen White
    * Ryan Harrison
* Intel
    * Brandon Jones
    * Bryan Bernhart
    * **Hao Li**
    * Jiajia Qin
    * **Jiawei Shao**
    * Jie Chen
    * **Shaobo Yan**
    * **Yang Gu**
    * **Yunchao He**
    * **Zhaoming Jiang**
* Microsoft
    * Jesse Natalie
    * **Rafael Cintron**
* Mozilla
    * **Erich Gubler**
    * **Jim Blandy**
    * **Kelsey Gilbert**
* Snap
    * Corvyn Kusuma
    * Dhritiman Sagar
    * Sumant Hanumante
* Unity
    * Brendan Duncan
    * Dave Shreiner
* Connor Fitzgerald
* Francois Daoust
* Mehmet Oguz Derin
* Michael Shannon
* Rob Conde
* Russell Campbell


## Administrivia

More APAC meetings



* Myles OOO in December
* Dan Glastonbury (Apple) can participate in APAC meetings
* Thinking - for Nov/Dec - more APAC meetings?
    * 2 APAC meetings : 1 non-APAC meeting?
* Nov/Dec - lose 1 week in Nov + 2 in Dec for holidays
* 1 hour before this would work for Dan. Timezones are different in China though
* Would 1 hour earlier work for Intel Shanghai?
    * YG: discussed yesterday. Works for us.
* ZJ: is this one hour earlier just for the next two months, or for all APAC meetings?
    * KG: haven't decided. Just deciding for the next 2 months.
    * ZJ: let's give that a try.
* 4 PM Pacific then. Translate to China / Australia times.
* Next meeting will be non-APAC. 3 weeks from now: next APAC meeting.
* KN: because of daylight savings - Nov. 6 US time change - will be same time in Shanghai.
* DG: I don't have daylight savings here. :) Will work around the time.
* 
* 
* 
* 


## CTS Update



* KN: Gyuyoung working on memory synchronization tests, expanding coverage, trying to do various things and mess up memory sync.
* Jiawei made fix for buffer binding size change in the spec.
* Work ongoing on WGSL.


## “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)



* workgroupStorageUsed is sum of values, each rounded up to multiple of 16 [#3522](https://github.com/gpuweb/gpuweb/pull/3522)
    * Handles the fact that padding between variables in workgroup storage class is implementation-defined. FYI only - already approved by WGSL group.
    * Backends don't define how they pack variables into workgroup memory, so this is a conservative estimate of how much space we'll use.
* MM: if workgroup variable is a struct, will padding be involved?
* KN: reason for picking 16 - it's the largest natural alignment you'd need.
* MM: so struct containing 4 floats - 16 bytes, not 128?
* KN: yes.
* KN: this was discussed by folks who understand Vulkan's constraints.
* 
* 
* 


## createPipelineAsync() doesn't reject its promise, even if the created pipeline is invalid [#3296](https://github.com/gpuweb/gpuweb/issues/3296)



* JB: general agreement that sync/async distinction isn't great indicator about pipeline creation on-thread vs. off-thread
* JB: browser can always take one or the other impl. Browser can always move off-thread if it wants.
* JB: lot of discussion in issue - what tactics do WebGPU users actually want to use when they're trying to meet a framerate, but can't guarantee pipeline creation in advance? Trying to introduce pipelines in the middle of play.
* JB: if they have fallback strategy, can use the fallback pipeline, and switch to the new one if created.
* If they don't have a fallback - their only strategy is to jank. Miss frames until pipeline's ready.
* Talking with different folks about what devs actually want to use - gotten different people saying different things. Overall - game engines would prefer to have fallbacks, but often don't. Jank when they can't get things ready.
* Our concern - seems to us that games most often jank. They don't want to render until they can draw everything. They'd like to miss frames. This is the actual practice in the industry. This is a use case WebGPU should make sure it supports.
* In the proposal right now - once we decide to use async, we have no way to jank, if we want to. Either await it, or use it and get an error. Can't say, just pause the whole queue until it's ready.
* We're concerned that there's this use case common in the industry that we're not facilitating.
* KR: This is why the two entry points exist. Developers can use the synchronous version if they want to jank when the pipeline is used. Async is very compatible with JavaScript. We are working with developers loading massive data who want to be able to avoid jank. The two entry points facilitate two use cases.
* JB: once someone chooses to use the async one, they have given up the ability to jank.
* KR: They can use the non-async version instead
* KG: one concern - when you have a case you're trying to load a pipeline, want to do work after it's loaded, but may need to say - I need to render - want to jank - you can do that in the current design. Call createPipeline again with the same descriptor. Not ideal. Feel a bit bad about it. It's a use case that feels strange. Wish you could createPipeline with the option to have a promise. But in the grand scheme of things we can accept - what I expect to happen - in the future we do add a Promise to GPUPipeline, so in the future you can await on a pipeline.
* In that case - maybe slightly vestigial createPipelineAsync. Like a polyfill around createPipeline and awaiting ready(). Probably OK.
* Feels like an acceptable outcome to me. Not super happy about it.
* Think we should tolerate the compromise and accept the cruft.
* JB: seconding what Kelsey said. We understand Google's under a lot of pressure. This is where we expect feedback based on impl experience. We expect our feedback will be handled by the committee.
* KR: Understood. My personal standpoint isn’t coming from pressure to ship, but from the fact we’ve had this entry point for years now and it was well reasoned. Structure of API designed so you can do both. I’m not sure what’s going to happen if we add a promise to the pipeline and what developers are going to do. Whether they’ll inadvertently jank vs understanding properly how to use the promise. Based on what developers have done - Babylon may use this for example. Want to base this decision on the experience we already have.
* KG: agree that it has value and is well-reasoned. Just think this could be even better.
* KN: fully agree that we may end up reaching a point where createPipelineAsync is cruft. I don't know whether we are or not. Need to prototype, work with developers.
    * KG: I'm proposing we don't do that work.
    * KN: not arguing with that.
* KN: I agree that we may end up in that future, and I strongly support keeping what we have right now rather than weakly supporting, but based on the same reasoning.
* KG: wrote [https://hackmd.io/aZvyrbEzSsO3r6homVh4kQ](https://hackmd.io/aZvyrbEzSsO3r6homVh4kQ)
* KG: figuring out what I would have wanted. Comparison, number of use cases. Not pursuing that direction any more. Interesting, wanted to share that exploration.
* JB: Moz saying overall - we're fine with proceeding with this question. Take the change where we reject the Promise. Otherwise the API stays the same. Just wanted to make sure the alternatives we were pursuing are properly recorded.
* KG: do we have consensus?
* MM: which consensus?
* KG: createPipelineAsync will reject Promise if pipeline creation fails.
* MM: OK. Reject with nothing? Reject with errors? Editor discussion?
* JB: **we can make that a separate PR**.
* MM: I prefer to let the editors handle this.
* KG: **would like it if the editors would bring a proposal here**.
* MM: OK. **Right now, rejects with nothing. Then CG will build on this**.


## mapAsync(WRITE) zero-fills the relevant region of the buffer [#2926](https://github.com/gpuweb/gpuweb/issues/2926)



* MM: socialized this. Not much discussion. Socialize it again?
* JB: I could use a recap
* MM: talking about map-write. Spec says: when you map-write, (considering it an atomic process), you get ArrayBuffer, write into it, unmap. Then map it again. When you see the ArrayBuffer you see the contents you saw before.
* One impl strategy: second map-write downloads the data from the GPU.
* Other way, more realistic: elsewhere in spec, any buffer that's map-writeable can't be used for any other usage, other than transfer-src. So, only situation that could write into the buffer is with a map-write. So all the impl does is keep the arraybuffer around in the web process. Second map-write returns the same AB. Nothing else could have changed that data.
* MM: unfortunate for 2 reasons. First, this involves double- or triple-allocations. Obvious extra alloc: map-writeable buffer can't use for any other usage, it's useless for anything else. They may want to bind it as a storage buffer. In order to use map-writeable buffer, you have to have a second buffer, and copy back and forth. Double allocation
* MM: then also a shadow buffer. First mapAsync, you keep the AB around on the CPU. That's another copy of the buffer. Lifetime of the shadow copy is at least as long as the buffer. Not transitory.
* MM: on mobile, this is a thumbs-down.
* MM: doesn't have to be this way. I'm proposing - relax the constraint that the second map-write sees the results of the first one. App doesn't intend to actually read the data. If we zero-fill the buffer, that should be fine from POV of app. Don't need the shadow copy - can just clear it. Can remove the other spec section where map-writeable buffers can't have other usages. Map-writeable buffers become useful. Removes need for second copy.
* MM: downside - need to zero-fill upon map-write. If GPU does the blit, can use blitting hardware. Can arrange it so GPU can do the clearing, should be fast. It is a tradeoff.
* MM: it is a tradeoff.
* KG: do you expect to be able to use the GPU for the clear?
* MM: yes. Considering the map call separately - map does the zero-fill on the content timeline - that'll work. Only way it won't work is that the second call does it. That would have to be on the CPU.
* JB: that'd be a big memset before the GPU communication will be faster. Does anyone do GPU fills to get zero-filled CPU buffers?
* KG: sounds unusual. Memory bandwidth is the highest cost. Depending on shmem caching properties - might not have a choice.
* MM: on our impl - we expect triply-mapped buffers to be the norm, from the GPU process to the web process.
* KG: interesting, we don't expect to do that ourselves.
* AE: two parts we see to this. One, as myles said - allowing other usages and copy-src with map-write. Second is zero-filling bit.
* AE: for first part - i don't think we support just lifting this usage restriction. Similar to [#2388](https://github.com/gpuweb/gpuweb/issues/2388). Would be great to reduce # copies in UMA - just lifting this won't perform well for discrete GPUs. Have a CPU-writeable thing readable from GPU - will have lot of memory traffic every time CPU writes. Myles had ideas about flushing of mapped writes - but think we need a more full proposal for UMA to support lifting the usage restriction.
* AE: for second part, about zero-filling - we also expect to do this triple-mapping concept. Have shmem, shared with GPU process, also visible to GPU hardware. In this scenario, not convinced by memory savings / saved allocations. In this world - there's no CPU-only shadow buffer. You have one buffer. No double-allocation there.
* AE: returning 0 in the mapping does let the impl delete the buffer whenever it wants - can be transient, don't need to preserve contents. But - this is pretty much the same as the web app destroying buffers themselves. I think this is strictly better - makes the app handle allocation explicitly . Handle OOM explicitly themselves. If we're creating buffers for you during mapAsync - will complicate the error handling mechanisms. There are already ways to do this in the current API that are working pretty well.
* KG: other Q - how different is this from impl of writeBuffer? Once you eventually say we'll discard the contents & replace them - we already have that. Theoretically you could write it into your triply mapped buffer, just memcpy it in.
* MM: what's the story we tell to web developers? Start using writeBuffer, and if it doesn't work for you because of perf, you can go to lower-level map primitive yourself. Round-robin your buffers.
* MM: confusing - for someone who starts using writeBuffer, and mapping has a huge memory cost, so we go back to writeBuffer?
* KG: what are you asking for here that's not satisfied by writeBuffer? Just that we have an indication that these buffers are triply-mapped?
* MM: right now, there's a subsystem in the WebGPU with pretty atrocious memory characteristics, and I want to avoid this.
* KG: I think the opposition is, the shadow copies not that expensive … and can be managed by app
* MM: my proposal - for triply mapped, you'd get 50% reduction. DIscrete GPU case where not triply mapped, 66% memory reduction.
* AE: we plan to always triply map. 50% reduction, and can not get that if we assume UMA. We don't think we should simply remove the restriction, but have a full UMA proposal.
* KR: The characterization of atrocious memory consumption is an implementation problem and not mandated by the spec. If we put in the zero-filling behavior then we’re going to make things worse for triple mapping. No mandate to keep a CPU shadow copy right now. Think any reasonable implementation should [...]. If we adopt zero-filling now we’ll constrain what the API can do in the future backward-compatibly.
* MM: can you describe the future?
* KR: Through clever passing of GPU handles between processes we can directly, safely access memory of an actual buffer from JavaScript. Maybe allow some racing but fundamentally seeing the real mapping in JS. Maybe only works in UMA. Maybe works with discrete on some systems. May require more OS primitives.
* AE: Exactly right. Safari planning to do this and we’re planning as well. Windows APIs exist for it.
* KR: If we mandate zero-filling it eliminates the possibility of doing this. Similar constraints in the WebGL API - can’t transform feedback to an index buffer. Here, in this one area, we don’t allow GPU writes to these buffers now, want to in the future. Internally we discussed options like “either zero or old data” but think it’s not good for developers.
* MM: OK. **Like to urge Google rep to write a position with the technical underpinnings to the issue, and I'd like to study them.**
* AE: sounds good.
* 
* 
* 
* 


## Canvas texture expiry timing is unpredictable [#3295](https://github.com/gpuweb/gpuweb/issues/3295)



* KN: previously: KG had concerns about clamping things down more significantly than WebGL, instead of relying on WebGL experience - where things seem to sort of be working OK.
* KG: discussed within Moz - felt that this the tricky part of the problem, would be nicely solved by explicit present option. People wouldn't have to worry about it.
* MM: in far future - looking forward to first-class support for variable refresh rate displays on the web. Right now, requestVideoFrame, and you watch a 24 FPS video - you get the 3:2 judder. Reason - web was designed with fixed framerate in mind. rVF could be driven by something more fundamental than screen refresh rate.
* MM: little worried that if web platform adds VRR support - if WebGPU adds explicit present, these would fight each other. Synchronized animations across multiple APIs is important.
* JB: how does explicit present ensure there's a conflict between WebGPU & VRR presentation of content?
* MM: I'm expecting that explicit present would take arguments. "Present this WebGPU frame at this time in the future with this duration.". Then this functionality comes to rAF, and they fight.
* KG: don't see why you couldn't implement VRR in rAF framework today.
* KN: API I envision for explicit present has none of that. Just saying you're done writing the frame. Don't think it conflicts with refresh rate considerations.
* KN: I think explicit present is a great possible direction. Also not the ideal API for everyone. Think there are a lot of use cases benefiting from it, like Wasm Promise integration. Frame spanning multiple rAFs. Want to build something more basic right now. Don't think having explicit present is the right API for everyone. Have to be able to do present at the end of your frame. Not easy to fit into all existing (web) engines. Would like to keep present API more similar to WebGL/2D canvas. And want that API, since it's the default, to have consistent behavior so it's easy to debug for developers. That's why I'd like to take the approach we've proposed - find a good deterministic point to terminate write access to the texture. Will look like WebGL/2D, and have repeatable behavior.
* KG: worried about how it changes vs. how WebGL does. Interested in pushing this to WebGL too?
* KR: Web compat concern with WebGL. Have encountered applications that relied on undefined behavior.
* KN: think we could do something like this for WebGL. As long as it conforms closely to what browsers are doing.
* KG: **would like to know more about the flickering problems you mentioned with WebGL. Hard for us to make an informed decision**.
    * KR: for others: we should look up the bug from Scirra with an async rAF callback - discussed that one already - there were others, let's discuss offline. They're all in Chromium's issue tracker somewhere.


## (stretch) Passive Fingerprinting Surface [#3101](https://github.com/gpuweb/gpuweb/issues/3101)



* 
* 
* 
* 


## (stretch) Permissions Policy for WebGPU and for unmaskHints [#3483](https://github.com/gpuweb/gpuweb/issues/3483)



* 
* 
* 
* 


## Agenda for next meeting



* Next meeting: non-APAC.
* 
* 
* 
* 