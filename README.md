# NumPy 图像特征提取

GitHub 仓库：https://github.com/xiaogex57/project1-numpy-image-encoder （私有；需向教师和助教授予访问权限）。

本项目用 NumPy 完成 OpenCLIP ViT-B/32 图像塔的前向计算。固定参数由课程提供；程序不训练模型。输入是预处理后的 float32 数组 `[B,3,224,224]`，输出是每张图像一行、单位长度的 float32 特征 `[B,512]`。

## 代码结构

| 路径 | 用途 |
| --- | --- |
| `image_encoder.py` | 六组练习实现：图像块、嵌入、多头注意力、Transformer、最终投影 |
| `preprocess_image.py`, `helpers.py` | 课程提供的预处理及数值辅助函数 |
| `runtime.py`, `run.py` | 加载参数和图像，运行命令行编码 |
| `check.py` | 与独立 FP32 参考比较各阶段结果 |
| `config.json`, `weights/manifest.json` | 架构配置和权重校验信息 |
| `assets/images/`, `assets/images.json` | 十张示例图像及来源信息 |
| `report/main.tex` | 按课程模板撰写的实验报告源文件 |

## 环境与运行

需要 Python 3.9 或更新版本、NumPy、Pillow 和 imageio；具体范围见 `requirements.txt`。

```bash
python3 -m pip install -r requirements.txt
python3 check.py --stage all
python3 run.py --output output_tokens.npz
python3 run.py --images path/to/a.jpg path/to/b.png --output my_tokens.npz
```

`run.py` 在 NPZ 文件中保存 `ids` 和 `output_tokens`，顺序与输入一致。运行命令应从本目录执行；提供完整脚本路径时也可从其他目录执行。

## 数据与复现

课程附件中有 `weights/image_encoder.npz`（约 311 MB，152 个图像权重张量）及 `assets/reference_outputs.npz`（约 7.3 MB），因体积大且是课程提供的资料，不纳入提交仓库。请从畅课平台下载本项目原始附件，按同名路径放回目录；`check.py` 会校验其 SHA-256、张量形状和样例图像指纹。没有参考文件时可以运行编码器，但不能运行完整数值检查。不要把参考结果当作编码器输入。

检查包含预处理、图像块、输入 token、首个 Transformer 块、最后 token、最终输出及改变顺序和重复图像的批次。2026-11-10 在 Python 3.14.3、NumPy 2.4.4、Pillow 12.2.0 上运行 `python3 check.py --stage all`，全部 PASS；最终输出形状 `(10,512)`，最大绝对误差 `6.780e-07`，平均绝对误差 `8.215e-08`。最终输出比较容差为 `atol=2e-5, rtol=2e-4`。详细分阶段数据见报告。

## 实现与说明

`image_encoder.py` 中的六组 TODO 由 Codex 辅助完成。课程提供的 `helpers.py`、预处理、检查器和模型参数未作为原创实现。提交者应独立核对代码、补充本人实际贡献，并在提交前确保能解释图像块排列、多头维度、残差和投影方向。报告源文件和 PDF 在 `report/`；使用 XeLaTeX 或 Tectonic 编译。PDF 正文为两页，另有封面与教师批阅页。封面个人信息尚需填写。
