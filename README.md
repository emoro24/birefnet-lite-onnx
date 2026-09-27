# BiRefNet lite — ONNX

ONNX exports of [BiRefNet lite](https://huggingface.co/ZhengPeng7/BiRefNet_lite)
(dichotomous image segmentation / background removal), converted from the official weights.

## Files

Download from the [releases](../../releases) page.

| Release | File | Input size | Weights | SHA-256 |
|---|---|---|---|---|
| v1 | `birefnet-lite-384.onnx` | 384 × 384 | float32 | `ce5dd7c713f4573b0a7a6ea7abb8aca6728eb585b5fe0f38a6f3ae0eee44f2ba` |
| **v2** | `birefnet-lite-1024-fp16.onnx` | 1024 × 1024 | float16 (float32 inputs/outputs) | `945ef7bcca23823f823ca00a22c1c08fedd9391bff8620658dfe292dfcea0a22` |
| v1 | `birefnet-lite-1024-fp16.onnx` | 1024 × 1024 | float16 | `3f57d6c9def6e7bf2a093fab490203368196067637aa372e1802f711f1eccfc9` — superseded by v2, see below |

## Usage

- Input `image`: `[1, 3, S, S]` float32, RGB, values divided by 255 then normalized with
  mean `[0.485, 0.456, 0.406]` and std `[0.229, 0.224, 0.225]`.
- Output `alpha`: `[1, 1, S, S]` float32 foreground probability (sigmoid applied).

## Export notes

- ONNX opset 17, static input size, basic graph optimizations applied.
- The deformable convolutions of the decoder are expressed with standard operators
  (Gather, MatMul) rather than `DeformConv` or `GridSample`, which some execution
  providers do not support. Outputs match the PyTorch model.
- Float16 copies are checked against the float32 export on real images (mask IoU at 0.5).
  In the v1 float16 file, the pixel indices of the deformable convolutions were computed
  in float16, exact only up to 2048: the decoder sampled pixels beside the right ones and
  masks were degraded. Since v2 they are computed in integers; the v2 float16 file matches
  the float32 export (IoU ≥ 0.9998 on the test images).

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
