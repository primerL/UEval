# UEval: A Benchmark for Unified Multimodal Generation

Official code of UEval: A Benchmark for Unified Multimodal Generation

> [**UEval: A Benchmark for Unified Multimodal Generation**](https://arxiv.org/abs/2502.12150) </br>
> *[Bo Li](https://primerl.github.io/), [Yida Yin](https://davidyyd.github.io), [Wenhao Chai](https://wenhaochai.com/), [Xingyu Fu](https://zeyofu.github.io/)\*, [Zhuang Liu](https://liuzhuang13.github.io)\* (* indicates co-advising) <br>
> Princeton University<br>
> [[Paper]](https://arxiv.org/abs/2502.12150) [[Project page]](https://wenhaochai.com/ueval/) [[Dataset]](https://huggingface.co/datasets/primerL/UEval)

```bibtex
@article{xxx,
    title    = {UEval: A Benchmark for Unified Multimodal Generation},
    author   = {xx},
    year     = {2025},
    journal  = {}
}
```

---

<p align="center">
<img src="https://github.com/user-attachments/assets/ac33d27a-a1c1-4e71-bfe7-f20af55b67c6" width=100% height=100%
class="center">
</p>

We introduce **UEval**, a benchmark to evaluate unified models, i.e., models capable of generating both images and text. UEval comprises 1,000 expert-curated prompts that require both images and text in the model outputs, sourced from 8 diverse real-world domains.


## Evaluation

### Quick Start

We provide `ueval_eval.py` for efficient evaluation using the Gemini API with caching strategy to reduce costs.

#### Prerequisites

1. Install required packages:
```bash
pip install google-generativeai datasets pillow
```

2. Set up your Gemini API key:
```bash
export GEMINI_API_KEY="your-api-key-here"
```

#### Evaluate Your Model

Prepare your model outputs in JSON format:
```json
[
  {
    "id": "1",
    "text_answer": "Your model's text response",
    "image_answer": ["path/to/generated/image1.jpg", "path/to/generated/image2.jpg"]
  },
  ...
]
```

Run evaluation:
```bash
python ueval_eval.py \
  --model_output_path path/to/your_model_outputs.json \
  --output_path results/your_model_results.json \
```

#### Advanced Options

```bash
# With caching (recommended for models generating multiple images per question)
python eval_cache_v2.py \
  --model_output_path path/to/outputs.json \
  --output_path path/to/results.json \
  --text_field text_answer \
  --image_field image_answer \
  --api_key YOUR_API_KEY \
  --limit 10 \

# Without caching (recommended for models generating single images)
python eval_cache_v2.py \
  --model_output_path path/to/outputs.json \
  --output_path results/results.json \
  --text_field text_answer \
  --image_field image_answer \
  --api_key YOUR_API_KEY \
  --limit 100 \
  --no_cache
```
```

**Key Arguments:**
- `--model_output_path`: Path to your model's output JSON file (required)
- `--output_path`: Where to save evaluation results (required)
- `--hf_dataset`: HuggingFace dataset ID (default: `primerL/UEval-all`)
- `--text_field`: Field name for text answers in your output file (default: `text_answer`)
- `--image_field`: Field name for image paths in your output file (default: `image_answer`)
- `--api_key`: Gemini API key (or set `GEMINI_API_KEY` environment variable)
- `--limit`: Number of examples to evaluate (default: all)
- `--checkpoint_interval`: Save checkpoint every N items (default: 1)
- `--no_cache`: Disable caching for single-image outputs (optional)

### Evaluation Strategy

UEval uses a rubric-based evaluation approach:

1. **Rubric Generation**: For each question, rubrics are automatically generated using Gemini API


3. **Scoring**: Each rubric criterion is evaluated as met/not met, with final scores computed as:
   - Text score = (text criteria met) / (total text criteria)
   - Image score = (image criteria met) / (total image criteria)
   - Overall score = (text score + image score) / 2

### Output Format

Evaluation results are saved in JSON format:
```json
{
  "results": [
    {
      "id": "1",
      "text_rate": 0.85,
      "image_rate": 0.90,
      "text_rubrics": [...],
      "image_rubrics": [...],
      "text_evaluations": [...],
      "image_evaluations": [...]
    },
    ...
  ],
  "summary": {
    "num_items": 1000,
    "num_items_with_text": 1000,
    "num_items_with_image": 1000,
    "text_avg_rate": 0.82,
    "image_avg_rate": 0.78,
    "overall_avg_rate": 0.80
  }
}
```

## Results

*Coming soon: Benchmark results for various unified multimodal models*

## License

This project is released under the MIT license. Please see the [LICENSE](LICENSE) file for more information.

## Questions

Feel free to discuss papers/code with us through issues/emails!

bettyli2332 at gmail.com
