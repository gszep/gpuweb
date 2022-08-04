# GPU Web meeting 2022-08-02/03 APAC-timed

Chair: Kelsey

Scribe: KN, KG

Location: Google Meet


## Tentative agenda



* Administrivia
* CTS Update
* “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)
* Specify that texture_external sampling functions clamp coords to [0, 1] [#2766](https://github.com/gpuweb/gpuweb/issues/2766)
* Drop support for macOS 10.12? [#3238](https://github.com/gpuweb/gpuweb/issues/3238)
* Expose the buffer state as an attribute [#3014](https://github.com/gpuweb/gpuweb/issues/3014)
* Buffer state validation in unmap() is unnecessary [#3251](https://github.com/gpuweb/gpuweb/issues/3251)
* Object labels require client-shared state [#3263](https://github.com/gpuweb/gpuweb/issues/3263)
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
    * **Hao Li**
    * Jiajia Qin
    * **Jiawei Shao**
    * **Jie Chen**
    * **Shaobo Yan**
    * **Yang Gu**
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



* Remember about the Call For Spec Editors!


## CTS Update



* KN: Some work ongoing on testing video interop and canvas interop. RH is writing up a doc about the floating point testing framework he wrote for WGSL, in case other people find that interesting.
    * MM: I’m interested!


## “Tacit Resolution” [Queue](https://github.com/gpuweb/gpuweb/issues?q=label%3A%22tacit+resolution+queue%22)



* Nothing!


## Specify that texture_external sampling functions clamp coords to [0, 1] [#2766](https://github.com/gpuweb/gpuweb/issues/2766)



* SY: Before discussing this, would like to share some performance data we collected. Similar example to Google Meet. Render a grid of videos by drawing the videos to a canvas. Using the WebGPU zero-copy path, compared with WebGL, we can find ~200% improvement in performance, and ~50% in power, on Apple M1 Mac.
    * KG: Twice the copies, twice the power? Seems very perfect!
    * SY: Probably too perfect - trying to understand results better.
    * [https://source.chromium.org/chromium/chromium/src/+/main:content/test/data/gpu/vc/README.md](https://source.chromium.org/chromium/chromium/src/+/main:content/test/data/gpu/vc/README.md) 
    * with flag --enable-unsafe-webgpu --disable-dawn-features=diallow_unsafe_apis
    * SY: The result is that (on M1 Max)webgpu has 200% better than webgl on fps and 23% powersaving(using powermetrics for power data collecting) compare with webgl. And WebGL path is better than CopyEI2T path in WebGPU. So the result of the 0-copy is cheerful.
* KG: Discussing filtering…
* MM: Think we’ve agreed on filtering, just address modes.
* KG: There’s a proposal to rename the function so that we don’t need to preserve the symmetry.
* MM: Another is to do whatever is necessary to make these work. Another is to bake the texture and sampler together. Another is to make an sampler_external object.
* KG: Off the cuff, it’s very tempting to go in the direction of prior art and have a sampler_external like in GLSL.
* MM: As KN has pointed out, upside is can reuse them. Downside is more API surface, which I think is minor.
* KN: Another minor downside is the binding cost. 1 sampler + 1 uniform buffer.
* …
* MM: Can optimize so 2 external samplers in the same bind group use only one uniform buffer.
* KN: But can only do that if the spec requires implementations to do this.
* MM: In this proposal, I think today we would just not allow repeat addressing modes in the GPUExternalSamplerDescriptor. Then later can add support. Works toward our goal of making repeating textures a well supported thing on the web platform. Think we can work that out later.
* KN: Think we should discuss now because …
* MM: 
* KN: Extension would be very limited in how much functionality it has if it doesn’t support non-mirrored repeat.
* MM: Would prefer to have repeat supported, just with seams at the edge (always). Almost no videos would tile nicely anyway.
* KN: OK.
* KR: Remain concerned that we’re picking up runtime pipeline compilation costs, depending on which address mode is used. (...)
* KR: Want to ensure we can capture the improvements Intel has shown and have it be workable on all platforms. Use 1-copy for when you want repeat modes other than clamp-to-edge.
* MM: Runtime/resource cost in unextended WebGPU would be 0.
* KG: Your phrasing is you want to bring tiled video to the web as a first class citizen. My feeling in the room is most people don’t want to do that. You want to produce an extension and show people it’s valuable to help change our minds? … I worry that you say you want it to be first class, agree to a compromise, and then we talk about an extension later and no one else wants to do it. Like, is that tolerable to you?
* MM: If you think back to the requirements for extensions discussion, one of the requirements is that 2 or more implementations should think it’s a good idea.
* KG: Just want to make sure you don’t expect this compromise will result in getting here in the future.
* MM: First problem is that the function looks like it will do the same thing as other textures. External sampler would solve this. Second is, gee it would be nice to have tiled videos. Less important, but this proposal paves the way.
* KR: Want to object to the phrasing of tiled video being first class. It is. The 1-copy path is that. External textures are a super-optimized fast path, don’t have to be used.
* KN: (Soft) object because it paves the way for something I don’t think we ever will do.
* KG: Question is whether it’s acceptable.
* KN: I think it’s kind of acceptable, but I don’t like that we’re taking a tradeoff that I don’t think will pay off.
* …
* KN: Add complexity now for the sake of not much benefit later.
* MM: The benefit now is we don’t have to add a WGSL function that specifically clamps.
* MM: Would like to move onto other topics in this meeting.
* **(Come back to this next week)**


## Drop support for macOS 10.12? [#3238](https://github.com/gpuweb/gpuweb/issues/3238)



* MM: 10.12 the only version that this group is claiming to support that doesn’t have argument buffers. Using argument buffers increases some limits.
* MM: …
* MM: Raising limits will force implementations to use argument buffers.
* KG: Today I don’t think wgpu requires them. Says here Dawn doesn’t use them.
* KN: I don’t think we need to decide this for v1. But we don’t expect to be using ABs for v1 shipping, but that eventually we could switch to ABs and increase our base limits for webgpu spec itself. 
* KN: But for right now, dropping 10.12 support *might* let us increase some limits that are orthogonal to AB support. I think we can agree that we don’t want to require ABs for v1.
* MM: What is included besides ABs?
* KN: Don’t know, but I should go look.
* MM: It sounds like we should not change anything right now, but that further investigation might change this.
* MM: So resolved?** “Don’t drop 10.12 for now”**


## Expose the buffer state as an attribute [#3014](https://github.com/gpuweb/gpuweb/issues/3014)



* KN: This has gone through a bunch of revisions. I’ve been working through state on client vs server for buffer mapping. What I came to is that client always needs to know whether it’s pending, can’t throw that away, so needs at least three states on client side. Observation from editors meeting from MM, is that it could be useful to know whether it’s mapped vs mappable. Also, it’s probably not worth hiding this from authors, and we should just let them check.
* KN: So unmapped/pending/mapped should work well, shouldn’t be onerous, and all three should be useful for devs, so that’s my proposal.
* KN: For context, in #3013, our resolution was that we should probably just expose this, so this proposal is that we just expose this.
* MM: Discussing this internally. We feel that it this is tracked on client, it’s useful to expose the enum, rather than a more arcane way to get this info.
* MM: Another option here (not advocating particularly) is that we don’t have any state on the client.
* KN: Sort of jives with some stuff I’m considering but I don’t think it’s possible.
* KG: I think we determined in Firefox it was possible but onerous. Cases where if you map a buffer and unmap it you’ll get the promise back with the shmem. And we’d have to then send it back and say “just kidding”.
* JB: We decided not to do this. Would have let us use some prebuilt shmem ownership tracking but decided it would be too baroque. Now using “unsafe shmem”.
* KN: Thought about this a bunch, I’m pretty sure that it’s not really possible to store *no* state on the client. The client needs to know certain things.
* MM: I think it’s possible to be somewhat simpler via tracking generation ids for these exchanges.
* KN: *different* client side state, but still have some.
* JB: Corentin said one of the reasons for behavior being specified the way it is is wanting to allow for post-v1 multiple mapAsync regions on the same buffer. It would be possible to have multiple regions mapped. In this proposal #3014, the state reported would be misleading.
* KN: I think there is room for support for sub-range mapping, but it gets a little strange, e.g. “pending” with some-pending-some-unmapped sub-ranges. But we’d also probably need a new e.g. Unmap() function too, since right now that’s only for the whole buffer.
* MM: Could have multi-map be a new thing that just exposes a “multi-mapped” state.
* JB: Then if they called multi-map they could get that state. If they don’t then they can’t get the new state.
* KN: MM’s suggestion makes sense. Think we can always make it work somehow.
* KN: “mappable buffer views” were another proposed API for this and wouldn’t have as much of an issue.
* …
* KN: Note multi-mapping would be in a world where you can also *use* parts of the buffer that aren’t mapped.
* KG: … Consensus was basically we had better things to do for V1.
* **(Let’s come back to this next week)**


## Buffer state validation in unmap() is unnecessary [#3251](https://github.com/gpuweb/gpuweb/issues/3251)



* KG: This is “unmap is pedantic”?
* KN:** I actually want to think more, and possibly put up a proposal that contradicts this, so let’s wait.**


## Object labels require client-shared state [#3263](https://github.com/gpuweb/gpuweb/issues/3263)



* KN: I thought there was client-side state e.g. mapping state that was shared between different workers because they’re in the same content process. I now think that this in untrue, and we don’t have any shared state like this.
* KN: Therefore I think that the only state that is shared would be labels, which are read+write attributes. You probably think that changing the label on one worker would cause it to (racily) update for other readers on other workers.
* KN: So I am enumerating four options for us to choose from.
* KN: One is to change the labels to be set via a function, and make stronger guarantees there.
* KG: I don’t think people should expect this to be shared client state. Didn’t we punt cross-worker sharing to post-v1?
* KN: Yes, but paving the way for that.
* RC: Think it’s valuable to allow changing the label. E.g. you’re using a cache of objects and want to reuse one. Was against allowing multiple threads to access the same object. Think we should do transfer semantics so it’s easy to know which thread owns which thing.
* MM: We have the same concern as Rafael with option 5. We think option 4 is probably the best one.
* MM: Isn’t there shared state for buffer mapping? Didn’t we just add a way to check the state?
* KN: My proposal with multithreading will be that that state is per-thread. Threads can rely on the server to tell it whether another thread has it mapped.
* **(Let’s come back to this next week)**


## Agenda for next meeting so far



* GPUBuffers shouldn't have to accumulate a list of mapAsync promises to reject [#3271](https://github.com/gpuweb/gpuweb/issues/3271)
* Specify that texture_external sampling functions clamp coords to [0, 1] [#2766](https://github.com/gpuweb/gpuweb/issues/2766)
* Expose the buffer state as an attribute [#3014](https://github.com/gpuweb/gpuweb/issues/3014)
* Object labels require client-shared state [#3263](https://github.com/gpuweb/gpuweb/issues/3263)
* 