
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
> **Pre-alpha: it does not validate invoices yet.** The XPath 2.0 parser, the XSLT loader, the atomic values with exact decimals and the CEN UBL 1.3.17 test corpus exist; the evaluator, the XSLT executor and UBL schema validation are in progress. Follow the [tracking issues](https://github.com/einvoice-validator/einvoice-validator/issues).

| | Project | What it is |
|---|---|---|
| 🧾 | [**einvoice-validator**](https://github.com/einvoice-validator/einvoice-validator) | The Cargo workspace: XPath 2.0 parser (`einvoice-xpath`), atomic values (`einvoice-xdm`), XSLT loader (`einvoice-xslt`), the vendored official artefacts and the test corpus with its Saxon-HE results. |
| 🌐 | [**einvoice-validator.github.io**](https://github.com/einvoice-validator/einvoice-validator.github.io) | The website. |

#### Why trust it

- **Official artefacts, not a rewrite.** No rule is translated into Rust by hand: the engine is built to execute the published stylesheets. The CEN artefacts are vendored byte for byte with their checksums, and an instruction outside the supported XSLT subset is refused when the stylesheet is loaded.
- **Reference results from Saxon-HE.** 1179 cases from the official CEN UBL 1.3.17 test files, each with the report Saxon-HE 9.9.1-8 gives, re-run in CI and cross-checked against the expectations of the official unit tests (0 disagreements). The atomic-value operations already match Saxon-HE on over 5000 test cases.
- **Exact decimals.** `xs:decimal` is arbitrary precision, never a floating-point number.
- **Strict engineering.** No `unsafe`, every warning and clippy `pedantic` denied, signed commits, GitHub Actions pinned by commit SHA, dependencies checked by `cargo-deny`.

<sub>Need another official rule set (CII, XRechnung, Factur-X / ZUGFeRD, a national CIUS)? <a href="https://github.com/einvoice-validator/einvoice-validator/issues">Open an issue</a>.</sub>
