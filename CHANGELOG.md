# Changelog

All notable changes to `pii-protector` are documented here.

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
