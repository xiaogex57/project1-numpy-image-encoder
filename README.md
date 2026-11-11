# NumPy 图像特征提取

GitHub 仓库：https://github.com/xiaogex57/project1-numpy-image-encoder

项目使用 NumPy 完成 OpenCLIP ViT-B/32 图像塔的前向计算，使用课程提供的固定权重，不训练模型。输入为预处理后的 float32 数组 `[B,3,224,224]`，输出为每张图像一个单位长度的 float32 特征 `[B,512]`。

## 项目结构

| 路径 | 用途 |
| --- | --- |
| `image_encoder.py` | 六组练习实现：图像块、嵌入、多头注意力、Transformer 和最终投影 |
| `preprocess_image.py`, `helpers.py` | 图像预处理和数值辅助函数 |
| `runtime.py`, `run.py` | 加载模型参数、处理图像并保存特征 |
| `check.py` | 检查模型资源并逐阶段比较参考结果 |
| `config.json` | 模型架构参数 |
| `weights/image_encoder.npz` | 模型权重，Git LFS 管理 |
| `weights/manifest.json` | 权重张量形状、数量和 SHA-256 清单 |
| `assets/reference_outputs.npz` | 数值检查参考结果，Git LFS 管理 |
| `assets/images/`, `assets/images.json` | 十张示例图像、来源和 SHA-256 清单 |
| `report/main.tex`, `report/main.pdf` | 实验报告源文件和 PDF |
| `report/check-results.txt` | 分阶段数值检查记录 |
| `lecture-demos/` | NumPy、图像读写和 token 流程示例 |
| `practice/project1_exercises.pdf` | 课程练习说明 |
| `PPT-CODE-MAP.txt` | 课程讲义与代码对应关系 |
| `requirements.txt` | Python 依赖清单 |

## 环境与运行

需要 Python 3.9 或更新版本、NumPy、Pillow 和 imageio；版本范围见 `requirements.txt`。模型权重和参考结果通过 Git LFS 下载。系统需安装 Git LFS，并在克隆前初始化：

```bash
git lfs install
git clone https://github.com/xiaogex57/project1-numpy-image-encoder.git
cd project1-numpy-image-encoder
python3 -m pip install -r requirements.txt
```

已有仓库若尚未取得大文件，可运行 `git lfs pull`。安装依赖后运行：

```bash
python3 check.py --stage all
python3 run.py --output output_tokens.npz
python3 run.py --images path/to/a.jpg path/to/b.png --output my_tokens.npz
```

`run.py` 在 NPZ 文件中保存 `ids` 和 `output_tokens`，顺序与输入一致。

## 数据与复现

`weights/image_encoder.npz`（约 311 MB，152 个图像权重张量）和 `assets/reference_outputs.npz`（约 7.3 MB）均已纳入公开仓库的 Git LFS。`check.py` 会校验文件 SHA-256、权重张量形状及样例图像指纹。参考结果仅用于数值检查，不作为编码器输入；权重和十张示例图像用于实际编码。

检查包含预处理、图像块、输入 token、首个 Transformer 块、最后 token、最终输出，以及改变顺序和重复图像的批次。2026-11-11 在 Python 3.14.3、NumPy 2.4.4、Pillow 12.2.0 上运行 `python3 check.py --stage all`，全部 PASS；最终输出形状 `(10,512)`，最大绝对误差 `6.780e-07`，平均绝对误差 `8.215e-08`。最终输出比较容差为 `atol=2e-5, rtol=2e-4`。分阶段数据见 `report/check-results.txt` 和实验报告。

实验报告 PDF 共 7 页：封面 1 页、正文恰好 5 页、教师批阅页 1 页。报告源文件可使用 XeLaTeX 或 Tectonic 编译。
