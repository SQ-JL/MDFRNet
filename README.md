# MDFRNet

Official code preparation for **MDFRNet: Modality-differentiated fusion and
prediction-guided hierarchical refinement for RGB-D salient object detection**.

At the current stage, this repository provides the benchmark datasets and the
best prediction results. The MDFRNet source code will be released after the
associated paper is accepted for publication.

The released implementation uses the same names as the manuscript:

- `CDFM`: Cross-modal Difference Fusion Module
- `MFGM`: Multi-scale Fusion-Guidance Module
- `LFRM`: Layer-wise Feature Refinement Module
- `MDFRNet`: the complete shared-parameter RGB-D network

## Repository Structure

```text
MDFRNet_open_source/
├── models/
│   ├── mdfrnet.py
│   ├── cdfm.py
│   ├── mfgm.py
│   ├── lfrm.py
│   └── smt.py
├── configs/
│   └── default.yaml
├── docs/
│   └── legacy_name_mapping.md
├── utils/
│   ├── data.py
│   └── training.py
├── pretrained/
├── train.py
└── requirements.txt
```

The public module names follow the manuscript. Historical experiment names are
documented in `docs/legacy_name_mapping.md`, and checkpoint loading keeps
backward compatibility with the former parameter prefixes.

## Environment

The manuscript experiments used PyTorch 1.12. A compatible environment can be
created with Python 3.8-3.10:

```bash
pip install -r requirements.txt
```

Install the PyTorch build matching the local CUDA version when the default pip
wheel is unsuitable.

The released code has also been smoke-tested with Python 3.10.18,
PyTorch 2.0.1+cu118, torchvision 0.15.2+cu118, and timm 1.0.16. This tested
configuration is compatible with the implementation but is not presented as
the original experimental environment.

## Dataset Layout

Both training and validation roots must contain paired files in three folders:

```text
dataset_root/
├── RGB/
├── GT/
└── Depth/
```

`RGB` stores RGB images, `GT` stores binary saliency masks, and `Depth` stores depth
maps. Corresponding files must have the same sorted order.

The RGB-D benchmark datasets can be downloaded from Baidu Netdisk:

- [Download datasets.zip](https://pan.baidu.com/s/1O2IGA3riO-poDBOwTu7Yug?pwd=sq75)  
  Extraction code: `sq75`

## Saliency maps

The `best/<dataset>/` directories contain the `level_4` saliency maps from the
best validation results for DUT, LFSD, NJU2K, NLPR, SIP, and STERE. The complete
saliency-map package can be downloaded as [`best.zip`](https://pan.baidu.com/s/1YNrZ9FTdCIG1EzX5LHF70w?pwd=hx55)
from Baidu Netdisk. Extraction code: `hx55`.

## Pretrained SMT Backbone

Download the official SMT-Base checkpoint and place it at:

```text
pretrained/smt_base.pth
```

The backbone checkpoint is loaded explicitly. No machine-specific path is used.

## Training

```bash
python train.py \
  --train-root /path/to/train_dataset \
  --val-root /path/to/validation_dataset \
  --backbone-pretrained pretrained/smt_base.pth \
  --output-dir outputs/MDFRNet
```

The default configuration follows the manuscript: input size 384, batch size 8,
initial learning rate `5e-5`, and equal supervision of five predictions.

## Model Outputs

`MDFRNet.forward(rgb, depth)` returns:

```text
(G, P4, P3, P2, P1)
```

`G` is the coarse saliency prior predicted by MFGM. `P4` to `P1` are the four
LFRM predictions from deep to shallow stages, and `P1` is used for evaluation.

## Code Availability

The source code and trained model will be made publicly available after the
associated paper is accepted for publication.

## Publication Checklist

- Add the final trained MDFRNet checkpoint or a stable download link after code release.
- Add the official paper citation after publication metadata are available.
- Add a licence file before making the repository public. No licence has been
  selected automatically because this decision must be made by the authors.
- Verify the redistribution terms of the SMT implementation and checkpoint.
