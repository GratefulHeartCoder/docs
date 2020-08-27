# 模型转换工具

<!-- TOC -->

- [模型转换工具](#模型转换工具)
    - [概述](#概述)
    - [环境准备](#环境准备)
    - [参数说明](#参数说明)
    - [模型可视化](#模型可视化)
    - [使用示例](#使用示例)

<!-- /TOC -->

<a href="https://gitee.com/mindspore/docs/blob/master/lite/tutorials/source_zh_cn/use/converter_tool.md" target="_blank"><img src="../_static/logo_source.png"></a>

## 概述

MindSpore Lite提供离线转换模型功能的工具，支持多种类型的模型转换，同时提供转化后模型可视化的功能，转换后的模型可用于推理。命令行参数包含多种个性化选项，为用户提供方便的转换途径。

目前支持的输入格式有：MindSpore、TensorFlow Lite、Caffe和ONNX。

## 环境准备

使用MindSpore Lite模型转换工具，需要进行如下环境准备工作。

- 编译：模型转换工具代码在MindSpore源码的`mindspore/lite/tools/converter`目录中，参考部署文档中的[环境要求](https://www.mindspore.cn/lite/docs/zh-CN/master/deploy.html#id2)和[编译示例](https://www.mindspore.cn/lite/docs/zh-CN/master/deploy.html#id5)，安装编译依赖基本项与模型转换工具所需附加项，并编译x86_64版本。

- 运行：参考部署文档中的[输出件说明](https://www.mindspore.cn/lite/docs/zh-CN/master/deploy.html#id4)，获得`converter`工具，并配置环境变量。

## 参数说明

使用`./converter_lite <args>`即可完成转换，同时提供了多种参数设置，用户可根据需要来选择使用。
此外，用户可输入`./converter_lite --help`获取实时帮助。

下面提供详细的参数说明。

| 参数  |  是否必选   |  参数说明  | 取值范围 | 默认值 |
| -------- | ------- | ----- | --- | ---- |
| `--help` | 否 | 打印全部帮助信息。 | - | - |
| `--fmk=<FMK>`  | 是 | 输入模型的原始格式。 | MS、CAFFE、TFLITE、ONNX | - |
| `--modelFile=<MODELFILE>` | 是 | 输入模型的路径。 | - | - |
| `--outputFile=<OUTPUTFILE>` | 是 | 输出模型的路径（不存在时将自动创建目录），不需加后缀，可自动生成`.ms`后缀。 | - | - |
| `--weightFile=<WEIGHTFILE>` | 转换Caffe模型时必选 | 输入模型weight文件的路径。 | - | - |
| `--quantType=<QUANTTYPE>` | 否 | 设置模型的训练类型 | PostTraining：训练后量化<br>AwareTraining：感知量化。 | - |

> - 参数名和参数值之间用等号连接，中间不能有空格。
> - Caffe模型一般分为两个文件：`*.prototxt`模型结构，对应`--modelFile`参数；`*.caffemodel`模型权值，对应`--weightFile`参数。

## 模型可视化

模型可视化工具提供了一种查验模型转换结果的方法。用户可使用Json命令生成`*.json`文件，与原模型相对比，确定转化效果。

TODO: 此功能还在开发中。

## 使用示例

首先，在源码根目录下，输入命令进行编译，可参考`deploy.md`。
```bash
bash build.sh -I x86_64
```
> 目前模型转换工具仅支持x86_64架构。

下面选取了几个常用示例，说明转换命令的使用方法。

- 以Caffe模型LeNet为例，执行转换命令。

   ```bash
   ./converter_lite --fmk=CAFFE --modelFile=lenet.prototxt --weightFile=lenet.caffemodel --outputFile=lenet
   ```

   本例中，因为采用了Caffe模型，所以需要模型结构、模型权值两个输入文件。再加上其他必需的fmk类型和输出路径两个参数，即可成功执行。

   结果显示为：
   ```
   INFO [converter/converter.cc:190] Runconverter] CONVERTER RESULT: SUCCESS!
   ```
   这表示已经成功将Caffe模型转化为MindSpore Lite模型，获得新文件`lenet.ms`。
   
- 以MindSpore、TensorFlow Lite、ONNX模型格式和感知量化模型为例，执行转换命令。

   - MindSpore模型`model.mindir`
      ```bash
      ./converter_lite --fmk=MS --modelFile=model.mindir --outputFile=model
      ```
   
   - TensorFlow Lite模型`model.tflite`
      ```bash
      ./converter_lite --fmk=TFLITE --modelFile=model.tflite --outputFile=model
      ```
   
   - ONNX模型`model.onnx`
      ```bash
      ./converter_lite --fmk=ONNX --modelFile=model.onnx --outputFile=model
      ```

   - TensorFlow Lite感知量化模型`model_quant.tflite`
      ```bash
      ./converter_lite --fmk=TFLITE --modelFile=model_quant.tflite --outputFile=model --quantType=AwareTraining
      ```

   以上几种情况下，均显示如下转换成功提示，且同时获得`model.ms`目标文件。
   ```
   INFO [converter/converter.cc:190] Runconverter] CONVERTER RESULT: SUCCESS!
   ```
   
你可以选择使用模型打印工具，可视化查验上述转化后生成的MindSpore Lite模型。本部分功能开发中。