# BiRefNet lite — ONNX

ONNX exports of [BiRefNet lite](https://huggingface.co/ZhengPeng7/BiRefNet_lite)
(dichotomous image segmentation / background removal), converted from the official weights.

## Files

Download from the [releases](../../releases) page.

| File | Input size | Weights | SHA-256 |
|---|---|---|---|
| `birefnet-lite-384.onnx` | 384 × 384 | float32 | `ce5dd7c713f4573b0a7a6ea7abb8aca6728eb585b5fe0f38a6f3ae0eee44f2ba` |
| `birefnet-lite-1024-fp16.onnx` | 1024 × 1024 | float16 (float32 inputs/outputs) | `3f57d6c9def6e7bf2a093fab490203368196067637aa372e1802f711f1eccfc9` |

## Usage

- Input `image`: `[1, 3, S, S]` float32, RGB, values divided by 255 then normalized with
  mean `[0.485, 0.456, 0.406]` and std `[0.229, 0.224, 0.225]`.
- Output `alpha`: `[1, 1, S, S]` float32 foreground probability (sigmoid applied).

## Export notes

- ONNX opset 17, static input size, basic graph optimizations applied.
- The deformable convolutions of the decoder are expressed with standard operators
  (Gather, MatMul) instead of `DeformConv`/`GridSample`, so the models run on any
  ONNX Runtime execution provider, DirectML included. Outputs match the PyTorch model.

## License

MIT, as the original weights — see [LICENSE](LICENSE). The model was trained on academic
datasets (DIS5K and others): check their terms for commercial use.

## Citation

```bibtex
@article{zheng2024birefnet,
  title={Bilateral Reference for High-Resolution Dichotomous Image Segmentation},
  author={Zheng, Peng and Gao, Dehong and Fan, Deng-Ping and Liu, Li and Laaksonen, Jorma and Ouyang, Wanli and Sebe, Nicu},
  journal={CAAI Artificial Intelligence Research},
  volume={3},
  pages={9150038},
  year={2024}
}
```
