
## Packaging the GPU Drivers

https://hub.docker.com/r/rocm/vllm-dev/tags

https://rocm.docs.amd.com/projects/radeon-ryzen/en/latest/docs/advanced/advancedryz/linux/llm/build-docker-image.html

- Does ROCm officially support gfx1151 ?

  AMD provides official ROCm container images that include support for
  gfx1151 (AMD Ryzen AI Max+ / Strix Halo APUs) starting with ROCm 6.4.1 and in newer versions like ROCm 7.x. 
  Official AMD ROCm Images
  AMD publishes various official container images (e.g., PyTorch, TensorFlow, vLLM) on their official Docker Hub profile. 
  Images that support gfx1151 can be identified by checking the PYTORCH_ROCM_ARCH or similar environment variables in their layer details, which list gfx1151 and gfx1150 among other supported architectures.

 --- 

 ollama/ollama:rocm
 https://docs.ollama.com/gpu


