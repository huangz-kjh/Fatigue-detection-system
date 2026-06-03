# 🚗 驾驶员疲劳检测系统 | Driver Fatigue Detection System

[![Python](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green.svg)](https://opencv.org/)
[![PyQt5](https://img.shields.io/badge/PyQt5-5.15+-orange.svg)](https://pypi.org/project/PyQt5/)
[![dlib](https://img.shields.io/badge/dlib-19.x-yellow.svg)](http://dlib.net/)

> 基于计算机视觉和面部特征分析的实时驾驶员疲劳检测系统，通过监测眼部、嘴部和头部姿态来识别疲劳驾驶行为。

---

## 📋 目录

- [项目简介](#-项目简介)
- [功能特点](#-功能特点)
- [检测原理](#-检测原理)
- [环境要求](#-环境要求)
- [安装部署](#-安装部署)
- [使用说明](#-使用说明)
- [项目结构](#-项目结构)
- [技术栈](#-技术栈)
- [注意事项](#-注意事项)
- [许可证](#-许可证)

---

## 🎯 项目简介

本项目是一个基于Python开发的驾驶员疲劳检测系统，利用计算机视觉技术实时分析驾驶员面部特征，通过检测以下指标判断疲劳状态：

- 👁️ **眼部状态** - 检测眼睛闭合程度和眨眼频率
- 👄 **嘴部状态** - 检测打哈欠行为
- 🔄 **头部姿态** - 检测头部倾斜和异常位置

当系统检测到疲劳驾驶迹象时，会通过声音警报提醒驾驶员。

---

## ✨ 功能特点

| 功能 | 描述 |
|------|------|
| 🎥 **实时检测** | 支持摄像头实时视频流检测 |
| 📹 **视频文件** | 支持本地视频文件分析 |
| 👁️ **眼睛检测** | EAR算法检测眼睛闭合状态 |
| 👄 **打哈欠检测** | MAR算法检测嘴部张开程度 |
| 🔄 **头部姿态** | 3D姿态估计检测头部倾斜 |
| 🔊 **声音警报** | 检测到疲劳时播放警告音 |
| 🖥️ **图形界面** | 基于PyQt5的友好用户界面 |
| ⚙️ **参数可调** | 支持自定义检测敏感度 |

---

## 🔬 检测原理

### 1. 眼睛纵横比 (EAR - Eye Aspect Ratio)
```
EAR = (|P2-P6| + |P3-P5|) / (2 * |P1-P4|)
```
- 当EAR值低于阈值时，判断为眼睛闭合
- 连续多帧闭合则判定为疲劳

### 2. 嘴部纵横比 (MAR - Mouth Aspect Ratio)
```
MAR = (|P51-P58| + |P53-P56|) / (2 * |P49-P55|)
```
- 当MAR值高于阈值时，判断为打哈欠
- 用于检测张嘴疲劳特征

### 3. 头部姿态估计
- 使用68个面部关键点
- 通过solvePnP计算旋转和平移矩阵
- 提取欧拉角（俯仰角、偏航角、翻滚角）
- 判断头部是否倾斜或偏离正常位置

---

## 🛠️ 环境要求

### 系统要求
- Windows 10/11
- Python 3.7 或更高版本
- 摄像头（用于实时检测）

### Python依赖
```
opencv-python
imutils
dlib
PyQt5
numpy
scipy
pygame
```

---

## 📦 安装部署

### 1. 克隆项目
```bash
git clone https://github.com/yourusername/fatigue-detection-system.git
cd fatigue-detection-system
```

### 2. 创建虚拟环境（推荐）
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Linux/Mac
source venv/bin/activate
```

### 3. 安装依赖
```bash
pip install opencv-python imutils dlib PyQt5 numpy scipy pygame
```

> ⚠️ **注意**：dlib安装可能需要CMake，如遇问题请参考[dlib官方安装指南](https://github.com/davisking/dlib)

### 4. 下载模型文件
项目已包含 `shape_predictor_68_face_landmarks.dat` 模型文件（约97MB），用于68点面部关键点检测。

---

## 🚀 使用说明

### 启动程序
```bash
python main.py
```

### 操作指南

#### 1. 选择视频源
- **摄像头**：选择下拉框中的摄像头设备
- **视频文件**：点击"打开视频文件"按钮选择本地视频

#### 2. 调整摄像头位置
点击"调整摄像头位置"按钮预览视频画面，确保人脸在视野中央。

#### 3. 显示设置
勾选右侧选项可开启/关闭以下视觉标记：
- ✅ 显示眼部标记
- ✅ 显示嘴部标记
- ✅ 显示头部姿态
- ✅ 显示关键点

#### 4. 开始检测
点击"开始检测"按钮启动疲劳检测。

#### 5. 下班模式
- 勾选"启用下班模式"可设置检测时长
- 到达设定时间后自动停止检测

---

## 📁 项目结构

```
fatigue-detection-system/
├── main.py                              # 主程序入口
├── UI.py                                # PyQt5界面定义
├── utils.py                             # 工具函数（EAR/MAR/头部姿态计算）
├── get_image.py                         # 图像采集工具
├── test.py                              # 测试脚本
├── shape_predictor_68_face_landmarks.dat # dlib 68点面部关键点模型
├── warning.mp3                          # 疲劳警报音频
├── README.md                            # 项目说明文档
└── src/                                 # 资源目录
```

### 核心文件说明

| 文件 | 说明 |
|------|------|
| `main.py` | 主窗口逻辑，处理UI交互和视频流 |
| `UI.py` | 使用PyQt5设计的图形界面 |
| `utils.py` | 核心算法：EAR、MAR计算，头部姿态估计 |
| `shape_predictor_68_face_landmarks.dat` | dlib预训练模型，检测68个面部关键点 |

---

## 💻 技术栈

- **Python 3.7+** - 主要编程语言
- **OpenCV** - 计算机视觉库，用于图像处理
- **dlib** - 机器学习库，用于面部检测和关键点定位
- **PyQt5** - GUI框架，构建用户界面
- **NumPy/SciPy** - 数值计算
- **imutils** - OpenCV辅助工具
- **pygame** - 音频播放

---

## ⚠️ 注意事项

1. **光照条件**：确保检测环境光线充足，避免过暗或过亮
2. **摄像头角度**：建议摄像头与驾驶员面部保持水平
3. **距离要求**：面部与摄像头保持适当距离（50-100cm）
4. **模型文件**：`shape_predictor_68_face_landmarks.dat` 为必需文件
5. **隐私保护**：仅本地运行，不会上传任何图像数据

---

## 📸 界面预览

> 主界面包含视频显示区域、控制面板和状态指示器，操作简洁直观。

---

## 🤝 贡献指南

欢迎提交Issue和Pull Request！

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交你的更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开一个 Pull Request

---

## 📄 许可证

本项目仅供学习和研究使用。

---

## 📧 联系方式

如有问题或建议，欢迎通过以下方式联系：

- GitHub Issues: [提交问题](https://github.com/yourusername/fatigue-detection-system/issues)

---

## 🙏 致谢

- [dlib](http://dlib.net/) - 提供面部检测和关键点定位
- [OpenCV](https://opencv.org/) - 提供计算机视觉支持
- [PyQt5](https://www.riverbankcomputing.com/software/pyqt/) - 提供GUI框架

---

<div align="center">

⭐ 如果这个项目对你有帮助，请给它一个Star！ ⭐

</div>
