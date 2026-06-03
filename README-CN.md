<h1 align="center">
  <img src="./assets/logo.png" alt="JiSi" width="72" align="absmiddle">
  集思: 在大规模下重新思考大语言模型的路由与聚合
</h1>

<p align="center" style="font-size: 1.15em;">
  <strong>一站式为你组建多智能体智囊团</strong>
</p>

<p align="center">
  <a href="README.md">English</a> | <a href="README-CN.md"><strong>简体中文</strong></a>
</p>

<p align="center">
  <a href="https://scholar.google.com/citations?user=K7drMDgAAAAJ">唐圣汲</a><sup>1,2,*</sup> &nbsp;&nbsp;
  <a href="https://scholar.google.com/citations?user=k5MQpaIAAAAJ">林炜豪</a><sup>1,3,*</sup> &nbsp;&nbsp;
  <a href="https://scholar.google.com/citations?user=UEZZP5QAAAAJ">叶鹏</a><sup>1,2,†</sup> &nbsp;&nbsp;
  <a href="https://openreview.net/profile?id=~Jingqi_Ye1">叶京棋</a><sup>4</sup> &nbsp;&nbsp;
  <a href="https://github.com/ynulihao">李昊</a><sup>1,5</sup> &nbsp;&nbsp;
  <a href="https://scholar.google.com/citations?user=63_0cG8AAAAJ">张逸群</a><sup>1,6</sup> &nbsp;&nbsp;
  <a href="https://xiaosongwang.github.io/">王潚崧</a><sup>1</sup>
</p>

<p align="center">
  <a href="https://bobrown.github.io/boZhang.github.io/">张铂</a><sup>1</sup> &nbsp;&nbsp;
  <a href="https://shuyuehu.github.io/">胡舒悦</a><sup>1</sup> &nbsp;&nbsp;
  <a href="https://eetchen.github.io/">陈涛</a><sup>3</sup> &nbsp;&nbsp;
  <a href="https://leibai.site/">白磊</a><sup>1</sup> &nbsp;&nbsp;
  <a href="https://wlouyang.github.io/">欧阳万里</a><sup>1,2</sup>
</p>

<p align="center">
  <sup>1</sup>上海人工智能实验室 &nbsp;&nbsp;
  <sup>2</sup>香港中文大学 &nbsp;&nbsp;
  <sup>3</sup>复旦大学
</p>

<p align="center">
  <sup>4</sup>中国科学技术大学 &nbsp;&nbsp;
  <sup>5</sup>西北工业大学 &nbsp;&nbsp;
  <sup>6</sup>东北大学
</p>

<p align="center">
  <sup>*</sup> 同等贡献 &nbsp;&nbsp;
  <sup>†</sup> 通讯作者
</p>

<p align="center">
  <a href="https://github.com/magent4aci/openJiSi"><img src="https://img.shields.io/badge/GitHub-JiSi-000000?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"></a>
  <a href="https://huggingface.co/datasets/aisfuture/jisi_data"><img src="https://img.shields.io/badge/Hugging%20Face-Dataset-fcd022?style=for-the-badge&logo=huggingface&logoColor=000000" alt="Hugging Face"></a>
  <a href="https://arxiv.org/abs/2601.01330"><img src="https://img.shields.io/badge/Paper-arXiv-A42C25?style=for-the-badge&logo=arxiv&logoColor=white" alt="arXiv Paper"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="MIT License"></a>
</p>

## 📰 最新消息
> - **[2026/06]** 🌟我们**开源了全部数据与代码**，包括 Hugging Face 上的 [`aisfuture/jisi_data`](https://huggingface.co/datasets/aisfuture/jisi_data) 数据集与本仓库。
> - **[2026/05]** 🏆论文被 **ICML 2026** 接收。

## 🗺️ 开源计划
我们将会在用于路由的基准库上持续更新 Qwen、Kimi、GLM、DeepSeek 等最新版本开源模型，如果有更新会在此添加通知。
- [x] **开源代码** — 本仓库中的集思路由与聚合实现
- [x] **开源数据集** — [Hugging Face](https://huggingface.co/datasets/aisfuture/jisi_data) 上的开箱即用划分、嵌入缓存与基准数据
- [ ] **Web Demo** — 浏览器端可交互集思（即将推出，敬请期待）
- [ ] **公开 API** — 托管的路由与聚合接口（即将推出）


本文档对应论文 [*超越 Gemini-3-Pro：在大规模下重新思考大语言模型的路由与聚合*](https://arxiv.org/abs/2601.01330)（*Beyond Gemini-3-Pro: Revisiting LLM Routing and Aggregation at Scale*）的官方代码仓库。仓库主要面向学术研究，你也可以基于它快速组建专属的多智能体智囊团。

集思 的核心设计简洁而有效：不依赖单一单体模型，而是将异构开源 LLM 组织为协作系统。给定 query 后，集思检索语义相近的 support set 问题，估计各模型表现，再路由到强专家或对多位专家回复进行聚合。

本仓库仅保留 集思 方法及复现、扩展所需的最小支撑代码。

![JiSi performance leaderboard](./assets/bar_v3.png)

## 🌐 简介

现代 LLM 能力强但分布不均：擅长数学推理的模型可能在长文本 QA、编程或领域推理上表现一般。集思将这种异质性视为资源，构建包含问题、模型回复、正误记录、用量信息与 embedding 的支持集，在推理时据此为每个实例选择或组合模型。

集思 包含三大机制：

- **Query-response mixed routing（查询-回复混合路由）**：检索 support set 邻居，结合 query 相似度与回复侧证据估计模型能力。
- **Support-set-based aggregator selection（基于 support set 的聚合器选择）**：从模型池中选取聚合候选，而非为每题固定单一聚合器。
- **Adaptive routing-aggregation switch（自适应路由-聚合切换）**：按实例决定由单一路由专家作答，还是对多专家回复聚合。

论文中，由十个开源 LLM 组成的 集思 在报告 benchmark 上达到 **72.15%** 平均性能，相较 Gemini-3-Pro 总成本降低 **53.23%**。

![JiSi framework](./assets/framework.png)

<p align="center">
  <em>集思整体框架概览。</em>
</p>

![JiSi method comparison](./assets/comparison_method.png)

<p align="center">
  <em>集思相对既有路由与聚合的改进：混合路由、基于 support set 的聚合器选择，以及路由与聚合之间的自适应切换。</em>
</p>

## 🌟 亮点

- **无训练协作**：通过 support set 检索与分数估计完成路由，无需训练路由器，从而天生具有强拓展性。
- **实例级决策**：每个 query 可使用不同的专家集合与聚合策略。
- **全面的综合性能**：在多种 benchmark 上相对领先开源与闭源 LLM 具备优势或竞争力。
- **良好的可扩展性**：随开源 LLM 加入模型池，集思表现出可扩展的性能提升。

<p align="center">
  <img src="./assets/radar_plot_open_2.png" alt="JiSi vs. open-source LLMs" width="49%" />
  <img src="./assets/radar_plot_close_2.png" alt="JiSi vs. closed-source LLMs" width="49%" />
</p>

<p align="center">
  <em>左：与开源 LLM 对比。右：与闭源 LLM 对比。</em>
</p>

![JiSi scalability](./assets/scale_curve.png)

<p align="center">
  <em>开源模型池从 5 个扩展到 10 个时，集思性能持续提升。</em>
</p>

## 🏗️ 发布组件

| 组件 | 路径 | 说明 |
| --- | --- | --- |
| JiSi runner | `baselines/JiSi/run_jisi.py` | 路由与聚合主入口 |
| JiSi config | `baselines/JiSi/config.py` | 运行时配置与校验 |
| Model API layer | `baselines/JiSi/utils/` | OpenAI 兼容的生成与聚合工具 |
| Data adaptor | `baselines/adaptors/jisi_adaptor.py` | 将 benchmark 采集结果转为 集思 JSONL |
| Data collector | `data_collector/` | 在支持的 evaluator 上采集模型输出的可选工具 |
| Evaluators | `evaluation/` | 各 benchmark 的评测工具 |

## 🏃 快速开始

### 1. 安装环境

建议使用 Python 3.10 或 3.11。大规模运行推荐配备 CUDA。

```bash
conda create -n jisi python=3.10
conda activate jisi
pip install -r requirements.txt
```

通过环境变量配置 API Key：

```bash
cp .env.example .env
# 本地编辑 .env，或在 shell 中 export：
export EMBEDDING_API_KEY="your-embedding-key"
export OPENAI_API_KEY="your-openai-compatible-key"
```

首次运行前复制示例配置：

```bash
cp baselines/JiSi/config/jisi/main.example.json baselines/JiSi/config/jisi/main.local.json
cp baselines/JiSi/config/jisi/api_config.example.json baselines/JiSi/config/jisi/api_config.local.json
cp config/embedding_config.example.yaml config/embedding_config.local.yaml
```

在 `main.local.json` 中指向上述本地路径（`embedding_config_path`、`api_config_path` 以及步骤 2 中的 `data/jisi/...` 文件）。

### 2. 下载数据

从 [`aisfuture/jisi_data`](https://huggingface.co/datasets/aisfuture/jisi_data) 下载开箱即用划分并放入仓库：

```bash
pip install -U "huggingface_hub[cli]"

DATASET_REPO=aisfuture/jisi_data

huggingface-cli download \
  --repo-type dataset "$DATASET_REPO" \
  --include "example_data/seed42_split0.7/*" "example_data/*.tar" \
  --local-dir .hf_jisi_data

mkdir -p data/jisi
cp -r .hf_jisi_data/example_data/* data/jisi/
```

目录结构应如下：

```text
data/jisi/
  seed42_split0.7/
    train.jsonl
    test.jsonl
    baseline_scores.json
  train_query_embed.tar
  train_response_embed.tar
  test_response_embed.tar
```

三个 `*.tar` 为预计算的 embedding 缓存；存在时 `run_jisi` 会自动加载，无需重建大规模 query/response embedding bank。

### 3. 启动 embedding 模型

推理时 集思 会调用 **OpenAI 兼容的 `/embeddings` 接口**。请编辑 `config/embedding_config.local.yaml`，使 `base_url`、`api_model_name`、`api_key` 与部署一致。

**方案 A — 本地服务（推荐用于复现）**

下载 [`gte-Qwen2-7B-instruct`](https://www.modelscope.cn/models/iic/gte-Qwen2-7B-instruct)，并使用 [vLLM](https://docs.vllm.ai/) 等 OpenAI 兼容栈部署：

```bash
vllm serve <path-or-repo-to-gte-Qwen2-7B-instruct> \
  --task embed \
  --host 0.0.0.0 \
  --port 8000
```

将 `embedding_config.local.yaml` 设为 `http://127.0.0.1:8000/v1`，并在 `main.local.json` 中同步 `embedding_base_url`、`embedding_model`。

**方案 B — 远程 API**

将 `embedding_config.local.yaml` 指向托管的 OpenAI 兼容 embedding API，并在 `.env` 中设置 `EMBEDDING_API_KEY`。

### 4. 运行集思

**仅路由（router-only）**：

```bash
python -m baselines.JiSi.run_jisi \
  --config baselines/JiSi/config/jisi/main.local.json
```

或通过命令行显式传参：

```bash
python -m baselines.JiSi.run_jisi \
  --train-data data/jisi/seed42_split0.7/train.jsonl \
  --test-data data/jisi/seed42_split0.7/test.jsonl \
  --baseline-scores data/jisi/seed42_split0.7/baseline_scores.json \
  --embedding-config config/embedding_config.local.yaml \
  --api-config baselines/JiSi/config/jisi/api_config.local.json \
  --mode router
```

**Aggregator 模式** — 在 `main.local.json` 中设置 `"mode": "aggregator"`（或传入 `--mode aggregator`），并确保 `api_config.local.json` 包含 集思 模型池中的全部模型名。

聚合结果写入 `result_dir` 下的 `result.jsonl`。对生成答案评分：

```bash
python -m baselines.JiSi.post_eval \
  --res_path results/jisi/<run_name>/result.jsonl \
  --datasets paper
```

## 💡 数据格式

本仓库不包含大规模 benchmark 结果文件。最简路径是从 Hugging Face 下载 [`aisfuture/jisi_data`](https://huggingface.co/datasets/aisfuture/jisi_data)，将 `example_data/` 复制到 `data/jisi/`。

期望结构：

```text
data/jisi/
  seed42_split0.7/
    train.jsonl
    test.jsonl
    baseline_scores.json
  train_query_embed.tar
  train_response_embed.tar
  test_response_embed.tar
```

每条 JSONL 至少包含：

```json
{
  "query": "Question or prompt text",
  "dataset": "aime",
  "index": 1,
  "split": "test",
  "records": {
    "ModelA": 1.0,
    "ModelB": 0.0
  },
  "usages": {
    "ModelA": {"prompt_tokens": 120, "completion_tokens": 64, "cost": 0.0}
  },
  "raw_output": {
    "ModelA": "ModelA response"
  },
  "gt": "ground-truth answer"
}
```

也可通过 adaptor 从 benchmark 采集结果构建该格式：

```bash
python -m baselines.adaptors.jisi_adaptor \
  --config config/adaptor/jisi_llm_v1.example.yaml \
  --seed 42 \
  --split-ratio 0.7 \
  --output-dir data/jisi
```

## 🤗 Hugging Face 数据

我们在 [`aisfuture/jisi_data`](https://huggingface.co/datasets/aisfuture/jisi_data) 持续更新集思相关数据集、缓存与 question bank。Hugging Face 发布目录结构如下：

```text
example_data/
  seed42_split0.7/
    train.jsonl
    test.jsonl
    baseline_scores.json
  train_query_embed.tar
  train_response_embed.tar
  test_response_embed.tar
benchmark_bank/
  <benchmark>/<split-or-mode>/<model>/*.json
datasets/
  <benchmark source files>
```

快速运行请参考[快速开始](#-快速开始)第 2 步。完整数据布局：

```text
example_data/
  seed42_split0.7/
    train.jsonl
    test.jsonl
    baseline_scores.json
  train_query_embed.tar
  train_response_embed.tar
  test_response_embed.tar
```

### 📈 支持的 Benchmark

发布的 question bank 覆盖以下 benchmark 系列。标准后评测通过 `baselines.JiSi.post_eval --datasets paper` 支持论文 benchmark 集合；SWE-Bench 亦包含在 question bank 中，但需通过单独的 SWE-Bench 提交流程验证，而非标准 `post_eval`。

| Dataset id | 评测内容 | 集思 用法 | `post_eval --datasets paper` |
| --- | --- | --- | --- |
| `aime` | 竞赛风格数学推理 | 最终答案的精确/数学判分 | 是 |
| `gpqa` | 研究生级别科学选择题 | 选项字母提取与精确匹配 | 是 |
| `hle` | Humanity's Last Exam 专家级问答 | 相对 benchmark 参考的 LLM 辅助判分 | 是 |
| `livecodebench` | 编程题求解 | 代码提取与内置 evaluator 执行测试 | 是 |
| `livemathbench` | 近期/实时数学推理 | 最终答案的精确/数学判分 | 是 |
| `mmlupro` | 多领域选择题知识与推理 | 选项字母提取与精确匹配 | 是 |
| `simpleqa` | 短形式事实 QA | 相对短参考答案的 LLM 辅助判分 | 是 |
| `arenahard` | 开放式指令遵循与对话质量 | 相对 baseline 答案的 pairwise LLM 评判 | 是 |
| `swe-bench` | SWE-Bench Verified 仓库级 issue 修复 | 单轮 patch 生成记录，用于路由与聚合研究 | 否 |

#### SWE-Bench 说明

发布数据使用经人工校验的 500 条 **SWE-bench Verified** 子集（`split=verified`）。集思将每个SWE-Bench 实例视为**单轮** patch 生成任务：将 issue、仓库上下文与 patch 格式说明打包为一条 prompt，模型返回一个 patch 候选。这些记录可用于比较路由与聚合决策，但并非交互式 coding-agent 轨迹。

SWE-Bench无法通过标准集思`post_eval` 流程验证，因其基于 patch 且依赖 SWE-Bench 提交/评测后端。

本仓库提供后续 SWE-Bench 验证辅助脚本：

```text
baselines/JiSi/test_swe.py
```

该脚本改编自 SWE-Bench 官方验证流程：读取 `result.jsonl`，筛选 `dataset` 含 `swe` 的行，必要时用 `swe_imap.json` 将 集思 本地 `index` 映射为官方 `instance_id`，从模型回复提取 patch，写入 SWE-Bench 预测文件，并可通过 `sb-cli submit swe-bench_verified test` 提交。其与 `post_eval` 分离，属于 patch 校验步骤，而非常规答案抽取评分。

重新运行时通过 CLI 传入路径与元数据：

| 参数 | 含义 |
| --- | --- |
| `--res_path` | 含生成答案的集思`result.jsonl` 路径 |
| `--output` | `sb-cli` 使用的预测 JSON；默认与 `result.jsonl` 同目录下的 `swe_result.json` |
| `--index-map` | 集思本地 SWE 索引到官方 `instance_id` 的 JSON 映射；仅当结果行已含 `instance_id` 时可省略 |
| `--benchmark-file` | 可选的已发布 `benchmark_bank` SWE-Bench JSON；可从 `records` 推导 `index` → `instance_id` |
| `--run-id` | 提交 run id |
| `--model-name` | 写入 SWE-Bench 预测文件的模型/系统名 |

脚本输出的预测文件以官方 SWE-Bench `instance_id` 为键：

```json
{
    "astropy__astropy-12907": {
        "model_patch": "... unified diff ...",
        "model_name_or_path": "jisi_run_name"
    }
}
```

生成 SWE-Bench 预测 JSON：

```bash
python -m baselines.JiSi.test_swe \
  --res_path results/jisi/<run_name>/result.jsonl \
  --index-map path/to/swe_imap.json \
  --model-name jisi_run_name \
  --run-id jisi_swe_verified
```

若使用已发布的 `benchmark_bank` SWE-Bench JSON 而非 `swe_imap.json`，请传入 `--benchmark-file`：

```bash
python -m baselines.JiSi.test_swe \
  --res_path results/jisi/<run_name>/result.jsonl \
  --benchmark-file .hf_jisi_data/benchmark_bank/swe-bench/verified/<model>/<file>.json \
  --model-name jisi_run_name \
  --run-id jisi_swe_verified
```

在已安装 `sb-cli` 且配置 `SWEBENCH_API_KEY` 时可加 `--submit`：

```bash
SWEBENCH_API_KEY=$SWEBENCH_API_KEY \
python -m baselines.JiSi.test_swe \
  --res_path results/jisi/<run_name>/result.jsonl \
  --index-map path/to/swe_imap.json \
  --model-name jisi_run_name \
  --run-id jisi_swe_verified \
  --submit
```

### 📈 Question Bank 模型池

发布的 question bank 包含以下开源模型的 benchmark 回复与正误记录。开箱即用的 `example_data/` 使用其中 10 个模型；更完整的 `benchmark_bank/` 在可用时保留更多模型输出。我们将持续用最新开源模型更新 question bank。

| Model | 在 `example_data/` | 在 `benchmark_bank/` |
| --- | --- | --- |
| `deepseek-r1-0528` | 是 | 是 |
| `deepseek-v3-0324` | 是 | 是 |
| `deepseek-v3.1-terminus` | 是 | 是 |
| `deepseek-v3.2-speciale` | 是 | 是 |
| `deepseek-v3.2-thinking` | 是 | 是 |
| `glm-4.6` | 是 | 是 |
| `glm-5` | 否 | 是 |
| `intern-s1` | 是 | 是 |
| `kimi-k2-0905` | 是 | 是 |
| `kimi-k2.5` | 否 | 是 |
| `minimax-m2.5` | 否 | 是 |
| `qwen3-235b-a22b-2507` | 是 | 是 |
| `qwen3-235b-a22b-thinking-2507` | 是 | 是 |
| `qwen3.5-397b-a17b` | 否 | 是 |

也可用 `datasets` 直接查看开箱即用划分：

```python
from datasets import load_dataset

repo_id = "aisfuture/jisi_data"
ds = load_dataset(repo_id, "jisi_example")

print(ds)
print(ds["train"][0].keys())
```

## ⚙️ 配置说明

若已按[快速开始](#-快速开始)操作，则已创建 `*.local.json` 与 `embedding_config.local.yaml`。请编辑这些本地副本且勿提交到仓库。主要字段：

| 字段 | 含义 |
| --- | --- |
| `train_data_path` / `test_data_path` | 预划分的 集思 support/test 文件 |
| `baseline_scores_path` | 用于报告的各模型 benchmark 分数 |
| `embedding_config_path` | OpenAI 兼容 embedding 端点配置 |
| `api_config_path` | 聚合用的 OpenAI 兼容 LLM 端点配置 |
| `mode` | `router` 或 `aggregator` |
| `rag_num` | 路由证据所检索的 support 问题数量 |
| `agg_model` | 聚合模型名，或 `auto` 从路由候选中选择 |
| `result_dir` | 聚合结果输出目录 |

API 配置支持字面量 key、key 列表、环境变量或命名 key：

```json
{
  "extra_api_keys": {
    "OPENAI": "OPENAI_API_KEY",
    "LOCAL": "EMPTY"
  },
  "model_configs": {
    "gpt-4.1-mini": {
      "mode": "openai",
      "model_name": "gpt-4.1-mini",
      "base_url": "https://api.openai.com/v1",
      "api_key_name": "OPENAI"
    }
  }
}
```

## 🧪 可选：数据采集

若需从零重建模型输出缓存，可使用 data collector：

```bash
python -m data_collector.cli info config/data_collector_example.yaml
python -m data_collector.cli run config/data_collector_example.yaml
```

采集结果写入 `results/bench/`，可通过 `baselines.adaptors.jisi_adaptor` 转换。

## 🏗️ 仓库结构

```text
JiSi/
  assets/                    论文与 README 插图
  baselines/
    JiSi/                    集思算法实现
    adaptors/                集思数据 adaptor
  common/cache/              可选MySQL缓存
  config/                    示例配置
  data/                      外部数据占位
  data_collector/            可选 benchmark 采集工具
  evaluation/                Benchmark evaluator
  generators/                OpenAI 兼容生成封装
  results/                   外部结果占位
```

## 🙏 致谢

感谢 [ynulihao/LLMRouterBench](https://github.com/ynulihao/LLMRouterBench)。本代码库由该项目重构而来，开源 集思 版本仅保留数据准备、路由、聚合与评测所需组件。

## 📜 引用

若集思对您的研究有帮助，请引用：

```bibtex
@misc{tang2026gemini3prorevisitingllmrouting,
  title={Beyond Gemini-3-Pro: Revisiting LLM Routing and Aggregation at Scale},
  author={Shengji Tang and Weihao Lin and Peng Ye and Jingqi Ye and Hao Li and Yiqun Zhang and Xiaosong Wang and Bo Zhang and Shuyue Hu and Tao Chen and Lei Bai and Wanli Ouyang},
  year={2026},
  eprint={2601.01330},
  archivePrefix={arXiv},
  primaryClass={cs.AI},
  url={https://arxiv.org/abs/2601.01330}
}
```

## 📄 许可证

本仓库采用 [MIT License](./LICENSE)。数据集、模型权重与外部 benchmark 资源可能适用各自许可证。
