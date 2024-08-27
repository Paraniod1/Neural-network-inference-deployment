# NCNN入门之编译与安装

## 一、NCNN介绍

ncnn 是一个为手机端极致优化的高性能神经网络前向计算框架。 ncnn 从设计之初深刻考虑手机端的部署和使用。 无第三方依赖，跨平台，手机端 cpu 的速度快于目前所有已知的开源框架。 基于 ncnn，开发者能够将深度学习算法轻松移植到手机端高效执行， 开发出人工智能 APP，将 AI 带到你的指尖。 ncnn 目前已在腾讯多款应用中使用，如：QQ，Qzone，微信，天天 P 图等。

<img src="https://repository-images.githubusercontent.com/494294418/207a2e12-dc16-41a6-a39e-eae26e662638" alt="NCNN overview" style="zoom:80%;" />

**功能概述**

1. 支持卷积神经网络，支持多输入和多分支结构，可计算部分分支
2. 无任何第三方库依赖，**不依赖 BLAS/NNPACK 等计算框架**
3. 纯 C++ 实现，跨平台，支持 Android / iOS 等
4. ARM Neon 汇编级良心优化，计算速度极快
5. 精细的内存管理和数据结构设计，内存占用极低
6. 支持多核并行计算加速，ARM big.LITTLE CPU 调度优化
7. 支持基于全新低消耗的 Vulkan API GPU 加速
8. 可扩展的模型设计，支持 8bit [量化](https://github.com/Tencent/ncnn/blob/master/tools/quantize) 和半精度浮点存储，可导入 caffe/pytorch/mxnet/onnx/darknet/keras/tensorflow(mlir) 模型
9. 支持直接内存零拷贝引用加载网络模型
10. 可注册自定义层实现并扩展

总之NCNN 是一个专门为移动设备和嵌入式系统设计的高效神经网络推理框架，以其轻量级、高性能、跨平台和易用性赢得了广泛的应用。它特别适合那些需要在资源有限的环境中部署深度学习模型的场景。





## 二、NCNN编译



使用以下命令安装所有必需的依赖项：

```shell
sudo apt install build-essential git cmake libprotobuf-dev protobuf-compiler libomp-dev libvulkan-dev vulkan-utils libopencv-dev
```

NCNN源码下载：

```shell
git clone https://github.com/Tencent/ncnn.git
cd ncnn 
git submodule update --init
```

<img src="C:\Users\75417\AppData\Roaming\Typora\typora-user-images\image-20240823170901902.png" alt="image-20240823170901902" style="zoom:80%;" />

进入 NCNN 源码目录后，使用 CMake 配置和编译项目：

```shell
#在ncnn主目录下
mkdir build && cd build
cmake .. -DNCNN_BENCHMARK=ON -DNCNN_VULKAN=ON
make -j8
```

这部分代码是ncnn的主目录下CMakeLists.txt里的内容，用于**控制构建选项**，`设置NCNN_BENCHMARK=ON可以打印出每个算子的耗时，NCNN_VULKAN=ON可以开启vulkan加速`。



<img src="C:\Users\75417\AppData\Roaming\Typora\typora-user-images\image-20240823172102726.png" alt="image-20240823172102726" style="zoom:50%;" />

成功编译，对编译后的库进行安装，默认安装的根目录是：`your_dir/ncnn/build/install/`,也就是会在`build`目录下新建一个`install`目录来安装。

```shell
sudo make install
```

<img src="C:\Users\75417\AppData\Roaming\Typora\typora-user-images\image-20240823172605832.png" alt="image-20240823172605832" style="zoom:80%;" />zhi





## 三、NCNN测试

安装成功后检查是否环境是否正确。

```shell
cd ../examples
../build/examples/squeezenet ../images/256-ncnn.png
```

运行结果：

<img src="C:\Users\75417\AppData\Roaming\Typora\typora-user-images\image-20240823172918660.png" alt="image-20240823172918660" style="zoom:50%;" />

至此ncnn的编译与安装已经完成。



## 四、参考文档

- 如果在其它平台(arm、树莓派、安卓)编译可以参考如下：https://github.com/Tencent/ncnn/wiki/how-to-build
- 编译成最小体积：https://github.com/Tencent/ncnn/wiki/build-minimal-library
- 学习文档：https://github.com/Tencent/ncnn/wiki
- ncnn文档：https://ncnn.readthedocs.io/en/latest/index.html



