# Project Details

## Overview

This repository documents a private English-to-S'gaw Karen rule-based translation pipeline. The source code is not public, but this file summarizes the engineering and linguistic scope of the project for portfolio and resume use.

## Role

Solo developer and project owner.

Responsibilities include:

- System architecture
- Python pipeline implementation
- Parser analysis and repair logic
- English grammar analysis
- Karen grammar modeling
- Dictionary and lexical taxonomy design
- Regression test design
- Translation behavior evaluation
- Long-term corpus generation planning

## System Type

Rule-based English-to-Karen translation pipeline and synthetic parallel corpus generator.

## Primary Goal

Build a structured translation system capable of generating high-quality Karen translation outputs and synthetic English-Karen parallel data for downstream model training.

## Core Components

- Parser normalization layer
- Custom lexical dictionary
- Grammar transfer rules
- Karen surface-order rules
- Particle/classifier/adposition handling
- Optional surface variant renderer
- Regression test suite

## Current Status

Active development.

The system is not presented as a finished production translator. It is currently focused on grammar coverage, parser robustness, dictionary growth, regression stability, and synthetic data quality.

## Technical Stack

- Python
- Stanza + UDPipe-style dependency parsing
- JSON lexical dictionary
- Custom rule pipeline
- Regression manifest testing

## Key Challenges

- Low-resource language support
- Parser variance and dependency misanalysis
- English-to-Karen grammar transfer
- Karen classifier and particle behavior
- Clause scope and embedded structures
- Separable and multi-word expressions
- Maintaining regressions across many interacting grammar rules

## Intended Long-Term Use

- Synthetic English-Karen parallel corpus generation
- Translation model training or fine-tuning
- Karen translation application development
- Possible future Karen-capable generative language tools
