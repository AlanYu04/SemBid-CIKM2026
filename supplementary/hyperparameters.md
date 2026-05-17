# Hyperparameters

## Model Architecture and Hyperparameters

We adopt a Language-Guided Decision Transformer. Unlike standard implementations, we utilize high-dimensional semantic embeddings to preserve the rich information from the LLM.

**Semantic Embedding Upscaling:** The base embeddings from Qwen-0.5B (d=896) are projected to a higher dimension (d=2048) using a randomized linear projection layer during preprocessing. These pre-computed embeddings are then fed into the Decision Transformer.

| Category | Parameter | Value |
|---|---|---|
| **Architecture** | Transformer Layers | 6 |
| | Attention Heads | 4 |
| | Hidden Dimension (d_model) | 128 |
| | Feedforward Dimension | 512 |
| | Activation Function | GELU |
| | Dropout Rate | 0.1 |
| **Semantic Encoder** | LLM Backbone | Qwen-0.5B (Frozen) |
| | Projection Type | Random Linear (896 → 2048) |
| | Input Embedding Dim | 2048 |
| **Training** | Optimizer | AdamW |
| | Learning Rate | 1 × 10⁻⁴ |
| | Weight Decay | 1 × 10⁻⁴ |
| | Batch Size | 64 |
| | Training Steps | 800,000 |
| | Max Episode Length | 48 |

## Computational Resources

- **Hardware:** 1 × NVIDIA A100 (40GB) GPU.
- **Preprocessing:** Offline embedding generation with Qwen-0.5B took ~2 hours.
- **Training:** The model was trained for 800k steps with persistent data workers.
