# OCRNet

[Object-Contextual Representations for Semantic Segmentation](https://arxiv.org/pdf/1909.11065)

## Introduction

OCRNet: object-contextual representations, characterizing a pixel by exploiting the representation of the corresponding object class,achieves competitive performance on various benchmarks: Cityscapes, ADE20K, LIP, PASCAL-Context and COCO-Stuff. 1st place on the
Cityscapes leaderboard on the ECCV 2020

## Requirement

| mindspore | ascend driver |  firmware   | cann toolkit/kernel |
|:---------:|:-------------:|:-----------:|:-------------------:|
|   2.3.1   |   24.1.rc2    | 7.3.0.1.231 |    8.0.RC2.beta1    |

  - OCRNet is implemented based on [mindcv 0.3.0](https://github.com/mindspore-lab/mindcv)
  - Install dependencies: pip install -r requirements.txt

## Data preparation

you need to download [cityscapes](https://www.cityscapes-dataset.com/)

your directory tree should be look like this:：

```text
└── Cityscapes
     ├── leftImg8bit
     |    ├── train
     |    ├── test
     |    ├── val
     └── gtFine
          ├── train
          ├── test
          └── val
```

## Training

```shell
# single card
python train.py --config config/ocrnet/config_ocrnet_hrw32_16k.yml
```

```shell
# multiple cards using openmpi
mpirun --allow-run-as-root -n 8 
    python train.py --config config/ocrnet/config_ocrnet_hrw32_16k.yml
```

Training on modelarts:

```text
1. Configure the ModelArts parameter in the config file:

- set enable_modelarts=True
- set OBS dataset path data_url: <the path of the dataset in OBS>
- set OBS output results path train_url: <The path of output results in OBS>

2. Refer to [ModelArts](https://support.huaweicloud.com/modelarts/index.html) start training.
```

## evaluation

```shell
# single card
python eval.py --config config/ocrnet/config_ocrnet_hrw32_16k.yml --ckpt_path [downloaded_ckpt]

# multiple cards using openmpi
mpirun --allow-run-as-root -n 8 
    python eval.py --config config/ocrnet/config_ocrnet_hrw32_16k.yml --ckpt_path [downloaded_ckpt]
```

## resume training

```shell
python train.py --config config/ocrnet/config_ocrnet_hrw32_16k.yml --resume_ckpt [ckpt_to_resume]
```

## Performance
Performance tested on Ascend 910(8p) with graph mode.

| model name | backbone  | cards | batch size | resolution | jit level | graph compile | s/step | img/s | mIoU  | mIoU(ms) |                      recipe                       |                                          weights                                           |
|:----------:|:---------:|:-----:|:----------:|:----------:|:---------:|:-------------:|:------:|:-----:|:-----:|:--------:|:-------------------------------------------------:|:------------------------------------------------------------------------------------------:|
| OCRNet_w48 | hrnet_w48 |   8   |     2      | 1024x2048  |    O0     |     549s      |  0.21  | 75.00 | 82.07 |  82.96   | [yaml](config/ocrnet/config_ocrnet_hrw48_16k.yml) | [weight](https://download.mindspore.cn/model_zoo/official/cv/ocrnet/OCRNet_hrw48_16k.ckpt) |
| OCRNet_w32 | hrnet_w32 |   8   |     2      | 1024x2048  |    O0     |     553s      |  0.20  | 79.00 | 81.13 |  82.27   | [yaml](config/ocrnet/config_ocrnet_hrw32_16k.yml) | [weight](https://download.mindspore.cn/model_zoo/official/cv/ocrnet/OCRNet_hrw32_16k.ckpt) |

## Citation

```sheel
@article{YuanCW20,
  title={Object-Contextual Representations for Semantic Segmentation},
  author={Yuhui Yuan and Xilin Chen and Jingdong Wang},
  booktitle={ECCV},
  year={2020}
}
```
