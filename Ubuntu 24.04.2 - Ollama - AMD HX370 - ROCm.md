NOTE: that is kind of working, it will load model to GTT, but count available memory using VRAM

- Stop service: `sudo systemctl stop ollama`
- Update `/etc/systemd/system/ollama.service` and add under `[Service]`
  - `Environment="HSA_OVERRIDE_GFX_VERSION=11.0.0"`
  - `Environment="HCC_AMDGPU_TARGETS=gfx1100"`
- Reload: `sudo systemctl daemon-reload`
- Start service: `sudo systemctl start ollama`
