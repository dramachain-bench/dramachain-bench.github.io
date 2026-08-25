<div align="center">

# DramaChain Bench

### An End-to-End Benchmark for Short-Drama Generation

**From Production Pipeline and Annotation System to Validated Automated Evaluation**

Haoyuan Shi<sup>1</sup> &nbsp; Mingtao Chen<sup>1</sup> &nbsp; Shuo Jiang<sup>1</sup> &nbsp; Ziyan Chen<sup>1,2</sup><br>
Xuyi Sheng<sup>3</sup> &nbsp; Yiming Liu<sup>1</sup> &nbsp; Ying Zhang<sup>1</sup> &nbsp; Miao Wang<sup>1,4</sup><br>
Jianxiang Lu<sup>1</sup> &nbsp; Fanyang Lu<sup>1</sup> &nbsp; Songyuanyi Lu<sup>1</sup> &nbsp; Xiele Wu<sup>1</sup><br>
Zhichao Hu<sup>1,†</sup> &nbsp; Lilin Wang<sup>1</sup> &nbsp; Yuhong Liu<sup>1</sup> &nbsp; Richeng Xuan<sup>1,§</sup>

<sup>1</sup>Hunyuan, Tencent &nbsp;&nbsp; <sup>2</sup>Beijing Film Academy &nbsp;&nbsp; <sup>3</sup>Peking University &nbsp;&nbsp; <sup>4</sup>Shenzhen University

<sup>†</sup>Project Lead &nbsp;&nbsp; <sup>§</sup>Corresponding Author

</div>

---

## Abstract

Commercial short-drama production follows a multi-stage chain: script, storyboard, keyframe imagery, shot-level video, and the finished short drama. Most existing benchmarks evaluate solely the video-generation stage using pre-authored inputs instead of real upstream pipeline outputs. This leaves two critical questions unanswerable: whether each stage adheres to the original *script* intent (rather than only its immediate input prompt), and whether disparate shots remain coherent after assembly into multi-episode releases.

We present **DramaChain Bench**, the first short-drama benchmark that evaluates every stage of the complete production chain. It is built upon three in-house systems sharing one dimension system, **DramaChain Dimensions**: five evaluation axes instantiated at every stage, resolving into 63 leaf dimensions. **DramaChain Agent** is calibrated against commercial short-drama platforms in both workflow and finished short-drama quality, enabling stage-wise fair comparison across models. The **DramaChain Labeling System** has each of the 5,785 items scored independently by three professional annotators, with all defects spatio-temporally localised and selected from a predefined defect list. This process produces 17,488 valid scores and 255,925 traceable attribution records. The human annotations confirm that upstream defects cascade across the pipeline, demonstrating that final episode quality is not governed by video generation alone. The **DramaChain Agentic Judge** then scores every leaf dimension automatically, gathering evidence over multiple agentic rounds before judging against a per-item checklist; it reproduces the model ranking at a mean PLCC of 0.918, enough to admit new models at no annotation cost.

<div align="center">
<img src="assets/overview.png" alt="DramaChain Bench overview: the dimension system, the production pipeline, the labeling system and the agentic judge" width="820">
</div>

---

## Highlights

**One dominant chain, operating near the usable line.** Excluding models added after the annotation round, the state-of-the-art pipeline is `gpt-5.5-xhigh` for storyboarding, `gpt-image-2` for keyframes and `seedance-2.0` for video. Within each modality the leading model is invariant to evaluation granularity. That chain delivers a finished short drama at 3.30, only 0.3 above the 3.0 usable line, with its best single-shot result at 3.24.

**Quality decays along the chain.** Mean scores fall from 3.78 at storyboard design to 3.62 for single keyframes, 3.30 for episode keyframes, 3.09 for single-shot video and 2.93 for episode video, rebounding modestly to 3.03 for the finished drama. No pixel- or video-generation stage reaches the mean achieved at the text stage.

**Upstream defects accumulate rather than stay local.** Degrading exactly one upstream stage costs a single clip 0.07–0.13 points but costs the finished short drama 0.25–0.83. What ships is therefore not determined by the video-generation stage alone.

**Discrimination tracks reference explicitness, not task difficulty.** Averaged across applicable stages, top-to-bottom gaps are 1.22 on input fidelity and 0.70 on internal consistency — both validated against an external artefact — versus 0.53 on generation plausibility and 0.42 on cinematic expressiveness, which have no external reference. Small gaps on weakly anchored metrics reflect a benchmark limit, not comparable models.

**Composite scores hide dimensional trade-offs.** `seedream-5.0-pro` and `nano-banana-pro` differ by 0.02 overall, yet by 0.39 on input fidelity and 0.60 on generation plausibility in opposite directions. Composites are suitable for tier grouping and misleading for model selection.

**Automated scoring reproduces the board.** The agentic judge reaches a mean model-level PLCC of 0.918 against the three-annotator panel, which is what allows 8 further models to enter the leaderboard at no annotation cost.

---

## Leaderboard

Automated board, 5-point scale, over six production stages and five evaluation axes. **All** is the cross-axis composite.

| Notation | Meaning |
|---|---|
| **bold** | best value in the column among fully annotated models |
| *italic* | below the 3.0 usable line |
| — | axis not defined at this stage |
| † | added after the annotation round, scored automatically only; excluded from agreement metrics and best-column marking |
| ‡ | partial corpus run (reduced dramas, paradigms or styles); scores from the evaluated subset |

Stage headers give model-level PLCC and SRCC against the three-annotator human board.

<details open>
<summary><b>① Storyboard design</b> &nbsp;·&nbsp; text → text &nbsp;·&nbsp; PLCC 0.936 &nbsp;·&nbsp; SRCC 0.800</summary>

<br>

| Model | F<br>Input fidelity | C<br>Internal consistency | P<br>Generation plausibility | Q<br>Visual quality | E<br>Cinematic expressiveness | All |
|---|---|---|---|---|---|---|
| `gpt-5.5-xhigh` | **4.84** | — | **3.89** | — | 4.10 | **4.23** |
| `claude-opus-4.8-max` | 4.51 | — | 3.80 | — | **4.19** | 4.17 |
| `hy3` | 4.36 | — | 3.65 | — | 3.78 | 3.89 |
| `doubao-seed-2.1-pro` | 4.26 | — | 3.65 | — | 3.82 | 3.89 |
| `glm-5.2` | 4.01 | — | 3.66 | — | 3.74 | 3.79 |
| `gemini-3.1-pro` | 4.08 | — | 3.43 | — | 3.67 | 3.71 |
| `kimi-k2.6` | 3.66 | — | 3.42 | — | 3.74 | 3.64 |
| `qwen3.7-max` | 3.24 | — | 3.48 | — | 3.59 | 3.48 |
| `mimo-v2.5-pro` | *2.68* | — | 3.53 | — | 3.41 | 3.26 |
| `gpt-5.6-sol-max` † | 4.95 | — | 4.02 | — | 4.31 | 4.40 |
| `claude-fable-5-max` † | 4.55 | — | 3.99 | — | 4.20 | 4.24 |
| `claude-opus-5-max` † | 4.45 | — | 3.85 | — | 4.30 | 4.22 |
| `kimi-k3` † | 4.67 | — | 3.87 | — | 4.17 | 4.22 |
| `qwen3.8-max` † | 4.55 | — | 3.88 | — | 3.94 | 4.08 |
| `grok-4.5` † | 3.96 | — | 3.85 | — | 3.92 | 3.92 |
| `gemini-3.6-flash-high` † | 4.04 | — | 3.65 | — | 3.71 | 3.78 |

</details>

<details>
<summary><b>② Single keyframe</b> &nbsp;·&nbsp; text + refs → pixels &nbsp;·&nbsp; PLCC 0.959 &nbsp;·&nbsp; SRCC 0.750</summary>

<br>

| Model | F<br>Input fidelity | C<br>Internal consistency | P<br>Generation plausibility | Q<br>Visual quality | E<br>Cinematic expressiveness | All |
|---|---|---|---|---|---|---|
| `gpt-image-2` | **3.65** | **4.32** | **4.47** | **4.13** | — | **3.99** |
| `gpt-image-1.5` | 3.25 | 3.96 | 4.46 | 3.90 | — | 3.72 |
| `nano-banana-2` | 3.33 | 3.92 | 4.25 | 3.71 | — | 3.68 |
| `seedream-5.0-pro` | 3.56 | 3.92 | 3.74 | 3.52 | — | 3.64 |
| `nano-banana-pro` | 3.17 | 3.89 | 4.34 | 3.79 | — | 3.62 |
| `wan2.7-image-pro` | *2.95* | 3.63 | 4.03 | 3.46 | — | 3.38 |
| `seedream-5.0-lite` ‡ | *2.96* | 3.40 | 3.89 | 3.40 | — | 3.32 |

</details>

<details>
<summary><b>③ Episode keyframes</b> &nbsp;·&nbsp; a whole episode of frames &nbsp;·&nbsp; PLCC 0.973 &nbsp;·&nbsp; SRCC 0.964</summary>

<br>

| Model | F<br>Input fidelity | C<br>Internal consistency | P<br>Generation plausibility | Q<br>Visual quality | E<br>Cinematic expressiveness | All |
|---|---|---|---|---|---|---|
| `gpt-image-2` | — | **3.82** | — | — | — | **3.82** |
| `gpt-image-1.5` | — | 3.60 | — | — | — | 3.60 |
| `nano-banana-2` | — | 3.47 | — | — | — | 3.47 |
| `seedream-5.0-pro` | — | 3.28 | — | — | — | 3.28 |
| `nano-banana-pro` | — | 3.04 | — | — | — | 3.04 |
| `wan2.7-image-pro` | — | 3.00 | — | — | — | 3.00 |
| `seedream-5.0-lite` ‡ | — | *2.91* | — | — | — | *2.91* |

</details>

<details>
<summary><b>④ Single-shot video</b> &nbsp;·&nbsp; pixels → time + audio &nbsp;·&nbsp; PLCC 0.755 &nbsp;·&nbsp; SRCC 0.829</summary>

<br>

| Model | F<br>Input fidelity | C<br>Internal consistency | P<br>Generation plausibility | Q<br>Visual quality | E<br>Cinematic expressiveness | All |
|---|---|---|---|---|---|---|
| `seedance-2.0` ‡ | 3.44 | **3.27** | 3.06 | **3.77** | 3.05 | **3.24** |
| `happyhorse-1.1` | 3.34 | 3.21 | 3.04 | 3.43 | 3.08 | 3.18 |
| `kling-3.0-omni` | 3.22 | 3.23 | **3.08** | 3.28 | *2.88* | 3.11 |
| `pixverse-c1` | 3.07 | 3.09 | *2.92* | 3.42 | **3.10** | 3.10 |
| `wan2.7` ‡ | **3.45** | *2.96* | *2.87* | 3.22 | *2.92* | 3.04 |
| `veo-3.1` ‡ | 3.17 | *2.77* | *2.73* | 3.32 | *2.76* | *2.88* |
| `seedance-2.5` †‡ | 3.24 | 3.40 | *2.97* | 3.71 | *2.85* | 3.17 |

</details>

<details>
<summary><b>⑤ Episode video</b> &nbsp;·&nbsp; shots in sequence &nbsp;·&nbsp; PLCC 0.935 &nbsp;·&nbsp; SRCC 0.943</summary>

<br>

| Model | F<br>Input fidelity | C<br>Internal consistency | P<br>Generation plausibility | Q<br>Visual quality | E<br>Cinematic expressiveness | All |
|---|---|---|---|---|---|---|
| `seedance-2.0` ‡ | — | ***2.96*** | 3.41 | — | 3.32 | **3.11** |
| `happyhorse-1.1` | — | *2.84* | **3.64** | — | 3.37 | 3.08 |
| `kling-3.0-omni` | — | *2.79* | 3.31 | — | **3.48** | 3.06 |
| `wan2.7` ‡ | — | *2.57* | 3.24 | — | 3.35 | *2.87* |
| `veo-3.1` ‡ | — | *2.47* | 3.27 | — | 3.18 | *2.76* |
| `pixverse-c1` | — | *2.42* | 3.08 | — | 3.16 | *2.71* |
| `seedance-2.5` †‡ | — | 3.16 | 3.11 | — | 3.50 | 3.25 |

</details>

<details>
<summary><b>⑥ Short drama</b> &nbsp;·&nbsp; the finished short drama &nbsp;·&nbsp; PLCC 0.947 &nbsp;·&nbsp; SRCC 1.000</summary>

<br>

| Model | F<br>Input fidelity | C<br>Internal consistency | P<br>Generation plausibility | Q<br>Visual quality | E<br>Cinematic expressiveness | All |
|---|---|---|---|---|---|---|
| `seedance-2.0` ‡ | **4.29** | 3.38 | — | — | *2.68* | **3.30** |
| `happyhorse-1.1` | 3.82 | 3.38 | — | — | ***2.69*** | 3.22 |
| `kling-3.0-omni` | 3.37 | **3.41** | — | — | *2.54* | 3.11 |
| `wan2.7` ‡ | 4.12 | *2.80* | — | — | *2.59* | *2.95* |
| `pixverse-c1` | 3.05 | *2.88* | — | — | *2.58* | *2.81* |
| `veo-3.1` ‡ | *2.67* | 3.07 | — | — | *2.44* | *2.79* |
| `seedance-2.5` †‡ | 4.40 | 3.93 | — | — | 3.00 | 3.70 |

</details>

> Absolute scores for single-shot video and episode keyframes are **not** comparable with the other granularities, because dimensions were retired from those scales. The main text reports raw means; the covariate-adjusted variant differs by up to 0.11 points.

---

## The three systems

### DramaChain Agent — item sourcing

<div align="center">
<img src="assets/agent.png" alt="The DramaChain Agent production pipeline, forking at exactly one stage per run" width="880">
</div>

An end-to-end short-drama production pipeline running fully automatically. It generates the 20 dramas and 60 episodes the benchmark rests on, and both its workflow and its finished-drama quality are calibrated against real commercial platforms, so that a flawed pipeline does not itself become the object of evaluation. The framework forks at exactly the stage under test: competing models receive identical upstream artefacts and prompt strings while every other stage stays fixed.

### DramaChain Labeling System — human reference

<div align="center">
<img src="assets/labeling.png" alt="The DramaChain labeling system: rubrics, attribution, spatio-temporal grounding and three-pass QC" width="880">
</div>

Three professional annotators independently score every item on all applicable dimensions, using a five-point decidable rubric per leaf dimension. Rubric tiers define observable criteria mapped to a closed vocabulary of attribution tags, and every deduction is localised in space and time. The result is 17,488 scores and 255,925 reviewable attributions contributed by 543 annotators.

### DramaChain Agentic Judge — automated evaluation

<div align="center">
<img src="assets/judge.png" alt="The DramaChain agentic judge: routed measurements, multi-round tool use and per-item checklist judging" width="820">
</div>

The judge replicates the human reference automatically. It aggregates routed measurements derived from dimension-specific criteria, may invoke external tools across multiple rounds, and produces final judgements against a per-item checklist. Mean PLCC against the human panel is 0.918.

---

## Case studies

**Equal scores, different failure modes.** Four items all score 1.3–1.7 on some dimension, so a composite ranks them together — but the localised defect boxes fall in four disjoint places: on faces, on limbs mid-action, on people who should not be present, and on hands with the wrong number of fingers. Within each pair the two models trade which dimension they fail: the model that loses identity holds action at 3.0, and the model that loses action holds identity at 3.33.

**Cross-shot consistency is a separate capability.** Two models draw the same episode from the same character sheets. The top-tier row holds its two leads across shots; the bottom-tier row does not, a 2.66-point gap on cross-shot character consistency. The important property is that every individual panel in the failing row is defensible — nothing in shot 7 alone is wrong, it is wrong relative to shot 5. A single-asset board is not under-reporting cross-shot quality; it is measuring a different quantity.

**Shot count decides executability.** One identical episode script, forked only at the storyboard stage:

| Dimension | Axis | `mimo-v2.5-pro`<br>3 shots | `gpt-5.5-xhigh`<br>6 shots | `claude-opus-4.8-max`<br>12 shots | Behaviour |
|---|---|---|---|---|---|
| S-A2 dialogue fidelity | F | *1.33* | **5.00** | **5.00** | stops losing lines at 6 shots |
| S-A1 event coverage | F | *2.67* | **4.67** | **4.67** | same |
| S-B1 executability | P | *1.00* | 3.00 | **4.00** | rises monotonically with shot count |
| S-C2 shooting rhythm | E | *1.00* | **3.67** | **3.67** | 6 shots is already enough |
| Overall impression | | *1.67* | **4.00** | **4.00** | — |
| Problems marked | | 23 | 18 | 15 | fewer, and they change in kind |
| Annotator disagreement | | 1.75 | 0.75 | 0.63 | worse artefacts are harder to score |

The three-shot output compresses the episode into 30 s of self-declared duration and 10 dialogue turns; all three annotators scored its executability and shooting rhythm at 1.00 with zero disagreement, marking the same few defects every time — several actions in one shot, blocking too complex to execute, no reaction shot.

**Each input paradigm carries its own failure mode.** Grid panels omit people, because one image must fill several cells; first/last-frame shots change person between the two frames, because the frames are not produced in one pass and identity is not held across them. Neither failure is available to the other paradigm, which is why difficulty is set by paradigm rather than by visual style.

**`hy3` reproduces the script and does not dramatise it.** It scores 3.43 on storyboard design against 3.60 for its nearest neighbour `doubao-seed-2.1-pro`, and the gap is grouped rather than general. Over the eight leaf dimensions on the same 533 items, `hy3` leads on everything that means *reproduce the script* — dialogue fidelity reaches 4.44, only 0.20 behind the leader — and trails on everything that means *turn it into television*, with emotional expression at 3.64 and audiovisual style at 3.91, both eighth of nine.

**`seedance-2.5` buys episode coherence with single-clip quality.** On paired items the newer version loses on single-shot video and gains at both episode and short-drama granularity. Everything that must hold continuously along a timeline got worse (audio-visual sync −0.658, motion smoothness, camera plausibility) and everything about joining shots together got better (cross-shot style consistency +1.143). It follows the prompt more literally, so a deficiency already present in the prompt is now executed faithfully.

**Automated scoring drifts where there is no reference to check against.** In one item, every dimension with a reference is scored correctly while the single purely perceptual dimension is overestimated by 3.67 points in the lenient direction — the item-level form of what metric validation found across the population.

---

## Release

DramaChain Bench supports extensible evaluation rather than static benchmarking. Stage-level branching plus model-consistent automatic scoring lets a new model enter the board with single-stage generation only, requiring no additional human annotation.

We will release **DramaChain Dimensions**, the **DramaChain Agentic Judge** framework, and a curated subset of the benchmark data.

## Citation

```bibtex
@techreport{shi2026dramachain,
  title       = {DramaChain Bench: An End-to-End Benchmark for Short-Drama Generation},
  author      = {Shi, Haoyuan and Chen, Mingtao and Jiang, Shuo and Chen, Ziyan and
                 Sheng, Xuyi and Liu, Yiming and Zhang, Ying and Wang, Miao and
                 Lu, Jianxiang and Lu, Fanyang and Lu, Songyuanyi and Wu, Xiele and
                 Hu, Zhichao and Wang, Lilin and Liu, Yuhong and Xuan, Richeng},
  institution = {Hunyuan, Tencent},
  year        = {2026}
}
```
