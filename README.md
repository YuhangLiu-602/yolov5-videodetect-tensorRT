# yolov5

The Pytorch implementation is [ultralytics/yolov5](https://github.com/ultralytics/yolov5).

## information of this version

this project is based on this version 
(https://github.com/wang-xinyu/tensorrtx/tree/yolov5-v4.0/yolov5).

add the vedio detect

## Config

- Choose the model s/m/l/x/s6/m6/l6/x6 from command line arguments.
- Input shape defined in yololayer.h
- Number of classes defined in yololayer.h, **DO NOT FORGET TO ADAPT THIS, If using your own model**
- INT8/FP16/FP32 can be selected by the macro in yolov5.cpp, **INT8 need more steps, pls follow `How to Run` first and then go the `INT8 Quantization` below**
- GPU id can be selected by the macro in yolov5.cpp
- NMS thresh in yolov5.cpp
- BBox confidence thresh in yolov5.cpp
- Batch size in yolov5.cpp

## How to Run, yolov5s as example

1. generate .wts from pytorch with .pt, or download .wts from model zoo

```
git clone -b v5.0 https://github.com/ultralytics/yolov5.git
cp {tensorrtx}/yolov5/gen_wts.py {ultralytics}/yolov5
cd {ultralytics}/yolov5
python gen_wts.py yolov5s.pt
// a file 'yolov5s.wts' will be generated.
```

2. build tensorrtx/yolov5 and run

```
cd {tensorrtx}/yolov5/
// update CLASS_NUM in yololayer.h if your model is trained on custom dataset
mkdir build
cd build
cp {ultralytics}/yolov5/yolov5s.wts {tensorrtx}/yolov5/build
cmake ..
make
sudo ./yolov5_v -s [.wts] [.engine] [s/m/l/x/s6/m6/l6/x6 or c/c6 gd gw]  // serialize model to plan file
sudo ./yolov5_v -d [.engine] [i/v] [image folder/vedio path]  // deserialize and run inference, the images in [image folder] will be processed.
// For example yolov5s
sudo ./yolov5_v -s yolov5s.wts yolov5s.engine s
sudo ./yolov5_v -d yolov5s.engine i ../samples
or
sudo ./yolov5_v -d yolov5s.engine v ../vedio/test.mp4


## More Information

See the readme in [csdn]

