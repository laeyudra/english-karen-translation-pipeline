# Example Translation Behaviors

These examples illustrate selected behaviors from the English-to-Karen
rule-based translation engine.

The source code and full dictionary are private. This file is a portfolio
artifact showing the kinds of grammar behavior the system handles. The examples
are not exhaustive.

## 1:N Surface Variants

The engine can produce multiple valid Karen-side outputs for one English input.
This is not just a convenience feature. It is purpose-built for Karen, where
multiple word orders, clause placements, particles, and topic structures can be
grammatically valid and natural depending on context.

Instead of forcing every English sentence into one rigid Karen output, the
system can preserve several acceptable surface realizations. This is useful for
human review, grammar testing, and synthetic 1:N English-Karen parallel corpus
generation.

### Input

```text
I don't understand what he said
```

### Example Outputs

```text
V00 default
ယတနၢ်ပၢၢ်ဘၣ်လၢအတဲမနုၤလဲၣ်
```

```text
V01 embedded_wh_complement_marker=without_marker
ယတနၢ်ပၢၢ်ဘၣ်အတဲမနုၤလဲၣ်
```

```text
V02 embedded_wh_clause_front=fronted
အတဲမနုၤလဲၣ်ယတနၢ်ပၢၢ်ဘၣ်
```

```text
V03 embedded_wh_clause_front=fronted_topic
အတဲမနုၤလဲၣ်န့ၣ်ယတနၢ်ပၢၢ်ဘၣ်
```

### What This Demonstrates

- Embedded WH clause detection.
- Optional complement marker handling.
- Embedded-clause fronting.
- Topic-marker insertion for fronted embedded clauses.
- Negative matrix clause handling.

## Embedded Locative WH Clauses

### Input

```text
I don't know where they are
```

### Example Outputs

```text
V00 default
ယတသ့ၣ်ညါဘၣ်လၢအသုအိၣ်ဖဲလဲၣ်
```

```text
V02 embedded_wh_clause_front=fronted
အသုအိၣ်ဖဲလဲၣ်ယတသ့ၣ်ညါဘၣ်
```

```text
V03 embedded_wh_clause_front=fronted_topic
အသုအိၣ်ဖဲလဲၣ်န့ၣ်ယတသ့ၣ်ညါဘၣ်
```

### What This Demonstrates

- Embedded `where` clause analysis.
- Karen clause-final WH/how behavior.
- Locative interpretation of English `be` in `where + be` contexts.
- Same variant framework applied across different embedded WH types.

## Negative Indefinites And Predicate Negation

English can place negation inside an indefinite word, as in `nothing`.
Karen-style output needs predicate negation as well.

### Input

```text
I know nothing about this
```

### Example Outputs

```text
V00 default
ယတသ့ၣ်ညါတၢ်ဘၣ်ဃးဒီးတၢ်ဝဲအံၤနီတမံၤဘၣ်
```

```text
V01 indefinite_tail_scope=local
ယတသ့ၣ်ညါတၢ်နီတမံၤဘၣ်ဃးဒီးတၢ်ဝဲအံၤဘၣ်
```

### Comparable Input

```text
I don't know anything about this
```

### Comparable Output

```text
V00 default
ယတသ့ၣ်ညါတၢ်ဘၣ်ဃးဒီးတၢ်ဝဲအံၤနီတမံၤဘၣ်
```

### What This Demonstrates

- Negative indefinite words such as `nothing` trigger clause-level negation.
- `I know nothing` and `I don't know anything` can converge to the same
  Karen-side negative structure.
- Indefinite words are modeled as a root plus a mood- and polarity-sensitive
  tail.
- Tail scope can be rendered globally or locally depending on the construction.
- English prepositions such as `about` are handled through Karen-side
  postposition/adpositional behavior.

## Modified Indefinites Under Existential Negation

### Input

```text
There isn't anyone smart in the building.
```

### Example Outputs

```text
V00 default
ၦၤခိၣ်နူၥ်ဂ့ၤတအိၣ်လၢတၢ်သူၣ်ထီၣ်အပူၤနီတဂၤဘၣ်.
```

```text
V01 indefinite_tail_scope=local
ၦၤခိၣ်နူၥ်ဂ့ၤတအိၣ်နီတဂၤလၢတၢ်သူၣ်ထီၣ်အပူၤဘၣ်.
```

```text
V02 postposed_indefinite_tail_position=after_tail, indefinite_tail_scope=local
ၦၤခိၣ်နူၥ်ဂ့ၤနီတဂၤတအိၣ်လၢတၢ်သူၣ်ထီၣ်အပူၤဘၣ်.
```

### What This Demonstrates

- Existential `there be` handling.
- Negative indefinite `anyone` behavior under clause negation.
- Modified indefinite structure: person-root + modifier + indefinite tail.
- Optional placement of separable indefinite tails around postposed predicates.
- Locative prepositional material such as `in the building` is converted into
  Karen-side postpositional/locative structure.

## Interrogative Indefinite Tails And Question Mood

The indefinite tail system is sensitive to both polarity and mood. In an
interrogative environment, `anyone` uses the interrogative indefinite tail
rather than the positive declarative or negative tail.

This example also shows that the engine can infer question mood structurally
from English subject-auxiliary inversion even without a final question mark.

### Input

```text
is there anyone who can help me
```

### Example Outputs

```text
V00 default
ၦၤအိၣ်လၢမၤစၢၤယၤသ့တဂၤဂၤဧါ
```

```text
V01 postposed_indefinite_predicate_scope=logical_scope
ၦၤလၢမၤစၢၤယၤသ့အိၣ်တဂၤဂၤဧါ
```

```text
V02 indefinite_tail_scope=local
ၦၤအိၣ်တဂၤဂၤလၢမၤစၢၤယၤသ့ဧါ
```

```text
V03 postposed_indefinite_tail_position=after_tail, indefinite_tail_scope=local
ၦၤတဂၤဂၤအိၣ်လၢမၤစၢၤယၤသ့ဧါ
```

```text
V04 yes_no_tail_scope=clause_local
ၦၤအိၣ်တဂၤဂၤဧါလၢမၤစၢၤယၤသ့
```

```text
V05 yes_no_tail_scope=clause_local, postposed_indefinite_tail_position=after_tail, indefinite_tail_scope=local
ၦၤတဂၤဂၤအိၣ်ဧါလၢမၤစၢၤယၤသ့
```

### What This Demonstrates

- Mood- and polarity-sensitive indefinite tails.
- Interrogative `anyone` behavior.
- Yes/no question-particle insertion even without an explicit `?`.
- Existential `there be` / `be there` behavior.
- Postposed auxiliary/modal behavior for `can`.
- Relative-clause handling around `who can help me`.
- Clause-local vs logical-scope question particle placement.

## Negative Imperative, No-Words, And Embedded WH Scope

This example produces many grammatical surface variants because several
independent systems interact:

- negative imperative prefix/default behavior
- negative imperative tail scope
- no-word indefinite tail scope
- embedded WH complement marker
- embedded WH fronting
- fronted-topic marker placement

### Input

```text
Tell no one what I was doing
```

The current engine produced 24 grammatical variants for this sentence before
lexical variants are even considered.

### Selected Outputs

```text
V00 default
တဲၦၤလၢယမၤမနုၤလဲၣ်နီတဂၤတဂ့ၤ
```

```text
V01 indefinite_tail_scope=local
တဲၦၤနီတဂၤလၢယမၤမနုၤလဲၣ်တဂ့ၤ
```

```text
V02 negative_imperative_tail_scope=clause_local
တဲၦၤနီတဂၤတဂ့ၤလၢယမၤမနုၤလဲၣ်
```

```text
V08 embedded_wh_complement_marker=without_marker
တဲၦၤယမၤမနုၤလဲၣ်နီတဂၤတဂ့ၤ
```

```text
V16 embedded_wh_clause_front=fronted
ယမၤမနုၤလဲၣ်တဲၦၤနီတဂၤတဂ့ၤ
```

```text
V20 embedded_wh_clause_front=fronted_topic
ယမၤမနုၤလဲၣ်န့ၣ်တဲၦၤနီတဂၤတဂ့ၤ
```

### What This Demonstrates

- `no one` is treated as a negative indefinite unit.
- The embedded WH clause is not flattened into the matrix clause.
- The negative imperative final particle can scope over the embedded WH clause
  or remain local to the matrix imperative.
- Variants are rendered from one structured token stream rather than from
  separate grammar pipelines.

## Comp-Governor And Infinitival `to`

The system distinguishes ordinary infinitival `to` from `to` licensed by
specific governor verbs and xcomp/control-like structures.

### Input

```text
I didn't ask her to do this but she still did it
```

### Output

```text
V00 default
ယတမၢအမၤတၢ်ဝဲအံၤဘၣ်သနၥ်က့အမၤဒံးအီၤ
```

### What This Demonstrates

- `ask + object + to + verb` control-like structure.
- Object/controller handling.
- Infinitival `to` suppression where it is grammatically licensed.
- Negative matrix clause handling.
- Contrastive continuation with `but`.

## Nested Complement And Quantity WH

### Input

```text
He wants you to tell him how much money he has to bring
```

### Example Outputs

```text
V00 default
အအဲၣ်ဒိးလၢနတဲအီၤလၢအဘၣ်ဟဲစိၥ်စ့ထဲလဲၣ်
```

```text
V02 embedded_wh_clause_front=fronted
အဘၣ်ဟဲစိၥ်စ့ထဲလဲၣ်အအဲၣ်ဒိးလၢနတဲအီၤ
```

```text
V03 embedded_wh_clause_front=fronted_topic
အဘၣ်ဟဲစိၥ်စ့ထဲလဲၣ်န့ၣ်အအဲၣ်ဒိးလၢနတဲအီၤ
```

### What This Demonstrates

- Nested complement structure.
- `want + object + to + verb`.
- Embedded `tell + object + embedded WH` frame.
- Quantity WH handling for `how much`.
- Infinitival obligation/necessity behavior in `has to bring`.

## Regular Number And Classifier Behavior

### Input

```text
There are two things you have to know about me
```

### Output

```text
V00 default
တၢ်အိၣ်ခံခါလၢနဘၣ်သ့ၣ်ညါဘၣ်ဃးဒီးယၤ
```

### What This Demonstrates

- Existential `there are`.
- Non-round cardinal quantity behavior.
- Classifier placement.
- Relative/complement-like continuation after the quantified noun.
- `have to` obligation behavior.
- Prepositional material such as `about me` is represented through Karen-side
  adpositional/postpositional handling.

## Round Number And Negative Tail Scope

Karen number/classifier behavior changes for multiples of ten.

### Input

```text
There are ten things you must not forget before you go
```

### Example Outputs

```text
V00 default
တၢ်အိၣ်အခါတဆံလၢနတဘၣ်သးပ့ၤနီၣ်တချုးလၢနလဲၤဘၣ်
```

```text
V01 negative_declarative_tail_scope=clause_local
တၢ်အိၣ်အခါတဆံလၢနတဘၣ်သးပ့ၤနီၣ်ဘၣ်တချုးလၢနလဲၤ
```

### What This Demonstrates

- Round-cardinal marker behavior.
- Classifier placement for round numbers.
- Negative declarative tail scope.
- Embedded/preposed temporal material with `before`.
- Modal/necessity-like negative structure.
- Temporal adpositional material such as `before you go` is handled as part of
  the clause-scope and surface-order system.

## Possessive And Genitive Flipping

English possession and `of`-genitives do not always surface in the same order
in Karen. The system models both possessive scope and internal genitive
reordering, including nested possessor chains.

### Quantified Possessor Inputs

```text
He fixed two people's homes
```

```text
He fixed the homes of two people
```

### Shared Output

```text
V00 default
အဘှီက့ၤၦၤခံဂၤအဟံၣ်
```

### What This Demonstrates

- Possessive scope over a full quantified possessor phrase, not just the head
  noun.
- `A of B` genitive flipping into a Karen-side `B of A`-style structure.
- Consistent treatment between English possessive `'s` and `of`-genitive
  paraphrases.
- Interaction between genitive handling, number/classifier placement, and noun
  phrase internal reordering.

### Nested Genitive Inputs

```text
The dog of the father of my friend is resting under the tree.
```

```text
The dog of my friend's father is resting under the tree.
```

### Shared Output

```text
V00 default
ယသကိးအပၢ်အထွံၣ်အိၣ်ဘှံးလၢသ့ၣ်ထူၣ်အဖီလၥ်.
```

### What This Demonstrates

- Nested `of`-genitive chains can converge with possessive-clitic paraphrases.
- English `dog of father of my friend` is internally reordered into a
  Karen-side possessor chain equivalent to `my friend + father + dog`.
- The locative phrase `under the tree` is handled through Karen-side
  preposition/postposition behavior.
- Genitive reordering remains local to the noun phrase and does not disrupt
  the main predicate.

## Parser Normalization

The private system does not blindly trust parser output. Before translation, it
applies narrow parser repairs when the parser structure would produce the wrong
Karen grammar.

Examples of repaired or normalized structures include:

- embedded WH clauses attached to the wrong head
- zero-relative clauses
- existential `there be` / `be there`
- separated existential forms such as `there might not be`
- ambiguous subject/object WH roles
- punctuation-sensitive parse changes
- verb-particle and separable-expression structures
- negative indefinite words such as `no one`

This parser-normalization layer is a major part of the system because the
English-side structure must be corrected before reliable Karen transfer rules
can apply.
