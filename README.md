# DeepInterestNetwork
Deep Interest Network for Click-Through Rate Prediction
## Important Statement
This code is a demo to implement DIN on Amazon data. Unfortunately, the code
quality is realy poor and there are many problems with this version of the code. Thus, we
did major code refactoring and publish the new version of code in [DIEN](https://github.com/mouna99/dien) and [XDL](https://github.com/alibaba/x-deeplearning/tree/master/xdl-algorithm-solution/DIEN/script).
We strongly recommend you to use [DIEN](https://github.com/mouna99/dien).
Moreover, the wrong way to apply BatchNorm in DIN brings a faulty experimental
result. We fix the bug and report the new results of DIN on Amazon(Electro):

| Model | GAUC|
| ------ | ------ |
|PNN|0.8679|
|deepFM|0.8683|
| DIN  |0.8698 | 
| DIN with Dice | 0.8711|

The updated training log can be found in [DIN](https://github.com/zhougr1993/DeepInterestNetwork/tree/master/din)
## Introduction
This is an implementation of the paper [Deep Interest Network for Click-Through Rate Prediction](https://arxiv.org/abs/1706.06978) Guorui Zhou, Chengru Song, Xiaoqiang Zhu, Han Zhu, Ying Fan, Na Mou, Xiao Ma, Yanghui Yan, Xingya Dai, Junqi Jin, Han Li, Kun Gai

Thanks Jinze Bai and Chang Zhou.

Bibtex:
```sh
@article{Zhou2017Deep,
  title={Deep Interest Network for Click-Through Rate Prediction},
  author={Zhou, Guorui and Song, Chengru and Zhu, Xiaoqiang and Ma, Xiao and Yan, Yanghui and Dai, Xingya and Zhu, Han and Jin, Junqi and Li, Han and Gai, Kun},
  year={2017},
}
```

## Requirements
* Python >= 2.6.1
* NumPy >= 1.12.1
* Pandas >= 0.20.1
* TensorFlow >= 1.4.0 (Probably earlier version should work too, though I didn't test it)
* GPU with memory >= 10G

## Download dataset and preprocess
* Step 1: Download the amazon product dataset of electronics category, which has 498,196 products and 7,824,482 records, and extract it to `raw_data/` folder.
```sh
mkdir raw_data/;
cd utils;
bash 0_download_raw.sh;
```
* Step 2: Convert raw data to pandas dataframe, and remap categorical id.
```sh
python 1_convert_pd.py;
python 2_remap_id.py
```

## Training and Evaluation
This implementation not only contains the DIN method, but also provides all the competitors' method, including Wide&Deep, PNN, DeepFM. The training procedures of all method is as follows:
* Step 1: Choose a method and enter the folder.
```
cd din;
```
Alternatively, you could also run other competitors's methods directly by `cd deepFM` `cd pnn` `cd wide_deep`,
and follow the same instructions below.

* Step 2: Building the dataset adapted to current method.
```
python build_dataset.py

We put a processed data 'dataset.pkl' in DeepInterestNetwork/din. Considering the GitHub's file size limit of 100.00 MB, we split it into 3 file aa ab ac.

cat aa ab ac > dataset.pkl


```
* Step 3: Start training and evaluating using default arguments in background mode. 
```
python train.py >log.txt 2>&1 &
```
* Step 4: Check training and evaluating progress.
```
tail -f log.txt
tensorboard --logdir=save_path
```

## Dice
There is also an implementation of Dice in folder 'din', you can try dice following the code annotation in `din/model.py` or replacing model.py with model\_dice.py
# DIN
