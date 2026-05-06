# Awesome PII Redaction [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of libraries, models, datasets, and resources for detecting, redacting, and anonymizing Personally Identifiable Information (PII) in text - and the structured data, images, and clinical notes around it.

Personal data leaks out of nearly every system that touches user input: chat logs, support tickets, training corpora, telemetry, clinical notes, codebases. Removing it correctly is a prerequisite for GDPR, HIPAA, the EU AI Act, PCI DSS, and basic professional hygiene - but the moment you go past `[A-Z][a-z]+@[a-z]+\.com`, things get hard fast: context, multilinguality, special-category data, false positives that break utility, format-preserving replacements, and adversaries who can re-identify anonymized data from quasi-identifiers.

This list is **text-first** (NLP-based detection and anonymization) with adjacent sections for tabular, image, and clinical-text de-identification.

## Contents

- [Libraries & Frameworks](#libraries--frameworks)
  - [General-Purpose Detection & Redaction](#general-purpose-detection--redaction)
  - [LLM Guardrails & Runtime Redaction](#llm-guardrails--runtime-redaction)
  - [Database & Structured Data](#database--structured-data)
  - [Image, OCR & Document Redaction](#image-ocr--document-redaction)
  - [Healthcare / Clinical De-identification](#healthcare--clinical-de-identification)
  - [Synthetic Data & Pseudonymization](#synthetic-data--pseudonymization)
- [Models](#models)
  - [Multilingual PII Detection](#multilingual-pii-detection)
  - [English PII Detection](#english-pii-detection)
  - [Healthcare / PHI Models](#healthcare--phi-models)
  - [Zero-Shot & GLiNER for PII](#zero-shot--gliner-for-pii)
  - [Code & Secrets](#code--secrets)
- [Datasets](#datasets)
- [Benchmarks](#benchmarks)
- [Differential Privacy](#differential-privacy)
- [Secrets Scanning](#secrets-scanning)
- [Standards & Regulations](#standards--regulations)
- [Hosted Services](#hosted-services)
- [Papers](#papers)
- [Tutorials, Talks & Demos](#tutorials-talks--demos)
- [Books](#books)
- [Contributing](#contributing)

## Libraries & Frameworks

### General-Purpose Detection & Redaction

- [Microsoft Presidio](https://github.com/microsoft/presidio) - The de-facto open-source PII SDK. Pluggable analyzers and anonymizers for text, images, and structured data; combines regex, NER, and custom recognizers. Python.
- [scrubadub](https://github.com/LeapBeyond/scrubadub) - Battle-tested Python library with locale-aware detectors for names, emails, phones, SSNs, credit cards. Plays nicely with spaCy and Stanford NER.
- [DataFog](https://github.com/DataFog/datafog-python) - Python SDK that combines regex with spaCy/GLiNER pipelines for PII detection in text and images.
- [PII-Codex](https://github.com/EdyVision/pii-codex) - Research-oriented library that detects, categorizes, and **risk-scores** PII, building on Presidio.
- [HydroXai/pii-masker](https://github.com/HydroXai/pii-masker) - DeBERTa-v3-based masker with a 1024-token window and a minimal Python API.
- [OpenAI Privacy Filter](https://github.com/openai/privacy-filter) - OpenAI's bidirectional token-classification model for context-aware PII masking. 1.5B params (50M active), 128k context, Apache 2.0.
- [OpenPipe pii-redaction](https://github.com/OpenPipe/pii-redaction) - Local LLM-driven redactor designed for cleaning fine-tuning corpora; supports tagging, redaction, or synthetic replacement.
- [Phileas](https://github.com/philterd/phileas) / [Philter](https://github.com/philterd/philter) - JVM redaction engine with policy-driven replacement, encryption, and a REST front-end.
- [PIISA pii-extract-base](https://github.com/piisa/pii-extract-base) and [pii-process](https://github.com/piisa/pii-process) - Modular plugin framework following the [PIISA](https://piisa.github.io/) specification.
- [thoughtworks/pii-anonymizer](https://github.com/thoughtworks/pii-anonymizer) - Python CLI/library with masking and bucketing strategies.
- [worka-ai/pii](https://github.com/worka-ai/pii) - Deterministic, CPU-only Rust library with a capability-aware NLP pipeline.
- [Faker](https://github.com/joke2k/faker) - Synthetic-data generator for 50+ locales. Not a redactor itself, but the universal "replacement values" library.

### LLM Guardrails & Runtime Redaction

- [LLM Guard](https://github.com/protectai/llm-guard) - Anonymize/Deanonymize, secret, prompt-injection, and toxicity scanners for LLM apps. Python.
- [NeMo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails) - NVIDIA's programmable guardrails framework with built-in Presidio and Private AI integrations for PII masking on input/output/retrieval.
- [Guardrails AI](https://github.com/guardrails-ai/guardrails) and [guardrails_pii](https://github.com/guardrails-ai/guardrails_pii) - Validator-based input/output Guards including drop-in PII detection.
- [PurpleLlama / Llama Guard](https://github.com/meta-llama/PurpleLlama) - Meta's content-safety classifiers (Llama Guard 1/2/3 and 3-1B), useful as a final filter for LLM I/O including data-leak categories.
- [LiteLLM](https://github.com/BerriAI/litellm) - Universal LLM proxy with a built-in [Presidio guardrail](https://docs.litellm.ai/docs/tutorials/presidio_pii_masking) for PII masking across 100+ providers.
- [anonipy](https://github.com/eriknovak/anonipy) - LLM-powered anonymization with consistent pseudonymization (same name → same surrogate across a document).
- [anonLLM](https://github.com/fsndzomga/anonLLM) - Anonymizes prompts before they hit the API; reverses substitutions on the response.
- [Promptfoo](https://github.com/promptfoo/promptfoo) - LLM eval and red-team CLI that probes for PII leaks, prompt injection, and jailbreaks.
- [openai-guardrails-python](https://github.com/openai/openai-guardrails-python) - Drop-in OpenAI client wrapper that adds configurable safety/PII guardrails.

### Database & Structured Data

- [Greenmask](https://github.com/GreenmaskIO/greenmask) - Stateless Postgres/MySQL logical-dump anonymizer and synthetic-data generator. Drop-in `pg_dump` replacement. Go.
- [Neosync](https://github.com/nucleuscloud/neosync) - Developer-first platform for PII anonymization, synthetic data, and environment sync with referential integrity.
- [ARX](https://github.com/arx-deidentifier/arx) - Comprehensive Java tool implementing k-anonymity, l-diversity, t-closeness, and differential privacy with risk analysis and a GUI. [ARXaaS](https://github.com/navikt/arxaas) wraps it as a service.
- [PostgreSQL Anonymizer](https://gitlab.com/dalibo/postgresql_anonymizer) (Dalibo) - Postgres extension with declarative masking, faking, pseudonymization, generalization, and partial scrambling.
- [pg-anonymizer](https://github.com/rap2hpoutre/pg-anonymizer) - Node.js CLI for streaming anonymized Postgres dumps.
- [Anonimatron](https://github.com/realrolfje/anonimatron) - Java tool with persistent synonyms - same input always produces the same anonymized output across copies.
- [PIICatcher](https://github.com/tokern/piicatcher) - Scans Postgres, Snowflake, Redshift, Athena, and BigQuery for PII; tags via Datahub/Amundsen.
- [pdscan](https://github.com/ankane/pdscan) - Fast Go scanner for unencrypted personal data across MySQL, Postgres, Elasticsearch, Mongo, S3, and files.
- [anjana](https://github.com/IFCA-Advanced-Computing/anjana) - Python implementations of k-anonymity, l-diversity, t-closeness, and δ-disclosure.
- [Mondrian](https://github.com/qiyuangong/Mondrian) - Reference Python implementation of multidimensional k-anonymity.

### Image, OCR & Document Redaction

- [presidio-image-redactor](https://microsoft.github.io/presidio/image-redactor/) - Presidio's OCR-driven PII redaction for images and DICOM pixel data.
- [Octopii](https://github.com/redhuntlabs/Octopii) - AI-powered PII scanner combining OCR, regex, NLP, and Haar cascades to find IDs/passports/faces in documents and images, including S3 and open directories.
- [pydicom/deid](https://github.com/pydicom/deid) - DICOM header and pixel anonymization with whitelist/blacklist/graylist policies.
- [dicognito](https://github.com/blairconrad/dicognito) - DICOM anonymizer that preserves patient/study/series grouping.
- [doc_redaction](https://github.com/seanpedrick-case/doc_redaction) - GUI tool for redacting PDFs, images, Word, and CSV/XLSX with optional VLMs.

### Healthcare / Clinical De-identification

- [Microsoft Tools-for-Health-Data-Anonymization](https://github.com/microsoft/Tools-for-Health-Data-Anonymization) - De-identification tools for FHIR and DICOM, integrated with Azure FHIR.
- [Philter (UCSF)](https://github.com/BCHSI/philter-ucsf) - Open-source clinical-text PHI de-identifier (regex + POS + NER + whitelists).
- [philter-lite](https://github.com/SironaMedical/philter-lite) - Production-oriented refactor of Philter.
- [pyDeid](https://github.com/GEMINI-Medicine/pyDeid) - Python rewrite of the PhysioNet Perl PHI de-identifier with site-specific extensions.
- [obi/ehr_deidentification](https://github.com/obi-ml-public/ehr_deidentification) - Robust transformer-based de-identification of EHR notes.
- [DEDUCE](https://github.com/vmenger/deduce) - Pattern-matching de-identifier for **Dutch** medical text.
- [nedap/deidentify](https://github.com/nedap/deidentify) - State-of-the-art clinical de-identification with Dutch pre-trained models.
- [CRATE](https://github.com/ucam-department-of-psychiatry/crate) - Clinical Records Anonymisation and Text Extraction: end-to-end research-DB pipeline.
- [medspaCy](https://github.com/medspacy/medspacy) - Clinical NLP toolkit (sectionizer, ConText, etc.) used as substrate for clinical de-id pipelines.
- [Synthea](https://github.com/synthetichealth/synthea) - Generates entirely synthetic FHIR/CCDA/CSV patient data - sometimes a cleaner answer than de-identifying real records.

### Synthetic Data & Pseudonymization

- [Mimesis](https://github.com/lk-geimfari/mimesis) - High-performance Python synthetic-data generator across 46 locales; faster Faker alternative.
- [SDV (Synthetic Data Vault)](https://github.com/sdv-dev/SDV) - Tabular, relational, and sequential synthetic data with constraints.
- [MOSTLY AI SDK](https://github.com/mostly-ai/mostlyai) - High-fidelity synthetic-data SDK with differential privacy and fairness controls.
- [veil](https://github.com/primait/veil) - Rust derive macro for safe `Debug` redaction (don't leak PII via logs).
- [redacter-rs](https://github.com/abdolence/redacter-rs) - Rust CLI to copy and redact files across filesystems and cloud storage.
- [cockroachdb/redact](https://github.com/cockroachdb/redact) - Go package that separates "safe" vs "unsafe" data for log and error redaction.
- [logstop](https://github.com/ankane/logstop) - Ruby gem (and [Python port](https://github.com/ankane/logstop.py)) that keeps personal data out of Rails/Django logs.
- [redact-pii](https://github.com/solvvy/redact-pii) - Node.js/TS library with regex + optional Google DLP integration.
- [redactomatic](https://github.com/eisenzopf/redactomatic) - CLI for redacting PII in conversation/voice CSV transcripts with consistent surrogate substitution.

## Models

### Multilingual PII Detection

- [bardsai/eu-pii-anonimization-multilang](https://huggingface.co/bardsai/eu-pii-anonimization-multilang) - XLM-RoBERTa-base, 24 EU languages, 36 entity types including GDPR Art. 9 special categories. ONNX INT8 weights included. Apache 2.0. [Demo](https://eu-pii.bards.ai/).
- [urchade/gliner_multi_pii-v1](https://huggingface.co/urchade/gliner_multi_pii-v1) - GLiNER zero-shot, 6 languages, 50+ PII labels including healthcare and government IDs. Apache 2.0.
- [iiiorg/piiranha-v1-detect-personal-information](https://huggingface.co/iiiorg/piiranha-v1-detect-personal-information) - mDeBERTa-v3-base, 17 PII types across EN/ES/FR/DE/IT/NL, 99.4% accuracy. CC-BY-NC-ND-4.0 (non-commercial).
- [knowledgator/gliner-pii-base-v1.0](https://huggingface.co/knowledgator/gliner-pii-base-v1.0) and [gliner-pii-edge-v1.0](https://huggingface.co/knowledgator/gliner-pii-edge-v1.0) - 60+ PII/PHI/PCI categories with ONNX FP16/UINT8 quantization. Apache 2.0.
- [betterdataai/PII_DETECTION_MODEL](https://huggingface.co/betterdataai/PII_DETECTION_MODEL) - Qwen2-0.5B causal LM, 7 languages, 29 PII tags. Apache 2.0.
- [Babelscape/wikineural-multilingual-ner](https://huggingface.co/Babelscape/wikineural-multilingual-ner) - General multilingual NER (mBERT) over 9 languages - useful as a name/location PII baseline.
- [Davlan/distilbert-base-multilingual-cased-ner-hrl](https://huggingface.co/Davlan/distilbert-base-multilingual-cased-ner-hrl) - DistilBERT NER over 10 high-resource languages including Arabic and Chinese.

### English PII Detection

- [lakshyakh93/deberta_finetuned_pii](https://huggingface.co/lakshyakh93/deberta_finetuned_pii) - DeBERTa fine-tune covering 15+ PII categories including crypto addresses and VIN/IMEI. MIT.
- [Isotonic/deberta-v3-base_finetuned_ai4privacy_v2](https://huggingface.co/Isotonic/deberta-v3-base_finetuned_ai4privacy_v2) - DeBERTa-v3-base with F1 0.976 over 54 PII classes from ai4privacy. CC-BY-NC-4.0.
- [Isotonic/distilbert_finetuned_ai4privacy_v2](https://huggingface.co/Isotonic/distilbert_finetuned_ai4privacy_v2) - DistilBERT (66M) variant of the above for edge use. CC-BY-NC-4.0.
- [ab-ai/pii_model](https://huggingface.co/ab-ai/pii_model) - bert-base-cased with 33 PII categories, F1 0.96. Apache 2.0.
- [dslim/bert-base-NER](https://huggingface.co/dslim/bert-base-NER) - Classic CoNLL-2003 NER (PER/LOC/ORG/MISC). Generic, but the most-downloaded English NER baseline.
- [Jean-Baptiste/roberta-large-ner-english](https://huggingface.co/Jean-Baptiste/roberta-large-ner-english) - RoBERTa-large NER tested on emails and chat-style text. MIT.
- [LiquidAI/LFM2-350M-PII-Extract-JP](https://huggingface.co/LiquidAI/LFM2-350M-PII-Extract-JP) - Japanese PII extractor producing structured JSON; reportedly outperforms 32B-class models on its target categories.

### Healthcare / PHI Models

- [obi/deid_roberta_i2b2](https://huggingface.co/obi/deid_roberta_i2b2) - RoBERTa-large trained on i2b2 2014 covering 11 PHI categories. The default starting point for English clinical de-id. MIT.
- [StanfordAIMI/stanford-deidentifier-base](https://huggingface.co/StanfordAIMI/stanford-deidentifier-base) - PubMedBERT-based PHI detector with F1 99.6 on out-of-institution radiology reports. MIT.
- [OpenMed/OpenMed-PII-SuperClinical-Large-434M-v1](https://huggingface.co/OpenMed/OpenMed-PII-SuperClinical-Large-434M-v1) - DeBERTa-v3-large covering 54 PII types including HIPAA-aligned medical record numbers. Apache 2.0.
- [OpenMed/OpenMed-PII-SuperClinical-Small-44M-v1](https://huggingface.co/OpenMed/OpenMed-PII-SuperClinical-Small-44M-v1) - Edge-friendly 44M-param sibling of the above.
- [OpenMed/OpenMed-PII-Portuguese-SnowflakeMed-Large-568M-v1](https://huggingface.co/OpenMed/OpenMed-PII-Portuguese-SnowflakeMed-Large-568M-v1) - Portuguese-language clinical PII detector.

### Zero-Shot & GLiNER for PII

GLiNER models accept entity types at inference time, which makes them especially flexible when your PII taxonomy doesn't match a pre-trained label set.

- [urchade/gliner_multi-v2.1](https://huggingface.co/urchade/gliner_multi-v2.1) - The multilingual GLiNER base model - foundation for several PII fine-tunes. Apache 2.0.
- [urchade/gliner_large-v2](https://huggingface.co/urchade/gliner_large-v2) - English GLiNER-large.
- [nvidia/gliner-PII](https://huggingface.co/nvidia/gliner-PII) - GLiNER (570M) fine-tuned on the Nemotron-PII dataset, 55+ PII/PHI categories.
- [knowledgator/gliner-multitask-large-v0.5](https://huggingface.co/knowledgator/gliner-multitask-large-v0.5) - Multi-task GLiNER (NER + relation + QA + sentiment) - convenient for prompt-tunable PII pipelines.

### Code & Secrets

- [bigcode/starpii](https://huggingface.co/bigcode/starpii) - Encoder NER trained to find PII (names, emails, keys, passwords, IPs, usernames) inside source code. Used to clean The Stack. Gated.

## Datasets

- [ai4privacy/pii-masking-300k](https://huggingface.co/datasets/ai4privacy/pii-masking-300k) - ~225k examples across 6 languages with 27 PII classes; tagged for GDPR / EU AI Act categories.
- [ai4privacy/pii-masking-400k](https://huggingface.co/datasets/ai4privacy/pii-masking-400k) - Larger 407k-row successor with the same schema.
- [ai4privacy/pii-masking-200k](https://huggingface.co/datasets/ai4privacy/pii-masking-200k) - 209k rows, 4 languages, 54 PII classes - the corpus that most published "PII fine-tunes" actually train on.
- [nvidia/Nemotron-PII](https://huggingface.co/datasets/nvidia/Nemotron-PII) - 200k English examples spanning 30+ entity types across 30 industry domains, including sensitive attributes.
- [Babelscape/multinerd](https://huggingface.co/datasets/Babelscape/multinerd) - 3.35M rows, 10 languages, 15 fine-grained entity types including disease and biological.
- [urchade/synthetic-pii-ner-mistral-v1](https://huggingface.co/datasets/urchade/synthetic-pii-ner-mistral-v1) - LLM-synthesized PII training data (5 languages) used for the multi-PII GLiNER.
- [bigcode/bigcode-pii-dataset](https://huggingface.co/datasets/bigcode/bigcode-pii-dataset) - 12k annotated code samples across 31 languages with 7 PII categories. Gated.
- [Text Anonymization Benchmark (TAB)](https://github.com/NorskRegnesentral/text-anonymization-benchmark) - 1,268 ECHR court cases annotated with PII spans, semantic categories, and co-reference for evaluating anonymization (not just detection).
- [n2c2 / i2b2 De-identification Datasets](https://n2c2.dbmi.hms.harvard.edu/data-sets) - Clinical discharge summaries with 18 HIPAA PHI categories (2006 and 2014 challenges). DUA required.
- [bigbio/n2c2_2014_deid](https://huggingface.co/datasets/bigbio/n2c2_2014_deid) - HF-hosted version of the 2014 i2b2 de-identification track.
- [CoNLL-2003](https://huggingface.co/datasets/eriktks/conll2003) - The canonical English/German NER benchmark - useful as a sanity check for PER/LOC/ORG.
- [OntoNotes 5.0](https://catalog.ldc.upenn.edu/LDC2013T19) - Multi-genre 18-entity corpus (English/Chinese/Arabic).
- [MultiCoNER v2](https://multiconer.github.io/) - 33 fine-grained entity classes across 12 languages (SemEval 2023 Task 2).

## Benchmarks

- [Text Anonymization Benchmark (TAB)](https://github.com/NorskRegnesentral/text-anonymization-benchmark) - Reference implementation of privacy/utility metrics on the ECHR corpus.
- [PII-Bench (2025)](https://arxiv.org/abs/2502.18545) - 2,842 samples / 55 PII categories evaluating both detection and query-relevance.
- [PII-Scope](https://arxiv.org/abs/2410.06704) - Benchmark for training-data PII leakage under varied attack budgets.
- [PrivLM-Bench](https://arxiv.org/abs/2311.04044) - Multi-level privacy evaluation across DP-fine-tuned and standard LMs.
- [LLM-PBE (VLDB 2024)](https://www.vldb.org/pvldb/vol17/p3201-li.pdf) - Toolkit covering memorization, MIAs, and prompt-injection privacy attacks.
- [Unmasking the Reality of PII Masking Models (2025)](https://arxiv.org/abs/2504.12308) - Independent third-party benchmark of popular PII maskers - sobering reading.

## Differential Privacy

PII redaction handles direct identifiers; differential privacy bounds *what can be inferred* from aggregate releases or trained models. The two are complementary, not substitutes.

- [OpenDP](https://github.com/opendp/opendp) - The Harvard-led DP library (Rust core, Python bindings).
- [Google differential-privacy](https://github.com/google/differential-privacy) - Google's open-source DP libraries (C++/Go/Java + PyDP).
- [PyDP](https://github.com/OpenMined/PyDP) - Python wrapper over the Google library, maintained by OpenMined.
- [Opacus](https://github.com/pytorch/opacus) - DP-SGD for PyTorch.
- [TensorFlow Privacy](https://github.com/tensorflow/privacy) - DP optimizers and analysis for TF/Keras.
- [DP-Wizard](https://github.com/opendp/dp-wizard) - Web app for differentially private data releases over OpenDP.

## Secrets Scanning

Adjacent territory - credentials are frequently mixed in with PII.

- [trufflehog](https://github.com/trufflesecurity/trufflehog) - 800+ credential detectors with active verification across git, S3, Docker, Slack, Jenkins.
- [gitleaks](https://github.com/gitleaks/gitleaks) - Fast regex/entropy-based secret scanner with pre-commit and CI integration.
- [detect-secrets (Yelp)](https://github.com/Yelp/detect-secrets) - Enterprise-friendly secret scanner with an audit workflow.

## Standards & Regulations

- [GDPR Art. 4(5) - Pseudonymisation](https://gdpr-info.eu/art-4-gdpr/) - The legal definition. Distinguishes pseudonymous (still personal) from anonymous (out of scope) data.
- [GDPR Recital 26](https://gdpr-info.eu/recitals/no-26/) - Establishes the "reasonably likely means" test for identifiability.
- [GDPR Art. 9 - Special categories](https://gdpr-info.eu/art-9-gdpr/) - Health, genetic, biometric, racial/ethnic, political, religious, sexual-orientation, trade-union data - handled under stricter rules.
- [EDPB Guidelines on Pseudonymisation (2025)](https://www.edpb.europa.eu/system/files/2025-01/edpb_guidelines_202501_pseudonymisation_en.pdf) - The most recent official EU guidance.
- [HHS HIPAA De-Identification Guidance](https://www.hhs.gov/hipaa/for-professionals/special-topics/de-identification/index.html) - Safe Harbor (18 PHI identifiers) and Expert Determination methodology.
- [NIST SP 800-122](https://csrc.nist.gov/pubs/sp/800/122/final) - US federal baseline for identifying and protecting PII.
- [ISO/IEC 27018](https://www.iso.org/standard/76559.html) - Cloud-processor controls for PII.
- [ISO/IEC 27701](https://www.iso.org/standard/71670.html) - Privacy Information Management System extension to ISO 27001.
- [EU AI Act, Art. 10 - Data and Data Governance](https://artificialintelligenceact.eu/article/10/) - Quality, bias, and special-category-data rules for high-risk AI training data.
- [California CCPA / CPRA](https://oag.ca.gov/privacy/ccpa) - California's omnibus privacy regime.
- [PCI DSS](https://www.pcisecuritystandards.org/) - Authoritative source for cardholder-data classification (PAN, SAD) and storage rules.

## Hosted Services

Closed-source for the most part, but commonly evaluated against the open-source options above.

- [Amazon Comprehend - Detect PII](https://docs.aws.amazon.com/comprehend/latest/dg/how-pii.html) - 20+ entity types, English and Spanish.
- [Google Cloud Sensitive Data Protection (DLP)](https://cloud.google.com/security/products/sensitive-data-protection) - 200+ infoType detectors with inspect / de-identify / risk-analysis APIs.
- [Azure AI Language - PII Detection](https://learn.microsoft.com/en-us/azure/ai-services/language-service/personally-identifiable-information/overview) - Cloud PII/PHI detection across text, conversations, and native documents.
- [Private AI](https://www.private-ai.com/) - Specialized PII/PHI/PCI detection across 50+ entity types and 52 languages; on-prem container delivery.
- [Skyflow](https://www.skyflow.com/) - Polymorphic-encryption vault architecture for governing PII used by AI/analytics.
- [Tonic Textual](https://www.tonic.ai/textual) - NER-based de-identification plus synthetic data for fine-tuning and RAG.
- [Lakera Guard](https://www.lakera.ai/) - Runtime LLM-security layer including PII redaction.
- [BigID](https://bigid.com/), [Privacera](https://privacera.com/), [Immuta](https://www.immuta.com/), [OneTrust](https://www.onetrust.com/) - Enterprise data-discovery, governance, and access-control platforms.
- [Snowflake Dynamic Data Masking](https://docs.snowflake.com/en/user-guide/security-column-ddm-intro) - Column-level masking policies in the warehouse.

## Papers

### Foundations of privacy-preserving data release

- [Sweeney (2002): k-Anonymity](https://epic.org/wp-content/uploads/privacy/reidentification/Sweeney_Article.pdf)
- [Machanavajjhala et al. (2007): l-Diversity](https://personal.utdallas.edu/~muratk/courses/privacy08f_files/ldiversity.pdf)
- [Li et al. (2007): t-Closeness](https://www.cs.purdue.edu/homes/ninghui/papers/t_closeness_icde07.pdf)
- [Dwork (2006): Differential Privacy](https://link.springer.com/chapter/10.1007/11787006_1)
- [Dwork, McSherry, Nissim, Smith (2006): Calibrating Noise to Sensitivity](https://people.csail.mit.edu/asmith/PS/sensitivity-tcc-final.pdf)

### Text anonymization

- [Lison et al. (2021): Anonymisation Models for Text Data - State of the Art](https://aclanthology.org/2021.acl-long.323/) - Bridges the NLP and privacy-preserving-data-publishing communities.
- [Pilán et al. (2022): The Text Anonymization Benchmark (TAB)](https://aclanthology.org/2022.cl-4.19/) - The reference paper for evaluating anonymization (not just detection).
- [Staab et al. (2024): LLMs are Advanced Anonymizers](https://arxiv.org/abs/2402.13846) - Adversarial anonymization where an attacker LLM informs an anonymizer LLM.
- [Vats et al. (2024): Robust Utility-Preserving Text Anonymization with LLMs](https://arxiv.org/html/2407.11770v2)
- [Wang et al. (2025): A False Sense of Privacy](https://arxiv.org/abs/2504.21035) - 74% of "removed" information remains inferable after standard scrubbing. Read this first.
- [A Survey on Current Trends and Recent Advances in Text Anonymization (2025)](https://arxiv.org/abs/2508.21587)

### LLM memorization and PII leakage

- [Carlini et al. (2021): Extracting Training Data from LLMs](https://arxiv.org/abs/2012.07805) - The canonical memorization paper.
- [Huang, Shao, Chang (2022): Are LPLMs Leaking Your Personal Information?](https://aclanthology.org/2022.findings-emnlp.148/)
- [Kim et al. (2023): ProPILE - Probing Privacy Leakage in LLMs](https://arxiv.org/abs/2307.01881)
- [Gao et al. (2025): Understanding PII Leakage in LLMs - A Systematic Survey](https://www.ijcai.org/proceedings/2025/1156.pdf)
- [Privacy Auditing of LLMs (2025)](https://arxiv.org/abs/2503.06808)

## Tutorials, Talks & Demos

- [Microsoft Presidio docs](https://microsoft.github.io/presidio/) and [simple anonymization tutorial](https://microsoft.github.io/presidio/tutorial/10_simple_anonymization/).
- [Plugging transformer NER into Presidio](https://microsoft.github.io/presidio/analyzer/nlp_engines/transformers/)
- [HuggingFace Blog - Automatic PII Detection on the Hub using Presidio](https://huggingface.co/blog/presidio-pii-detection)
- [HuggingFace Cookbook - LLM Gateway for PII Detection](https://huggingface.co/learn/cookbook/llm_gateway_pii_detection)
- [LiteLLM - Presidio PII Masking Tutorial](https://docs.litellm.ai/docs/tutorials/presidio_pii_masking)
- [Philipp Schmid - Advanced PII Detection with HF Transformers and SageMaker](https://www.philschmid.de/pii-huggingface-sagemaker)
- [Ploomber - Preventing PII Leakage when Using LLMs](https://ploomber.io/blog/presidio/)
- [DifferentialPrivacy.org - How to Deploy ML with DP](https://differentialprivacy.org/how-to-deploy-ml-with-dp/)
- [eu-pii.bards.ai](https://eu-pii.bards.ai/) - Browser demo of multilingual EU PII detection across 24 languages including special-category data.

## Books

- [El Emam & Arbuckle - Anonymizing Health Data (O'Reilly, 2014)](https://www.oreilly.com/library/view/anonymizing-health-data/9781449363062/)
- [Dwork & Roth - The Algorithmic Foundations of Differential Privacy (2014)](https://www.cis.upenn.edu/~aaroth/privacybook.html) - Free PDF.

## Contributing

Contributions are very welcome - see [CONTRIBUTING.md](CONTRIBUTING.md). The bar is "would you actually use this in production, or recommend it to a colleague?" Generic NER models, archived projects, and vendor blog posts will generally be declined.

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related or neighboring rights to this work.
