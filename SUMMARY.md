# Summary

## DARKNET

* [DarkNet](README.md)  <!-- 완료 -->
* [설치하기](INSTALL.md) <!-- 완료 -->

## Paper

* [YOLOv1](part1_paper/yolov1.md) <!-- 완료 -->
* [YOLOv2](part1_paper/yolov2.md) <!-- 완료 -->
* [YOLOv3](part1_paper/yolov3.md) <!-- 완료 -->
* [YOLOv4](part1_paper/yolov4.md) <!-- 완료 -->

## SOURCE

* [project](/part2_source/0_Project.md)  <!-- 완료 -->
* [/data](/part2_source/1_DATA.md)       <!-- 완료 -->
* [/cfg](/part2_source/2_CFG.md)         <!-- 완료 -->
* [/example](/part2_source/3_EXAMPLE.md) <!-- 완료 -->
* [/src](/part2_source/4_SRC.md)         <!-- 완료 -->

## Structure

* [network](/part4_structure/network.md)
* [layer](/part4_structure/layer.md)
* [etc](/part4_structure/etc.md)

## /SRC

* [activation_layer.c](/part3_src/activation_layer.md)        <!-- 완료 -->
* [activations.c](/part3_src/activations_1.md)                <!-- 완료 -->
  + [activation.h](/part3_src/activations_2.md)               <!-- 완료 -->
* [avgpool_layer.c](/part3_src/avgpool.md)                    <!-- 완료 -->
* [batchnorm_layer.c](/part3_src/batchnorm_layer.md)          <!-- 완료 -->
* [blas.c](/part3_src/blas.md)                                <!-- 완료 -->
* [box.c](/part3_src/box.md)                                  <!-- 완료 -->
* [col2im.c](/part3_src/col2im.md)                            <!-- 완료 -->
* [compare.c](/part3_src/compare.md)
* [connected_layer.c](/part3_src/connected_layer.md)          <!-- 완료 -->
* [convolutional_layer.c](/part3_src/convolutional_layer.md)  <!-- 완료 -->
* [cost_layer.c](/part3_src/cost_layer.md)                    <!-- 완료 -->
* [crnn_layer.c](/part3_src/crnn_layer.md)                    <!-- 완료 -->
* [crop_layer.c](/part3_src/crop_layer.md)                    <!-- 완료 -->
* [data.c](/part3_src/data.md)                                <!-- 완료 -->
* [deconvolutional_layer.c](/part3_src/deconvolutional_layer.md)
* [demo.c](/part3_src/demo.md)
* [detection_layer.c](/part3_src/detection_layer.md)
* [dropout_layer.c](/part3_src/dropout_layer.md)
* [gemm.c](/part3_src/gemm.md)                                <!-- 완료 -->
* [gru_layer.c](/part3_src/gru_layer.md)
* [im2col.c](/part3_src/im2col.md)                            <!-- 완료 -->
* [image.c](/part3_src/image.md)                              <!-- 완료 -->
* [iseg_layer.c](/part3_src/iseg_layer.md)
* [l2norm_layer.c](/part3_src/l2norm_layer.md)
* [layer.c](/part3_src/layer.md)                              <!-- 완료 -->
* [list.c](/part3_src/list.md)                                <!-- 완료 -->
* [local_layer.c](/part3_src/local_layer.md)
* [logistic_layer.c](/part3_src/logistic_layer.md)
* [lstm_layer.c](/part3_src/lstm_layer.md)                    <!-- 완료 -->
* [matrix.c](/part3_src/matrix.md)                            <!-- 완료 -->
* [maxpool_layer.c](/part3_src/maxpool.md)                    <!-- 완료 -->
* [network.c](/part3_src/network.md)
* [normalization_layer.c](/part3_src/normalization_layer.md)
* [option_list.c](/part3_src/option_list.md)
* [parser.c](/part3_src/parser_1.md)                          <!-- 완료 -->
  + [+ parser.c](/part3_src/parser_2.md)                      <!-- 완료 -->
* [region_layer.c](/part3_src/region_layer.md)
* [reorg_layer.c](/part3_src/reorg_layer.md)
* [rnn_layer.c](/part3_src/rnn_layer.md)                      <!-- 완료 -->
* [route_layer.c](/part3_src/route_layer.md)
* [shortcut_layer.c](/part3_src/shortcut.md)                  <!-- 완료 -->
* [softmax_layer.c](/part3_src/softmax_layer.md)              <!-- 완료 -->
* [tree.c](/part3_src/tree.md)
* [upsample_layer.c](/part3_src/upsample_layer.md)            <!-- 완료 -->
* [utils.c](/part3_src/utils.md)                              <!-- 완료 -->
* [yolo_layer.c](/part3_src/yolo_layer.md)

* [image_opencv.cpp](/part3_src/image_opencv.md)

## /EXAMPLE

* [art.c](/part5_examples/art.md)
* [attention.c](/part5_examples/attention.md)
* [captcha.c](/part5_examples/captcha.md)
* [cifar.c](/part5_examples/cifar.md)
* [classifier.c](/part5_examples/classifier.md)
* [coco.c](/part5_examples/coco.md)
* [darknet.c](/part5_examples/darknet.md)
* [detector.c](/part5_examples/detector.md)
* [dice.c](/part5_examples/dice.md)
* [go.c](/part5_examples/go.md)
* [instance-segmenter.c](/part5_examples/instance-segmenter.md)
* [lsd.c](/part5_examples/lsd.md)
* [nightmare.c](/part5_examples/nightmare.md)
* [regressor.c](/part5_examples/regressor.md)
* [rnn.c](/part5_examples/rnn.md)
* [rnn_vid.c](/part5_examples/rnn_vid.md)
* [segmenter.c](/part5_examples/segmenter.md)
* [super.c](/part5_examples/super.md)
* [swag.c](/part5_examples/swag.md)
* [tag.c](/part5_examples/tag.md)
* [voxel.c](/part5_examples/voxel.md)
* [writing.c](/part5_examples/writing.md)
* [yolo.c](/part5_examples/yolo.md)

<!-- ## CUDA

* [cuda.c]()
* [avgpool_layer_kernels.cu]()
* [maxpool_layer_kernels.cu]()
* [im2col_kernels.cu]()
* [blas_kernels.cu]()
* [activation_kernels.cu]()
* [col2im_kernels.cu]()
* [convolutional_kernels.cu]()
* [crop_layer_kernels.cu]()
* [deconvolutional_kernels.cu]()
* [dropout_layer_kernels.cu]() -->
