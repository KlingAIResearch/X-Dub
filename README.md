<h1 align="center">From Inpainting to Editing: Unlocking Robust Mask-Free Visual Dubbing via Generative Bootstrapping</h1>

<div align='center'>
    <a href='https://scholar.google.com/citations?user=KMrFk2MAAAAJ&hl=en&oi=sra' target='_blank'><strong>Xu He</strong></a><sup>1,*</sup>&emsp;
    <a href='' target='_blank'><strong>Haoxian Zhang</strong></a><sup>2,†</sup>&emsp;
    <a href='' target='_blank'><strong>Hejia Chen</strong></a><sup>3</sup>&emsp;
    <a href='' target='_blank'><strong>Changyuan Zheng</strong></a><sup>1</sup>&emsp;
    <a href='' target='_blank'><strong>Liyang Chen</strong></a><sup>1</sup>&emsp;
</div>

<div align='center'>
    <a href='' target='_blank'><strong>Songlin Tang</strong></a><sup>2</sup>&emsp;
    <a href='' target='_blank'><strong>Jiehui Huang</strong></a><sup>4</sup>&emsp;
    <a href='' target='_blank'><strong>Xiaoqiang Liu</strong></a><sup>2</sup>&emsp;
    <a href='' target='_blank'><strong>Pengfei Wan</strong></a><sup>2</sup>&emsp;
    <a href='' target='_blank'><strong>Zhiyong Wu</strong></a><sup>1,5,&#9993;</sup>&emsp;
</div>

<div align='center'>
    <sup>1</sup> Tsinghua University &emsp; <sup>2</sup> Kling Team, Kuaishou Technology &emsp; <sup>3</sup> Beihang University &emsp; <sup>4</sup> HKUST &emsp; <sup>5</sup> CUHK
</div>
<div align='center'>
    <small><sup>*</sup> Work done at Kling Team, Kuaishou Technology</small>&emsp;
    <small><sup>†</sup> Project leader</small>&emsp;
    <small><sup>&#9993;</sup> Corresponding author</small>
</div>

<br>

<div align="center">
  <p>
    <a href="https://arxiv.org/abs/2512.25066" target="_blank"><img src="https://img.shields.io/badge/ArXiv-2512.25066-red" alt="arXiv"></a>&nbsp;
    <a href="https://hjrphoebus.github.io/X-Dub/" target="_blank"><img src="https://img.shields.io/badge/Project-Homepage-green" alt="project homepage"></a>&nbsp;
    <a href="https://github.com/KlingAIResearch/X-Dub" target="_blank"><img src="https://img.shields.io/github/stars/KlingAIResearch/X-Dub?style=social" alt="GitHub stars"></a>
  </p>
  
https://github.com/user-attachments/assets/5b2d3fab-0de8-4682-9b95-36c11dfae3f5

  <p>🔥 For more results, visit our <a href="https://hjrphoebus.github.io/X-Dub/" target="_blank"><strong>homepage</strong></a> 🔥</p>
</div>


## 🔥 Updates
- **`2026/03/19`**: 🔥 We release the [inference code](#3--inference) and [pretrained weights](https://huggingface.co/KlingTeam/X-Dub) for the public Wan-based X-Dub release.
- **`2025/12/31`**: 🔥 We release the paper and project homepage: [paper](https://arxiv.org/abs/2512.25066) | [homepage](https://hjrphoebus.github.io/X-Dub/).


## 📖 Introduction

This repository contains the official PyTorch implementation of **X-Dub**, introduced in [*From Inpainting to Editing: Unlocking Robust Mask-Free Visual Dubbing via Generative Bootstrapping*](https://arxiv.org/abs/2512.25066) (formerly *From Inpainting to Editing: A Self-Bootstrapping Framework for Context-Rich Visual Dubbing*).

Due to company policy, we cannot open-source the internal model used in the paper. This repository instead releases a migrated public X-Dub (Wan-5B) version based on Wan2.2-TI2V-5B. In our extensive experiments, X-Dub (Wan-5B) can also generate satisfying lip-synced results broadly aligned with the internal version X-Dub (internal-1B):

<details>
<summary>More qualitative results of X-Dub (Wan-5B)</summary>
    
https://github.com/user-attachments/assets/b1105660-dc26-46f9-b34a-df6d8b08c05e

https://github.com/user-attachments/assets/241330b5-3ec2-4c04-a414-0f570551a50a

https://github.com/user-attachments/assets/2b705f7f-9461-48db-833c-0dd63d079da3

https://github.com/user-attachments/assets/c254e450-ce65-45df-a3a5-900387b081c1

</details>


We still observe some differences in the current public release.
Compared with the internal version, X-Dub (Wan-5B) shows the following practical differences:

- Better generalization to non-human characters such as cartoons, animated roles, and animals.
- Slightly weaker temporal stability, with occasional flickering.
- Slightly weaker subject consistency, including possible identity drift or color drift.
- Occasional severe noisy frames in a small portion of cases (~2%).
- Roughly 2× slower inference without acceleration strategies.

<details>
<summary>Some failure cases of X-Dub (Wan-5B)</summary>
    
https://github.com/user-attachments/assets/f9c0a303-135a-4261-8000-ea263ea41dd5

</details>

These gaps likely come from both the Wan backbone and the current implementation. Wan2.2-TI2V-5B generalizes better, but can be less temporally stable and may introduce texture or color drift. The final quality also depends on checkpoint selection, CFG scales, and cropping strategy. We are still trying to find the best implementation strategy.

🏃 We are actively improving this repository and the public model. Quantitative comparisons between the public release and the internal version will be reported in future updates. If you have suggestions, please open an issue for discussion.




## 🏁 Getting Started

⚠️ Inference typically requires ~21 GB VRAM.

### 1. 🛠️ Clone the code and prepare the environment

```bash
git clone https://github.com/KlingAIResearch/X-Dub.git
cd X-Dub

conda create -n x-dub python=3.10 -y
conda activate x-dub
```

Install Python dependencies:

```bash
pip install -r requirements.txt
```

Install OpenMMLab dependencies:

```bash
pip install chumpy==0.70 --no-build-isolation
pip install mmengine==0.10.7
pip install mmcv==2.1.0 --no-build-isolation
pip install mmdet==3.2.0
pip install mmpose==1.3.2
```

Install this repository (adapted from [DiffSynth-Studio](https://github.com/modelscope/DiffSynth-Studio)):

```bash
pip install -e . --no-deps
```


### 2. 📥 Download pretrained weights

Download the released bundle directly to `checkpoints/`:

```bash
pip install -U "huggingface_hub[cli]"
hf download KlingTeam/X-Dub --local-dir ./checkpoints --repo-type model
```

Move the DWpose files into `dwpose_tools/models/`:

```bash
mkdir -p dwpose_tools/models
cp -r ./checkpoints/dwpose_tools/models/. ./dwpose_tools/models/
rm -rf ./checkpoints/dwpose_tools
```

After download, the expected layout is:

```text
checkpoints/
├── X-Dub_model.safetensors
├── Wan2.2_VAE.safetensors
├── models_t5_umt5-xxl-enc-bf16.safetensors
├── umt5-xxl/
├── whisper/
│   └── large-v2.pt
└── wav2vec2-base-960h/

dwpose_tools/models/
├── yolox_l_8xb8-300e_coco.py
├── yolox_l_8x8_300e_coco_20211126_140236-d3bd2b23.pth
├── rtmw-x_8xb320-270e_cocktail14-384x288.py
└── rtmw-x_simcc-cocktail14_pt-ucoco_270e-384x288-f840f204_20231122.pth
```


### 3. 🚀 Inference

```bash
python infer_lip_sync_pipeline.py \
  --video_path assets/examples/video.mp4 \
  --audio_path assets/examples/audio.wav \
  --ckpt_path checkpoints/X-Dub_model.safetensors \
  --ref_cfg_scale 2.5 \
  --audio_cfg_scale 10.0 \
  --num_inference_steps 30 \
  --output_dir ./results
```


## 📢 Input Video Auto-Cropping
The inference pipeline supports arbitrary-size input videos and performs online auto-cropping. The current version supports **single-person** videos only. The inference script will:

- crop the faicial region
- run lip-sync generation on the cropped and resized video (512x512)
- map the generated result back to the original complete video

<details>
  <summary>Current cropping limitations</summary>

  For ease of use, this repository uses DWPose to estimate facial ldmks for cropping. This differs from the more complex offline FLAME-mesh-based cropping pipeline used in the paper.

  The current online strategy may introduce visible jitter and may fail to follow the face reliably when the head moves rapidly. The current release also does not support target tracking in multi-person scenes.

</details>

🏃 We plan to improve the cropping strategy and add better multi-person support in future updates.


## 💡 Practical Hints
- `ref_cfg_scale` and `audio_cfg_scale` control the balance between reference appearance fidelity and audio-driven mouth motion. Different cases may prefer slightly different values.
- We recommend setting `num_inference_steps` in the range of `25-50`. Higher values increase runtime and may improve quality, but this has not been exhaustively evaluated yet.


## 📝 TODO
- [ ] Report quantitative comparisons between the public version and the paper version.
- [ ] Support multi-person video dubbing.
- [ ] Improve the cropping pipeline.
- [ ] Inference acceleration.


## ⚖️ Ethical Considerations
This work can be misused for identity impersonation or deceptive synthetic media. We support clear labeling of AI-generated content and encourage further work on reliable detection methods. All models and materials in this repository are intended for academic research and technical demonstration only.

If you have questions, please contact: `hexu18@mails.tsinghua.edu.cn`


## 🙏 Acknowledgments
We thank [**Wan2.2**](https://github.com/Wan-Video/Wan2.2) for the open-source model backbone, and [**DiffSynth-Studio**](https://github.com/modelscope/DiffSynth-Studio) for the training and inference framework.


## 🔖 Citation

```bibtex
@misc{he2025inpaintingeditingselfbootstrappingframework,
      title={From Inpainting to Editing: A Self-Bootstrapping Framework for Context-Rich Visual Dubbing}, 
      author={Xu He and Haoxian Zhang and Hejia Chen and Changyuan Zheng and Liyang Chen and Songlin Tang and Jiehui Huang and Xiaoqiang Liu and Pengfei Wan and Zhiyong Wu},
      year={2025},
      eprint={2512.25066},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2512.25066}, 
}
```
