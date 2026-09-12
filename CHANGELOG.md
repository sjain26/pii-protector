# Changelog

All notable changes to `pii-protector` are documented here.

## [2.3.0]

### Performance
- **Hardware-aware ONNX runtime selection.** The loader now picks the ONNX
  weight file and execution provider per device instead of always loading fp16.
  FP16 has no native kernels on x86 CPU, so onnxruntime runs an fp16 graph by
  casting every op to fp32 at runtime — a single NER inference took ~1150 ms on
  CPU (onnxruntime 1.29).
  - **NER model (Layer 3, RoBERTa)** — **CPU → `model_int8.onnx`**: **~23 ms**
    (~50x faster than fp16 on CPU), download ~340 MB (was 677 MB). Accuracy is
    unchanged: 99.88% token agreement with fp16 and **0.02%** accuracy delta on
    the CoNLL-2003 test set. **GPU → `model_fp16.onnx`** (fp16 tensor cores).
  - **PII model (Layer 4, DeBERTa)** — stays **fp32 on CPU** (~37 ms). int8 was
    evaluated and rejected: DeBERTa's disentangled attention degrades badly under
    dynamic int8 (entity-token agreement dropped to ~36%). **GPU → fp16** when a
    fp16 file is present.
  - Missing weight files fall back gracefully (int8 → fp32 → fp16), so existing
    model repos keep working.

## [2.2.6]

### Fixed
- **Transformer / PII model layers failing to load on modern PyTorch.** The
  optional `transformers` extra pulled `optimum` 1.x, whose ONNX code imports a
  private symbol (`torch.onnx.symbolic_opset14._attention_scale`) that was
  removed in newer torch releases, causing Layers 3 & 4 to silently fall back to
  regex-only. The extras now require `optimum>=2.0` + `optimum-onnx`, which are
  compatible with current torch. No API changes.

## [2.2.5]

- Sequential layered detection (regex → Presidio → NER transformer → PII model)
  with smart escalation.
- India + North America PII patterns, API secrets, cloud-provider keys.
- Zero-dependency regex layer; optional `presidio`, `transformers`, `gpu`, `full` extras.
