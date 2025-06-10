---
layout: post
title: Guide to Fine-tuning yolox (COCO-JSON)
subtitle: My 3 days of suffering so you dont
cover-img: /assets/img/python1.jpg
thumbnail-img: /assets/img/pythonlogo.png
share-img: /assets/img/python1.jpg
tags: [YOLOX]
author: Yash Vijay
---
## Setting up the enviroment
I suggest you use conda because I had to run it on python 3.7.12!
Newer python versions may work but then you would have to tweak requirements.txt
and all. For Windows or Mac Search up how to. Personally I use linux (Fedora) and 
terminal commands were:

~~~
conda init
conda create -n yolox python=3.7
conda activate yolox
~~~
if you did it right the enviroment name should come up like this in the terminal:
~~~
(yolox) yash@fedora:~$ 
~~~
## Download the Github files and Model

Yolox is available in Github by Megvii-BaseDetection. [Click here](https://github.com/Megvii-BaseDetection/YOLOX)
for the link. Go to the 'code' section and a drop down menu should appear. Download as a ZIP file.
Extract the ZIP and open it in your IDE or editor (I use Pycharm but alot of people use VScode, 
should work with anything really).

Once you have opened the project open your terminal, make sure your directories are correct,
it should look something like this:
~~~
(yolox) yash@fedora:~/Downloads/YOLOX-main$:
~~~
Next step would be to download a pretrained model.. Choose your model based on what you need. Whether its a 
yolox-Nano, Tiny, Medium, Large.. etc. 

|Model |size |mAP<sup>val<br>0.5:0.95 |mAP<sup>test<br>0.5:0.95 | Speed V100<br>(ms) | Params<br>(M) |FLOPs<br>(G)| weights |
| ------        |:---: | :---:    | :---:       |:---:     |:---:  | :---: | :----: |
|[YOLOX-s](./exps/default/yolox_s.py)    |640  |40.5 |40.5      |9.8      |9.0 | 26.8 | [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_s.pth) |
|[YOLOX-m](./exps/default/yolox_m.py)    |640  |46.9 |47.2      |12.3     |25.3 |73.8| [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_m.pth) |
|[YOLOX-l](./exps/default/yolox_l.py)    |640  |49.7 |50.1      |14.5     |54.2| 155.6 | [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_l.pth) |
|[YOLOX-x](./exps/default/yolox_x.py)   |640   |51.1 |**51.5**  | 17.3    |99.1 |281.9 | [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_x.pth) |
|[YOLOX-Darknet53](./exps/default/yolov3.py)   |640  | 47.7 | 48.0 | 11.1 |63.7 | 185.3 | [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_darknet.pth) |
|[YOLOX-Nano](./exps/default/yolox_nano.py) |416  |25.8  |N.A  |N.A  | 0.91 |1.08 | [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_nano.pth) |
|[YOLOX-Tiny](./exps/default/yolox_tiny.py) |416  |32.8 |N.A  |N.A  |5.06 |6.45 | [github](https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_tiny.pth) |

Install it and you can keep it anywhere, just know the location of it. Preferably in the same project folder.

## Install the Dependencies
Make sure your terminal looks like 
```
(yolox) yash@fedora:~/Downloads/YOLOX-main$:
```
Here the documentation was a bit off, First of all run the 
```
pip3 install -r requirements.txt
```
and then 
```
python3 setup.py develop
pip3 install cython
```
## Dataset Preparation

Make sure your dataset is in the COCO-JSON format and properly labelled, infact to make life a bit easier
I suggest you rename the test,train,validation folders according to this format:
```css
COCO/
├── annotations/
│   ├── instances_train2017.json
│   ├── instances_test2017.json
│   ├── instances_val2017.json
├── test2017/
│   └── your_image.jpg
├── train2017/
│   └── your_image.jpg
└── val2017/
    └── your_image.jpg
```
Like the dataset is called "coco", train folder is "instances_train2017.json" etc.
You only need to rename the folders and .json files, jpg images and content in the .json
do not need to be changed.

Once it seems correct, place the "COCO" named dataset into the YOLOX-main/datasets folder, filepath
should be something like:
```
/Downloads/YOLOX-main/datasets/COCO
```

## Setting up the exp file
Now most of the heavy lifting is complete.



## Realizing how PRS work for Cpython

Man did I underestimate the making a "pull request" that too on cpython. Despite it being a documentation fix, I was grilled a little on why 
"starred_expression_list" should be a good fix, me constantly changing small things like typos unrelated to the PR (they take their one fix
at a time policy very seriously even if you have small typo changes on documentation). Each line has a maximum character limit. Basically
it took literally 5 days of going back and forth, literally 44 comments and 26 commits to finally get it merged. But boy how glad I felt
afterwards is something only I know.. I was one of the 3,000 people who have fixed an inconsistency between python.gram and its documentation. 
All from catching the issues to fixing it on my own. [The Merged PR is here](https://github.com/python/cpython/pull/134034).

Hopefully this is just a beginning to a killer profile work on The official Interpreter of python.
