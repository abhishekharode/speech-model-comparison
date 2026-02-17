**Comparative Evaluation of Speech-to-Text Models Under Noisy Conditions**

**1\. Introduction**

Automatic Speech Recognition (ASR) systems are widely used in transcription services, voice assistants, and real-time communication systems. Selecting the appropriate model requires evaluating trade-offs between accuracy, speed, memory consumption, and robustness to noise.

This report presents a comparative analysis of:

- Whisper
- Faster Whisper
- Wav2Vec2

The evaluation focuses on performance under noisy audio conditions using Word Error Rate (WER), inference latency, GPU memory usage, and deployment complexity.

**2\. Model Research Overview**

**2.1 Whisper**

Architecture:

- Transformer-based encoder-decoder
- Sequence-to-sequence transcription
- Multilingual

Training Data:

- ~680,000 hours of multilingual speech data

Licensing:

- MIT License (OpenAI)

Hardware Requirements:

- GPU recommended
- High memory usage for larger variants

Strengths:

- Strong robustness to noise
- Multilingual support
- High accuracy

Weaknesses:

- Higher GPU memory usage
- Slower inference compared to optimized variants

**2.2 Faster Whisper**

Architecture:

- Optimized Whisper implementation
- CTranslate2 backend
- Quantization support

Training Data:

- Same pretrained weights as Whisper

Licensing:

- MIT License

Hardware Requirements:

- Lower GPU memory usage
- Efficient on CPU and GPU

Strengths:

- Lower latency
- Lower GPU memory usage
- Comparable accuracy to Whisper

Weaknesses:

- Slight accuracy variation at extreme noise levels

**2.3 Wav2Vec2**

Architecture:

- Self-supervised transformer encoder
- CTC-based decoding

Training Data:

- Pretrained on large unlabeled speech datasets
- Fine-tuned on labeled datasets (e.g., LibriSpeech)

Licensing:

- Apache 2.0 (Hugging Face implementation)

Hardware Requirements:

- Moderate GPU usage
- Can run on CPU

Strengths:

- Strong performance on clean audio
- Efficient for fine-tuning

Weaknesses:

- Significant degradation under high noise
- Less robust compared to Whisper-based models

**3\. Experimental Setup**

**Dataset:**

- Small open subset (LibriSpeech-style dataset)

**Noise Injection:**

- White Gaussian noise
- SNR levels: Clean, 20dB, 10dB, 5dB

**Evaluation Metrics:**

- Word Error Rate (WER)
- Inference Time
- GPU Memory Usage
- Setup Complexity

**Hardware:**

- Google Colab GPU environment
- Disk space: 99.8GB / 112GB available

**4\. Experimental Results**

**4.1 Word Error Rate (WER)**

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| **Model** | **Clean** | **20dB** | **10dB** | **5dB** |
| Whisper | 0.0458 | 0.0491 | 0.0604 | 0.0829 |
| Faster Whisper | 0.0443 | 0.0521 | 0.0617 | 0.0712 |
| Wav2Vec2 | 0.0409 | 0.0437 | 0.0922 | 0.3879 |

**Key Observation:**

- Wav2Vec2 performs best on clean audio.
- Severe degradation at 5dB (WER = 0.3879).
- Faster Whisper shows strongest robustness at extreme noise.

**4.2 GPU Memory Usage :**

|     |     |
| --- | --- |
| **Model** | **GPU Memory** |
| Whisper | 1.6 GB |
| Faster Whisper | 0.8 GB |
| Wav2Vec2 | 0.7 GB |

**Observation:**  
Faster Whisper uses 50% less GPU memory than Whisper.

**5\. Comparative Analysis :**

|     |     |     |     |
| --- | --- | --- | --- |
| **Criteria** | **Whisper** | **Faster Whisper** | **Wav2Vec2** |
| Clean Accuracy | High | High | Very High |
| Noisy Robustness | Strong | Strongest | Weak |
| Latency | Medium | Fastest | Medium |
| GPU Usage | High | Moderate | Low |
| Ease of Deployment | Moderate | Easy | Moderate |
| Quantization Support | Limited | Strong | Limited |

![GPU Memory](results/gpu_memory.png)

![GPU Memory](results/inference_time.png)

![GPU Memory](results/wer_vs_snr.png)

![GPU Memory](results/Full_execution_time.png)

**6\. Final Recommendation**

**Selected Model: Faster Whisper**

**Justification:**

1.  Comparable accuracy to Whisper
2.  Strongest performance under high noise (5dB)
3.  50% lower GPU memory usage
4.  Fastest inference time
5.  Production-ready optimization via CTranslate2

Faster Whisper provides the best balance between robustness, efficiency, and deployability.

**7\. Optimization Strategy**

1.  Use INT8 quantization
2.  Batch inference
3.  Mixed precision (FP16)
4.  Noise-aware fine-tuning
5.  Use VAD preprocessing to trim silence

**8\. Fine-Tuning Strategy**

1.  Collect domain-specific speech data
2.  Inject synthetic noise at various SNR levels
3.  Fine-tune on noisy-clean mixed dataset
4.  Evaluate robustness curve

**9\. Suggested Production Architecture**

- Input → Audio Preprocessing → VAD → Faster Whisper (Quantized) → Post-processing → API Output
- Deploy via FastAPI
- Containerize using Docker
- GPU autoscaling on cloud (AWS/GCP)
