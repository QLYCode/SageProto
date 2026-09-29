# SageProto: Mitigating Class-Wise Learning Disparity in Semi-Supervised Multi-Organ Segmentation

#### [📌] The full implementation details will be released upon acceptance.

## Datasets

We evaluate SageProto on three multi-organ segmentation datasets:
**Synapse**, **FLARE22**, and **AMOS**.

### Synapse

The Synapse dataset contains **30 abdominal CT cases** with
**3,779 annotated slices** covering eight organs:

- Aorta
- Gallbladder (Gal)
- Right Kidney (R.Kid)
- Left Kidney (L.Kid)
- Liver
- Pancreas (Pan)
- Spleen
- Stomach (Sto)

We use **18 cases for training** and **12 cases for testing**.

### FLARE22

The FLARE22 dataset contains annotations for **13 abdominal organs**:

- Liver
- Spleen
- Pancreas (Pan)
- Right Kidney (R.Kid)
- Left Kidney (L.Kid)
- Stomach (Sto)
- Gallbladder (Gal)
- Esophagus (Eso)
- Aorta
- Inferior Vena Cava (IVC)
- Right Adrenal Gland (RAG)
- Left Adrenal Gland (LAG)
- Duodenum (Duo)

We divide the dataset into training, validation, and testing sets using a
**6:2:2 split**.

### AMOS

We further evaluate SageProto on the **AMOS** dataset, which contains
**300 CT volumes** with annotations for **15 anatomical structures**:

- Liver
- Spleen
- Pancreas (Pan)
- Right Kidney (R.Kid)
- Left Kidney (L.Kid)
- Stomach (Sto)
- Gallbladder (Gal)
- Esophagus (Eso)
- Aorta
- Inferior Vena Cava (IVC)
- Right Adrenal Gland (RAG)
- Left Adrenal Gland (LAG)
- Duodenum (Duo)
- Bladder (Bl)
- Prostate/Uterus (P/U)

We adopt the same **6:2:2 split** for training, validation, and testing.

## Evaluation Metrics

We report the following metrics:

- Dice Similarity Coefficient (DSC)
- Jaccard Index (Jac)
- Average Surface Distance (ASD)
- Number of Parameters (Params)

## Implementation Details

All axial CT slices are normalized to **[0, 1]**.

### Input Resolution

- **FLARE22:** `512 × 512`
- **AMOS:** `512 × 512`
- **Synapse:** native resolution with zero-padding to multiples of 32

### Training Framework

- Framework: Mean Teacher
- Student backbone: U-Mamba
- Teacher network: EMA teacher
- Batch size: `4`
- Labeled/unlabeled composition: `3L + 1U`
- Optimizer: SGD
- Momentum: `0.9`
- Weight decay: `1e-4`
- Polynomial learning-rate decay power: `0.9`
- Warm-up iterations: `500`

### Dataset-Specific Settings

| Setting | Synapse | FLARE22 | AMOS |
|---|---:|---:|---:|
| Training iterations | 30k | 80k | 40k |
| Initial learning rate | `1e-2` | `5e-4` | `1e-3` |
| CE-DSC weight $\alpha$ | `0.50` | `0.36` | `0.30` |
| Boundary weight $\lambda_b$ | `0.02` | `0.03` | `0.03` |
| Boundary activation | 3k | 5k | 4k |
| Gradient clipping | None | Norm 12 | Norm 12 |
| EMA decay | `0.99` | `0.997 → 0.9995` | `0.995 → 0.999` |
| Confidence threshold $\tau$ | `0.75` | `0.82 → 0.76` | `0.82 → 0.70` |

The number of prototype rectification steps is set to `K = 4`, and the
prototype regularization coefficient is set to `0.06`.


## Requirements
Some important required packages include:
* Python 3.8
* CUDA 11.8
* causal-conv1d 1.2.0.post2
* einops 0.8.0
* imageio 2.35.1
* mamba-ssm 2.2.2
* matplotlib 3.7.5
* medpy 0.5.2
* nibabel 5.2.1
* opencv-python 4.11.0.86
* pillow 10.2.0
* scikit-image 0.21.0
* scipy 1.10.1
* simpleitk 2.4.1
* six 1.17.0
* tensorboardx 2.6.2.2 
* torch 2.0.0+cu118
* torchaudio 2.0.1+cu118
* torchvision 0.15.1+cu118
* tqdm 4.67.1 





