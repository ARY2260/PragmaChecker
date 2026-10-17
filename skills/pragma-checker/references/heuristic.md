# Plausibility heuristic

Purpose: give each candidate reading a comparable score, then turn scores into relative shares.

Design basis. The structure follows the Rational Speech Act (RSA) idea (Degen 2023): the plausibility of a
meaning *m* given the observed text *u* is proportional to **(how likely *m* is a priori in this frame) x (how likely
a rational author who meant *m* would be to write exactly *u* rather than the alternatives she had)**. In log terms
the two parts add. Relevance Theory contributes the effect-versus-effort trade-off, Grice contributes the triggers
(which alternatives were conspicuously avoided), and the anchoring gate is added because plausible-sounding reasoning
can rationalise almost anything (Sato et al.). This is a coarse, hand-scored approximation of RSA, not an
implementation of it: RSA itself needs an explicit alternative set and literal semantics and is intractable at
real-text scale (Degen 2023, section 6).

## Before scoring: name the alternatives and the question

1. **Question Under Discussion (QUD).** State the question the text (or passage) is implicitly answering. Implicature
   strength depends on whether a stronger or more explicit statement would have been relevant to that question
   (Degen 2023, 5.1). If the QUD is itself uncertain, candidates may legitimately differ in which QUD they assume;
   say so, because that is a clean source of orthogonality.
2. **Alternatives.** For each candidate, write 1-3 concrete things the author could have written instead (more
   explicit, stronger, shorter, differently ordered) and why a rational author with meaning *m* might still have
   chosen the actual wording (cost, politeness, deniability, obviousness, convention). A candidate with no
   believable avoided alternative has no trigger.
3. **Baseline for the genre.** Over-saying and redundancy are not automatically flouts: speakers rationally add
   redundant detail when descriptors are noisy, and formal genres (legal, regulatory, boilerplate) are redundant by
   convention (Degen 2019 slides). Judge Quantity and Manner oddities against the genre's normal level, not against
   an ideal minimal text.

## Type each trigger (Levinson 2000)

Levinson's three default-inference heuristics give each candidate a type, and the type tells you how to score L.
Write the type next to each candidate.

| Type | Heuristic | Typical text signal | What is inferred | Use in scoring |
|---|---|---|---|---|
| **Q** | What isn't said, isn't | a weaker or less specific term where a stronger one on the same scale was available (some/all, may/will, good/excellent, listing some items) | the stronger alternative does not hold, or the author cannot vouch for it | L is high only if the stronger alternative is relevant to the QUD and the author plausibly knew whether it held |
| **M** | What is said in an abnormal way isn't normal | marked or roundabout wording where a plain synonym existed ("caused the car to stop", "not impossible", "the man" for "he", a euphemism, a passive hiding an agent) | the situation is marked: indirect, less likely, deviant, or deliberately distanced | name the plain alternative that was passed over; L is high if the marked form is not explained by genre or politeness convention |
| **I** | What is simply described is stereotypically exemplified | an unmarked statement with no contrastive alternative | enrichment to the stereotypical case | Usually elaboration of the explicit content, not a separate direction. Report an I-reading only if the text then breaks the stereotype or the stereotype is itself the point |

Conflict rule: where two candidates make inconsistent claims, Q takes priority over M, and M over I
(Levinson's ordering Q > M > I). Use it as a tie-breaker when scores are within one point.

Diagnostics: Q and M readings are negative and metalinguistic, meaning they exist only by reference to what else
could have been said. A quick test is the "not X, Y" paraphrase: "he didn't *some*-do it, he did it all" for Q;
"it wasn't *just* a smile" for M. A candidate that cannot be stated as "the author avoided Z" is probably I-type or a
particularised guess and should be scored on Pr and anchoring instead.

Scope caution: Levinson's account covers *generalized* implicature (default, tied to the form of words).
Context-bound *particularised* implicature, the kind Wilson & Bishop test and the kind most real documents hinge on,
is outside it and still needs the QUD, frame and anchoring steps. Levinson's theory is also contested by
Relevance Theory (both are surveyed in Degen's slides as rival process accounts). Here it is used as a source of
concrete trigger types, not as a claim about how readers process text.

## Sub-scores (integers 0-3)

| Code | Name | 0 | 3 | RSA / theory term |
|---|---|---|---|---|
| Pr | Prior in frame | contradicts genre, audience, culture, power/distance/stakes | exactly what this frame makes expected | prior over meanings P(m); Brown & Levinson P, D, R |
| L | Speaker likelihood | an author meaning *m* would very likely have written something different | an author meaning *m* would very likely have written exactly this, and the avoided alternative is conspicuous | pragmatic speaker S1(u given m), informativeness and cost; Gricean triggers |
| E | Explanatory coverage | explains one minor oddity | explains most oddities found | Relevance Theory: cognitive effect |
| C | Inference cost | needs 0-1 extra unstated premises | needs 4+ premises or exotic background | Relevance Theory: processing effort. Count premises |
| A | Anchoring | no verbatim quote | 2+ independent quotes from different places | guard against rationalisation |

Hard gate: **A = 0 drops the candidate.**

## Score

```
S = Pr + L + E + A - C
```

Unit weights. The only structural claim taken from RSA is that prior and likelihood combine additively in log
space, so Pr and L get equal weight. The weights on E, A and C are not derived from any ref. Treat the scheme as a
starting point to tune against labelled data (see sources.md).

## Decoy screen (apply before selecting)

Hu et al. (2023) built wrong answers of fixed kinds, and models and humans both fell for them. Reject or heavily
downgrade a candidate that is:

- **Literal re-statement** presented as an inference (the most common wrong choice for strong models).
- **Lexical echo**: supported only by words that happen to overlap with the passage's vocabulary.
- **Generic social-convention story** ("they are just being polite", "they want to avoid conflict") with no
  text-specific anchor. Wilson & Bishop also warn that politeness and social rules are not the same as pragmatic inference.
- **Associative neighbour**: topically related but not needed to make the text relevant.
- **Plausible-literal alternative reading**: a different literal scene that fits the words.

## Consequence check (generative test)

Yerukola et al. (2024) found models often detect an intention yet fail to use it, and that self-generated intention
statements helped little. So test each surviving candidate by its consequences, not by how convincingly it is narrated:
write one line on what a reader who accepts it should do, ask or conclude that a reader taking only the literal
meaning would not. If the two are indistinguishable, reduce E by one (the direction changes nothing).

## Literal-only control

Score "nothing significant is implied" with the same formula: Pr = how conventional the text is for its genre (0-3);
L = 3 if the wording is what a plain informative author would write, down to 0 if conspicuous avoided alternatives
exist; E = 3 minus the share of oddities the best candidate explains (scaled 0-3); C = 0; A = 3 if the text is
complete and plainly informative for its purpose, else 0-2. This mirrors the explicit-answer and "don't know"
control items in Wilson & Bishop's test, whose purpose is to stop over-reading.

## Orthogonality

Every pair in the selected set must pass:

1. **Different primary axis** (lenses.md), or the same axis with a different assumed QUD that changes the reading.
2. **Not nested**: neither is a special case or paraphrase of the other.
3. **Independent in evidence**: if A were false, B would still stand on its own anchors. Shared anchors are fine,
   shared reasoning is not.

Orthogonal does not mean mutually exclusive; several can hold at once. The shares therefore describe relative
plausibility among the shown directions, not a partition of all possibilities.

## Shares

```
p_i = exp(S_i / tau) / sum_j exp(S_j / tau)      over the selected directions only, tau = 2
```

tau plays the role of RSA's rationality parameter (alpha is its inverse): lower tau makes the top direction dominate.
2 is a default. Round to the nearest 5%. If the top two differ by 5 points or less, call them roughly tied.

## Evidence-strength label (separate from the share)

Base label from the score:
- **strong**: S >= 8, A >= 2, and S exceeds the control by >= 3.
- **moderate**: S >= 5, A >= 1, and S exceeds the control.
- **weak**: otherwise.

Reliability caps from the empirical record. Apply the lowest cap that matches the direction's evidence type:

| Evidence type the reading mainly rests on | Cap | Why |
|---|---|---|
| Quantity scalar/numeral contrast ("some" vs "all", exact numbers), clear sarcasm with rich context | none | Near-ceiling for humans and strong models (Kang 2026; Hu 2023) |
| Indirect request or refusal visible from an adjacent question or goal | none | Easiest category in Hu 2023; conventionalised |
| Irony, deceit or maxim-flout that depends on situational context | moderate unless Pr >= 2 and context is quoted | Largest accuracy drop when context is removed (Hu 2023); weak on flouted maxims (Sato; Yerukola) |
| Manner implicature (marked wording, causality, modality) | moderate unless A = 3 | Lowest and most variable accuracy for humans (79%) and models (Kang 2026) |
| Metaphor / figurative reading | moderate unless the fuller frame is quoted | Needed fuller theory and global knowledge, not local cues (Sato; Hu 2023) |
| Culturally specific norm | moderate unless the norm is stated and the author's culture is evidenced | Eragamreddy; no empirical test here |

If every selected direction is weak, open the report with "No well-supported implied meaning found" and show them as tentative.

## Do not use model token probabilities as the plausibility measure

Kang (2026) compared surprisal (negative log probability) with direct prompting for implicature: surprisal gave
44-56% accuracy with no consistent pattern, while prompted responses reached 81-86%, close to humans (92%).
The scores here are therefore explicit, quotable judgments, not likelihoods read off a language model.

## Known weak spots (state these when relevant)

- All empirical grounding is from short, constructed English items. Long documents, other languages and non-conversational genres are extrapolation.
- Several results are from small samples (Kang: 20 humans, 36 items, 2 LLMs) or older models (Hu 2023). Treat tier differences as directional.
- Some items have no single defensible answer when context is thin. Do not force a winner.
