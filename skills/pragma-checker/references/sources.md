# Sources, provenance, and gaps

## What each ref contributes

**Sato, Kawano, Yoshino (2025), "Pragmatic Theories Enhance Understanding of Implied Meanings in LLMs", IJCNLP.**
- Giving a model a summary of Gricean pragmatics or Relevance Theory and asking it to reason in line with that gave up to +9.6 points accuracy over zero-shot CoT on PragMega (520 items; Deceits, Indirect speech, Irony, Maxims, Metaphor). Merely naming the theories gave about +1-3 points in larger models. Basis for making the theories explicit steps in SKILL.md.
- Gricean prompting beat Relevance prompting in most models (authors suspect training-data frequency, not theoretical superiority). Basis for using maxim-flouting as the trigger step and relevance as the effect/cost step.
- Controls: unrelated theories (X-bar, graph theory, computational complexity) tended to hurt; fictitious pragmatic theories sometimes helped when plausible; none beat the real ones. Basis for the **anchor-or-drop** rule: plausible-sounding structure alone can help, so it cannot be treated as evidence.
- Input/output length barely correlated with accuracy (r about 0.18 and 0.21, R2 about 0.03-0.04). Basis for "length is not evidence".
- Weakest: Irony and Maxims; metaphor needed the fuller theory text; Deceits might need Politeness Theory. Basis for the weak-spots list and the politeness lens.
- Limitation: short, context-poor items. Basis for lowered confidence on long documents.

**Wilson & Bishop (2019), "If you catch my drift...", Wellcome Open Research.**
- Definition used throughout: pragmatics is inference from context beyond decoded dictionary meaning ("reading between the lines"). Explicature (local enrichment) vs implicature (global, wholly inferential). Basis for step 2 of the procedure (strip to explicit; report only implicature).
- Particularised conversational implicature (context-dependent) is distinct from generalised (e.g. scalar). Basis for lens 3's guidance.
- Relevance as the guiding heuristic: we assume the reply is relevant, so we seek a meaning that makes it so (the "Can you pick Sally up?" / "I'm working all day" example). Basis for candidate generation in step 4.
- Test design: positive control items (explicit answers) and "don't know" items so that abstaining is legitimate. Basis for the literal-only control and the "nothing implied" outcome.
- Empirical: implicature comprehension separable from vocabulary/grammar (two-factor model beat one-factor; vocabulary and grammar together explained about 9% of variance in implicature scores). Supports treating the task as inference, not word knowledge. Does not give any plausibility weights.
- Warning that "pragmatics" is often overextended to mean politeness, social rules or theory of mind. Basis for the caution in lens 2.

**Eragamreddy (2024), "Exploring Pragmatics: Uncovering the Layers of Language and Meaning", IJCSRR.**
- A narrative review. Used **only as a lens catalog**: speech acts, conventional vs conversational implicature, politeness strategies with P/D/R factors (via Brown & Levinson), culture, emotion, context.
- Source-quality note: the text has duplicated paragraphs (culture appears twice nearly verbatim), several garbled sentences, and cites theory second-hand. It supplies no quantitative or experimental evidence. Do not derive weights from it, and where it matters go to the primary sources (Grice 1975/1989, Brown & Levinson 1987, Sperber & Wilson 1986/1995, Levinson 1983/2000).

### Further sources

**Degen (2023), "The Rational Speech Act Framework", Annual Review of Linguistics 9:519-40.**
- The formal backbone. Listener posterior is proportional to speaker likelihood times prior over meanings, `P_L1(m|u) ∝ P_S1(u|m) P(m)`, with the speaker choosing by informativeness minus cost over a set of alternatives; rationality parameter alpha. Interpretation is gradient, not categorical. Basis for the Pr x L structure in heuristic.md, for naming avoided alternatives, and for treating the softmax temperature as a rationality parameter.
- Question Under Discussion changes implicature strength; QUD itself can be uncertain and inferred jointly with meaning (hyperbole, indirect speech). Basis for the QUD step and for the "same axis, different QUD" orthogonality route.
- Limits stated by the author: toy meaning spaces, hand-specified alternatives and semantics, intractable Bayesian inference at scale. Basis for calling the heuristic a coarse approximation and not an implementation.

**Degen (2019), RSA slides for XPrag.**
- Mostly reference-game experiments (referring expressions) and scalar "some" with QUD. Little direct use. One transferable point: over-informative (redundant) wording is rational when descriptors are noisy, so redundancy is not automatically a Quantity or Manner flout. Basis for the genre-baseline rule.
- Also frames Levinson's default theory versus Relevance Theory versus constraint-based accounts as competing process theories; RSA is a computational-level account that does not choose between them.

**Hu, Floyd, Jouravlev, Fedorenko, Gibson (2023), ACL.**
- The PragMega materials that Sato used. Wrong options are of fixed types (Literal, lexical overlap, social convention, associative, non-literal, plausible-literal, funny, non-sequitur). Strong models, when wrong, mostly pick the literal option rather than low-level decoys, so basic semantic decoding is not what fails. Basis for the decoy screen.
- Removing the context story hurts most for Irony, then Deceits and Maxims, much less for Indirect speech and Metaphor. Models perform poorly on humor, irony and conversational maxims. Basis for context requirements and the reliability caps.
- Model and human item-level accuracy correlate most for Indirect speech, Irony, Maxims and Metaphor. Evidence that models use cues similar to humans' for those types.
- Public code and data: github.com/jennhu/lm-pragmatics.

**Yerukola, Vaduguru, Fried, Sap (2024), ACL short.**
- Generative evaluation: LLMs respond appropriately to non-literal utterances only 50-55% of the time on average, despite doing better at multiple-choice intent detection. Flouted Maxims is the worst case when generating (below chance) yet among the easiest to detect when given options. Giving the true intention lifts accuracy (Mistral-Instruct 75%); asking the model to spell out its own intention first helps much less (about 60%).
- Basis for the consequence check and for distrusting a candidate that is only narrated, not anchored. Multiple-choice discrimination overstates real competence, so any evaluation of this skill should be generative or free-form, not only multiple choice.
- Data and code: github.com/Akhila-Yerukola/generative-intention-resolution.

**Kang (2026), "Understanding Conversational Implicatures in Humans and LLMs", ACL SRW.**
- Human accuracy 92.1% overall (20 participants, 36 items). Sarcasm (Quality) highest at 99.2%; Manner implicatures triggered by causality lowest at 79.2%. Humans slightly better on literal (93.7%) than implied (89.8%) readings. Gemini-flash-1.5 80.6% and GPT-4o-mini 86.1% with direct prompting, at ceiling on Quantity (numerals, some-all) and weakest on Manner (modality).
- Surprisal-based scoring with GPT-Neo and RoBERTa reached only 44.4% and 55.6% with no consistent trigger pattern. Basis for not using token probabilities as plausibility.
- Small sample and two LLMs only. Treated as directional.

**Vignolo, Domaneschi, D'Agruma (2025), "Semantics, lying, and additive particularised conversational implicatures", Philosophical Studies.**
- A survey study in philosophy of language. Laypeople do not classify deceptive additive particularised implicatures as lies; they treat them as misleading. Earlier opposite findings are attributed to forced binary elicitation without a misleading option. Lying versus misleading turns on commitment to the disbelieved content and on deniability.
- Tangential to inference quality. Used for two output rules: add a deniability field, and never describe implied content as a lie or admission; avoid forced binary labels.

**Levinson (2000), Presumptive Meanings: The Theory of Generalized Conversational Implicature, MIT Press.** (book; only the core framework in chapter 1 was read, not the whole text)
- Three default-inference heuristics: Q ("what isn't said, isn't"), I ("what is simply described is stereotypically exemplified"), M ("what's said in an abnormal way isn't normal"). Priority when they conflict: Q > M > I. Q and M inferences are negative and metalinguistic (they depend on what else could have been said); I inferences are direct enrichments to the stereotype. Basis for typing triggers in heuristic.md and for the tie-breaker.
- Scope: generalized (default) implicature. Particularised, context-bound implicature is treated as a separate category. Levinson's theory is a rival of Relevance Theory, not an empirical test; he does not offer weights or probabilities. It supplies trigger types, not a likelihood.
- Fit with the empirical refs: Kang finds Manner (M-type) inferences hardest for humans and models, so the typing does not remove the reliability cap on M-based readings.

## What is and is not empirically grounded in the heuristic

| Element | Status |
|---|---|
| Q / M / I trigger typing and Q > M > I tie-breaker | Theoretical (Levinson); no ref tests it on real documents |
| Explicit theory steps (Grice, Relevance) | Empirically supported (Sato) on short dialogues |
| Prior x speaker-likelihood structure, avoided alternatives, QUD | Theoretically grounded in RSA (Degen); well supported for small constructed domains, applied here by hand to real text, which is extrapolation |
| Decoy screen | Motivated by the distractor types and error patterns in Hu et al. |
| Reliability caps by evidence type | Directional, from Hu (older models), Kang (20 humans, 2 LLMs), Sato, Yerukola |
| Consequence check | Motivated by Yerukola; design choice, untested |
| Anchor-or-drop gate | Motivated by Sato's fictitious-theory result; design choice |
| Literal-only control | Motivated by Wilson & Bishop test design; design choice |
| Deniability field, no-accusation rule | Motivated by Vignolo et al.; design choice |
| Sub-score definitions, unit weights, tau = 2 | **Arbitrary starting values** |
| Shares as probabilities | **Not calibrated**; present as ordinal |
| Extension to long PDFs | Extrapolation: every empirical ref here uses short items |

## Still needed

Items 1-2 would turn the heuristic from reasoned into tested. Item 3 is cited by the sources above but not yet incorporated; item 4 is only needed if the inputs call for it.

1. **A labelled evaluation set for the real use case.** PragMega and the Hu et al. and Yerukola et al. repos are short and mostly multiple-choice or scripted. A small hand-labelled set of real document excerpts, with several accepted implied readings per excerpt and a few "nothing implied" excerpts, is the missing piece. Per Yerukola, include a free-form or generative scoring route, not only multiple choice.
2. **A decision on what "likelihood" is being estimated.** The shares are relative plausibility among shown directions. If you need something closer to the probability a human reader would pick that reading, that requires human judgments on your own documents to fit tau and the weights.
3. **RSA applied to figurative language and indirect speech**: Kao, Wu, Bergen and Goodman (2014) on hyperbole, and Yoon, Tessler, Goodman and Frank (2020) on politeness and indirect speech (both cited by Degen). These would ground lenses 2 and 4 with actual joint inference over QUD and speaker goals. The Scontras et al. interactive web book "Probabilistic Language Understanding" is the practical way to implement RSA if you later want a real model instead of hand scoring.
4. **Document-level and cross-cultural pragmatics** if inputs will be reports and papers across cultures: hedging and stance in professional writing, genre analysis, Brown & Levinson (1987), House (2006). Pr currently relies on the model's own cultural and genre priors.
