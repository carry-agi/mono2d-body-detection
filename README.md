## 人体检测和跟踪

### 效果演示

<iframe src="//player.bilibili.com/player.html?aid=529276653&bvid=BV1au411p7ZC&cid=1149415893&page=1" scrolling="no" border="0" frameborder="no" width="800px" height="450px" framespacing="0" allowfullscreen="true"> </iframe>

### 功能介绍
人体检测和跟踪算法示例订阅图片，利用BPU进行算法推理，发布包含人体、人头、人脸、人手框和人体关键点检测结果msg，并通过多目标跟踪（multi-target tracking，即MOT）功能，实现检测框的跟踪。X86版本暂不支持多目标跟踪以及Web端展示功能。

算法支持的检测类别，以及不同类别在算法msg中对应的数据类型如下：

| 类别     | 说明 | 数据类型 |
| -------- | ----------- | ----------- |
| body     | 人体框      | Roi |
| head     | 人头框      | Roi |
| face     | 人脸框      | Roi |
| hand     | 人手框      | Roi |
| body_kps | 人体关键点  | Point |

人体关键点算法结果索引如下图：

![](./_static/_images/face_body_skeleton/kps_index.jpeg)

代码仓库：<https://github.com/HorizonRDK/mono2d_body_detection>

应用场景：人体检测和跟踪算法是人体运动视觉分析的重要组成部分，可实现人体姿态分析以及人流量统计等功能，主要应用于人机交互、游戏娱乐等领域。

### 物料清单

| 物料选项    | 清单      | 
| ------- | ------------ | 
| RDK X3  | RDK X3 * 1,MIPI/USB 摄像头 * 1, sd卡 * 1, typeC数据线 * 1, 5V2A以上电源适配器 * 1 | 
| RDK X3 module | RDK X3 module * 1, RDK X3 module载板 * 1, MIPI摄像头 * 1, sd卡 * 1, typeC数据线 * 1, 5V2A以上电源适配器 * 1 | 

### 使用方法

#### 准备工作

1. 地平线RDK已烧录好地平线提供的Ubuntu 20.04系统镜像。

2. 地平线RDK已安装MIPI或者USB摄像头。

3. 确认PC机能够通过网络访问地平线RDK。

人体检测和跟踪(mono2d_body_detection)package订阅sensor package发布的图片，经过推理后发布算法msg，通过websocket package实现在PC端浏览器上渲染显示sensor发布的图片和对应的算法结果。

#### 地平线RDK平台

**使用MIPI摄像头发布图片**

```shell
# 配置tros.b环境
source /opt/tros/setup.bash

# 从tros.b的安装路径中拷贝出运行示例需要的配置文件。
cp -r /opt/tros/lib/mono2d_body_detection/config/ .

# 配置MIPI摄像头
export CAM_TYPE=mipi

# 启动launch文件
ros2 launch mono2d_body_detection mono2d_body_detection.launch.py
```

**使用USB摄像头发布图片**

```shell
# 配置tros.b环境
source /opt/tros/setup.bash

# 从tros.b的安装路径中拷贝出运行示例需要的配置文件。
cp -r /opt/tros/lib/mono2d_body_detection/config/ .

# 配置USB摄像头
export CAM_TYPE=usb

# 启动launch文件
ros2 launch mono2d_body_detection mono2d_body_detection.launch.py
```

**使用本地回灌图片**

```shell
# 配置tros.b环境
source /opt/tros/setup.bash

# 从tros.b的安装路径中拷贝出运行示例需要的配置文件。
cp -r /opt/tros/lib/mono2d_body_detection/config/ .
cp -r /opt/tros/lib/dnn_node_example/config/ .

# 配置本地回灌图片
export CAM_TYPE=fb

# 启动launch文件
ros2 launch mono2d_body_detection mono2d_body_detection.launch.py
```

### 结果分析

在运行终端输出如下信息：

```shell
[mono2d_body_detection-3] [WARN] [1660219823.214730286] [example]: This is mono2d body det example!
[mono2d_body_detection-3] [WARN] [1660219823.417856952] [mono2d_body_det]: Parameter:
[mono2d_body_detection-3]  is_sync_mode_: 0
[mono2d_body_detection-3]  model_file_name_: config/multitask_body_head_face_hand_kps_960x544.hbm
[mono2d_body_detection-3]  is_shared_mem_sub: 1
[mono2d_body_detection-3]  ai_msg_pub_topic_name: /hobot_mono2d_body_detection
[mono2d_body_detection-3] [C][31082][08-11][20:10:23:425][configuration.cpp:49][EasyDNN]EasyDNN version: 0.4.11
[mono2d_body_detection-3] [BPU_PLAT]BPU Platform Version(1.3.1)!
[mono2d_body_detection-3] [HBRT] set log level as 0. version = 3.14.5
[mono2d_body_detection-3] [DNN] Runtime version = 1.9.7_(3.14.5 HBRT)
[mono2d_body_detection-3] [WARN] [1660219823.545293244] [mono2d_body_det]: Create hbmem_subscription with topic_name: /hbmem_img
[mono2d_body_detection-3] (MOTMethod.cpp:39): MOTMethod::Init config/iou2_euclid_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (IOU2.cpp:34): IOU2 Mot::Init config/iou2_euclid_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (MOTMethod.cpp:39): MOTMethod::Init config/iou2_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (IOU2.cpp:34): IOU2 Mot::Init config/iou2_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (MOTMethod.cpp:39): MOTMethod::Init config/iou2_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (IOU2.cpp:34): IOU2 Mot::Init config/iou2_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (MOTMethod.cpp:39): MOTMethod::Init config/iou2_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] (IOU2.cpp:34): IOU2 Mot::Init config/iou2_method_param.json
[mono2d_body_detection-3] 
[mono2d_body_detection-3] [WARN] [1660219824.895102286] [mono2d_body_det]: input fps: 31.34, out fps: 31.22
[mono2d_body_detection-3] [WARN] [1660219825.921873870] [mono2d_body_det]: input fps: 30.16, out fps: 30.21
[mono2d_body_detection-3] [WARN] [1660219826.922075496] [mono2d_body_det]: input fps: 30.16, out fps: 30.00
[mono2d_body_detection-3] [WARN] [1660219827.955463330] [mono2d_body_det]: input fps: 30.01, out fps: 30.01
[mono2d_body_detection-3] [WARN] [1660219828.955764872] [mono2d_body_det]: input fps: 30.01, out fps: 30.00
```

输出log显示，程序运行成功，推理时算法输入和输出帧率为30fps，每秒钟刷新一次统计帧率。

在PC端的浏览器输入http://IP:8000 即可查看图像和算法（人体、人头、人脸、人手检测框，检测框类型和目标跟踪ID，人体关键点）渲染效果（IP为地平线RDK/X86设备的IP地址）：

![](./_static/_images/face_body_skeleton/render1.jpeg)

### 接口说明

| 参数名                | 类型        | 解释                                                                                                                                  | 是否必须 | 支持的配置           | 默认值                                               |
| --------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------------------- | ---------------------------------------------------- |
| is_sync_mode          | int         | 同步/异步推理模式。0：异步模式；1：同步模式                                                                                           | 否       | 0/1                  | 0                                                    |
| model_file_name       | std::string | 推理使用的模型文件                                                                                                                    | 否       | 根据实际模型路径配置 | config/multitask_body_head_face_hand_kps_960x544.hbm |
| is_shared_mem_sub     | int         | 是否使用shared mem通信方式订阅图片消息。0：关闭；1：打开。打开和关闭shared mem通信方式订阅图片的topic名分别为/hbmem_img和/image_raw。 | 否       | 0/1                  | 1                                                    |
| ai_msg_pub_topic_name | std::string | 发布包含人体、人头、人脸、人手框和人体关键点感知结果的AI消息的topic名                                                                 | 否       | 根据实际部署环境配置 | /hobot_mono2d_body_detection                         |


### 参考资料
姿态检测案例：[5.3. 姿态检测 — 地平线机器人平台用户手册 1.0 文档](https://developer.horizon.ai/api/v1/fileData/TogetherROS/app/fall_detection.html)    
小车人体跟随案例：[5.4. 小车人体跟随 — 地平线机器人平台用户手册 1.0 文档](https://developer.horizon.ai/api/v1/fileData/TogetherROS/app/car_tracking.html)  
基于人体姿态分析以及手势识别实现游戏人物控制案例：[玩转X3派，健身游戏两不误](https://developer.horizon.ai/forumDetail/112555512834430487)

### 常见问题



