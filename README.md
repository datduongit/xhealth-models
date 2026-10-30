# XHealth models

Files the XHealth app downloads on the user's request to read medical papers on the device. Nothing is sent
anywhere: the app runs the model locally with ONNX Runtime 1.31.

## reader-v1 — Qwen3-0.6B document reader

| File | Size | sha256 |
|---|---|---|
| `model.onnx` | 377,562,847 bytes | `8837e92a6d2ddd8f7a5c5c1834d6e202a0d1a5634b7469eecae1b0a75d7127ef` |
| `tokenizer.json` | 9,117,040 bytes | `e7a95fce95bf5b0946d0ddb3f9d7caa030b7e850bbe92b0edb26bcf563e9f3d5` |

Derived from [Qwen/Qwen3-0.6B](https://huggingface.co/Qwen/Qwen3-0.6B) (Apache 2.0, © Alibaba Cloud / Qwen team),
via the ONNX export [onnx-community/Qwen3-0.6B-ONNX](https://huggingface.co/onnx-community/Qwen3-0.6B-ONNX)
(`onnx/model_q4.onnx`, `tokenizer.json` unchanged). Changes, all weight-only (no fine-tuning):

- the float32 embedding table and lm_head quantized to 4-bit and shared (GatherBlockQuantized + MatMulNBits);
- rotary sin/cos caches trimmed to 4,096 positions;
- logits computed for the last position only.

919 MB → 378 MB. Runs on ONNX Runtime ≥ 1.31 (CPU) with the `com.microsoft` contrib ops.

The app only uses it to copy text spans from a lab-report row or the page header; every number must be printed
on the paper, and anything the model reads differently is shown to the user for review. It does not diagnose.

Licensed under Apache 2.0 (see `LICENSE`), as the original Qwen3 weights.
