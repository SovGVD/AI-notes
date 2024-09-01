Based on:
 - https://www.youtube.com/watch?v=NKR_1TUO6go and https://github.com/nktice/AMD-AI/blob/main/ROCm6.0.md
 - ROCm docs https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/amdgpu-install.html and https://rocm.docs.amd.com/projects/install-on-linux/en/latest/install/3rd-party/pytorch-install.html
 - Rust: https://www.rust-lang.org/tools/install

Hardware and software:
 - Ubuntu 24.04.1, Linux kernel 6.8.0-41-generic, GPD Win max 2 6800U 16GB (DON'T SET 8GB to iGPU in BIOS, left is to Auto, you can check with memory available for AI `sudo dmesg|grep GTT`)
 - Python 3.11.9 (is not default version Ubuntu 24.04.1)
 - A1111 SD 82a973c04367123ae98bd9abdf80d9eda9b910e2 tag: v1.10.1
 - SD Forge UI 3b9b2f653ef9579e871e73f34b3c5037395cc351

Installation process:
1. Update: `sudo apt update && sudo apt install git libstdc++-12-dev libjpeg-dev libssl-dev`
2. Install older version of python: `sudo add-apt-repository ppa:deadsnakes/ppa && sudo apt update && sudo apt install python3.11 python3.11-venv python3.11-dev`
3. Install Rust: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
4. Create folder: `mdkir AI && cd AI`
5. Download and Install AMD drivers: `sudo apt update && wget https://repo.radeon.com/amdgpu-install/6.2/ubuntu/noble/amdgpu-install_6.2.60200-1_all.deb && sudo apt install ./amdgpu-install_6.2.60200-1_all.deb`
6. Install ROCm and drivers: `sudo amdgpu-install --usecase=graphics,rocm`
7. Add user to groups: ```sudo adduser `whoami` video && sudo adduser `whoami` render```
8. Probably better to reboot here: `sudo reboot`
9. (optional) Install monitoring tools: `sudo apt install radeontop`
10. Get A1111 OR SD Forge (for Flux for example): `git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git && cd stable-diffusion-webui` or `git clone https://github.com/lllyasviel/stable-diffusion-webui-forge && cd stable-diffusion-webui-forge`
11. Enter into environment: `python3.11 -m venv venv && source venv/bin/activate`. You should see something like: `(venv) yourLogin@yourHost:~/stable-diffusion-webui$`
12. Install python packages: `pip3 install -r requirements.txt && pip3 uninstall torch torchvision && pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/rocm6.2/` - that will install requirements, delete CUDA related packages and install ROCm 6.2 (I'm pretty sure should be proper way to do it)
13. Exit: `deactivate`
14. Update/add `webui-user.sh`:
    - `python_cmd="python3.11"`
    - `export TORCH_COMMAND="pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/rocm6.2/"`
    - `export HSA_OVERRIDE_GFX_VERSION=10.3.0` - that is for RDNA2 that is in AMD 6800U
15. Run it: `./webui.sh` - first start could take longer as well as first render
