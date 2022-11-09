# GPU Web meeting 2022-11-02/03 APAC-timed

Chair: KG

Scribe: 

Location: Google Meet


## Tentative agenda



* Administrivia
* CTS Update
* “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)
* mapAsync(WRITE) zero-fills the relevant region of the buffer [#2926](https://github.com/gpuweb/gpuweb/issues/2926)** **(any offline progress?)
* How do we specify the timing of external texture expiration? [#3324](https://github.com/gpuweb/gpuweb/issues/3324)
* Multiple components cannot effectively share a GPUDevice because it is stateful [#3250](https://github.com/gpuweb/gpuweb/issues/3250)
* (stretch) Should "mark adapters stale" be called more quickly? [#1630](https://github.com/gpuweb/gpuweb/issues/1630)
* Agenda for next meeting


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Myles C. Maxfield**
* Google
    * **Austin Eng**
    * Ben Clayton
    * **Brandon Jones**
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
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Jesse Natalie
    * Rafael Cintron
* Mozilla
    * Erich Gubler
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


# Administrivia



* 
* 
* 
* 


# CTS Update



* KN: nothing big. Most of our team's working on exploratory things the past couple weeks.
* Gyuyoung from Igalia's still working on tests, mostly validation tests, hammering stuff out.
* 
* 
* 


# “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)



* Empty!
* 
* 
* 


# mapAsync(WRITE) zero-fills the relevant region of the buffer [#2926](https://github.com/gpuweb/gpuweb/issues/2926)** **(any offline progress?)



* MM: did a lot of research the past week about this.
* MM: somewhat in the same place I was previously. Not sure how to characterize the problem I think exists and what I'm asking for.
* MM: aiming for: a way to enable some mapping code path that works on both UMA and non-UMA. Ideally, sooner rather than later, but - as long as we can implement something in the future, that's OK. Without an opt-in, though. Trying to ensure there's a path forward that we can add a feature in the future, works great on UMA, don't have to opt-in. That's the concern about not zero-filling mappable buffers. Where we want mapping to work for non-UMA and UMA, having the non-UMA use case have to download the contents of the buffer is unfortunate.
* (...more…)
* If mapping such a buffer requires JS to have to be able to see contents of the buffer - for discrete devices, they'd have to download the contents.
* MM: not 100% sure what I want to get to. Worried that if the contents have to exist when mapping the buffer now, that'll make our job harder in the future.
* JB: to clarify - right now, we can't create buffers that are both mappable, and visible to shaders. In the future we hope to have buffers that are mappable and visible to shaders. And you want that to be not opt-in, you don't have to specify anything to get that behavior.
* MM: yes. One way to achieve that goal's relaxing that restriction, but that options' otp of mind right now.
* JB: with the goal you're aiming for - the ArrayBuffer's contents are the bytes the GPU is reading from - it's possible to have 0 copies in the entire path. That's what you're aiming for. You're willing to have that path be required to 0 the buffers in some cases.
* MM: think that's right.
* JB: on the non-UMA architectures, it's always cheaper to leave the contents there. Zeroing it will always be a cost that's not necessary. We don't have any option here which is optimal for both situations. Make one side ideal by imposing a cost on the other side.
* KN: I don't quite understand the concern about future-proofing. Assuming we add a way to make mappable buffers usable as other things, surely we can do what we collectively want at that point. Maybe a flag saying zero the memory. Or, we don't do that until JS adds a write-only ArrayBuffer type. Seems to me that we have flexibility. If we have to add a new map function to do this, we can. Don't see how we're designing ourselves into a corner.
* JB: the fact that that combination of flags is forbidden now, is the opt-in.
* MM: I don't want it to be an opt-in. Adding something with new semantics is opt-in.
* JB: already, today, people can't write programs that treat things as mappable and shader-accessible. We can specify that the combination of those flags means we zero it, for example. We'll always know when we need to apply new rules.
* MM: that's a good point. A little spooky but maybe that's OK.
* KN: I wouldn't necessarily design it that way, but we can design it differently later on. Maybe you have to specify a MAP_ZERO flag during mapAsync call. But Jim's point stands.
* JB: not advocating any particular design.
* KN: wouldn't be as spooky.
* MM: understand that's a tradeoff, pros/cons. Can discuss later. Most important part of this that I think is worth discussing now - don't want the thing we've described to be behind an extension. Want to just add it. Can be feature detectable - but not that to make your code work on UMA, you need to enable this. It'll just come to all browsers.
* KN: sounds fine, as long as things are feature detectable. Don't need people to enable features unless they won't be universal.
* KG: this isn't mentally actionable for me right now - don't understand the user story that we're solving here. The way I see it - there's no way to have a solution that's good for both UMA and non-UMA. You have to pick one.
* MM: one example - referencing Metal - in Metal there is a storage mode on buffers. One in particular is "managed". On UMA devices, you can map it and the thing you map is the buffer. On non-UMA, you can map it, and the thing you map is not the buffer. On unmap, the data's copied to the right place. API works great for both UMA and non-UMA. You can make a buffer that's not this way. Reason it's useful to not do the thing I just described - when you want to populate a texture. On discrete GPU, using managed buffers - you map, populate, unmap - causes copy to show up in dest buffer on GPU. Then to get it into texture, have to schedule another copy. 2 copies to fill a texture. Metal therefore has 2 methods. One is that (managed); the other's shared. Even on discrete cards, can have a buffer that lives in CPU-accessible memory. That's for staging buffers, populating texture rather than buffer. There's room for an API that works great everywhere. Such asn API isn't the only one that has to exist. We could have different APIs that have different strengths/weaknesses.
* KR: Our team has gained some experience with that Metal API recently for both upload and readback cases. While it’s certainly possible to use these APIs optimally on various GPU types, practically they perform very differently. Many factors in performance difference that means we have to heuristically chose an approach, basically by looking at the GPU architecture. … Think there will have to at least be flags to tell applications which of several available approaches they should use to be optimal. Hope we won’t try to design something that is optimal everywhere because it’s really not possible.
    * KR: I can provide links to ANGLE bugs where both the upload and readback paths were tuned.
* KG: fwiw that experience lives up to my trepidations. Not convinced that it unconditionally works great on both APIs (GPU types?). We can explore the space of compromises.
* JB: Would like to hear more information about what you had to do on Metal
* KR: Need to go back to ANGLE notes. Gregg just did optimizations in ANGLE’s readPixels. Had to write three codepaths, one for Intel, one for M1, one for discrete. Choosing the technique by architecture gained substantial performance.
* DG: Ken, do we need to expose these directly to the application, or are these things we would need to do inside WebGPU implementations?
* KR: … Will always need to know whether to write to a staging buffer or write to the resource directly. 
* KG: concerned we can't make forward progress. Focus on user stories, and prioritize them. Working in the abstract, we can't make progress.
* MM: was about to say what you did. Think it's an interesting discussion. Want to have it, but don't need to have it right now. Think Jim/Kai were convincing that we can create something in the future, after V1, without an extension, without opt-in, that'll slot in and be great. Think we're in agreement.
* KG: philosophical disagreement about how things like this are exposed. In WebGL, needing to know how various things interact - people don't understand them. When we have things that aren't opt-ins - causes problems. Had to write more clarifying docs recently, and we have warnings - for instance - if you try to use Float32 blending and you didn't enable it explicitly, it's not guaranteed. Not enough to enable 32-bit render targets. For historical reasons we said, we'll just enable it because we'll break too much content - but we wish we could have made people conscious about it. It just works, until you find a GPU where it doesn't.
* JB: don't think Kai/I advocated for a particular API. Maybe it should be exposed more clearly describing the tradeoffs. Using this feature will require new behavior on the part of content, so we have the chance to introduce new semantics too.
* KR: my main question was whether we can close out this issue.
* MM: had the zero-filling question, think we can close this, but would like to keep UMA issue open. Convinced by Kai/Jim, we can consider UMA later.
* KN: responding to Kelsey - fair. Wanting to enable this in the future without a feature flag. We can make that decision in the future. We could also put it behind a feature flag and take it out from behind it once all browsers implement it. Can decide later.
* KG: **we're going to close the zeroing issue, leave the UMA issue open**.


# How do we specify the timing of external texture expiration? [#3324](https://github.com/gpuweb/gpuweb/issues/3324)



* SY: … Corentin proposed another option - tryRefresh. Things invalidated after current task. If user doesn't cause tryRefresh, they'll get invalidated external texture. Browser can also say whether the video frame was expired. Left a comment - leaning to option (3). Compared to option (2), if you don't call tryRefresh, can't use external texture across frames. Compared to option (1), gives opportunity to reuse previous external texture, bind group, etc.
* KN: I'm in favor of tryRefresh too. Have an idea of slight variation on it, but generally, think it's a good idea.
* KG: tryRefresh satisfies my concerns. Any concerns?
* KN: any concerns on tryRefresh on the object, rather than allowing import() to return the same object again? Slight preference for the latter. tryRefresh on the object means you could import the texture again, refresh the old one - then have 2 references to the same thing. Seems more complex, would rather avoid. Maybe you can't import if the refresh would succeed. Little odd, maybe fine. Or maybe import gives you the same object again. Caching complexity. Don't leak the object.
* MM: I'm most interested in what happens if the app never calls tryRefresh.
* KN: in that case, if they just import again?
* MM: (1) they import a new frame every rAF. (2) they import a frame once and use it for the next 10 minutes.
* KN: it'll expire. Refresh would be the only way to un-expire it.
* MM: tryRefresh revives lifetime to end of current task?
* KN: takes external texture that's already expired, and attempts to un-expire it. Tells you whether it could do that. Expiry follows previous pattern.
* MM: expiry doesn't mean the bytes are gone?
* KN: correct.
* MM: is'nt this non-portable too? tryRefresh might succeed, or might fail.
* KG: difference - if you don't call tryRefresh, it absolutely won't work.
* BJ: we're consistently expiring the texture at the end of every task, every time. Only way you get back a usable texture for the next task is to try refreshing it again.
* KN: not too concerned about people calling tryRefresh and ignoring the result. If the browser advances the video frame, the video texture will expire, and tryRefresh will fail. Hard to ignore the result.
* MM: say 24 FPS video. 2 rAFs between frames. No tryRefresh => second rAF can't use the external texture?
* KN: correct.
* MM: but it could import a new one?
* KN: yes.
* RC: can't use => you get a black texture?
* KN: you get an error.
* RC: so it is an object.
* KG: invalid object.
* RC: not neutered, a valid JS object.
* KN: it's a validation error in WebGPU.
* RC: ok.
* MM: causes the whole command buffer to not be run. Result potentially disastrous.
* KG: that's why I want people checking the result. Want an API where you have to be paranoid the video's going away. At task boundaries - because it might expire there - force it to go away. Want to keep using it - you need to ask for it to be given back to you.
* KN: strongly agree. Exactly the problem I wanted to solve here.
* KG: interesting idea to have it return the same object. Can't tell when they give us back the same object without making them recreate BindGroups.
* KN: we could also return an new object and make them create new BindGroups. But forcing the creation of new Bindgroups when you don't need them isn't ideal.
* KN: sounds like we're more or less in agreement, anyone object to these two refresh-based options?
* KG: any objections going for returning the same object, 
* KR: I vote for whatever option produces the least garbage, i.e., doesn't require artificially creating new BindGroups.
* KG/KN: OK. Both solutions address this.


# Multiple components cannot effectively share a GPUDevice because it is stateful [#3250](https://github.com/gpuweb/gpuweb/issues/3250)



* KN: apologies, forgot to prepare this for the agenda.
* KN: IIRC my conclusion from this work (in July) - we should probably add something into the prototype chain for GPUDevice so that there's a single place for the uncaptured error event to go. Copies of GPUDevice => won't have that.
* KN: very small change. Technically should do it for V1 because it can affect libraries that shim the API.
* KN: idea of issue - since there's only one ErrorScope stack - using same device you compete for that. Don't need to solve that for V1. Just want to unblock solving this in the future.
* KG: can imagine something like a device, has its own stack. device.createContext(). Contexts have their own error scope. Want to share the real device, and let people use objects from different contexts on different devices (from different shards).
* KN: think that's fine. Spent some time exploring different solutions - they all look something like that. Trickiest - you can create a view from a texture. Where do its errors go? You don't do that on a specific device/context. Think that's a solvable problem.
* KG: is there anything in createView that' couldn't be an exception instead?
* KN: possible to make it an exception but then have to duplicate validation between client and server. Would be a breaking change if we did it now.
* KN: there's a number of options. GPUDevice refers to ErrorScope - but also GPUQueue, GPUTexture. Calling methods on those objects makes error go to the associated object. Or, it goes to the associated device. Think these are solvable, generally.
* KG: that'd narrow the set of problems needing a better solution than the stateful thing.
* KN: people probably wont' check for view creation errors often. Actually we don't expect them to check validation errors in production. Maybe if you want good debug logging for a particular section of your application.
* KG: think repeated validation is tolerable, if it isn't heavy (especially on the client side).
* KN: right.
* KG: also as long as information flows in one direction, doesn't have to flow from server->client.


# (stretch) Should "mark adapters stale" be called more quickly? [#1630](https://github.com/gpuweb/gpuweb/issues/1630)



* KG: not a small item.
* KN: can explain what it's about. Have discussed in the (distant) past.
* **Defer so we can talk about meeting scheduling**


# Agenda for next meeting



* KG: hoping API meetings will match APAC/not-APAC state of WGSL meetings
* KG: concretely: next week, APAC again. Does that work/
* DG: works for me
* KR: works for me
* MM: appreciate it if this discussion could include all the subsequent meetings
* DG: so WGSL call is an hour earlier than this one?
* KG: also true.
* KG: normalize to same time? Matching the WGSL time?
* Works for Shanghai, Dan (in Australia)
* So, next week - 4 PM Pacific.
* KN: schedule for weeks after that?
* KG: non-APAC, then have to look it up.
* WebGPU API schedule:
    * November 02: APAC
    * (Meeting timezone change)
    * November 09: APAC
    * November 16: Non-APAC
    * _November 23: _APAC
    * (US Thanksgiving Nov24, Thursday)
    * November 30: APAC
    * December 07: Non-APAC
    * December 14: APAC
    * _December 21: _Non-APAC
    * (Christmas Eve&Day Dec24&25, Saturday&Sunday)
    * _December 28: No Meeting_
    * January 04: APAC