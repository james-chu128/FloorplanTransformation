# Raster-to-Vector: Revisiting Floorplan Transformation
By Chen Liu, Jiajun Wu, Pushmeet Kohli, and Yasutaka Furukawa

## Introduction

This paper addresses the problem of converting a rasterized
floorplan image into a vector-graphics representation.
Our algorithm significantly outperforms
existing methods and achieves around 90% precision and
recall, getting to the range of production-ready performance. 
To learn more, please see our ICCV 2017 [paper](https://www.cse.wustl.edu/~chenliu/floorplan-transformation/paper.pdf) or visit our [project website](https://www.cse.wustl.edu/~chenliu/floorplan-transformation.html).

This code implements the algorithm described in our paper in Torch7.

## Requirements

- Please install the latest Torch
- Please install Python 2.7

### Torch packages
- [nn](https://github.com/torch/nn)
- [cunn](https://github.com/torch/cunn)
- [cudnn](https://github.com/soumith/cudnn.torch)
- [image](https://github.com/torch/image)
- [ffi](http://luajit.org/ext_ffi.html)
- [csvigo](https://github.com/clementfarabet/lua---csv)
- [penlight](https://github.com/stevedonovan/Penlight)
- [opencv](https://github.com/marcoscoffier/lua---opencv)
- [lunatic-python](https://labix.org/lunatic-python)

### Python packages
- [numpy](http://www.scipy.org/scipylib/download.html)
- [Gurobi](http://www.gurobi.com)
- [OpenCV](https://opencv.org/)

## Trained models
To use our trained model, please first download it from [Google Drive](https://drive.google.com/file/d/0B2rs82y7tjKrQk0yRFB3RHVDUXM/view?usp=sharing), and put it under folder "checkpoint/" (or specify the its path via option -loadModel="path to the downloaded model").

Our model is fine-tuned based on the pose estimation network introduced in the paper, "Human pose estimation via Convolutional Part Heatmap Regression". You can downloaded their model [here](https://www.adrianbulat.com/human-pose-estimation) (the MPII one), and put it under folder "PoseEstimation/" (or specify the its path via option -loadPoseEstimationModel)

## Data
Our vector-graphics annotation is under "data/" folder. Lists of (raster floorplan image path, vector-graphics annotation path) can be found in either "train.txt", "val.txt", or "test.txt'.

The link to 100,000+ vector-graphics representation generated by our algorithm is coming soon. Please contact me (chenliu@wustl.edu) if you want to use them right now.

## Usage
To train the network from the pretrained pose estimation network, simply run
```bash
th main.lua -loadPoseEstimationModel  "path to the downloaded pose estimation model"
```

To load our trained model and resume training, please run
```bash
th main.lua -loadModel  "path to the downloaded pretrained model"
```

Here are som useful options for the main script:
- -batchSize  specifies the batch size
- -LR specifies the learning rate
- -nEpochs  specifies the number of epochs
- -checkpointEpochInterval  specifies the number of training epochs between two checkpoints (useful if you want to save less number of checkpoints instead of saving one checkpoint for every epoch)
- useCheckpoint specifies how the training resumes
  * -1: starting from the beginning even when checkpoints previously trained are found
  * 0 (default) resuming from checkpoints if found
  * n (n > 0) resuming from the nth checkpoint

To make prediction on a floorplan image, run
```bash
th predict.lua -loadModel "model path" -floorplanFilename "path to the floorplan image" -outputFilename "output filename"
```
Note that the above script will produce the vectorization result (saved in ".txt" file) as well as the rendering image (saved in ".png" file).

To evaluate performance on the benchmark, run
```bash
th evaluate.lua -loadModel "model path" -resultPath "path to save results"
```

## Contact

If you have any questions, please contact me at chenliu@wustl.edu.
