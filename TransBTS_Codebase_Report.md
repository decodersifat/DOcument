# 📖 Complete Technical & Architecture Report: TransBTS

---

## 1. Executive Summary & Publications

**TransBTS** is a hybrid deep learning architecture designed for **3D Volumetric Multimodal Medical Image Segmentation** (specifically 3D MRI brain tumor segmentation, liver tumor segmentation, and kidney tumor segmentation).

* **TransBTS Paper (MICCAI 2021):** [TransBTS: Multimodal Brain Tumor Segmentation Using Transformer](https://arxiv.org/abs/2103.04430)
* **TransBTSV2 Paper (ArXiv 2022):** [TransBTSV2: Towards Better and More Efficient Volumetric Segmentation of Medical Images](https://arxiv.org/abs/2201.12785)
* **Authors:** Wenxuan Wang, Chen Chen, Meng Ding, Hong Yu, Sen Zha, Jiangyun Li (MICCAI 2021 / Springer).

---

## 2. Benchmark Datasets & Acquisition Links

1. **BraTS 2019 & BraTS 2020 (Multimodal Brain Tumor Segmentation)**
   * **Acquisition Portal:** [CBICA Image Processing Portal](https://ipp.cbica.upenn.edu/)
   * **Challenge Portal:** [UPenn CBICA BraTS 2020](https://www.cbica.upenn.edu/sbia/brats2020/) / [Synapse Platform](https://www.synapse.org/#!Synapse:syn25829070)
2. **LiTS 2017 (Liver Tumor Segmentation)**
   * **Acquisition Portal:** [CodaLab LiTS 2017 Challenge](https://competitions.codalab.org/competitions/17094#participate-get-data)
3. **KiTS 2019 (Kidney Tumor Segmentation)**
   * **Acquisition Portal:** [KiTS19 Grand Challenge](https://kits19.grand-challenge.org/data/)

---

## 3. Theoretical Architecture & Flow

Standard 3D U-Nets suffer from limited receptive fields due to local 3D convolution kernels, which fail to capture long-range global spatial relationships across 3D medical volumes. TransBTS bridges this by placing a **3D Vision Transformer (ViT)** inside the bottleneck of a 3D U-Net.

```
                  ┌─────────────────────────────────────────┐
                  │ Input: 4-Channel 3D MRI                 │
                  │ (FLAIR, T1, T1ce, T2) -> [4, 128, 128, 128] │
                  └────────────────────┬────────────────────┘
                                       │
                        ┌──────────────▼──────────────┐
                        │    3D CNN Encoder Subnet    │  (Downsampling x8)
                        │ (Conv3D, ResNet blocks)    │ ───► Skip Connections
                        └──────────────┬──────────────┘
                                       │ Feature Map [512, 16, 16, 16]
                        ┌──────────────▼──────────────┐
                        │   Patch Embedding & Linear   │  Patching: (16x16x16 -> 4096 tokens)
                        │     Positional Encoding     │  Flatten + Linear Projection (dim=512)
                        └──────────────┬──────────────┘
                                       │
                        ┌──────────────▼──────────────┐
                        │ 3D Transformer Bottleneck   │  Multi-Head Self-Attention
                        │   (L=4 Layers, Heads=8)     │  Encoder Blocks
                        └──────────────┬──────────────┘
                                       │ Embedded Features (Token Reshape)
                        ┌──────────────▼──────────────┐
                        │    3D CNN Decoder Subnet    │ ◄─── Skip Connections
                        │ (Transposed Conv3D, U-Net)  │
                        └──────────────┬──────────────┘
                                       │
                  ┌────────────────────▼────────────────────┐
                  │ Output: 4-Class 3D Segmentation Mask    │
                  │  (Background, NCR/NET, ED, ET)          │
                  └─────────────────────────────────────────┘
```

---

## 4. File-by-File Technical Breakdown

### A. Root Execution Modules

#### 1. [`train.py`](file:///e:/Research%20Code/TransBTS/train.py)
* **Purpose:** Main execution entry point for multi-GPU distributed training using PyTorch `torch.distributed` (DDP).
* **Key CLI Arguments:**
  * `--root`: Path to preprocessed data directory.
  * `--lr`: Initial learning rate (Default: `0.0002`).
  * `--batch_size`: Batch size per GPU (Default: `8`).
  * `--end_epoch`: Total training epochs (Default: `1000`).
  * `--gpu`: GPU IDs (e.g. `'0,1,2,3'`).
  * `--num_class`: Number of output segmentation classes (Default: `4`).
* **Workflow:**
  1. Initializes distributed environment (`dist.init_process_group`).
  2. Sets up seed initialization for reproducibility (`seed=1000`).
  3. Instantiates `BraTS` dataloaders for training (`train.txt`) and validation (`valid.txt`).
  4. Builds `TransBTS` model, wraps it in `DistributedDataParallel`.
  5. Optimizes using `torch.optim.Adam` with weight decay (`1e-5`) and Cosine Annealing learning rate scheduling.
  6. Computes loss via `softmax_dice` from `models/criterions.py`.
  7. Logs metrics to TensorBoard (`SummaryWriter`).

#### 2. [`test.py`](file:///e:/Research%20Code/TransBTS/test.py)
* **Purpose:** Main execution entry point for model validation and submission artifact generation.
* **Key Features:**
  * Loads trained model checkpoints.
  * Sets up Test-Time Augmentation (TTA) if `--use_TTA=True`.
  * Calls `validate_softmax` in [`predict.py`](file:///e:/Research%20Code/TransBTS/predict.py).
  * Writes predicted volume segmentations in `.nii` or `.npy` format into output/submission folders for uploading to challenge evaluation portals (e.g., CBICA IPP).

#### 3. [`predict.py`](file:///e:/Research%20Code/TransBTS/predict.py)
* **Purpose:** High-performance sliding-window inference and volume reconstruction.
* **Key Functions:**
  * `tailor_and_concat(x, model)`: Splits large full-size 3D MRI volumes ($240 \times 240 \times 155$) into 8 overlapping sub-patches of size $128 \times 128 \times 128$, feeds them to the model, and seamlessly stitches the predicted output probabilities back together.
  * `one_hot(ori, classes)`: Converts integer voxel class labels to multi-channel one-hot tensor encoding.
  * `dice_score(o, t)` & `mIOU(o, t)`: Computes validation Dice and mean IoU scores.
  * `softmax_output_dice(output, target)`: Evaluates Dice scores across the three clinical target regions:
    * **Whole Tumor (WT):** Labels $\{1, 2, 4\}$
    * **Tumor Core (TC):** Labels $\{1, 4\}$
    * **Enhancing Tumor (ET):** Label $\{4\}$

---

### B. Data Processing Pipeline (`data/`)

#### 1. [`data/preprocess.py`](file:///e:/Research%20Code/TransBTS/data/preprocess.py)
* **Purpose:** Converts raw NIfTI medical images (`.nii.gz`) into fast-loading PyTorch binary pickle objects (`.pkl`).
* **Modalities processed per patient:**
  * `flair`: Fluid Attenuated Inversion Recovery
  * `t1`: T1-weighted
  * `t1ce`: T1-weighted Contrast Enhanced
  * `t2`: T2-weighted
* **Normalizations:** Normalizes non-zero voxel intensity values with Z-score normalization:
  $$\text{voxel}_{\text{norm}} = \frac{x - \mu}{\sigma}$$
* **Output:** Generates `data_f32.pkl` containing normalized image shapes $(4, 240, 240, 155)$ and corresponding ground-truth segmentation masks.

#### 2. [`data/BraTS.py`](file:///e:/Research%20Code/TransBTS/data/BraTS.py)
* **Purpose:** PyTorch `Dataset` implementation (`BraTS`) and data augmentation pipeline.
* **Data Augmentations:**
  * `MaxMinNormalization`: Rescales voxel values to $[0, 1]$.
  * `Random_Crop`: Extracts sub-volumes of shape $(128, 128, 128)$ during training.
  * `Random_Flip`: Random 3D spatial axis flipping (X, Y, Z axes).
  * `Random_intensity_shift`: Adds random Gaussian noise/intensity variations.
  * `ToTensor`: Converts NumPy arrays to `torch.FloatTensor`.

#### 3. [`train.txt`](file:///e:/Research%20Code/TransBTS/data/train.txt) & [`valid.txt`](file:///e:/Research%20Code/TransBTS/data/valid.txt)
* List of patient directory IDs used for splitting dataset into training and validation sets.

---

### C. Neural Network Architecture (`models/`)

#### 1. [`models/TransBTS/TransBTS_downsample8x_skipconnection.py`](file:///e:/Research%20Code/TransBTS/models/TransBTS/TransBTS_downsample8x_skipconnection.py)
* **Main Class:** `TransBTS` / `TransformerBTS`
* **Flow:**
  1. Input shape: $(B, 4, 128, 128, 128)$
  2. Passes input through 3D CNN encoder (`Unet` encoder from `Unet_skipconnection.py`), downsampling features by $8\times$ to shape $(B, 512, 16, 16, 16)$.
  3. Flattens spatial dimensions to sequence tokens: $16 \times 16 \times 16 = 4096$ tokens.
  4. Passes tokens through linear embedding layer and positional encodings.
  5. Feeds tokens through $L=4$ Transformer encoder blocks (`TransformerModel`).
  6. Reshapes 1D Transformer token sequence back into 3D feature tensor shape $(B, 512, 16, 16, 16)$.
  7. Passes features through 3D CNN decoder (`Unet` decoder) with skip connections to compute final output map $(B, 4, 128, 128, 128)$.

#### 2. [`models/TransBTS/Transformer.py`](file:///e:/Research%20Code/TransBTS/models/TransBTS/Transformer.py)
* **Components:**
  * Multi-Head Self-Attention (MHSA) module.
  * Position-wise Feed-Forward Network (FFN).
  * Layer Normalization and Dropout layers.

#### 3. [`models/TransBTS/PositionalEncoding.py`](file:///e:/Research%20Code/TransBTS/models/TransBTS/PositionalEncoding.py)
* Implements both **Learned 1D Positional Embeddings** (`LearnedPositionalEncoding`) and **Fixed Sinusoidal Positional Embeddings** (`FixedPositionalEncoding`) to inject 3D relative positional information into tokenized sequences.

#### 4. [`models/TransBTS/Unet_skipconnection.py`](file:///e:/Research%20Code/TransBTS/models/TransBTS/Unet_skipconnection.py)
* Implements the 3D Convolutional blocks (`ConvBlock3D`), Group Normalizations (`GroupNorm`), ReLU activations, and 3D Transposed Convolutions (`ConvTranspose3d`) forming encoder and decoder branches.

#### 5. [`models/criterions.py`](file:///e:/Research%20Code/TransBTS/models/criterions.py)
* Defines training loss functions:
  * **Dice Loss Formula:**
    $$\mathcal{L}_{\text{Dice}} = 1 - \frac{2 \sum (p \cdot y) + \epsilon}{\sum p + \sum y + \epsilon}$$
  * **Softmax Dice Loss (`softmax_dice`):** Sum of Dice losses across individual target tumor sub-regions.

#### 6. [`utils/tools.py`](file:///e:/Research%20Code/TransBTS/utils/tools.py)
* Helper utilities for PyTorch DDP synchronization (`all_reduce_tensor`) across GPUs.

---

## 5. Summary Matrix of Primary Entry Points

| Workflow Stage | Entry Point Script | Primary Functionality | Example Execution Command |
| :--- | :--- | :--- | :--- |
| **Data Preprocessing** | [`data/preprocess.py`](file:///e:/Research%20Code/TransBTS/data/preprocess.py) | Converts `.nii.gz` scans to `.pkl` normalized volumes | `python data/preprocess.py` |
| **Model Training** | [`train.py`](file:///e:/Research%20Code/TransBTS/train.py) | Trains multi-GPU 3D TransBTS architecture via DDP | `python -m torch.distributed.launch --nproc_per_node=4 --master_port 20003 train.py` |
| **Testing & Inference** | [`test.py`](file:///e:/Research%20Code/TransBTS/test.py) | Evaluates trained weights & generates `.nii` files | `python test.py` |
