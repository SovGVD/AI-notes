- Stop service: `sudo systemctl stop ollama`
- Update `/etc/systemd/system/ollama.service` and add under `[Service]`
  - `Environment="HSA_OVERRIDE_GFX_VERSION=10.3.0"`
  - `Environment="HCC_AMDGPU_TARGETS=gfx1030"`
- Reload: `sudo systemctl daemon-reload`
- Start service: `sudo systemctl stop ollama`
