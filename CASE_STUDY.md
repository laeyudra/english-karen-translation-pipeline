# Case Study: English-to-S'gaw Karen Translation Pipeline

## Problem

Karen is spoken by millions of people, but it remains largely unsupported by mainstream translation systems and frontier language models. In practice, many systems do not merely make grammar mistakes; they often appear to have little functional knowledge of Karen at all.

This creates a low-resource NLP problem where basic translation quality, grammatical structure, and natural Karen output cannot be assumed from existing tools.

The project addresses this by building a rule-based English-to-Karen translation pipeline that explicitly models grammar, lexical behavior, and surface structure before using the system for synthetic parallel corpus generation.

## Approach

The system uses a staged NLP pipeline:

1. Parse English input with dependency parsers.
2. Repair or normalize parser output when needed.
3. Apply project-specific POS, subtype, and grammar metadata.
4. Translate tokens through a custom dictionary.
5. Reorder tokens and insert Karen particles/classifiers/postpositions.
6. Render optional surface variants where multiple valid outputs exist.
7. Protect behavior with sentence-level regression tests.

## Why Parser Repair Matters

Dependency parsers are not always reliable for the structures this system needs. The project includes targeted parser repair logic before translation, so downstream grammar rules operate on a structure closer to the intended English meaning.

Examples of parser-repair targets include:

- Embedded WH clauses.
- Zero-relative clauses.
- Existential `there be`.
- Separable expressions.
- Ambiguous subject/object roles.
- Punctuation-sensitive parse changes.

## Example Grammar Coverage

The system includes rules for many grammar-transfer cases, including:

- English `have`.
- Existential `there be`.
- Embedded WH/how clauses.
- Relative and zero-relative clauses.
- Negative scope.
- Question particles.
- Karen classifiers and counters.
- Compound and range numbers.
- Possessive/genitive structures.
- Indefinite words such as `someone`, `anything`, `nothing`, and `nowhere`.
- Multi-word and separable expressions.

## Testing Strategy

The project uses a regression manifest of sentence-level cases. Each grammar change is tested against existing examples to reduce regressions.

The regression suite checks token metadata, rule provenance, ordering behavior, inserted particles, and translated surface output.

## Result So Far

The system is still in active development, but it has established:

- A working multi-stage rule pipeline.
- A custom lexical dictionary model.
- Parser-normalization infrastructure.
- Hundreds of protected grammar regression cases.
- Optional variant rendering for multiple valid Karen outputs.
- A path toward synthetic English-Karen parallel corpus generation.

## Long-Term Direction

The rule-based pipeline is not intended to be the final product by itself. It is a foundation for generating structured parallel data that can be paired with real Karen text and used for downstream model training.

The eventual goal is to support higher-quality Karen translation tools and potentially Karen-capable generative language applications.
