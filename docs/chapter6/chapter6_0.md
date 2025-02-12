# 6.0 简介

本章将结合常用的推理工具比如tensorrt-llm，OpenVINO，Ipex-llm，从硬件，计算，内存管理和拓展性等不同的方面介绍在大模型推理中常用的运行加速方法。

大模型的推理优化是一项很复杂的工作。一方面，使用llm需要很大的计算成本，一个7B的Qwen模型拥有70亿级别的参数参数，做一次推理可能涉及数十亿次的计算，计算量相当大；另外一方面，则是模型加载需要庞大的空间。近几年随着使用者数量的增加，主流平台和开源社区正逐渐完善对模型在线推理优化。

Tensorrt和Tensorrt-LLM专为NVIDIA Gpu设计，利用Cuda和Tensor Cores进行加速；llm，OpenVion，Ipex和Ipex-llm则是专注于英特尔硬件，包括Cpu，显卡，VPU等产品；VLLM则是平台独立，专注于语言模型的推理。

具体来说：
| 工具   | 硬件 |     计算 | 内存管理 | 拓展性
| :----- | :--: | :-------: | :-------: | -------:|
| OpenVINO |  支持英特尔Cpu,Gpu等，利用硬件特性进行加速，异构计算优化  | 模型优化器；FP16，INT8量化 | 优化内存和缓存，通过批处理和流水提高吞吐| 开源，丰富的开发文档 |
| Ipex-llm |  英特尔Cpu，AVX-512等  | 自动混合精度，FP16加速 | 提高利用效率，优化布局 | 与pytorch适配|
| tensorrt-llm |  英伟达GPU，通过CUDA和Tensor Core进行加速  | 图融合，FP16，INT8量化等 | 提高利用效率，优化布局 | 提供插件，可以自定义算子和图层|
| VLLM |  平台独立，支持多种平台  | 高效的内存管理和并行计算 | 提高利用效率，优化布局 | 提供高效分布式计算支持，可以在不同平台部署|
​

// 最后一章看情况要不要介绍mlc-llm或者SGLand，这个比较陌生，但是各有优势

不同的工具着重点不一致，我们接下来会从不同的层级结合代码了解各自的优化方案，考虑到Tensorrt llm现在还是半开源的状态(英伟达只是放出机器码)，核心优化手段可能并不能实际获取。VLLM在其他章节已经有详细介绍，本章将不再赘述

