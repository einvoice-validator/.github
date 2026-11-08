
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
  <a href="https://github.com/einvoice-validator/einvoice-validator/releases/tag/v0.2.0"><img alt="Release v0.2.0" src="https://img.shields.io/github/v/release/einvoice-validator/einvoice-validator?style=flat-square&label=release&color=1c6b52"></a>
  <a href="https://crates.io/crates/einvoice-validator"><img alt="crates.io" src="https://img.shields.io/crates/v/einvoice-validator?style=flat-square&label=crates.io&color=1c6b52"></a>
  <a href="https://docs.rs/einvoice-validator"><img alt="docs.rs" src="https://img.shields.io/docsrs/einvoice-validator?style=flat-square&label=docs.rs"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/actions/workflows/ci.yml"><img alt="CI" src="https://img.shields.io/github/actions/workflow/status/einvoice-validator/einvoice-validator/ci.yml?branch=main&style=flat-square&label=ci"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/LICENSE"><img alt="MIT" src="https://img.shields.io/badge/license-MIT-1c6b52?style=flat-square"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/rust-toolchain.toml"><img alt="Rust 1.99.1" src="https://img.shields.io/badge/rust-1.99.1-1c6b52?style=flat-square&logo=rust"></a>
</p>

European e-invoices are checked against business rules that CEN (EN 16931) and OpenPeppol (Peppol BIS Billing 3.0) publish as Schematron, which validators run compiled to XSLT 2.0. einvoice-validator is a library and a command-line tool, `einvoice-validate`, in **pure Rust** (no JVM, no C library, no FFI): an XSLT 2.0 and XPath 2.0 engine that runs those **official stylesheets unchanged**, with exact `xs:decimal` arithmetic and UBL 2.1 schema validation, and gives the same report as Saxon-HE, rule by rule. It reads both syntaxes of EN 16931, UBL 2.1 and UN/CEFACT CII D16B, and picks the rule set from each document's root element.

> [!IMPORTANT]
> **0.2.0 released** ([release notes](https://github.com/einvoice-validator/einvoice-validator/releases/tag/v0.2.0)), still pre-1.0: a minor version may change the API. Install the command-line tool with `cargo install --locked einvoice-cli`; the library is [`einvoice-validator`](https://crates.io/crates/einvoice-validator) on crates.io (`einvoice-validator = "0.2"`), documented on [docs.rs](https://docs.rs/einvoice-validator). CEN EN 16931 UBL and, new in 0.2.0, CII 1.3.17 are built in (no XML Schema validation of CII documents yet, [#32](https://github.com/einvoice-validator/einvoice-validator/issues/32)); Peppol BIS Billing 3.0.22 is used as a bundle built locally, in a clone of the repository, from its pinned release, whose files are not redistributed. On every case of the three test corpora, official and mutated, the reports are the same as Saxon-HE 12.10's. Still open: CII schema validation, WebAssembly bindings and an npm package, other rule sets.
>
> **Security fix:** in 0.2.0 a deeply nested document no longer aborts the process with a stack overflow ([#29](https://github.com/einvoice-validator/einvoice-validator/issues/29)): documents may be nested at most 256 elements deep by default (`--max-depth`), and the stylesheets' recursion is bounded in nesting and in stack. Upgrade from 0.1.0 if you validate documents you do not produce yourself. See the [website](https://einvoice-validator.github.io/#use) and the [tracking issues](https://github.com/einvoice-validator/einvoice-validator/issues).

| | Project | What it is |
|---|---|---|
| 🧾 | [**einvoice-validator**](https://github.com/einvoice-validator/einvoice-validator) | The Cargo workspace: the library (`einvoice-validator`) and the command-line tool (`einvoice-validate`, crate `einvoice-cli`), built on the XPath 2.0 parser (`einvoice-xpath`), atomic values (`einvoice-xdm`), evaluator (`einvoice-xpath-eval`), XSLT loader (`einvoice-xslt`) and executor (`einvoice-xslt-exec`) and XML Schema validation (`einvoice-xsd`); the vendored CEN UBL and CII artefacts, the test corpora with their Saxon-HE results, and the scripts that build the Peppol corpus and bundle locally. |
| 🌐 | [**einvoice-validator.github.io**](https://github.com/einvoice-validator/einvoice-validator.github.io) | The website. |

#### Why trust it

- **Official artefacts, not a rewrite.** No rule is translated into Rust by hand: the engine executes the published stylesheets. The CEN artefacts are vendored byte for byte with their checksums, the Peppol release and its compiled stylesheets are checked against pinned checksums, and an instruction outside the supported XSLT subset is refused when the stylesheet is loaded.
- **Same reports as Saxon-HE.** Each test case carries the report Saxon-HE 12.10 gives, cross-checked against the expectations of the official unit tests (0 disagreements). The engine's report is identical to Saxon's, rule by rule, on all 1179 CEN EN 16931 UBL 1.3.17 cases, all 24 CEN EN 16931 CII 1.3.17 cases and all 707 Peppol BIS Billing 3.0.22 cases with each of its two stylesheets.
- **Mutation cases.** 504 CEN UBL, 808 CEN CII and 505 Peppol edits of the official examples, aimed at totals, rounding, decimal precision, dates, code lists, check digits and namespaces, give the same results as Saxon-HE too; they make 973 of the 982 distinct CEN UBL asserts fire, against 235 with the official cases alone, and 785 of the 810 CII assert ids fail in at least one case.
- **Schema validation compared with xmllint** on 1555 cases; every difference is listed with its cause.
- **Fast and small.** Measured with 0.1.0, `einvoice-validate` checks the whole CEN UBL corpus (1179 documents, XML, schema and rules, process start included) in about 0.6 s on one thread and 0.13 s on eight; Saxon-HE 12.10 runs the rules alone over the same documents in about 1.4 s on one thread, JVM start included. In 0.2.0, the schema stage takes 23 % less time and, on the largest corpus document (3.3 MB), given as a buffer, the whole validation peaks at 3.3 MiB of heap instead of 12.6 MiB. The library runs on `wasm32-unknown-unknown`.
- **Exact decimals.** `xs:decimal` is arbitrary precision, never a floating-point number.
- **Bounded recursion.** A document nested deeper than the limit (256 by default) is refused with an error, and the stylesheets' template and function invocations are bounded in nesting and in stack: no stack overflow on a deeply nested document.
- **Strict engineering.** No `unsafe`, every warning and clippy `pedantic` denied, signed commits, GitHub Actions pinned by commit SHA, dependencies checked by `cargo-deny`.

<sub>Need another official rule set (XRechnung, Factur-X / ZUGFeRD, a national CIUS)? <a href="https://github.com/einvoice-validator/einvoice-validator/issues">Open an issue</a>.</sub>
