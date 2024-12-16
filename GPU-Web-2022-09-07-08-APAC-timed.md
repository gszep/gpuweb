# GPU Web 2022-09-07/08 APAC-timed

Chair: KG

Scribe: KR

Location: Google Meet


## Tentative agenda



* Administrivia
* CTS Update
* “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)
* Colorspace conversions for GPUExternalTexture are arbitrarily complex [#3293](https://github.com/gpuweb/gpuweb/issues/3293)
* max buffer size is 256 MB, [#3366](https://github.com/gpuweb/gpuweb/pull/3366)
* Add HTMLVideoElement to copyExternalImageToTexture [#3410](https://github.com/gpuweb/gpuweb/pull/3410)
* Misunderstanding of external texture expiration frequency [#3324](https://github.com/gpuweb/gpuweb/issues/3324)
* createPipelineAsync() doesn't reject its promise, even if the created pipeline is invalid [#3296](https://github.com/gpuweb/gpuweb/issues/3296)
* Agenda for next meeting


## Attendance

WIP, it is the list of all the people invited to the meeting. **In bold the people that have been seen in the meeting:**



* Apple
    * **Dan Glastonbury**
    * **Myles C. Maxfield**
* Google
    * Austin Eng
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
    * Hao Li
    * Jiajia Qin
    * **Jiawei Shao**
    * Jie Chen
    * **Shaobo Yan**
    * Yang Gu
    * Yunchao He
    * **Zhaoming Jiang**
* Microsoft
    * Jesse Natalie
    * **Rafael Cintron**
* Mozilla
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



* KG: mentioned in yesterday's meeting too - the charter did technically expire. Don't worry about it. We expect to get an extension on it, working on the renewal process now.
* If any changes you'd like to talk about regarding scope of charter - here's a chance to put it in as part of the normal cycle.
* (If you don't want any changes, that's fine.)
* KR: congrats to Apple employees on product announcements today!


## CTS Update



* KN: Ryan working on float16 testing. PR open wiring things up, including float16 utilities in JS.
* Gyuyoung from Igalia just starting to look at operation tests. More on this later.


## “Tacit Resolution” Queue



* [Add the "internal" error type #3123](https://github.com/gpuweb/gpuweb/pull/3123) 
    * Added internal error type to error scope filter. Third error scope / error type.
    * Used for pipeline creation errors.
* [max buffer size limit, #3366](https://github.com/gpuweb/gpuweb/pull/3366) - see below.


## Colorspace conversions for GPUExternalTexture are arbitrarily complex [#3293](https://github.com/gpuweb/gpuweb/issues/3293)



* MM: **we're actively discussing how we think this should work. I volunteered to put it on the agenda for this week, but our internal discussions haven't concluded yet**.
* MM: we are making good progress though.
* KG: you've had good feedback on long running topics like this in the past - appreciate your in-depth thought.


## max buffer size is 256 MB, [#3366](https://github.com/gpuweb/gpuweb/pull/3366)



* KN: what would it take to raise this limit? Is it worth it?
* KN: think the only device with this limit is iPhone 6 or that hardware generation.
* MM: it's AMD GPUs (in Metal) that impose this limit; there's more than one of them. iPhone 6 has a higher limit than that.
* KN: numbers from Stack Overflow reported 256 MB on iPhone 6 for this limit.
* KG: would expect this to be in the Metal feature set tables.
* MM: if you run a process and the process asks for the limit and you go to bed, wake up in the morning, run the app again - the limit can be different.
* KG: so there is logic associated with this.
* KG: with that in mind - the 256 MB was what we expected we could always support.
* MM: yes.
* KG: possibly good candidate - have a lower bar for this - when we want to have things not stuck at the minimum/maximum values. How much we want things to float around.
* KN: can you clarify?
* KG: don't remember our consensus - was a strong push for limits to be equal to the required value (min/max). This one, since we want it to be bigger and sometimes dynamic - could be valuable to say, it'll be at least 256, and may be bigger.
* KN: without asking?
* KG: what's actionable? Maybe - proposal to raise this limit, can negotiate there.
* KN: 2 ways we can raise it. 1) drop device support. 2) say it's OK some devices will have OOMs between 256 MB and whatever we raise it to. (2) not a great option.
* MM: third option - back a WebGPU buffer by two platform buffers.
* KG / KN: sure.
* KG: would be useful to have proposal to make this bigger. 256 MB sounded like a starting point. Want to solidify things more now. Well positioned if we wanted to make this change. Make it bigger either dynamically or statically.
* KN: q for right now - could we drop a couple of devices that are very old? Do we even run on devices that have this limit?
* MM: I don't think we should drop them - don't remember the model names, but they're big fancy Macs that people spend thousands of dollars on. Should make WebGPU run on them.
* KG: useful info.
* KN: yes, useful, **can leave it as it is.**


## Add HTMLVideoElement to copyExternalImageToTexture [#3410](https://github.com/gpuweb/gpuweb/pull/3410)



* KN: proposal meant to make for a long time. Right now, two ways to use a video. importExternalTexture, or to async turn it to ImageBitmap and copy that in. The latter's a weird path - even weirder than I originally thought. Looks more like WebGL, think it's good to add this, for people who need the one-copy path for various reasons, it'll be better than the ImageBitmap path.
* KG: main reason I see we might not want this - if we want to push people away from the one-copy solution and toward zero-copy solution. Think people will make their own choices and let them do what they want. Polymorphic solution.
* KN: worth adding that we don't have HTMLImageElement here because it has a good ImageBitmap path, and it's better to turn Blob into ImageBitmap.
* KG: interesting. I'd rather put HTMLImageElement here too.
* KN: not much reason to do image element.
* MM: general for this proposal - we should add HTMLVideoElement to all the things, sounds great.
* BJ: to understand - don't have a problem with this - wouldn't remove the need for the video -> ImageBitmap -> texture paths, right? Won't reject ImageBitmaps because they came from video?
* KN: right. It's an efficiency thing. You tell us exactly what you need.
* KG: in FF in WebGL it's better to preserve fast path from video elements. ImageBitmaps don't know their destination, coming from video directly makes it easier. +1 to directly listing source of truth.
* KG: I'd like to add HTMLImageElement to this…not difficult.
* KN: since we have consensus on videos - I don't have a problem with HTMLImageElements specifically. Think there's value in pushing people toward Blob -> ImageBitmap; keeps your image decode entirely asynchronous. Images are async anyway since getting something from the network, so not painful to add another async - this is why we have Canvases here.
* MM: if app is passing static image data from network to WebGPU - you're saying that's better if it doesn't go through HTMLImageElement?
* KN: it's better regardless of whether it comes from network, because decode is async. There is some argument to be said - why are we doing the decode here? Want to know what we'll use it for. Some value for adding images here, but not as much.
* MM: there's an attribute on images to decode async.
* KG: don't want to remove ImageBitmap, but having other inputs here is useful. We have consensus on this.
* KN: if you'd like to put up PR for HTMLImageElement please do so.
* KG: I will.
* Resolved: **merge this, KG to make a follow-on PR**.


## Misunderstanding of external texture expiration frequency [#3324](https://github.com/gpuweb/gpuweb/issues/3324)



* KN: **haven't had time to get back to this one**. Can discuss a little to get people up to speed.
* KG: we can table this, too.
* KN: going on right now - how do we spec when external textures expire? Difficult because there's no algorithm for how frames advance. requestVideoFrameCallback's defined as - on every rAF, see if timestamp's changed, and if so, we'll give you a new frame. Polling based. Specs for video don't have a way to hook events like this. How do we trigger expiration?
* KN: original thing we wrote down, it expires every task. Event loop can run many many times per frame. You wouldn't implement it this way (hopefully). Can optimize, but not clear - what's a better way to spec this? Add a note, "you should optimize this"?
* KN: misunderstanding in the title - didn't realize it was running every task. Thought it was once per frame.
* MM: in researching this - in our platform we have a similar situation for one of our internal objects. Every produced object lives for 1 second. That's an option.
* KN: interesting.
* KG: have come up before - if they need to live for some time, let them live for that time frame.
* KN: we could do this - some other browsers may have something working in a similar way.
* KR: Only caveat is some platforms have a certain number of these frames, and once they hand them out, they can’t make forward progress. This means a second is probably not possible, but maybe something in that direction. 
* MM: that's fair.
* KN: may still be similar solutions.
* MM: ideally this issue would get a more descriptive title.
* KN: **I'll retitle it.**
* KG: **I'll double check with our media people too**.

(checking in with APAC folks - no additional topics)


## createPipelineAsync() doesn't reject its promise, even if the created pipeline is invalid [#3296](https://github.com/gpuweb/gpuweb/issues/3296)



* MM: if you use createPipelineAsync you get a Promise, and it's resolved even if it's a compile failure. Might expect rejection in that case because that's what you'd reject for. Understand why we might not do this synchronously. Infrastructure's all set up to use. Why not use it?
* BJ: reminds me of WebXR issue - had Promise resolving, you'd ask, does my system support this mode? Promise coming back, originally resolved/rejected depending on support. Got feedback from web platform folks (W3C) that this is an antipattern. Had request - always resolves to a boolean. Logic: we asked a question. If it could provide an answer - it's functioning correctly. Rejecting should be used in an exceptional case - the thing you requested has failed ot happen. Not sure which bucket I'd put this in. You're saying, yes, we tried to do the thing and it failed - in all other parts of the API we try to keep trucking along. I'd probably do that. Can look at it as - you asked for a thing and we couldn't provide an answer for it. These are reasons why you might want to reject in stead of resolve. Since we return valid-looking objects in other cases, I'm inclined to do the same here.
* MM: example you gave from WebXR, makes sense to be a boolean resolve rather than reject. In this case it's pretty clear it's not comparable - app says do something, and we couldn't do that. Analogy: fetch. Please download this file; if it can't, the Promise rejects. Here, we can't compile the shader.
* KN: don't think that's how Fetch works. 404 - Fetch will resolve, not reject.
* KG: which question are you asking? Give me a successfully compiled thing, or a compilation attempt to be made?
* KN: couple arguments from the issue in favor of keeping it as-is, and not rejecting.
* KN: makes it more symmetric with other createPipeline methods.
* KN: we already have the notion of async errors in WebGPU. Other cases where we reject when this happens; but it's more symmetric with this API.
* createRenderPipeline -> createRenderPipelineAsync - more symmetric if we don't reject, but return an invalid pipeline.
* Corentin's argument - if we put compilation / error messages on the pipelines, we'll need an object to put those on. Technically, could reject with that object - would have to duplicate that API to do that if we had a rejection. If we don't reject we can put it on the pipeline object.
* KN: I think these are strong enough arguments to keep things as is.
* MM: second argument - it's natural to reject with a reason why it failed. Maybe I don't agree with how invasive this factoring would be.
* KN: we'll still need to give you that info when you create it synchronously. A function you call on the pipeline to give you the info object. You could get a pipeline object from the async creation path.
* BJ: shader modules right now - no way to fetch errors, but can fetch messages, including deprecation warnings. Want these even in success case. Seems weird to bifurcate this given you want this for the synchronous case as well.
* KG: one idea - feels terrible but checks some boxes - when we accept, we give you a pipeline. What if we also give you a pipeline with the rejection? Won't work, but you can ask it for messages. Would let you await compileshadermodule / createpipelineasync - get it back and expect it to work. Alternatively, like try/await. If you don't get the right thing we'll limp along. Have a clear "here's the good pipeline".
* MM: sounds reasonable to me.
* BJ: not aware of anywhere else on the platform that has a pattern like that. Typically when you reject there's an amorphous object coming out of it, but it's usually an exception object.
* KN: it's like throwing the pipeline object, which we try not to do.
* DG: in web standards folks, wanting a boolean, shouldn't we give back the pipeline object and let you query it?
* MM: would this task have completed successfully if you tried it? One's a boolean, one's a task.
* JB: two different mechanisms for permitting async/streaming behavior. Stream a bunch of requests- not equivalent to async code. Just writing things with awaits everywhere, you have to await for completion before submitting next command. There isn't a parity between these. Both are ways to permit computation to keep on going. Seems to me that in ways where we report errors, we signal exceptions eagerly. Whole point of providing async entry points is that the caller says, I don't mind waiting for you. There's more info available at that time. The motivation for other entry points to return invalid objects doesn't apply to the async operations, and I think we should give people all the information they have, in the way they're expecting.
* KN: think its' best to think about what's practical for developers rather than what's philosophical meaning of asking for a pipeline asynchronously. Right now, don't have a way to ask whether a pipeline's valid, even asynchronously. To know, you have to put ErrorScope around pipeline creation and check its result. This is something we can still do with async creation. You'd use the ErrorScope around the createPipelineAsync call, and it'd report the error there. Things do get tricky. If you put ErrorScope around an await, it can catch other stuff due to the await. Might be an argument that we should reject; that similarity with synchronous API might be undesirable for this reason.
* BJ: KG you mentioned the web platform folks might ask us to things a certain way anyway. True. I can see both sides' arguments. **Maybe reach out to web platform mentors in our various orgs and ask? Have a design issue, what's the webby thing to do here?** Have had good success asking this.
* KG: sounds good - let's all of us ask this.
* DG: reject / resolve - as the API user, if we reject, allow us to move error handling - refactor code. Get resolved promise - can use without error checking, and if rejected - handle that there and return an error code. Something to consider.
* KN: we should consider the programming pattern in these two cases.
* JB: I don't understand the rationale for these async functions in the first place.
* MM: 1) i wrote what I think the programming model should be today - not great.
* MM: reason for async functions - if you didn't have them, you wouldn't know when your pipeline was ready. will take multiple frames. don't want to createPipeline then draw, will require it to be compiled this frame. want to let user wait until it's ready.
* KG: feels like we can write down more things to look at this from more sides.
* KN: i'll send a link in a moment - talks about why we have this.
    * [https://github.com/gpuweb/gpuweb/issues/3297](https://github.com/gpuweb/gpuweb/issues/3297)
* DG: I'd appreciate that.


## Agenda for next meeting



* Please add items here.
* Next week: TPAC. We'll meet normally here online though.
* 
* 
* 
* 