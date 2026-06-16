# SychicAI Inference Engine

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Rust](https://img.shields.io/badge/made%20with-Rust-red)](https://www.rust-lang.org/)
[![CUDA](https://img.shields.io/badge/CUDA-NVIDIA-green)](https://developer.nvidia.com/cuda-toolkit)

**SychicAI** 是一个纯 Rust 编写的大语言模型 (LLM) 推理引擎，专为在入门级 NVIDIA GPU 上实现极致性能而设计。它在资源受限的硬件上，通过精细的底层优化，达到了与业界标杆 `llama.cpp` 同等级别的推理速度。

## ✨ 核心亮点

*   **极致性能**：在 **NVIDIA GTX 1650 Ti 4GB** 笔记本显卡上，运行 **Gemma 2B Q6_K** 模型，推理速度可达 **37.4 tok/s**，显存带宽利用率高达 **52.6%**，触及该硬件的物理极限。
*   **纯 Rust 自研**：从 GGUF 模型解析、BPE 分词器，到 CUDA 内核 (Q6_K 解量化、FlashAttention) 和 CUDA Graph 全管线调度，全部由 Rust 手动实现，无任何 C++ 依赖。
*   **极致轻量**：核心二进制文件仅约 **4MB**，启动迅速，资源占用极低。
*   **内存安全**：借助 Rust 语言的所有权模型，在编译期保证了内存安全，杜绝了 C++ 中常见的未定义行为 (UB)。
*   **开箱即用**：提供预编译的 Windows 可执行文件 (`.exe`)，下载解压后双击即可运行，无需复杂的环境配置。

## 🖥️ 硬件兼容性

| GPU 系列 | 架构 | 兼容性 | 备注 |
| :--- | :--- | :--- | :--- |
| **RTX 30/40 系列** | Ampere/Ada | ✅ **完美支持** | 即插即用，性能卓越 |
| **RTX 20/GTX 16 系列** | Turing | ✅ **完美支持** | 包含 GTX 1650 Ti，基准测试平台 |
| **RTX 50 系列** | Blackwell | ⚠️ **需新驱动** | 需 NVIDIA 驱动 ≥ 570.xx，首次运行有 JIT 编译延迟 |
| **GTX 10 系列及更早** | Pascal | ❌ **不支持** | 架构过老，无法适配 |
| **AMD / Intel / Apple** | - | ❌ **不支持** | 引擎基于 CUDA 原生开发 |

> **对于 RTX 50 系列显卡用户**：首次运行可能需要等待 5-10 秒进行 PTX JIT 编译（仅一次），之后即可享受预计 **500+ tok/s** 的极速推理体验。

## 🚀 快速开始

### 下载与运行 (Windows)

1.  从 [Releases](https://github.com/TIMJEK520/SychicAI-Inference-Engine/releases) 页面下载最新的 `SychicAI_Release.zip` 压缩包。
2.  解压到任意文件夹。
3.  双击 `双击我启动.bat` 文件，即可开始与模型对话。

### 从源码构建

1.  确保已安装 [Rust](https://www.rust-lang.org/) 和 [CUDA 工具包](https://developer.nvidia.com/cuda-toolkit)。
2.  克隆仓库：
    ```bash
    git clone https://github.com/TIMJEK520/SychicAI-Inference-Engine.git
    cd SychicAI-Inference-Engine
    ```
3.  构建项目：
    ```bash
    cargo build --release
    ```
4.  运行引擎 (请确保模型文件 `gemma-2b-q6_k.gguf` 在正确路径)：
    ```bash
    ./target/release/sychic -m /path/to/gemma-2b-q6_k.gguf -p "你的提示词"
    ```

## 🛠️ 技术架构

*   **编程语言**: Rust
*   **计算后端**: NVIDIA CUDA
*   **模型格式**: GGUF
*   **核心模块**:
    *   **自定义 CUDA 内核**: 实现高效的 Q6_K 解量化和 FlashAttention。
    *   **CUDA Graph**: 实现解码阶段的零 CPU 启动开销。
    *   **纯 Rust Tokenizer**: 实现 BPE 分词器，无外部依赖。
    *   **GGUF 解析器**: 纯 Rust 实现，高效解析模型文件。

## 📊 与 llama.cpp 性能对比

在同硬件 (GTX 1650 Ti 4GB) 和同模型 (Gemma 2B Q6_K) 下的对比：

| 指标 | SychicAI | llama.cpp | 备注 |
| :--- | :--- | :--- | :--- |
| **解码速度** | **37.4 tok/s** | 35–45 tok/s | 性能持平，互有胜负 |
| **首字延迟 (TTFT)** | ~30ms | ~20-25ms | 略微落后，但在可接受范围 |
| **二进制体积** | **~4 MB** | >100 MB | SychicAI 绝对优势 |
| **内存安全** | **编译期保证** | 潜在 UB 风险 | SychicAI 绝对优势 |
| **自研纯度** | **100%** | 100% | 持平 |

## 🤝 贡献与支持

*   **报告问题**: 请通过 [GitHub Issues](https://github.com/TIMJEK520/SychicAI-Inference-Engine/issues) 提交 bug 或功能请求。
*   **商业合作**: 如需闭源商业使用授权，请联系：[2910139559@qq.com]
*   **交流讨论**: [QQ:]

## 📜 许可证

本项目采用 **GNU Affero General Public License v3.0 (AGPL-3.0)** 许可证。

简单来说，你可以在 AGPL-3.0 许可下自由使用、修改和分发此项目。但请注意，**如果你将修改后的版本作为网络服务 (SaaS) 部署，你必须将你的修改部分对应的源代码向所有用户公开**。

有关详细信息，请参阅 [LICENSE](LICENSE) 文件。
