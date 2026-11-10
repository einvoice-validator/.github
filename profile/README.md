
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
  <a href="https://github.com/einvoice-validator/einvoice-validator/releases/tag/v0.3.0"><img alt="Release v0.3.0" src="https://img.shields.io/github/v/release/einvoice-validator/einvoice-validator?style=flat-square&label=release&color=1c6b52"></a>
  <a href="https://crates.io/crates/einvoice-validator"><img alt="crates.io" src="https://img.shields.io/crates/v/einvoice-validator?style=flat-square&label=crates.io&color=1c6b52"></a>
  <a href="https://docs.rs/einvoice-validator"><img alt="docs.rs" src="https://img.shields.io/docsrs/einvoice-validator?style=flat-square&label=docs.rs"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/actions/workflows/ci.yml"><img alt="CI" src="https://img.shields.io/github/actions/workflow/status/einvoice-validator/einvoice-validator/ci.yml?branch=main&style=flat-square&label=ci"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/LICENSE"><img alt="MIT" src="https://img.shields.io/badge/license-MIT-1c6b52?style=flat-square"></a>
  <a href="https://github.com/einvoice-validator/einvoice-validator/blob/main/rust-toolchain.toml"><img alt="Rust 1.99.1" src="https://img.shields.io/badge/rust-1.99.1-1c6b52?style=flat-square&logo=rust"></a>
</p>

European e-invoices are checked against business rules that CEN (EN 16931) and the bodies whose specifications build on it (OpenPeppol, KoSIT, FNFE-MPE and FeRD) publish as Schematron, which validators run compiled to XSLT 2.0. einvoice-validator is a library and a command-line tool, `einvoice-validate`, in **pure Rust** (no JVM, no C library, no FFI): an XSLT 2.0 and XPath 2.0 engine that runs those **official stylesheets unchanged**, with exact `xs:decimal` arithmetic and UBL 2.1 and UN/CEFACT CII D16B schema validation, and gives the same report as Saxon-HE, rule by rule. It reads both syntaxes of EN 16931, UBL 2.1 and UN/CEFACT CII D16B, as XML or embedded in a Factur-X / ZUGFeRD / Order-X PDF file, and picks the rule set from each document's root element.

> [!IMPORTANT]
> **0.3.0 released** ([release notes](https://github.com/einvoice-validator/einvoice-validator/releases/tag/v0.3.0)), still pre-1.0: a minor version may change the API. Install the command-line tool with `cargo install --locked einvoice-cli`; the library is [`einvoice-validator`](https://crates.io/crates/einvoice-validator) on crates.io (`einvoice-validator = "0.3"`), documented on [docs.rs](https://docs.rs/einvoice-validator). CEN EN 16931 UBL and CII 1.3.17 are built in, each after its XML schema (UBL 2.1, UN/CEFACT CII D16B). Peppol BIS Billing 3.0.22, XRechnung 3.0.2, Factur-X 1.09.2 and the French e-invoicing rules 1.4.0.04 may not be redistributed: `einvoice-validate rules fetch NAME` downloads them from their publishers, checksums pinned, and assembles them locally; the Peppol Schematron is compiled in pure Rust, without Java, byte for byte as ph-schematron compiles it, and XRechnung runs as the KoSIT configuration runs it (scenarios). On every case of every test corpus, official and mutated, the reports are the same as Saxon-HE 12.10's, read as the reference validators read them; on `main`, for the next release, the Factur-X and French bundles also validate the XML schema their reference validators run ([#37](https://github.com/einvoice-validator/einvoice-validator/issues/37)), and documents in UTF-16, US-ASCII, ISO-8859-1, ISO-8859-15 and windows-1252 are read as xmllint and Saxon-HE read them ([#6](https://github.com/einvoice-validator/einvoice-validator/issues/6)). The npm package is built and tested, not yet published.
>
> **Security fix:** in 0.3.0 a deeply nested stylesheet in an untrusted rule-set bundle no longer aborts the process with a stack overflow ([#33](https://github.com/einvoice-validator/einvoice-validator/issues/33)): expressions nested deeper than 128 levels and instructions deeper than 64 are refused when the bundle is loaded (the official stylesheets need 32 and 9). Documents stay limited to 256 levels by default (`--max-depth`). See the [website](https://einvoice-validator.github.io/) and the [tracking issues](https://github.com/einvoice-validator/einvoice-validator/issues).

| | Project | What it is |
|---|---|---|
| 🧾 | [**einvoice-validator**](https://github.com/einvoice-validator/einvoice-validator) | The Cargo workspace: the library (`einvoice-validator`) and the command-line tool (`einvoice-validate`, crate `einvoice-cli`), built on the XML reader (`einvoice-xml`), big integers (`einvoice-bigint`), the XPath 2.0 parser (`einvoice-xpath`), atomic values (`einvoice-xdm`), evaluator (`einvoice-xpath-eval`), XSLT loader (`einvoice-xslt`) and executor (`einvoice-xslt-exec`), the Schematron compiler (`einvoice-schematron`), XML Schema validation (`einvoice-xsd`) and PDF extraction (`einvoice-pdf`); the vendored CEN UBL and CII artefacts, the test corpora with their Saxon-HE results, the scripts that build the other corpora locally, and the npm package. |
| 🌐 | [**einvoice-validator.github.io**](https://github.com/einvoice-validator/einvoice-validator.github.io) | The website. |

#### Why trust it

- **Official artefacts, not a rewrite.** No rule is translated into Rust by hand: the engine executes the published stylesheets. The CEN artefacts are vendored byte for byte with their checksums, every fetched rule set is checked against pinned digests, the compiled Peppol stylesheets against pinned SHA-256, and an instruction outside the supported XSLT subset is refused when the stylesheet is loaded.
- **Same reports as Saxon-HE.** Each test case carries the report Saxon-HE 12.10 gives, cross-checked against the expectations of the official unit tests (0 disagreements). The engine's report is identical to Saxon's, rule by rule, on every case and mutation case of every corpus, with each stylesheet: CEN UBL (1179 cases and 508 mutation cases), CEN CII (24 and 808), Peppol (707 and 509, two stylesheets), Factur-X (850 cases, five profiles), XRechnung (3198 cases, two stylesheets per syntax) and the French rules (1514 cases, thirteen stylesheets).
- **Mutation cases.** Edits of the official examples aimed at totals, rounding, decimal precision, dates, code lists, check digits, namespaces and element order: 973 of the 982 distinct CEN UBL asserts fire in the corpus, against 235 with the official cases alone, and 785 of the 810 CII assert ids fail in at least one case.
- **Schema validation compared with xmllint**, UBL 2.1 and CII D16B; every difference is listed with its cause.
- **Fast and small.** Measured on `main` after 0.3.0 against Saxon-HE 12.10 on the whole corpus of each of the nine rule sets (every document through every stylesheet, process start included, median of 5 runs): on one thread the nine jobs take 37.2 s against 80.6 s (2.2 times faster; 2.0 times against Saxon-HE without its JVM start), from about as fast (XRechnung UBL, large invoices) to 16 times faster per rule set; on eight threads 5.4 s against 33.9 s (6.2 times), 2.9 to 53 times per rule set; peak memory 19 to 46 times lower; one invoice in a new process 46 ms against 614 ms. Details in [docs/benchmarks.md](https://github.com/einvoice-validator/einvoice-validator/blob/main/docs/benchmarks.md).
- **Exact decimals.** `xs:decimal` is arbitrary precision, never a floating-point number.
- **Bounded.** Documents nested deeper than the limit (256 by default) and stylesheets nested too deep are refused with an error; template and function invocations are bounded in nesting and in stack; PDF files and downloads are bounded too. No stack overflow on hostile input.
- **Pure Rust.** No `unsafe` in our crates, no C or assembly file built anywhere; downloads on rustls with a RustCrypto provider; every third-party `unsafe`, build script and inline assembly audited in [docs/dependencies.md](https://github.com/einvoice-validator/einvoice-validator/blob/main/docs/dependencies.md). The library runs on `wasm32-unknown-unknown`.
- **Strict engineering.** Every warning and clippy `pedantic` denied, signed commits, GitHub Actions pinned by commit SHA, dependencies checked by `cargo-deny`.

<sub>Need another official rule set (a national CIUS, another syntax)? <a href="https://github.com/einvoice-validator/einvoice-validator/issues">Open an issue</a>.</sub>
