# Emotion Classification using Recent DNN and LLM Approaches

**Name:** Le Viet Hoang Anh<br>
**Project ID:** CCDS26032<br>
**Project Title:** Emotion classification using recent DNN and LLM approaches<br>
**Supervisor:** Prof Chng Eng Siong<br>
**Prerequisite:** Strong AI and development skills<br>
**CLRI Type:** Corporate Lab<br>
**Corporate Lab / Research Institute:** Alibaba-NTU JRI<br>
**Start Date:** 30th August 2026<br>
**End Date:** 30th July 2027

## Overview

Emotion classification involves analyzing spoken audio to detect affective states (e.g., happiness, anger, sadness). Modern approaches typically fall into three main architectures: Cascaded Pipelines, Multimodal/Fusion Adapters, and End-to-End AudioLLMs.

In this work, the student will examine open-sourced existing models to extend as well as evaluate their capabilities.

## Tasks

The project is organised into two parallel tasks.

| Task | Focus | Supported by |
|---|---|---|
| **Engineering task** | Turn the existing emotion recognition pipeline into an end-to-end inference service: the audio feature models and the LLM are loaded once in memory, and inference runs in single, multiple and benchmarking modes. | Vu Thi Ly, Kyaw Zin Tun |
| **LLM research** | Evaluate the multimodal LLM (LLaMA-3-8B-Instruct with LoRA, using transcript, dialogue history, gender, VAD and eGeMAPS cues) on IEMOCAP, analyse its errors, and test targeted improvements. | Chao Yi-Wen |

### Engineering task

- **Goal:** replace the current "reload every model on every request" scripts with two long-running services (audio features and LLM) that load once and answer many requests.
- **Baseline measured:** one sample takes 524 s on the TC1 cluster (audio preprocessing 31 s, LLM stage 493 s); 94% of the time is loading the LLM.
- **Status:** load-once wrappers, the audio service, the LLM `--serve` patch, a benchmark harness and a start script are written; validation on the GPU cluster is the next step (the output must match the current Session 5 results, then load time and per-request inference time are measured).

### LLM research

- **Evaluation:** on IEMOCAP Session 5 the model scores 53.92% accuracy over all 2,170 utterances, or 72.13% accuracy (Macro-F1 70.59) over the 1,622 utterances that have one of the six target emotions.
- **Error analysis:** Neutral is over-predicted, Happy is the weakest class, and Frustrated is confused with Neutral and Angry.
- **Experiments so far:** output calibration does not improve Macro-F1; cue ablation shows the model relies most on the dialogue history.
- **Next:** class-weighted fine-tuning, longer dialogue history with cue masking, and more data (starting with MELD), all chosen on a held-out validation session.

## Video Updates

Playlist:

[YouTube Playlist](https://studio.youtube.com/channel/UCfznYB-UN1EVLfSTDuXUX3A/videos/upload?filter=%5B%5D&sort=%7B%22columnType%22%3A%22date%22%2C%22sortOrder%22%3A%22DESCENDING%22%7D)

[Report 1](https://www.youtube.com/watch?v=QiBCJLFsShI)

[Report 2](https://youtu.be/c3IHxJDqlKg)

## Roadmap / Milestones

- **Phase 1:** Environment setup and baseline model integration
  - Engineering: run the existing pipeline on the TC1 cluster and time it.
  - LLM research: reproduce the baseline results on IEMOCAP.
- **Phase 2:** Data collection / preprocessing
  - Engineering: audio preprocessing (gender, VAD, eGeMAPS, Whisper) for IEMOCAP.
  - LLM research: select additional corpora (MELD first, then MSP-IMPROV) and a unified label mapping.
- **Phase 3:** Model training/fine-tuning
  - Engineering: load-once services for audio and LLM.
  - LLM research: class-weighted fine-tuning and longer dialogue history with cue masking.
- **Phase 4:** Evaluation and benchmarking
  - Engineering: validate the service against the baseline results; benchmark load time and per-request inference time.
  - LLM research: evaluate on Session 5 once, with confidence intervals; cross-corpus tests.
- **Phase 5:** Final report and presentation
