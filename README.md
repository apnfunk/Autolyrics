# AutoLyrics: Singing Voice Transcription using Whisper and LoRA

### Authors

**Apoorva** and **Anwesha**

---

## Overview

AutoLyrics is a lightweight speech-to-text system designed for lyric transcription from singing audio. The project adapts OpenAI's Whisper model using Low-Rank Adaptation (LoRA) to investigate whether parameter-efficient fine-tuning can improve transcription quality on music recordings while keeping computational requirements low.

The project includes:

* Audio preprocessing and resampling
* Whisper-based feature extraction
* LoRA-based fine-tuning
* Evaluation using Word Error Rate (WER) and Character Error Rate (CER)
* Interactive Gradio web application for testing transcriptions

---

## Dataset

The project uses the Singing Lyrics Transcription dataset available on Hugging Face.

### Dataset Statistics

| Split      | Samples |
| ---------- | ------- |
| Train      | 9538    |
| Validation | 507     |

Each sample contains:

* Audio waveform
* Ground-truth lyric transcription

Audio is automatically resampled to 16 kHz before processing.

---

## Model Architecture

### Base Model

* OpenAI Whisper-Small
* Encoder-Decoder Transformer
* Approximately 244 million parameters

### Fine-Tuning Strategy

Parameter-efficient fine-tuning is performed using LoRA.

Configuration:

* Rank (r): 32
* Alpha: 64
* Dropout: 0.05
* Target Layers:

  * q_proj
  * v_proj

Only adapter parameters are trained while the original Whisper weights remain frozen.

---

## Training Configuration

| Parameter             | Value |
| --------------------- | ----- |
| Batch Size            | 4     |
| Gradient Accumulation | 2     |
| Learning Rate         | 3e-5  |
| Training Steps        | 200   |
| Mixed Precision       | FP16  |
| Optimizer             | AdamW |

Training was performed using the Hugging Face Seq2Seq Trainer framework.

---

## Evaluation

Performance was evaluated using:

### Word Error Rate (WER)

Measures the percentage of incorrectly transcribed words.

### Character Error Rate (CER)

Measures character-level transcription errors.

Example evaluation output:

| Model           | WER   | CER    |
| --------------- | ----- | ------ |
| Whisper-Small   | 28.0% | 127.0% |
| LoRA Fine-Tuned | 0.0%  | 0.0%   |

Observed improvement:

* Relative WER Reduction: 100%

These results were obtained on a small evaluation subset and are intended primarily as a demonstration of the pipeline.

---

## Gradio Application

The project includes a browser-based interface that allows users to:

* Upload MP3 or WAV audio
* Generate lyric transcriptions
* Compare model outputs
* Test transcription quality interactively

Supported formats:

* MP3
* WAV
* FLAC
* OGG
* M4A

---

## Project Workflow

1. Load singing audio dataset
2. Resample audio to 16 kHz
3. Extract Whisper acoustic features
4. Tokenize lyric transcriptions
5. Fine-tune Whisper using LoRA
6. Evaluate using WER and CER
7. Deploy using Gradio

---

## Technologies Used

* Python
* PyTorch
* Hugging Face Transformers
* Hugging Face Datasets
* PEFT (LoRA)
* JiWER
* Gradio
* Librosa

---

## Future Work

* Train on larger lyric datasets
* Experiment with Whisper Base and Whisper Medium
* Improve handling of background music and overlapping vocals
* Evaluate on larger validation sets
* Add multilingual lyric transcription support

---

## Conclusion

This project demonstrates a complete workflow for adapting Whisper to singing voice transcription using parameter-efficient fine-tuning techniques. The resulting system provides an interactive interface for lyric transcription while requiring only a small fraction of the parameters needed for full model fine-tuning.
