# FlashAvatar Setup Guide

This document explains how to set up and run FlashAvatar for high-fidelity digital avatar reconstruction.

## Repository Information
- **Official Repository**: https://github.com/USTC3DV/FlashAvatar-code
- **Paper**: https://arxiv.org/abs/2312.02214
- **Project Page**: https://ustc3dv.github.io/FlashAvatar/

## Requirements
- NVIDIA RTX 3090 GPU (tested configuration)
- CUDA 11.8
- Python 3.7-3.11
- Conda (recommended) or Python venv

## Setup Steps

### 1. Clone the Repository
```bash
git clone https://github.com/USTC3DV/FlashAvatar-code.git
cd FlashAvatar-code
git submodule update --init --recursive
```

### 2. Create Environment

**Option A: Using Conda (Recommended)**
```bash
conda env create --file environment.yml
conda activate FlashAvatar
```

**Option B: Using Python venv**
```bash
python3 -m venv flashavatar-env
source flashavatar-env/bin/activate  # On Linux/Mac
# OR
flashavatar-env\Scripts\activate  # On Windows
```

### 3. Install PyTorch with CUDA Support
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### 4. Install Additional Dependencies
```bash
pip install scipy chumpy scikit-image opencv-python ninja lpips loguru tqdm plyfile
```

### 5. Install Submodule Packages
```bash
pip install submodules/diff-gaussian-rasterization
pip install submodules/simple-knn
```

### 6. Install PyTorch3D (Conda only)
```bash
conda install -c fvcore -c iopath -c conda-forge fvcore iopath
conda install -c bottler nvidiacub
conda install pytorch3d -c pytorch3d
```

For venv users, install PyTorch3D following the official guide:
https://github.com/facebookresearch/pytorch3d/blob/main/INSTALL.md

## Data Organization

FlashAvatar expects data organized as:
```
dataset/
├── <id1_name>/
│   ├── alpha/          # raw alpha prediction
│   ├── imgs/           # extracted video frames
│   └── parsing/        # semantic segmentation
├── <id2_name>/
└── ...

metrical-tracker/
└── output/
    ├── <id1_name>/
    │   └── checkpoint/
    ├── <id2_name>/
    └── ...
```

## Running FlashAvatar

### Download Example Data (Optional)
Download preprocessed example data and pretrained model:
https://drive.google.com/file/d/1_WLvlmHD73jOAO178N7eX5UQqlrL2ghD/view

### Test with Pre-trained Model
```bash
python test.py --idname <id_name> --checkpoint dataset/<id_name>/log/ckpt/chkpnt.pth
```

### Train on Your Own Data
```bash
python train.py --idname <id_name>
```

## Performance
- Reconstruction time: Minutes for monocular video
- Rendering speed: 300+ FPS at 512×512 resolution on RTX 3090

## Citation
```bibtex
@inproceedings{xiang2024flashavatar,
  author    = {Jun Xiang and Xuan Gao and Yudong Guo and Juyong Zhang},
  title     = {FlashAvatar: High-fidelity Head Avatar with Efficient Gaussian Embedding},
  booktitle = {The IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  year      = {2024}
}
```

## Troubleshooting

### CUDA Out of Memory
- Reduce batch size
- Use a GPU with more VRAM
- Process fewer frames at a time

### PyTorch3D Installation Issues
- Ensure CUDA version matches PyTorch
- Build from source if conda installation fails
- Check compatibility: https://github.com/facebookresearch/pytorch3d

### Missing Dependencies
- Verify all submodules are initialized: `git submodule update --init --recursive`
- Reinstall submodule packages if needed

## Notes
- The FlashAvatar-code directory is excluded from this repository's version control
- This is an external research project - refer to the official repository for updates
- GPU with CUDA support is required for training and inference
