## caffe-yolo


## DarkNet
REFENRENCE https://pjreddie.com/darknet/yolo/
### YOLO TEST
1. git clone https://github.com/pjreddie/darknet.git
2. vim Makefile and modify
```
-GPU=0
+GPU=1
 CUDNN=0
-OPENCV=0
+OPENCV=1
```
3. make 
nvcc should be in PATH. (/usr/local/cuda/bin/nvcc)

4. wget http://pjreddie.com/media/files/yolo.weights
5. test
```
./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg
./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg -thresh 0
```
If there is no output, change the cfg/*.cfg
So we should understand the cfg/*.cfg 

-----
### YOLO TRAIN on VOC

1. DATA SOURCE. `cd dartknet && ln -s /home/xxx/VOCdevkit`
2. GENERATE FORMAT DATA. `python scripts/voc_label.py`
```
a. generate VOCdevkit/VOC2007/labels
b. 2012_train.txt 2012_val.txt 2007_train.txt 2007_val.txt 2007_test.txt
c. cat 2007_train.txt 2007_val.txt 2012_*.txt > train.txt
d. cat train.txt 2007_test.txt > train.all.txt
```
  It will generate train and test .txt file for each image with a line for each ground truth object in the image that looks like this:
```
object-class x y width height
10 0.294 0.828 0.58 0.338666666667
```
and `VOCdevkit/VOC2007/labels/008801.txt` is generate like above
008801.txt generates from `VOCdevkit/VOC2007/JPEGImages/008801.jpg` and `VOCdevkit/VOC2007/Annotations/008801.xml`
3. configure cfg/voc.data
4. prepare finetuning weight `wget http://pjreddie.com/media/files/darknet19_448.conv.23`
5. TRAIN. `./darknet detector train cfg/voc.data cfg/yolo-voc.cfg darknet19_448.conv.23`

