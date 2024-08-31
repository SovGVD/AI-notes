Based on:
 - https://www.youtube.com/watch?v=NKR_1TUO6go and https://github.com/nktice/AMD-AI/blob/main/ROCm6.0.md
 - ROCm docs https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/amdgpu-install.html and https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/3rd-party/pytorch-install.html
 - Rust: https://www.rust-lang.org/tools/install

Hardware
 - Ubuntu 24.04.1, Linux kernel 6.8.0-41-generic, GPD Win max 2 6800U 16GB (8GB to iGPU set in BIOS)
 - Python 3.12.3
 - A1111 82a973c04367123ae98bd9abdf80d9eda9b910e2 tag: v1.10.1

1. Update: `sudo apt update && sudo apt install git python3-pip python3-venv python3-dev libstdc++-12-dev libjpeg-dev`
2. Install older version of python???
3. Create folder: `mdkir AI && cd AI`
4. Download and Install AMD drivers: `sudo apt update && wget https://repo.radeon.com/amdgpu-install/6.2/ubuntu/noble/amdgpu-install_6.2.60200-1_all.deb && sudo apt install ./amdgpu-install_6.2.60200-1_all.deb`
5. Install ROCm and drivers: `sudo amdgpu-install --usecase=graphics,rocm`
6. Add user to groups: ```sudo adduser `whoami` video && sudo adduser `whoami` render```
7. Probably better to reboot here: `sudo reboot`
8. (optional) Install monitoring tools: `sudo apt install radeontop`
9. Get A1111: `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git && cd stable-diffusion-webui`
10. Install Rust: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
11. Enter into environment: `python3 -m venv venv && source venv/bin/activate`. You should see something like: `(venv) yourLogin@yourHost:~/stable-diffusion-webui$`
12. Install python packages: `pip3 install -r requirements.txt && pip3 uninstall torch torchvision && pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/rocm6.2/` - that will install requirements, delete CUDA related packages and install ROCm 6.2 (I'm pretty sure should be proper way to do it)
13. Exit: `deactivate`
14. Create script to run, eg name it `run.sh`:
    ```bash
    #!/bin/sh
    
    source venv/bin/activate
    
    HSA_OVERRIDE_GFX_VERSION=10.3.0 python launch.py --listen --enable-insecure-extension-access --opt-sdp-attention --theme dark

    deactivate
    ```
    And make it executable: `chmod +x run.sh`
15. Run it: `./run.sh` - first start could take longer
    
