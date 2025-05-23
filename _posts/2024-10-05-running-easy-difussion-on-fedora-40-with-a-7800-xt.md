---
title: "Running Easy Diffusion on Fedora 40 with a 7800 XT"
date: 2024-10-05
---

1. Install ROCm as per these instructions [https://fedoraproject.org/wiki/SIGs/HC\#Installation](https://fedoraproject.org/wiki/SIGs/HC#Installation)  
   1. In case the link goes down, the steps are:  
      1. Add current user to render and video groups: `sudo usermod -a -G render,video $LOGNAME`  
      2. Install ROCm packages: `sudo dnf install rocminfo rocm-opencl rocm-clinfo rocm-hip`  
         * Take note of the version of rocm-runtime that is installed (e.g. 6.1.2)  
      3. You can verify if everything is working with commands: rocminfo rocm-clinfo  
2. Install EasyDiffusion from [https://easydiffusion.github.io/](https://easydiffusion.github.io/)  
3. Once installed, follow these instructions:  
   1. Run ./developer\_console.sh  
   2. Install pytorch by going to [https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/) and select  
      1. PyTorch build: Stable  
      2. Your OS: Linux  
      3. Package: pip  
      4. Language: Python  
      5. Compute platform: ROCm 6.1 (to this date only 6.1 is supported)  
      6. This will generate a command like this  
         `pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm6.1`  
   3. Run the following command:  
      `sed -i 's/from torchvision.transforms.functional_tensor import rgb_to_grayscale/from torchvision.transforms.functional import rgb_to_grayscale/' ./installer_files/env/lib/python3.8/site-packages/basicsr/data/degradations.py`  
4. Before running ./start.sh again, first set the following environment variables (directly on the terminal you are using):  
   1. `export HSA_OVERRIDE_GFX_VERSION=11.0.0`  
   2. (Optional: If your CPU has integrated graphics): `export CUDA_VISIBLE_DEVICES=0`
