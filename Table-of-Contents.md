This page aims to track the big picture of what sections the specification is missing.

A bullet is a section header (appears in the spec's table of contents).
A `.` is an item that should appear in the section body.

*   [x] Introduction \
    . basic description
*   [x] Security Considerations
    *   CPU-Based Undefined Behavior
    *   GPU-Based Undefined Behavior
    *   Out-of-Bounds Access in Shaders
    *   Invalid Data
    *   Driver Bugs
    *   Timing Attacks
    *   Denial of Service
    *   Fingerprinting
    *   (anything missing?)
*   [ ] Fundamentals
    *   [x] Conventions
        *   [x] Dot Syntax
        *   [x] Internal Objects
        *   [x] WebGPU Interfaces
        *   [x] Object Descriptors
    *   [x] Invalid Internal Objects & Contagious Invalidity
    *   [x] Coordinate Systems \
        . (just an overview; details are throughout the spec)
    *   [ ] Fixed-point data conversions (see [Vulkan](https://www.khronos.org/registry/vulkan/specs/1.1-extensions/html/vkspec.html#fundamentals-fixedconv) for example) - coordinate with WGSL
    *   [ ] Programming Model
        *   [x] Timelines \
            . describes the kinds of timelines we have in the programming model
        *   [x] Memory Model \
            . describes the major types of memory WebGPU operates on under the hood and what guarantees we provide on their state
        *   [ ] Multi-Threading
        *   [x] Resource Usages
        *   [x] Synchronization
    *   [x] Core Internal Objects
        *   [x] Adapters \
            . prose: adapter \
            . internal object definition: adapter
        *   [x] Devices \
            . prose: device \
            . internal object definition: device
    *   [x] Optional Capabilities
        *   [x] Limits \
            . describe limits mechanism, not actual limits
        *   [x] Features \
            . describe extension mechanism, not actual features
*   [x] Initialization \
    . prose describing initialization \
    . high-level explanation of limits and extensions
    *   [ ] Examples
    *   [x] navigator.gpu
    *   [x] GPU (interface)
        *   [x] requestAdapter
    *   [x] GPUAdapter
        *   [x] requestDevice
            *   [x] GPUDeviceDescriptor
    *   [x] GPUDevice
    *   [x] GPUExtensionName \
        . (shouldn’t provide definitions for the extension names, those should be in the Extensions section at the bottom)
    *   [x] GPULimits \
        . list and describe limits
*   [ ] Queues \
    . internal object definition: queue \
    . GPUQueue \
    . device.defaultQueue
    *   Queue Operations
        *   [x] submit()
        *   [x] writeBuffer()
        *   [x] writeTexture()
        *   [x] copyImageBitmapToTexture() \
            . GPUImageBitmapCopyView
        *   [x] onSubmittedWorkDone()
*   [ ] Resources
    *   [x] Buffers \
        . prose: buffers \
        . internal object definition: buffer \
        . internal object definition: content-timeline buffer state tracker \
        . GPUBuffer
        *   Creation \
            . createBuffer() \
            . GPUBufferDescriptor
            *   Buffer Usage Flags
        *   Destruction \
            . destroy()
        *   Mapping \
            . mapAsync() \
            . getMappedRange() \
            . unmap()
    *   [ ] Textures \
        . prose: textures and their uses, with reference to texture views \
        . internal object definition: texture \
        . GPUTexture
        *   Creation \
            . createTexture() \
            . GPUTextureDescriptor
            *   Texture Usage Flags
        *   Destruction \
            . destroy()
    *   [ ] Texture Views \
        . prose: describe texture views and their uses \
        . internal object definition: texture view \
        . GPUTextureView
        *   Creation \
            . createView() \
            . GPUTextureViewDescriptor
    *   [x] Samplers \
        . internal object definition: sampler \
        . GPUSampler
        *   Creation \
            . createSampler() \
            . GPUSamplerDescriptor et al
    *   [x] Resource Binding
        *   [x] Bind Group Layouts \
            . internal object definition: bind group layout \
            . GPUBindGroupLayout
            *   Creation \
                . createBindGroupLayout() \
                . GPUBindGroupLayoutDescriptor et al
        *   [x] Bind Groups \
            . internal object definition: bind group \
            . GPUBindGroup
            *   Creation \
                . createBindGroup() \
                . GPUBindGroupDescriptor et al
*   [ ] Pipelines & Programmability
    *   [ ] Pipelines
        *   [x] Pipeline Layouts \
            . internal object definition: pipeline layout \
            . GPUPipelineLayout
            *   Creation \
                . createPipelineLayout() \
                . GPUPipelineLayoutDescriptor
        *   [ ] Pipeline Creation \
            . prose: common descriptors for render and compute
            *   GPUPipelineDescriptorBase
            *   GPUProgrammableStageDescriptor
    *   [x] Compute Pipelines \
        . internal object definition: compute pipeline \
        . GPUComputePipeline
        *   Creation \
            . createComputePipeline() \
            . GPUComputePipelineDescriptor
        *   [ ] Compute Shaders
    *   [x] Render Pipelines \
        . internal object definition: render pipeline \
        . GPURenderPipeline
        *   Creation \
            . createRenderPipeline() \
            . GPURenderPipelineDescriptor
        *   [ ] Stages
            *   [x] Vertex Fetch \
                . vertexState
            *   [x] Vertex Shader \
                . vertexStage
            *   [x] Primitive Assembly \
                . primitiveTopology
            *   [ ] Rasterization \
                . rasterizationState
            *   [ ] Fragment Shader \
                . fragmentStage
            *   [ ] Stencil & Depth \
                . depthStencilState
            *   [ ] Color State \
                . colorStates
    *   [ ] Shaders \
        . prose: about WGSL
        *   [ ] Security \
            . describe what we do in order to execute shaders securely
        *   [ ] Shader Modules \
            . internal object definition: shader module \
            . GPUShaderModule
            *   Creation \
                . createShaderModule() \
                . GPUShaderModuleDescriptor
*   [ ] Command Buffers & Encoding
    *   [ ] Command Buffers \
        . prose: command buffers and how encoding works
        *   [ ] Command Buffers \
            . internal object definition: command buffer \
            . GPUCommandBuffer \
            . finish() \
            . GPUCommandBufferDescriptor
        *   [ ] Command Encoders \
            . (no internal) \
            . GPUCommandEncoder \
            . createCommandEncoder() \
            . GPUCommandEncoderDescriptor
        *   [ ] Non-Pass Commands \
            . prose: see Non-Pass Commands
        *   [ ] Programmable Passes \
            . prose: ... \
            . GPUProgrammablePassEncoder
    *   [ ] Compute Passes \
        . prose: compute pass encoding \
        . (no internal) \
        . GPUComputePassEncoder
        *   [x] Beginning Compute Passes \
            . beginComputePass() \
            . GPUComputePassDescriptor
        *   [ ] Commands \
            . prose: see Compute Commands
        *   [x] Ending Compute Passes \
            . endPass()
    *   [ ] Render Passes \
        . prose: render pass encoding \
        . (no internal) \
        . GPURenderPassEncoder
        *   [x] Beginning Render Passes \
            . beginRenderPass() \
            . GPURenderPassDescriptor
            *   Color Attachments
            *   Depth/Stencil Attachments
            *   Load & Store Operations
        *   [ ] Commands \
            . prose: see Render Pass Commands and Render Commands
        *   [x] Ending Render Passes \
            . endPass()
    *   [ ] Render Bundles \
        . prose: render bundle encoding \
        . internal object definition: render bundle \
        . GPURenderBundle
        *   [ ] Encoding Render Bundles \
            . createRenderBundleEncoder() \
            . GPURenderBundleEncoderDescriptor
        *   [ ] Commands \
            . prose: see Render Commands
    *   [x] Commands
        *   [x] Non-Pass Commands: Copy
            *   [x] GPUTextureDataLayout
            *   [x] GPUBufferCopyView
            *   [x] GPUTextureCopyView
            *   [x] copyBufferToBuffer()
            *   [x] copyBufferToTexture()
            *   [x] copyTextureToBuffer()
            *   [x] copyTextureToTexture()
        *   [x] Non-Pass Commands: Debug
            *   [x] pushDebugGroup()
            *   [x] popDebugGroup()
            *   [x] insertDebugMarker()
        *   [x] Programmable Pass Commands
            *   [x] pushDebugGroup()
            *   [x] popDebugGroup()
            *   [x] insertDebugMarker()
            *   [x] setBindGroup()
        *   [x] Compute Pass Commands
            *   [x] setPipeline()
            *   [x] dispatch()
            *   [x] dispatchIndirect()
        *   [x] Render Pass Commands
            *   [x] setViewport()
            *   [x] setScissorRect()
            *   [x] setBlendColor()
            *   [x] setStencilReference()
            *   [x] executeBundles()
        *   [x] Render Commands \
            . GPURenderEncoderBase
            *   [x] setPipeline()
            *   [x] setIndexBuffer()
            *   [x] setVertexBuffer()
            *   [x] draw()
            *   [x] drawIndexed()
            *   [x] drawIndirect()
            *   [x] drawIndexedIndirect()
*   [ ] Image operations (see [Vulkan](https://www.khronos.org/registry/vulkan/specs/1.1-extensions/html/vkspec.html#textures) for example)
    *   [ ] Texel Input Operations - https://github.com/gpuweb/gpuweb/issues/1116
    *   [ ] Texel Output Operations
    *   [ ] Texel Coordinates \
        . (normalized/integer)
*   [ ] Rasterization (see [Vulkan](https://www.khronos.org/registry/vulkan/specs/1.1-extensions/html/vkspec.html#primsrast))
    *   [ ] Rasterization Order
    *   [ ] Multisampling
    *   [ ] Barycentric Interpolation
    *   [ ] Line Segments
    *   [ ] Triangles
*   [ ] Fragment Operations (see [Vulkan](https://www.khronos.org/registry/vulkan/specs/1.1-extensions/html/vkspec.html#fragops))
*   [ ] Framebuffer
    *   [ ] Blending
*   [ ] Device-Generated Commands (see [Vulkan](https://www.khronos.org/registry/vulkan/specs/1.1-extensions/html/vkspec.html#device-generated-commands))
*   [ ] Canvas Rendering & Swap Chains
    *   [ ] Canvas Context \
        . GPUCanvasContext \
        . getSwapChainPreferredFormat()
        *   Creation \
            . prose: canvas.getContext()
    *   [ ] Swap Chains \
        . GPUSwapChain
        *   Creation \
            . configureSwapChain() \
            . GPUSwapChainDescriptor
*   [ ] Errors & Debugging
    *   [ ] Fatal Errors \
        . GPUDevice.lost \
        . GPUDeviceLostInfo
    *   [ ] Error Scopes \
            . GPUDevice.pushErrorScope() \
            . GPUErrorFilter \
            . GPUDevice.popErrorScope() \
            . GPUError/GPUOutOfMemoryError/GPUValidationError
    *   [ ] Telemetry \
        . GPUDevice.onuncaptureerror \
        . GPUUncapturedErrorEvent \
        . GPUUncapturedErrorEventInit
*   [x] Features
    *   Short section for each extension (most extension text should be inline) \
        . define GPUFeatureName enum member \
        . point to parts added to the rest of the spec
*   [x] Type Definitions \
    . numeric types
    *   Colors & Vectors \
        . GPUOrigin2D \
        . GPUOrigin3D \
        . GPUExtent3D
*   [ ] Data Formats
    *   [ ] Vertex Formats \
        . bit layouts \
        . required extensions
    *   [ ] Texture Formats \
        . bit layouts
    *   [x] Texture format capabilities \
        . required extensions for different capabilities