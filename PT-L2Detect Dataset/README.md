# PT-L2Detect: A Benchmark for AI-Generated Text Detection in Portuguese L2 Essays

**PT-L2Detect** is the first benchmark dataset for detecting AI-generated Portuguese essays written by second-language (L2) learners across CEFR proficiency levels (A1–C1). The dataset pairs real learner essays from established L2 Portuguese corpora with AI-generated counterparts produced by five modern large language models under CEFR-aware prompting, enabling research on proficiency-calibrated machine-generated text (MGT) detection.

## Overview

| Property | Value |
|---|---|
| **Task** | Binary classification: Human-written vs. AI-generated |
| **Language** | Portuguese (L2 learners) |
| **Total essays** | 2,850 |
| **Human essays** | 1,350 (COPLE2 + PEAPL2) |
| **AI essays** | 1,500 (5 models × 15 topics × 20 essays) |
| **CEFR levels** | A1, A2, B1, B2, C1 |
| **Split strategy** | Topic-disjoint (held-out topics per CEFR for test) |
| **License** | CC BY-SA-NC 4.0 (COPLE2 subset) + Research-only (PEAPL2 + AI-generated) |

## Why PT-L2Detect?

Existing MGT detection benchmarks focus on native or high-proficiency English text. However, real-world educational settings involve **L2 learners at various proficiency levels**, where:

1. **Surface features of beginner writing overlap with AI outputs** (short sentences, simple vocabulary, predictable structures), causing detectors to disproportionately flag low-proficiency students.
2. **CEFR proficiency is the strongest known predictor of detector error** — yet no existing benchmark provides CEFR labels for studying this interaction.
3. **Portuguese L2 NLP lacks any MGT detection resource**, despite growing demand for Portuguese language assessment tools.

PT-L2Detect fills this gap by providing a **proficiency-labeled, topic-disjoint** benchmark designed to evaluate both detection accuracy and fairness across learner proficiency levels.

## Dataset Composition

### Human Essays

Human essays are sourced from two established Portuguese L2 learner corpora:

| Source | Essays | CEFR Range | Description |
|---|---|---|---|
| **COPLE2** | 942 | A1–C1 | Corpus de Português Língua Estrangeira (Universidade de Lisboa) |
| **PEAPL2** | 480 | A1–C1 | Plataforma de Ensino e Aprendizagem do Português como L2 |

All human essays are authentic learner productions, written under exam or coursework conditions, and preserve original spelling, grammar, and punctuation (including learner errors).

### AI-Generated Essays

AI essays were generated using **CEFR-aware prompts** that specify the target proficiency level and writing topic. For each of 15 topics, 20 essays were generated per model.

| Model | Essays | Provider |
|---|---|---|
| **ChatGPT** (GPT-4o) | 300 | OpenAI |
| **Qwen** (Qwen2.5) | 300 | Alibaba |
| **DeepSeek** (DeepSeek-V3) | 300 | DeepSeek |
| **GLM** (GLM-4) | 300 | Zhipu AI |
| **Gemini** (Gemini 1.5 Pro) | 300 | Google |

**CEFR-aware prompting strategy**: Each generation prompt includes the CEFR level descriptor, expected vocabulary range, typical grammatical structures, and approximate word count for the target level. This ensures AI essays mirror the linguistic characteristics of authentic learner writing at each proficiency band.

### CEFR Distribution

| Level | Human | AI | Total |
|-------|-------|-----|-------|
| A1 (Beginner) | 314 | 400 | 714 |
| A2 (Elementary) | 324 | 200 | 524 |
| B1 (Intermediate) | 367 | 300 | 667 |
| B2 (Upper-Intermediate) | 233 | 300 | 533 |
| C1 (Advanced) | 112 | 300 | 412 |
| **Total** | **1,350** | **1,500** | **2,850** |

### Word Count by CEFR Level

The dataset exhibits a natural word-count gradient consistent with CEFR proficiency expectations:

| Level | Min | Max | Mean |
|-------|-----|-----|------|
| A1 | 51 | 99 | 67 |
| A2 | 54 | 89 | 65 |
| B1 | 99 | 173 | 130 |
| B2 | 122 | 200 | 152 |
| C1 | 205 | 276 | 243 |

### Topics

Fifteen writing topics span personal narratives, opinion essays, and argumentative prompts:

| Topic # | Topic (Portuguese) | CEFR |
|---------|-------------------|------|
| 1 | O meu dia a dia | A1 |
| 2 | O seu tempo livre | A1 |
| 3 | Descrever a sua casa | A1 |
| 4 | O dia a dia da mãe | A1 |
| 5 | Contar a sua infância | A2 |
| 6 | O que fez nas últimas férias de verão | A2 |
| 7 | Descrever a sua cidade | B1 |
| 13 | A escola ideal para os jovens | B1 |
| 14 | Os jovens passam demasiado tempo na Internet? | B1 |
| 8 | Os impactos positivos da tecnologia | B2 |
| 9 | Causas da poluição ambiental | B2 |
| 15 | A escolha da profissão deve seguir o sonho ou o mercado? | B2 |
| 10 | O impacto da IA no mercado de trabalho | C1 |
| 11 | As alterações climáticas e a responsabilidade individual | C1 |
| 12 | A imigração e os desafios da integração cultural | C1 |

## Split Design

PT-L2Detect uses a **topic-disjoint split** to prevent topic memorization from inflating detection performance. One topic per CEFR level is held out entirely for testing. The remaining topics are split 82/18 (stratified) into train and validation sets.

| Split | Essays | Human | AI |
|-------|--------|-------|-----|
| **Train** | 1,781 (62.5%) | 942 | 839 |
| **Validation** | 361 (12.7%) | 200 | 161 |
| **Test** | 708 (24.8%) | 208 | 500 |

### Held-Out Test Topics

| CEFR | Test Topic |
|------|-----------|
| A1 | Topic 4: O dia a dia da mãe |
| A2 | Topic 6: O que fez nas últimas férias de verão |
| B1 | Topic 14: Os jovens passam demasiado tempo na Internet? |
| B2 | Topic 15: A escolha da profissão... sonho ou mercado? |
| C1 | Topic 12: A imigração e os desafios da integração cultural |

## Data Format

Each essay is stored as a JSON object (one per line in `.jsonl` files) with the following schema:

```json
{
  "essay_id": "cople2_VE_1_19_55.2M",
  "source": "cople2",
  "text": "A publicidade tem um papel muito importante na nossa vida...",
  "cefr_level": "A1",
  "is_ai": false,
  "word_count": 176,
  "l1": "Italian",
  "format": "paragraph-level",
  "category": "learner",
  "license": "CC BY-SA-NC 4.0",
  "model": null,
  "topic_num": null,
  "topic_title": null,
  "prompt_topic": null
}
```

| Field | Type | Description |
|-------|------|-------------|
| `essay_id` | string | Unique identifier |
| `source` | string | `"cople2"`, `"peapl2"`, or `"ai-generated"` |
| `text` | string | Full essay text (original spelling preserved) |
| `cefr_level` | string | CEFR proficiency level: A1, A2, B1, B2, or C1 |
| `is_ai` | bool | `true` for AI-generated, `false` for human-written |
| `word_count` | int | Number of tokens (whitespace-split) |
| `l1` | string | Learner's first language (human only; may be `null`) |
| `format` | string | Always `"paragraph-level"` |
| `category` | string | Always `"learner"` |
| `license` | string | License string |
| `model` | string | AI model name (AI essays only; `null` for human) |
| `topic_num` | int | Topic number 1–15 (AI + topic-mapped human essays) |
| `topic_title` | string | Topic description (AI + topic-mapped human essays) |
| `prompt_topic` | string | Writing prompt (AI essays only; `null` for human) |

## File Structure

```
data/processed/
├── all_full.jsonl        # All 2,850 essays (human + AI)
├── all_essays.jsonl      # 1,422 human essays only
├── ai_essays.jsonl       # 1,500 AI-generated essays
├── train_full.jsonl      # Training split (1,781 essays)
├── val_full.jsonl        # Validation split (361 essays)
├── test_full.jsonl       # Test split (708 essays)
├── train.jsonl           # Training split (detector-preprocessed)
├── val.jsonl             # Validation split (detector-preprocessed)
├── test.jsonl            # Test split (detector-preprocessed)
└── dataset_stats.json    # Summary statistics
```

## Quick Start

### Prerequisites

- Python 3.11 or 3.12
- 1–2 GB disk space for dataset and embeddings

### Installation

```bash
git clone https://github.com/<user>/PT-L2Detect.git
cd PT-L2Detect
python3.12 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Loading the Dataset

```python
import json

# Load all essays
essays = []
with open('data/processed/all_full.jsonl', 'r', encoding='utf-8') as f:
    for line in f:
        essays.append(json.loads(line))

# Filter by CEFR level
a1_essays = [e for e in essays if e['cefr_level'] == 'A1']
human_essays = [e for e in essays if not e['is_ai']]
ai_essays = [e for e in essays if e['is_ai']]

print(f"Total: {len(essays)} essays")
print(f"Human: {len(human_essays)}, AI: {len(ai_essays)}")
print(f"A1 human: {len([e for e in a1_essays if not e['is_ai']])}")
print(f"A1 AI: {len([e for e in a1_essays if e['is_ai']])}")
```

## Benchmarks

The following table summarizes baseline and state-of-the-art results on PT-L2Detect. All results use the topic-disjoint test split.

| Method | Global F1 | A1 FPR | A2 FPR | B1 FPR | B2 FPR | C1 FPR | Description |
|--------|-----------|--------|--------|--------|--------|--------|-------------|
| XLM-R + MLP (single τ=0.5) | 0.896 | 8.33% | 6.00% | 3.57% | 2.78% | 5.56% | Baseline: global threshold |
| **ProAID** (CEFR-calibrated τ) | **0.895** | **4.17%** | **4.00%** | **3.57%** | **2.78%** | 11.11% | Per-group thresholds via 3-class CEFR classifier |

> **Key finding**: Per-CEFR calibration reduces Beginner (A1+A2) false positive rate by **43%** while maintaining global detection F1 within 0.2% of the single-threshold baseline. See [ProAID paper](paper/) for full results, ablation studies, and sensitivity analysis.

## Related Resources

- **ProAID Paper**: `paper/` — Full LaTeX source and compiled PDF
- **Experiment Logs**: `results/` — Raw metrics, training curves, threshold sweeps
- **PROGRESS.md** — Full project status, experiment tracking, and claim-evidence matrix
- **PAPER_PLAN.md** — Paper outline, figure plan, and citation strategy

## Ethical Considerations

- **Fairness motivation**: This benchmark is explicitly designed to study and mitigate detector bias against low-proficiency L2 learners, a vulnerable population in educational assessment contexts.
- **AI essay generation**: AI-generated essays were produced solely for research purposes and are clearly labeled. They should not be used to train production detectors without careful validation.
- **Human data**: COPLE2 and PEAPL2 essays are used under their original licenses. PEAPL2 data requires research-only usage. Do not redistribute without checking original corpus terms.
- **Dual-use awareness**: MGT detection tools can be used for both supporting academic integrity and unfairly penalizing non-native writing patterns. We release this benchmark to advance fairness-aware detection research.

## Citation

If you use PT-L2Detect in your research, please cite:

```bibtex
@inproceedings{proaid2026,
  title     = {{ProAID}: Proficiency-Calibrated {AI}-Generated Text Detection for {Portuguese} {L2} Essays},
  author    = {Nguyen, Thang and others},
  booktitle = {Proceedings of the 4th International Conference on Intelligent System (ICIS)},
  year      = {2026},
  series    = {Lecture Notes in Computer Science},
  publisher = {Springer}
}
```

## License

The dataset is released under a **mixed license**:
- **COPLE2 subset**: [CC BY-SA-NC 4.0](https://creativecommons.org/licenses/by-sa-nc/4.0/)
- **PEAPL2 subset**: Research-only (contact PEAPL2 authors for other uses)
- **AI-generated essays**: Released for research purposes with attribution

Code in this repository is available under the MIT License.

## Acknowledgments

We thank the authors of COPLE2 (Santos et al., Universidade de Lisboa) and PEAPL2 (Antunes & Mendes) for making their Portuguese L2 learner corpora available for research. AI essay generation was performed using public APIs from OpenAI, Alibaba, DeepSeek, Zhipu AI, and Google.
