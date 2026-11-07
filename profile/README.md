
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="banner-dark.svg">
  <img alt="einvoice-validator — an e-invoice validator for the official EN 16931 and Peppol BIS Billing 3.0 rules, in pure Rust" src="banner-light.svg" width="100%">
</picture>

<p align="center">
  <a href="https://einvoice-validator.github.io"><b>Website</b></a> ·
  <a href="https://github.com/einvoice-validator/einvoice-validator">Repository</a> ·
  <a href="https://github.com/einvoice-validator/einvoice-validator/issues">Issues and roadmap</a> ·
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/CHANGELOG.md">Changelog</a> ·
  <a href="https://github.com/einvoice-validator/einvoice-validator/security">Security</a>
</p>

<p align="center">
  <a href="https://github.com/einvoice-validator/einvoice-validator/actions/workflows/ci.yml"><img alt="CI" src="https://img.shields.io/github/actions/workflow/status/einvoice-validator/einvoice-validator/ci.yml?branch=main&style=flat-square&label=ci"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/LICENSE"><img alt="MIT" src="https://img.shields.io/badge/license-MIT-1c6b52?style=flat-square"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/rust-toolchain.toml"><img alt="Rust 1.99.1" src="https://img.shields.io/badge/rust-1.99.1-1c6b52?style=flat-square&logo=rust"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/issues"><img alt="Status: pre-alpha" src="https://img.shields.io/badge/status-pre--alpha-1c6b52?style=flat-square"></a>
</p>

European e-invoices are checked against business rules that CEN (EN 16931) and OpenPeppol (Peppol BIS Billing 3.0) publish as Schematron, which validators run compiled to XSLT 2.0. einvoice-validator is an XSLT 2.0 and XPath 2.0 engine in **pure Rust** (no JVM, no C library, no FFI) built to run those **official stylesheets unchanged**, with exact `xs:decimal` arithmetic, and to give the same report as Saxon-HE, rule by rule.

> [!IMPORTANT]
> **Pre-alpha: not usable as a validator yet.** There is no release, no public API and no command-line tool. The engine already runs the official CEN EN 16931 UBL 1.3.17 stylesheet and writes the same SVRL report as Saxon-HE 12.10 on all 1179 cases of its test corpus, and UBL 2.1 schema validation works in Rust. Still open: the public API and command-line tool (in progress), Peppol BIS Billing 3.0.22, a WebAssembly build. Follow the [tracking issues](https://github.com/einvoice-validator/einvoice-validator/issues).

| | Project | What it is |
|---|---|---|
| 🧾 | [**einvoice-validator**](https://github.com/einvoice-validator/einvoice-validator) | The Cargo workspace: XPath 2.0 parser (`einvoice-xpath`), atomic values (`einvoice-xdm`), evaluator (`einvoice-xpath-eval`), XSLT loader (`einvoice-xslt`) and executor (`einvoice-xslt-exec`), UBL 2.1 schema validation (`einvoice-xsd`), the vendored official artefacts and the test corpus with its Saxon-HE results. |
| 🌐 | [**einvoice-validator.github.io**](https://github.com/einvoice-validator/einvoice-validator.github.io) | The website. |

#### Why trust it

- **Official artefacts, not a rewrite.** No rule is translated into Rust by hand: the engine is built to execute the published stylesheets. The CEN artefacts are vendored byte for byte with their checksums, and an instruction outside the supported XSLT subset is refused when the stylesheet is loaded.
- **Same reports as Saxon-HE.** 1179 cases from the official CEN UBL 1.3.17 test files, each with the report Saxon-HE 12.10 gives, re-run in CI and cross-checked against the expectations of the official unit tests (0 disagreements). On every one of them, the engine's report is identical to Saxon's, rule by rule.
- **Schema validation compared with xmllint** on 1546 cases; every difference is listed with its cause.
- **Fast and small.** The whole CEN corpus takes about 0.5 s on one thread (0.4 ms per document), against about 1.4 s for Saxon-HE 12.10 on one thread, JVM start included; the prepared stylesheet keeps about 3.7 MiB of heap.
- **Exact decimals.** `xs:decimal` is arbitrary precision, never a floating-point number.
- **Strict engineering.** No `unsafe`, every warning and clippy `pedantic` denied, signed commits, GitHub Actions pinned by commit SHA, dependencies checked by `cargo-deny`.

<sub>Need another official rule set (CII, XRechnung, Factur-X / ZUGFeRD, a national CIUS)? <a href="https://github.com/einvoice-validator/einvoice-validator/issues">Open an issue</a>.</sub>
