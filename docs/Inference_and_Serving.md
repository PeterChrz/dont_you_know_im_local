## Inference and Serving Software

? Wait for ROCm support - AMD will likely add gfx1150 in a future ROCm release
1. Use CPU mode with Ollama - works now, just slower
2. Try llama.cpp with Vulkan - may have better support for newer AMD GPUs
3. vLLM seems to struggle to get gfx1150 - Strix Halo HX375 working with iGPU and ROCm.

###  To use vLLM directly:

- Inside the container, run vLLM server:
```
python -m vllm.entrypoints.openai.api_server \
  --model ibm-granite/granite-3b-code-base \
  --host 0.0.0.0 \
  --port 11434
```

Then connect from host with any OpenAI-compatible client:
 - Using curl
```
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "ibm-granite/granite-3b-code-base", "messages": [{"role": "user",
"content": "Hello"}]}'
```

# Or use the openai CLI/Python library
pip install openai
export OPENAI_BASE_URL=http://localhost:11434/v1
export OPENAI_API_KEY=dummy