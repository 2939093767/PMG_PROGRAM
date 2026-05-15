# Pmg-encoder-Tripletloss

## Introduction
```
This project is used for fine-grained cat and dog image category and individual recognition, 
drawing on the ideas of Progressive Multi Granularity Training and making lightweight 
improvements to the model.

This method performs image shuffling with three different granularities, and trains the three channels separately to implement weighted classification.
```
![界面截图](code/Data_visualization/attention_hotmap/Abyssinian_1.jpg)
![界面截图](code/Data_visualization/attention_hotmap/output_cam_image_1.jpg)
![界面截图](code/Data_visualization/attention_hotmap/output_cam_image_2.jpg)
![界面截图](code/Data_visualization/attention_hotmap/output_cam_image_3.jpg)

## Environment

- command for creat conda environment 

```
conda env create -f environment.yml
```
## Result

```
dataset | inputsize | acc rate | pre rate | recall | f1 score | split_thread | split_acc
---|--- |--- |--- |---
oxford  | 224 | 92.38 | 95.08 | 92.02 | 92.73 | 14.5 | 91.86
oxford  | 448 | 92.80 | 95.91 | 92.80 | 93.53 | 14.5 | 93.28
stanforddogs | 224 | 80.77 | 87.17 | 80.15 | 82.19 | 14.5 | 91.09
stanforddogs | 448 | 83.35 | 89.60 | 83.47 | 85.24 | 14.5 | 93.85
tsinghuadogs | 224 | 77.12 | 84.31 | 77.12 | 79.24 | 14.5 | 94.98
tsinghuadogs | 448 | 77.87 | 84.79 | 77.87 | 79.88 | 14.5 | 95.23
```

## Install
1. Clone the repo

```
git clone https://github.com/2939093767/Pmg-encoder-Tripletloss.git
```

2. Prepare dataset

- Download the following datasets. 

Dataset | Category | images

---|--- |--- |--- |---

[The Oxford-IIIT Pet](https://thor.robots.ox.ac.uk/~vgg/data/pets/images.tar.gz) | Dogs and Cats | 37 | 7349

[Tsinghua Dogs](https://cloud.tsinghua.edu.cn/f/80013ef29c5f42728fc8/?dl=1) | Dogs | 130 | 70428

[Stanford-Dogs](http://vision.stanford.edu/aditya86/ImageNetDogs/images.tar) | Dogs | 120 | 20580

- prepare for the train and test file
```
├── data
│   ├── Oxford
│   ├── Tsinghua Dogs
│   └── Stanford-Dogs
├── metedata
│   ├── Oxford
│       ├── train.txt
│       └── test.txt
│   ├── Tsinghua Dogs
│       ├── train.txt
│       └── test.txt
│   └── Stanford-Dogs
│       ├── train.txt
│       └── test.txt

```

## Usage

- Train
- train network in oxford dataset by 224 input 

```
python main.py  --dataroot ./data/oxford --infopath ./metedata/oxford --nclass 37 --epochs 80 --trainbatch 12 --testbatch 12 --lr 0.0005 --momentum 0.9 --weight_decay 0.0005 --eval_epoch 2 --input_size 224 --split_thread 14.5
```


- Predict
- To predict the categories of two images and determine their similarity by model trained on the Oxford dataset

```
python predict.py --model_path ./model_weight/pmg_oxford_224/best_model_92.245844.pth --input_size 224 --thread 0.8
```

-Use a model trained on the Oxford dataset to predict the category and similarity of two images and save the results along with a heatmap

```
python predict.py --model_path ./model_weight/pmg_oxford_224/best_model_92.245844.pth --input_size 224 --save_image_cam_path ./Data_visualization/attention_hotmap --save_result_image_path ./Data_visualization/result.jpg --thread 0.7

```
