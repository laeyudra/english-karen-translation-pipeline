# english-karen-translation-pipeline
Portfolio overview of a rule-based English-to-S'gaw Karen translation pipeline with custom grammar transfer, parser normalization, lexical dictionary design, and regression-tested NLP behavior.

> Source code is private. This repository documents the system architecture, engineering scope, linguistic work, and long-term purpose of the project.

## Project Summary

This project is a custom rule-based English-to-Karen translation pipeline for a low-resource language context. It combines dependency parsing, parser repair, dictionary-driven lexical behavior, Karen-specific grammar rules, optional surface variants, and regression testing.

The system is not meant to be only a hand-written translator. It is also designed as a synthetic 1:N parallel corpus generator: one English input can produce one or more valid Karen-side outputs or surface variants. The long-term goal is to pair this synthetic data with public, licensed, or otherwise usable Karen text to support model training, evaluation, and eventually higher-quality Karen translation tools.

Karen is not well supported by current frontier language models or mainstream translation systems despite being spoken by millions. This project explores a practical path toward better Karen NLP by combining linguistic knowledge, rule-based generation, dictionary engineering, and testable software systems.

## What I Built

This is not an exhaustive list, but the project includes:

- A Python-based translation pipeline with separate stages for tagging, annotation, lexical translation, post-processing, and optional surface variant rendering.
- A parser normalization layer that repairs and overrides Stanza/UDPipe dependency analyses before translation rules run.
- A custom English-to-Karen lexical dictionary with POS/subtype metadata, multi-word expressions, classifier behavior, lexical variants, and grammar-triggering entries.
- Karen-specific grammar transfer rules for word order, particles, adpositions, classifiers, possession/genitives, negation, questions, embedded clauses, relative clauses, and existential constructions.
- A regression suite for sentence-level grammar behavior, used to prevent new rules from breaking previously handled constructions.
- Token-level provenance tracking through flags and source labels so grammar decisions can be inspected and debugged.

## Core Technical And Linguistic Areas

- Natural language processing
- Rule-based machine translation
- Dependency parsing
- Parser normalization and repair
- Low-resource language engineering
- Grammar transfer
- Computational linguistics
- Lexical taxonomy design
- English syntax and grammar
- Karen language structure and grammar
- Regression testing for linguistic behavior
- Python pipeline architecture

## Example Grammar Problems Addressed

The system handles or is actively being expanded to handle many English-to-Karen grammar transfer problems, including:

- Karen equivalents of English `have`.
- Existential `there be` / `be there`, including separated forms such as `there might not be`.
- Karen classifier and counter placement.
- Compound numbers, numeric ranges, and round-number behavior.
- Embedded WH/how clauses such as “I don’t know what he said.”
- Relative and zero-relative clauses such as “a book I want to read.”
- Negative scope across matrix and embedded clauses.
- Yes/no, WH/how, negative declarative, and negative imperative tail particles.
- Indefinite words such as `someone`, `anything`, `nothing`, and `nowhere`.
- Possessive and genitive structures such as `people's lives` and `the lives of two people`.
- Multi-word and separable expressions.
- Parser variance caused by punctuation, contractions, ambiguous dependency structures, and Stanza/UDPipe disagreements.

## Parser Normalization

A major part of the project happens before translation.

The pipeline does not blindly trust parser output. It inspects dependency parses and applies targeted repairs when the parser structure does not match the grammar needed for Karen translation.

Examples include:

- Repairing embedded WH clauses that are attached to the wrong head.
- Normalizing zero-relative clauses.
- Recovering intended verb-particle or phrasal structures.
- Correcting parser treatment of existential `there be`.
- Distinguishing subject/object WH roles where parser labels are incomplete.
- Preserving useful parser information while adding project-specific grammar metadata.

This parser-repair layer is important because low-resource translation quality depends heavily on getting the English-side structure right before transfer rules are applied.

## Synthetic Parallel Corpus Goal

The rule-based system is intended to generate structured English-to-Karen training data.

Instead of producing only one translation per sentence, the pipeline can produce multiple valid surface variants where appropriate. This makes it useful as a synthetic 1:N parallel corpus generator.

Long-term goals include:

- Generating high-quality synthetic English-Karen parallel data.
- Combining synthetic data with real Karen text.
- Training or fine-tuning models for Karen translation.
- Supporting a practical Karen translation tool or application.
- Exploring future Karen-capable generative language tools.

## Engineering Highlights

- Designed a multi-stage translation pipeline with clear boundaries between syntax analysis, lexical lookup, surface ordering, and variant rendering.
- Built dictionary-driven grammar behavior instead of relying only on hardcoded string rules.
- Added parser-variance repairs for specific dependency shapes rather than broad rewrites.
- Implemented optional variant rendering without duplicating the main grammar pipeline.
- Built regression tests for hundreds of sentence-level grammar cases.
- Maintained token-level debug visibility through provenance fields, flags, subtypes, and source labels.

## Current Status

This is an active, ongoing project. It is not presented as a finished production translator.

Current work focuses on:

- Expanding grammar coverage.
- Improving parser robustness.
- Growing dictionary coverage.
- Strengthening regression benchmarks.
- Improving synthetic corpus quality.
- Preparing the system for downstream model-training experiments.

## Why This Project Matters

Karen is spoken by millions of people, but it remains largely unsupported by mainstream translation systems and frontier language models. In practice, many systems do not merely make grammar mistakes; they often appear to have little functional knowledge of Karen at all.

The project addresses this by building a rule-based English-to-Karen translation pipeline that explicitly models grammar, lexical behavior, and surface structure before using that grammar engine to generate structured translation data that can support future machine learning work.

