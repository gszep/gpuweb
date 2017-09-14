From [Minutes for the 2017-07-26 meeting](https://lists.w3.org/Archives/Public/public-gpu/2017Jul/0004.html).

> DJ: what are next steps?
> 
> Should Corentin or someone define an API? Or move on with the assumption that we have an agreement of the overall shape of this part? 
> 
> KR / MM: seems too early to define an API
> 
> DM: agree, also tied in to memory barriers
> 
> CW: maybe we can make a comprehensive list of things we want to talk about before looking at the shape of the API
> 
> DJ: Could do this on the wiki/github
> 
> MM: an issue per issue, then close them?
> 
> DM: Github milestones?
> 
> KR: spreadsheet?
> 
> DJ: volunteers?
> 
> MM: I can start!

("MVP" means "Minimum Viable Product" and refers to a version 0 of the API.)

- High level object model (Relationship between buffers, queues, renderpasses, etc.)
  - Consensus: There should be at least one type of queue: a queue that can perform rendering, compute, and memory operations
  - Consensus: MVP will include compute facilities
  - Open question: MVP may only allow one instance of a queue object
  - Open Question: We may want additional types/capabilities of queues
  - Consensus: Queues will need to be created at device creation time
- Render Passes
  - Consensus: Metal’s render encoders and Vulkan’s render sub-passes will be encapsulated in the same API object (casually referred to as a “render pass”)
  - Consensus: All rendering must be done while a render pass is “open”. No compute may be done while a render pass is “open.” Only one pass may be open at a time.
  - Consensus: Compute passes need to be opened and closed too
    - Open Question: Do blit/memory passes need to be opened and closed?
  - Open Question: Should consecutive render passes inherit state?
  - Consensus: The destination set of textures you’re drawing into can only change at a render pass boundary
  - Open Question: Should render passes include synchronization dependencies?
- Binding model
  - Consensus: Generally, use Vulkan’s binding model: A 3-layered hierarchy: Descriptors, a set of descriptors, and a (single) collection of sets.
    - Open Question: D3D has to keep samplers separately from all other resources
    - Open Question: How does allocation of descriptor sets and pooling of descriptors work in D3D?
- Pipeline States
  - Consensus: In general, include the union of all 3 platform API’s pipeline state
  - Consensus: Includes depth/stencil state
  - Consensus: Does not include sample mask
  - Open Question: Should Renderpass information be included? (Vulkan backends would need to create dummy renderpasses)
  - Consensus: Separate blend state is included (thereby requiring a Vulkan extension)
  - Consensus: Does not include per-face stencil state
  - Consensus: MVP does not include depth bounds test
  - Consensus: Render targets and rasterizer need to have the same sample count. This sample count is specified in the pipeline state.
  - Consensus: MVP does not include an explicit pipeline caching API
  - Consensus: MVP does not include derivative pipelines
  - Consensus: Pipeline state object will have to include the format of the index buffer (int16 or int32) to make primitive restart sentinel value work
  - Open question: Should the API make a distinction between having a depth test which always passes and disabling the depth test?
  - Open question: Should there be an extra bool to disable independent blending, or should it be implicit from the blend attachments?
- Synchronization
  - Consensus: Fences will have 1 bit of information in them
  - Consensus: It should be difficult to have undefined behavior
  - Open Question: Should resource barriers be explicit or issued by the driver?
  - Open Question: How to do GPU-CPU synchronization?
- Resources
  - Consensus: GPU-visible buffers will have to be aligned
    - Open Question: Aligned to what?
  - Open Question: Should resources be allowed to automatically migrate between different memory regions on the GPU
- Shading Language
  - Open Question: Should the API accept human authorable source? Should it accept bytecode? Both?
  - Consensus: Some text form of the shading language would need to be specified (not just a binary form)
  - Consensus: The shading language must be no more expressive then SPIR-V in Logical Addressing Mode
- GPU-Driven Rendering
- Threading model
- Consensus: Don't include bundles / secondary command buffers in the MVP
- Consensus: Don't include stream-out / transform feedback in the MVP
- Consensus: Don't include predicated rendering in the MVP
- Consensus: Don't include tessellation in the MVP
- Consensus: Don't include sparse resources in the MVP
- Queries (Performance, or fragment pass count & similar info)