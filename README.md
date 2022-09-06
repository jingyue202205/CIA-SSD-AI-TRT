# CIA-SDD-AI-TRT

CIA-SSD-AI-TRT(CIA-SSD ALL IN TensorRT,NMS not implemented in TensorRT,implemented in c++) 
CIA-SSD consists of five parts:
- preprocess: generate voxel, it is implemented in voxelGenerator.cu,it is a TensorRT plugin
- 3D backbone: 3D backbone include 3D sparse conv and 3D subm conv. sparseConv3dlayer.cu is a TensorRT plugin for 3D sparse conv, and submConv3dlayer.cu is a TensorRT plugin for 3D subm conv.
- neck: this part is mainy implemented by TensorRT aip, because they are all general modules. the function of sparse2Dense.cu is  from sparse tensor to dense tensor
- head: this part is mainy implemented by TensorRT aip.
- postprocess: it includes anchorGenerate and decoder, they are implemented by generateAnchorDecode.cu, it is also a plugin.
- 3D NMS: it comes from  https://github.com/NVIDIA-AI-IOT/CUDA-PointPillars/blob/main/src/postprocess.cpp

## Config

- all config in params.h
- FP16/FP32 can be selected by USE_FP16
- GPU id can be selected by DEVICE
- NMS thresh in params.h

## How to Run

1. build tensorrtx/yolov3-spp and run

```
cd CIA-SSD-AI-TRT
mkdir build
cd build
cmake ..
make
sudo ./cia-ssd-ai-trt -s             // serialize model to plan file i.e. 'cia-ssd-ai-trt.engine'
sudo ./cia-ssd-ai-trt -d    // deserialize plan file and run inference, the images in samples will be processed.
**one frame takes 1-2 seconds, it is very slow, needs to be optimized in the future.**
```

2. check the outputs generated

```
frist install python moudles by tools/requirements.txt
cd tools
python show_box_in_points.py, type C, you can show lidar points and boxes one by one,

```

## More Information

See the readme in [home page.](https://github.com/wang-xinyu/tensorrtx)

