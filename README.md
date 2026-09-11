# Brain Tumor Classification Using ResNet50

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-1.13%2B-ee4c2c)
![License](https://img.shields.io/badge/License-MIT-green)

Official implementation of our paper **"Classification of Brain Tumor Using ResNet50"**,
which classifies brain-tumor MRI scans into three categories — **Meningioma, Glioma, and
Pituitary** — using **ResNet-50** transfer learning with rotation-based data augmentation.

> **Anand A., Ridhuparan K., Karthik G.S., Sooraj Veer R., Raghu Prasath V.**
> *Classification of Brain Tumor Using ResNet50.*
> Solid State Technology, Vol. 63, No. 5, 2020.
> https://solidstatetechnology.us/index.php/JSST/article/view/5888

## Abstract

Brain tumor is a deadly cancer type; early and precise identification is significant for
the medication process. We apply convolutional neural networks to MRI images, converting
scans to RGB and augmenting them before classification with a fine-tuned ResNet-50. Our
approach achieves **98% classification accuracy** across the three tumor classes.

## Method

| Component        | Choice |
|------------------|--------|
| Backbone         | ResNet-50, pretrained on ImageNet, fully fine-tuned |
| Classifier head  | `2048 → 2048 → 2048 → 3` with LeakyReLU, Dropout(0.4), LogSoftmax |
| Input size       | 3 × 512 × 512 RGB |
| Augmentation     | 8× per image — original + 7 rotations (30°–270°) |
| Loss             | `NLLLoss` (matches the `LogSoftmax` head) |
| Optimizer        | SGD (lr = 3e-4, momentum = 0.9) |
| Split            | 70% train / 15% validation / 15% test |
| Checkpointing    | Best model saved by lowest validation loss |

## Results

| Metric            | Value |
|-------------------|-------|
| Overall accuracy  | **98%** |

The notebook also produces training/validation **loss** and **accuracy** curves, a
**confusion matrix**, and a per-class **classification report** (precision / recall / F1)
over the three tumor classes.

## Repository structure

```
.
├── README.md
├── LICENSE
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Brain_Tumour_Classifier.ipynb   # end-to-end training + evaluation
└── data/
    └── README.md                       # expected dataset format (data is not committed)
```

## Setup

```bash
git clone https://github.com/<your-username>/brain-tumor-classification-resnet.git
cd brain-tumor-classification-resnet
python -m venv .venv
# Windows: .venv\Scripts\activate     Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
```

Place the dataset at `data/training_data.pickle` — see [`data/README.md`](data/README.md)
for the expected format. A CUDA-capable GPU is recommended for training on 512×512 images.

## Usage

```bash
jupyter notebook notebooks/Brain_Tumour_Classifier.ipynb
```

Set `DATA_PATH` and `CKPT_DIR` in the **Configuration** cell if your paths differ, then
run the cells top-to-bottom. The notebook trains the model, saves checkpoints to
`checkpoints/`, and reports metrics on the held-out test set.

## Citation

If you use this work, please cite:

```bibtex
@article{anand2020braintumor,
  title   = {Classification of Brain Tumor Using ResNet50},
  author  = {Anand, A. and Ridhuparan, K. and Karthik, G. S. and Sooraj Veer, R. and Raghu Prasath, V.},
  journal = {Solid State Technology},
  volume  = {63},
  number  = {5},
  year    = {2020}
}
```

## Authors

Anand A., Ridhuparan K., Karthik G.S., Sooraj Veer R., Raghu Prasath V.

## License

Released under the [MIT License](LICENSE).
