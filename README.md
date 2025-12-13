# dont_you_know_im_local

Our goal is to create containers running ollama and hosting local llms. From there we can build out more complex designs of agents, and bots. 

This guide is centered around Strix Halo AMD CPU's. My test maching being an HX375 which does not yet have ROCm drivers available.


Models:
---
## Granite_Coder

 Quick Deploy:

>  #### 1. Build the image (once)
>  podman build -t granite-coder:latest .

>  #### 2. Install Quadlet file
>  sudo cp granite-coder.container ~/.config/containers/systemd/
>  sudo chmod 644 ~/.config/containers/systemd/granite-coder.container

>  #### 3. Start as systemd service
>  systemctl --user daemon-reload
>  systemctl --user enable --now granite-coder.service

>  #### 4. Pull the model via API
>  curl http://localhost:11434/api/pull -d '{"name":"granite-code:3b"}'

