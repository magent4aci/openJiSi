# Data Collector

The data collector is an optional utility for producing the model-output records used by JiSi. It runs configured models on supported evaluators, stores raw outputs, evaluator scores, token usage, and cost estimates, then writes JSON result files under `results/bench/`.

## Quick Start

```bash
python -m data_collector.cli info config/data_collector_example.yaml
python -m data_collector.cli run config/data_collector_example.yaml
python -m data_collector.cli list
```

Use `demo_mode: true` in the config for a cheap smoke test before launching a full collection run.

## Configuration

Start from `config/data_collector_example.yaml`. API keys should be environment variable names, for example:

```yaml
models:
  - name: gpt-4.1-mini
    api_model_name: gpt-4.1-mini
    base_url: https://api.openai.com/v1
    api_key: OPENAI_API_KEY
```

The loader resolves `api_key` from the environment when the value matches an environment variable name. Local OpenAI-compatible endpoints can use `api_key: EMPTY`.

## Output

Collection results are written to:

```text
results/bench/<dataset>/<split>/<model>/<timestamp>.json
```

After collection, convert the results to JiSi format:

```bash
python -m baselines.adaptors.jisi_adaptor \
  --config config/adaptor/jisi_llm_v1.example.yaml \
  --output-dir data/jisi
```

Large generated outputs and caches should remain outside git.
