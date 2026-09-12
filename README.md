# pii-protector

**Production-grade PII (Personally Identifiable Information) detection** with a sequential multi-model ensemble. Fast by design — 90%+ of texts are handled by the regex layer alone in under 3ms, with heavier NER models invoked only when needed.

[![PyPI version](https://img.shields.io/pypi/v/pii-protector.svg)](https://pypi.org/project/pii-protector/)
[![Python](https://img.shields.io/pypi/pyversions/pii-protector.svg)](https://pypi.org/project/pii-protector/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Note on this repository:** This is the **community hub** for `pii-protector` — the place to report bugs, ask questions, and request features. The library's source code is not hosted here. To use the library, install it from PyPI (below).

---

## Installation

```bash
# Regex layer only — zero dependencies, works out of the box
pip install pii-protector

# + Presidio / spaCy NER (Layer 2)
pip install "pii-protector[presidio]"

# + NER transformer & PII model (Layers 3 & 4, ONNX Runtime)
pip install "pii-protector[transformers]"

# Everything
pip install "pii-protector[full]"
```

## Quick start

```python
from pii_detector import AdvancedPIIDetector

detector = AdvancedPIIDetector()

text = "My name is Rahul Sharma, email rahul@example.com, PAN ABCDE1234F"
for hit in detector.detect(text):
    print(hit["entity_type"], "->", hit["text"], f"({hit['score']:.2f})")
```

```
NAME  -> Rahul Sharma (1.00)
EMAIL -> rahul@example.com (0.95)
PAN   -> ABCDE1234F (0.95)
```

> The import module is `pii_detector` (the PyPI/distribution name is `pii-protector`).

## Command line

```bash
echo "Contact test@mail.com or Aadhaar 1234 5678 9012" | pii-detect
```

## Architecture

```
Layer 1 — Regex                  always runs              ~0.3–3ms
    ↓  escalation score >= 3?
Layer 2 — Presidio + spaCy NER   names / orgs / loc       ~5ms
    ↓  confidence low or conflict?
Layer 3 — NER Transformer        high-accuracy names      ~20ms
    +
Layer 4 — PII Model              structured PII           ~15ms
```

Each layer decides whether the next one is needed. On clean structured text, only Layer 1 runs.

## What it detects

Emails, phone numbers, credit cards, and a broad set of **India-specific** (PAN, Aadhaar, mobile) and **North America** identifiers, plus API secrets, cloud-provider keys, and more — with optional NER layers for names, organizations, and locations.

---

## 🐛 Found a bug? 💡 Have an idea?

This repo exists so the community can help make `pii-protector` better:

- **Report a bug / detection error** → [open a Bug report](https://github.com/sjain26/pii-protector/issues/new?template=bug_report.yml)
- **Request a feature or a new PII type** → [open a Feature request](https://github.com/sjain26/pii-protector/issues/new?template=feature_request.yml)
- **Ask a question / share an idea** → [Discussions](https://github.com/sjain26/pii-protector/discussions)
- **Report a security/privacy issue** → see [SECURITY.md](SECURITY.md) (please don't open a public issue)

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to write a report that gets fixed fast.

## License

[MIT](LICENSE) © tensoryug
