
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
