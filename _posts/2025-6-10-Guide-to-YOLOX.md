---
layout: post
title: Guide to Fine-tuning YOLOX (COCO-JSON)
subtitle: My 3 days of suffering so you dont
cover-img: /assets/img/Python2.png
thumbnail-img: /assets/img/CV.png
share-img: /assets/img/Python2.png
tags: [YOLOX]
author: Yash Vijay
---
## Setting up the environment
I suggest you use conda because I had to run it on python 3.7.12!
Newer python versions may work but then you would have to tweak requirements.txt
and all. For Windows or Mac Search up how to. Personally I use linux (Fedora) and 
terminal commands were:

~~~
conda init
conda create -n yolox python=3.7
conda activate yolox
~~~
if you did it right the environment name should come up like this in the terminal:
~~~
(yolox) yash@fedora:~$ 
~~~
## Download the Github files and Model

Yolox is available in Github by Megvii-BaseDetection. [Click here](https://github.com/Megvii-BaseDetection/YOLOX)
for the link. Go to the 'code' section and a drop down menu should appear. Download as a ZIP file.
Extract the ZIP and open it in your IDE or editor (I use Pycharm but a lot of people use VScode,
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
~~~
(yolox) yash@fedora:~/Downloads/YOLOX-main$:
~~~
Here the documentation was a bit off, First of all run the 
~~~
pip3 install -r requirements.txt
~~~
and then 
~~~
python3 setup.py develop
pip3 install cython
~~~
## Dataset Preparation

Make sure your dataset is in the COCO-JSON format and properly labelled, in fact to make life a bit easier
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
~~~
/Downloads/YOLOX-main/datasets/COCO
~~~

## Setting up the exp file
Now most of the heavy lifting is complete. The exp file is basically just.. configurations, like how
are you going to train on this dataset? the parameters can be tweaked and a full list of parameters is
given in the 
~~~
/Downloads/YOLOX-main/yolox/exp/yolox_base.py
~~~
Have a look at all the parameters you see. I suggest you dont modify anything here, rather just see
what all you want can change and just get a general idea of everything.
Next step you are going to head to the 
~~~
/Downloads/YOLOX-main/exps/default
~~~
directory and those are the default training configuration files for different pretrained models.
Look through them and if you feel like modifying anything you can change there only, or make another one store it
somewhere else etc. The important thing is that the exp .py file is what tells yolox how to run what its running.
If this is your first time fine-tuning i suggest you leave it as it is except maybe modify the number of epochs to run.
For example if I was running 30 epochs on a default yolox-s model my yolox_s.py file would look like:
~~~
import os

from yolox.exp import Exp as MyExp


class Exp(MyExp):
    def __init__(self):
        super(Exp, self).__init__()
        self.depth = 0.33
        self.width = 0.50
        self.exp_name = os.path.split(os.path.realpath(__file__))[1].split(".")[0]
        self.max_epoch = 30 #I added this parameter myself onto the default file
~~~
For a look at all the parameters to modify look into the /Downloads/YOLOX-main/yolox/exp/yolox_base.py file. I suggest
you dont change anything there, just put the parameter you wish to modify in the exp file specific to that training run,
dont change the base file.

## Training

Now everything is done,you have downloaded yolox-main, configured the settings and parameters for your training, renamed and
properly made the dataset. Make sure your directory is proper then run the command:
**python tools/train.py -f /path/to/your/Exp/file -d 8 -b 64 --fp16 -o -c /path/to/the/pretrained/weights [--cache]**
~~~
(yolox) yash@fedora:~/Downloads/YOLOX-main$:python tools/train.py -f /path/to/your/Exp/file -d 8 -b 64 --fp16 -o -c /path/to/the/pretrained/weights [--cache]
~~~
if you encounter memory errors ask chatgpt or perplexity for a proper max_split or any other error, there can be
some cuda mismatch. It should start training then.
If you have any doubts feel free to hit me up at [here](https://www.linkedin.com/in/yashvija/)
