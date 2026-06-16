# SychicAI 推理引擎

[![License: Evaluation](https://img.shields.io/badge/License-Evaluation-blue)](./LICENSE)
[![Rust](https://img.shields.io/badge/made%20with-Rust-red)](https://www.rust-lang.org/)
[![CUDA](https://img.shields.io/badge/CUDA-NVIDIA-green)](https://developer.nvidia.com/cuda-toolkit)

**SychicAI** 是一个纯 Rust 编写的大语言模型（LLM）推理引擎，专为在入门级 NVIDIA GPU 上实现极致性能而设计。通过对底层硬件的精细优化，它在资源受限的设备上达到了与业界标杆 `llama.cpp` 同级别的推理速度。

## ✨ 核心特性

*   **极致性能**：在 **NVIDIA GTX 1650 Ti 4GB** 笔记本显卡上，运行 **Gemma 2B Q6_K** 模型，推理速度达到 **37.9-38.2 tok/s**，显存带宽利用率高达 **52.6%**，触及该入门级显卡的物理极限。
*   **纯 Rust 自研**：从 GGUF 模型解析、BPE 分词器，到 CUDA 内核（Q6_K 解量化、FlashAttention）和 CUDA Graph 全管线调度，全部由 Rust 手动实现，无任何 C++ 依赖。
*   **极致轻量**：核心二进制文件仅约 **4MB**，资源占用极低，启动迅速。
*   **内存安全**：借助 Rust 的所有权模型，在编译期保证内存安全，杜绝 C++ 中常见的未定义行为（UB）。
*   **开箱即用**：提供预编译的 Windows 可执行文件（`.exe`），下载解压后双击即可运行，无需配置环境。

## 🖥️ 硬件兼容性

| GPU 系列 | 架构 | 兼容性 | 备注 |
| :--- | :--- | :--- | :--- |
| **RTX 30/40 系列** | Ampere/Ada | ✅ **完美支持** | 即插即用，性能卓越 |
| **RTX 20/GTX 16 系列** | Turing | ✅ **完美支持** | 包含 GTX 1650 Ti，基准测试平台 |
| **RTX 50 系列** | Blackwell | ⚠️ **需要新驱动** | 需 NVIDIA 驱动 ≥ 570.xx，首次运行有 JIT 编译延迟 |
| **GTX 10 系列及更早** | Pascal | ❌ **不支持** | 架构过老，无法适配 |
| **AMD / Intel / Apple** | - | ❌ **不支持** | 引擎基于 CUDA 原生开发 |

> **RTX 50 系列用户注意**：首次运行可能需要等待 5-10 秒进行 PTX JIT 编译（仅一次），之后即可享受预计 **500+ tok/s** 的极速推理体验。

## 🚀 快速开始

### 下载与运行（Windows）

1.  从 [Releases](https://github.com/TIMJEK520/SychicAI-Inference-Engine/releases) 页面下载最新的 `SychicAI_Release.zip` 压缩包。
2.  解压到任意文件夹。
3.  双击 `run.bat` 文件，即可开始与模型对话。

> **提示**：本版本仅提供预编译二进制文件，不包含源代码。

## 🛠️ 技术架构

*   **编程语言**：Rust
*   **计算后端**：NVIDIA CUDA
*   **模型格式**：GGUF
*   **核心模块**：
    *   **自定义 CUDA 内核**：高效实现 Q6_K 解量化和 FlashAttention。
    *   **CUDA Graph**：解码阶段零 CPU 启动开销。
    *   **纯 Rust 分词器**：BPE 分词，无外部依赖。
    *   **GGUF 解析器**：纯 Rust 实现，高效解析模型文件。

## 📊 与 llama.cpp 性能对比

同硬件（GTX 1650 Ti 4GB）和同模型（Gemma 2B Q6_K）下的对比：

| 指标 | SychicAI | llama.cpp | 胜负 |
| :--- | :--- | :--- | :--- |
| **解码速度** | **37.9-38.2/s** | 35–45 tok/s | 打平 / 互有胜负 |
| **首字延迟 (TTFT)** | ~30ms | ~20-25ms | 略微落后，但在可接受范围 |
| **二进制体积** | **~2.3MB** | >100 MB | **SychicAI 胜** |
| **内存安全** | **编译期保证** | 潜在 UB 风险 | **SychicAI 胜** |
| **自研纯度** | **100%** | 100% | 打平 |

## 🤝 支持与联系

*   **报告问题**：请通过 [GitHub Issues](https://github.com/TIMJEK520/SychicAI-Inference-Engine/issues) 提交 bug 或功能请求。
*   **商业授权与定制开发**：请联系 [2910139559@qq.com](mailto:2910139559@qq.com)。
*   **社区交流**：加入我们的 [QQ 群](https://qun.qq.com/universal-share/share?ac=1&authKey=YO0gbM0CQ0Hvy8midHGVPBdUY9ZgrDpMqTcPaSWB3hnKcaZf62EoGMeKWpi5n%2B2p&busi_data=eyJncm91cENvZGUiOiI3NjA1MjQwNzgiLCJ0b2tlbiI6ImhXd1pEdDNTNW9nVFUvNHVnOHRjRnZOb3dtQjRSV1ZJc281dWkrSXYyUE5NNVlxY2pHZ2ZZR091bXJNV0dVQ1giLCJ1aW4iOiIyNTk1MTIxNjMxIn0%3D&data=fULPf4uznqgM86hVTyQOaheixdafnWlBLEnu5UMcyaewu3L22etsF6UxmuCwGEtxfHM53KdqIQHrbV-FO7AIPQ&svctype=4&tempid=h5_group_info)。

## 📜 许可证

本二进制版本采用 **评估版许可证（Evaluation License）** 发布。您可以免费下载并用于个人、非商业的研究和测试。禁止商业使用、逆向工程以及修改版本的分发。

完整条款请查看 [LICENSE](./LICENSE) 文件。

---

## 📜 商标声明

**SychicAI®** 是赛刻灵算科技（深圳）有限公司的注册商标（注册号：76273384）。未经赛刻灵算科技（深圳）有限公司书面许可，任何组织或个人不得使用 **SychicAI** 名称及相关标识进行商业活动。
