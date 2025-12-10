# dont_you_know_im_local
- Creating containers running ollama and hosting local llm. 

## Most Efficient Build? 
1. Kube YAML (inline install)

  - ❌ Reinstalls ollama every start
  - ❌ Runs install script each time
  - ❌ Slow restarts (minutes)
  - ✅ Simple single file
  - Efficiency: WORST


  2. Containerfile + Quadlet

  - ✅ Ollama installed once at build time
  - ✅ Ollama runs as PID 1 (minimal overhead)
  - ✅ Fast restarts (seconds)
  - ✅ Systemd manages container lifecycle
  - ✅ Pull models via HTTP after start (persisted in volume)
  - ❌ Two-step: build then deploy
  - Efficiency: BEST


  3. Systemd-inside-container + Quadlet

  - ❌ Runs systemd inside container (extra overhead)
  - ❌ More complex, more processes
  - ❌ Still need to install ollama somehow
  - ❌ Heavier than PID 1 approach
  - Efficiency: WORSE than #2

  Winner: Containerfile + Quadlet


### Granite_Coder
(inside the granite_coder dir)

 Quick Deploy:

  #### 1. Build the image (once)
  podman build -t granite-coder:latest .

  #### 2. Install Quadlet file
  mkdir -p ~/.config/containers/systemd/
  cp granite-coder.container ~/.config/containers/systemd/

  #### 3. Start as systemd service
  systemctl --user daemon-reload
  systemctl --user enable --now granite-coder.service

  #### 4. Pull the model via API
  curl http://localhost:11434/api/pull -d '{"name":"granite-code:3b"}'

