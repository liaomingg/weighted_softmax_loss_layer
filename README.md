## WeightedSoftmaxWithLossLayer

### Introduction

WeightedSoftmaxLossLayer for Caffe is modified from SoftmaxWithLossLayer in Caffe. You could set the class weight of each class in the prototxt file. 
If it helps your research, please consider give me a start.

### Add to Caffe

1. Clone this repository
    ```Shell
    git clone https://github.com/liaomingg/weighted_softmax_loss_layer.git
    ```

2. Modify your caffe.proto
    ```
    // Add the next contents to message SoftmaxParameter
    message SoftmaxParameter {
        // the next added for WeightedSoftmaxWithLossLayer
        // pos_mult: class weight, default = 1
        // pos_cid: class id, specify the class id repected to the class weight.
        repeated float pos_mult = 3;    // class weight, you should select appropriate id, such as 3.
        repeated float pos_cid = 4;     // class id
    }
    ```

3. Add source files to your Caffe
    Add weighted_softmax_loss_layer.hpp to Caffe/include/caffe/layers/
    Add weighted_softmax_loss_layer.cpp to Caffe/src/caffe/layers/
    Add weighted_softmax_loss_layer.cu to Caffe/src/caffe/layers/

4. Compile your Caffe

### How to use this layer

1. Write codes such as follows in your prototxt file.
    ```
    layer {
        name: "weighted_loss"
        type: "WeightedSoftmaxWithLoss"
        bottom: "conv_cls"
        bottom: "label"
        top: "loss"
        include {
            phase: TRAIN
        }
        loss_param {
            ignore_label: 255
        }
        softmax_param {
            pos_cid: 0          # for class 0
            pos_mult: 2.3434    # the weight of class 0 is 2.3434
            pos_cid: 1          # for class 1
            pos_mult: 1.23456   # the weight of class 1 is 1.23456
                                # and etc.
        }
    }
    ```
