[toc]
# 笔记
## 内容
### compiler路径
`export TOOL_CHAIN=<交叉编译工具链的路径>`

*之前的记录，不知所云*

---

### 编译rknn_yolov5_demo报错
执行 build-linux_RK3566_3568.sh 时，报错：
```
..../librockchip_mpp.so: file format not recognized; 
treating as linker script
```
librockchip_mpp.so file format 不识别，`file librockchip_mpp.so`后发现是ASCII text格式，而正确应该是`librockchip_mpp.so: symbolic link to librockchip_mpp.so.1`。
因为是先在windows下拉取的rknpu2仓库，然后copy到wsl2的ubuntu下的，动态链接库.so文件链接失效。
重新直接从wsl2拉取库即可。

---

### wsl2 连接usb

wsl2当前版本已经具备了连接usb的能力，不用在编译内核了

1. windows安装[usbipd-win](https://github.com/dorssel/usbipd-win/releases/tag/v2.4.1)
2. powershell 中运行`usbipd wsl list`查看usb列表
3. 运行`usbipd wsl attach --busid <BUSID>`连接usb到wsl
4. 在wsl中`lsusb`查看

---

### 车牌识别

github
crnn_plate_recognition


### 模型转化部署

~~参考博客：<https://blog.csdn.net/m0_46825740/article/details/128818516>~~
参考官方github说明
- 使用rknn的yolov5仓库，而不是yolov5官方。 
  仓库地址：
  <https://github.com/airockchip/yolov5>
- 安装rknn-toolkit 或 rknn-toolkit2，根据目标平台选择。
  仓库地址：
  <https://github.com/rockchip-linux/rknn-toolkit>
  <https://github.com/rockchip-linux/rknn-toolkit2>
- 模型转化，使用RKNN模型库
  仓库地址：
  https://github.com/airockchip/rknn_model_zoo
- 注意事项
  1. 因为 ① RK3566/RK3568/RK3588/RK3588S/RV1103/RV1106 与 ② RK1808/RK1806/RV1109/RV1126、RK3399Pro 并不兼容，因此需要anaconda创建两个环境，分别转化模型，包括python版本和依赖库的版本

模型转化流程：
1. 先将训练好的.pt模型在airochchi_yolov5中转为torch模型
   ```python export.py --rknpu {rk_platform} --weight yolov5s.pt```
2. 将结果拷贝到`rknn_model_zoo`中的`RKNN_model_convert`中转为rknn模型


---

==以下内容为==
==<https://github.com/airockchip/rknn_model_zoo/blob/main/models/CV/object_detection/yolo/README.md>==
==的copy，时间：2023/11/17==


# YOLO - demo

## 内容说明

针对 RKNN 平台，本仓库提供了yolo系列模型的使用教程，包含模型转换、精度测试、C++ Demo。提供部分 YOLO 模型在 RKNN 平台上的性能、精度测试结果。


仓库使用的 yolo 模型，针对 RKNN 平台对模型做了一些结构适配优化，适配代码提交在原始仓库的fork分支上 (相比之前的版本，现在把**模型结构尾部的 sigmoid 函数整合进模型里面**，理论上可以获得精度更高的量化效果。而之前的处理是丢掉最后的 sigmoid 函数放在后处理上进行，这个**差异会导致模型、测试脚本与之前的不兼容，使用时请留意**）。

<br>

支持简表:

<details>
<summary>Detail</summary>
<table>
    <tr>
        <td></td>
        <td>Yolov5</td>
        <td>Yolov6</td>
        <td>Yolov7</td>
        <td>Yolov8</td>
        <td>ppyoloe</td>
        <td>YOLOX</td>
    </tr>
    <tr>
        <td>Toolkit1 - python demo</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
    </tr>
    <tr>
        <td>&#160&#160&#160&#160 &#160&#160&#160&#160 &#160&#160&#160&#160 &#160- C demo</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
    </tr>
    <tr>
        <td>Toolkit2 - python demo</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
    </tr>
    <tr>
        <td>&#160&#160&#160&#160 &#160&#160&#160&#160 &#160&#160&#160&#160 &#160- C demo</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
        <td>&#10004;</td>
    </tr>
</table>
</details>

<br>

## 使用步骤

### 	步骤1. 获取模型		

​		使用 [airockchip](https://github.com/airockchip) 组织下的 [yolov5](https://github.com/airockchip/yolov5)/[yolov6](https://github.com/airockchip/YOLOv6) /[yolov7](https://github.com/airockchip/yolov7)/ [yolov8](https://github.com/airockchip/ultralytics_yolov8)/ [YOLOX](https://github.com/airockchip/YOLOX) / 仓库，根据仓库里的 README_rkopt_manual.md 提示，进行模型训练、模型导出。获取 torchscript / onnx 格式的模型。(ppyoloe 等待整理补充)

​		对于获取 ONNX 格式 的 ppyoloe 模型，请参考[文档](./patch_for_model_export/ppyoloe/README.md)

​		或下载已经提供的RKNN模型，获取[网盘(密码:rknn)](https://eyun.baidu.com/s/3humTUNq) ，链接中的具体位置为rknn_model_zoo/models/CV/object_detection/yolo/{YOLO}/deploy_models/{toolkit}/{platform}

<br>

### 	步骤2. 模型转换

​		参考 [RKNN_model_convert](./RKNN_model_convert/README.md) 说明，将步骤1获取的 torchscript / onnx 格式的模型转为 RKNN 模型。

​		在连板调试功能可以正常使用的情况下，转换脚本除了转出模型，会调用  rknn.eval_perf 接口获取 RKNN 模型推理性能、计算 RKNN 模型与原模型的推理结果相似度(基于cos距离)。

<br>

### 	步骤3. 使用 Python demo 测试 RKNN/ torchscript/ onnx模型

​		参考 [RKNN_python_demo](./RKNN_python_demo/README.md) 说明，测试 RKNN/ torchscript/ onnx 格式的 yolo 模型在图片上推理结果并绘图。

<br>

### 	步骤4. 使用 C demo 测试 RKNN 模型

​		RK1808/ RV1109/ RV1126/RK3399pro 请使用 [RKNN_toolkit_1](./RKNN_C_demo/RKNN_toolkit_1/rknn_yolo_demo) 版本

​		RK3562/ RK3566/ RK3568/ RK3588/ RK3562/ RV1103/ RV1106 请使用 [RKNN_toolkit_2](./RKNN_C_demo/RKNN_toolkit_2/rknn_yolo_demo) 版本

<br>

<br>


## 附录：

### 前后处理差异

- 不同的 YOLO 模型存在前后处理差异，需要注意这些差异避免预测结果与原始框架不一致。

<details>
<summary>&#160&#160&#160差异详情</summary>
<table>
    <tr>
        <td></td>
        <td>Yolov5/6/7/8, ppyoloe</td>
        <td>Yolox</td>
    </tr>
    <tr>
        <td>Color</td>
        <td>RGB</td>
        <td>BGR</td>
    </tr>
    <tr>
        <td>Mean</td>
        <td>0</td>
        <td>0</td>
    </tr>
    <tr>
        <td>Std</td>
        <td>255</td>
        <td>1</td>
    </tr>
    <tr>
        <td>Letter_box pad_color</td>
        <td>(114,114,114)</td>
        <td>(114,114,114)</td>
    </tr>
    <tr>
        <td>NMS - class agnostic</td>
        <td>False</td>
        <td>Yes</td>
    </tr>
    <tr>
        <td>conf threshold</td>
        <td>0.25@box<br>0.25@(box* class)</td>
        <td>0.25@(box* class)</td>
    </tr>
    <tr>
        <td>nms threshold</td>
        <td>0.45</td>
        <td>0.45</td>
    </tr>
    <tr>
        <td>Anchors</td>
        <td>Yes</td>
        <td>No</td>
    </tr>
</table>
*这些差异实现已经整合进本仓库的demo及脚本里
</details>

<br>



### 性能

![YOLO_perf](./perf_img.png)

- 更详细的数据可以查看 [yolo_perf](./yolo_perf(need open in web browser).html) 文件

