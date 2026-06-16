# SychicAI Inference Engine

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![Rust](https://img.shields.io/badge/made%20with-Rust-red)](https://www.rust-lang.org/)
[![CUDA](https://img.shields.io/badge/CUDA-NVIDIA-green)](https://developer.nvidia.com/cuda-toolkit)

**SychicAI** is a pure-Rust large language model (LLM) inference engine, designed to deliver extreme performance on entry-level NVIDIA GPUs. It achieves inference speeds comparable to the industry benchmark `llama.cpp` on resource-constrained hardware through meticulous low-level optimization.

## ✨ Key Features

*   **Extreme Performance**: Achieves **37.4 tok/s** on an **NVIDIA GTX 1650 Ti 4GB** laptop GPU running the **Gemma 2B Q6_K** model, with **52.6%** memory bandwidth utilization — hitting the physical limit of this entry-level card.
*   **Pure Rust, Self-Contained**: From GGUF parsing and BPE tokenization to CUDA kernels (Q6_K dequantization, FlashAttention) and CUDA Graph pipeline scheduling — all written from scratch in Rust. Zero C++ dependencies.
*   **Ultra-Lightweight**: Core binary is only **~4MB**. Minimal resource footprint, instant startup.
*   **Memory Safety**: Leverages Rust's ownership model to guarantee memory safety at compile time, eliminating undefined behavior (UB) common in C++.
*   **Zero-Config Run**: Pre-built Windows executable (`.exe`) provided. Download, extract, and double-click to run. No environment setup required.

## 🖥️ Hardware Compatibility

| GPU Series | Architecture | Compatibility | Notes |
| :--- | :--- | :--- | :--- |
| **RTX 30/40 Series** | Ampere/Ada | ✅ **Perfect** | Plug & Play, exceptional performance |
| **RTX 20/GTX 16 Series** | Turing | ✅ **Perfect** | Includes GTX 1650 Ti, the baseline test platform |
| **RTX 50 Series** | Blackwell | ⚠️ **New Driver Required** | Requires NVIDIA Driver ≥ 570.xx. First run has JIT compilation delay |
| **GTX 10 Series & Older** | Pascal | ❌ **Not Supported** | Architecture too old, cannot be adapted |
| **AMD / Intel / Apple** | - | ❌ **Not Supported** | Engine is CUDA-native |

> **For RTX 50 series users**: The first run may take 5-10 seconds for PTX JIT compilation (one-time only). After that, expect **500+ tok/s** inference speed.

## 🚀 Quick Start

### Download & Run (Windows)

1.  Download the latest `SychicAI_Release.zip` from the [Releases](https://github.com/TIMJEK520/SychicAI-Inference-Engine/releases) page.
2.  Extract it to any folder.
3.  Double-click `run.bat` to start chatting with the model.

### Build from Source

1.  Make sure [Rust](https://www.rust-lang.org/) and the [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) are installed.
2.  Clone the repository:
    ```bash
    git clone https://github.com/yourusername/SychicAI-Inference-Engine.git
    cd SychicAI-Inference-Engine
    ```
3.  Build the project:
    ```bash
    cargo build --release
    ```
4.  Run the engine (make sure the model file `gemma-2b-q6_k.gguf` is at the correct path):
    ```bash
    ./target/release/sychic -m /path/to/gemma-2b-q6_k.gguf -p "Your prompt"
    ```

## 🛠️ Technical Architecture

*   **Language**: Rust
*   **Compute Backend**: NVIDIA CUDA
*   **Model Format**: GGUF
*   **Core Modules**:
    *   **Custom CUDA Kernels**: Efficient Q6_K dequantization and FlashAttention.
    *   **CUDA Graph**: Zero CPU launch overhead during decoding.
    *   **Pure Rust Tokenizer**: BPE tokenization, no external dependencies.
    *   **GGUF Parser**: Pure Rust implementation for efficient model file parsing.

## 📊 Performance Comparison vs. llama.cpp

Same hardware (GTX 1650 Ti 4GB) and same model (Gemma 2B Q6_K):

| Metric | SychicAI | llama.cpp | Verdict |
| :--- | :--- | :--- | :--- |
| **Decoding Speed** | **37.4 tok/s** | 35–45 tok/s | Tied / Mutual wins |
| **Time to First Token (TTFT)** | ~30ms | ~20-25ms | Slightly behind, but acceptable |
| **Binary Size** | **~4 MB** | >100 MB | **SychicAI wins** |
| **Memory Safety** | **Guaranteed** | Potential UB | **SychicAI wins** |
| **Self-Developed Purity** | **100%** | 100% | Tied |

## 🤝 Contributing & Support

*   **Report Issues**: Submit bugs or feature requests via [GitHub Issues](https://github.com/TIMJEK520/SychicAI-Inference-Engine/issues).
*   **Commercial Licensing**: For closed-source commercial use, please contact: [2910139559@qq.com]
*   **Community Discussion**: [ QQ group link:点击链接加入群聊【SychicAI Inference Engine】：https://qun.qq.com/universal-share/share?ac=1&authKey=YO0gbM0CQ0Hvy8midHGVPBdUY9ZgrDpMqTcPaSWB3hnKcaZf62EoGMeKWpi5n%2B2p&busi_data=eyJncm91cENvZGUiOiI3NjA1MjQwNzgiLCJ0b2tlbiI6ImhXd1pEdDNTNW9nVFUvNHVnOHRjRnZOb3dtQjRSV1ZJc281dWkrSXYyUE5NNVlxY2pHZ2ZZR091bXJNV0dVQ1giLCJ1aW4iOiIyNTk1MTIxNjMxIn0%3D&data=fULPf4uznqgM86hVTyQOaheixdafnWlBLEnu5UMcyaewu3L22etsF6UxmuCwGEtxfHM53KdqIQHrbV-FO7AIPQ&svctype=4&tempid=h5_group_info]

## 📜 License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**.

In short, you are free to use, modify, and distribute this project under the AGPL-3.0 license. However, **if you deploy a modified version as a network service (SaaS), you must make the corresponding source code of your modifications publicly available to all users.**

For full details, see the [LICENSE](LICENSE) file.
