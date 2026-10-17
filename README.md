# PragmaChecker

> "I'm just paying attention."
> — Patrick Jane, *The Mentalist* (2008), "Pilot"

## Pragmatics and implicature, in brief

**Pragmatics** is the study of how context shapes meaning: what a speaker conveys beyond the literal words. Take this exchange (adapted from Wilson & Bishop, 2019):

> "Can you pick Sally up from the station?" — "I'm working all day."

The reply never says no, but everyone hears it. That unstated meaning is an **implicature**. Grice (1975) argued that listeners recover it by assuming the speaker is cooperative and relevant: if the reply doesn't answer the question directly, there must be a reason, and the most natural one is "I can't." Implicatures also arise when someone says less than they could ("some" instead of "all"), uses oddly marked wording, or shifts the topic. PragmaChecker looks for this kind of implied meaning in written text.

## What it is

A Claude Code skill that reads between the lines of a document. Give it a PDF or pasted text and it replies in a skeptical, first-person voice, like a sharp colleague who is good at reading between the lines. It is skeptical of the text and of its own readings, never of the author. The report has a short takeaway and **up to 3 orthogonal implied readings** the text does not state, each with quoted anchors, why it seems implied, and a rough relative-plausibility share. Ask for "details" to see the full scoring and the assumptions behind each reading.


> **Disclaimer: vibe-coded, use with your own caution.** This is a vibe-coded tool, built with minimal effort by giving an AI model the text of several real research papers (see [References](#references)) and iterating on the result. It has not been peer reviewed, formally evaluated, or validated against labelled data, and the authors of those papers did not write, review or endorse it. Its readings are interpretations, not facts: they can be wrong, miss context, or sound more confident than they should. Check anything that matters against the source text, and do not use it as the basis for judging or accusing a person.

> **Status: v0.1, unvalidated.** The scoring weights are starting values chosen by hand, not fitted to data. The shares rank the readings against each other; they are **not calibrated probabilities**. The method's empirical support comes from short English dialogues (e.g., Sato et al., 2025; Kang, 2026), so confidence is lower on long documents. See [sources.md](skills/pragma-checker/references/sources.md) for what each source contributed and what is and is not grounded.

## Install

```
/plugin marketplace add ARY2260/PragmaChecker
/plugin install pragma-checker@pragma-checker
```

Or copy `skills/pragma-checker/` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project).

PDF input needs `pdftotext` (poppler-utils) on the PATH.

## Use

```
/pragma-checker path/to/document.pdf
/pragma-checker <paste an email, memo or transcript>
```

When installed as a plugin the command may be namespaced, for example `/pragma-checker:pragma-checker`. You can also just ask: "read between the lines of this report".

## What it does

1. Frames the text (author, audience, genre, the question under discussion) and strips it to what is explicitly stated.
2. Finds triggers tied to quoted spans, using the Gricean maxims (Grice, 1975), and types them Q / M / I (Levinson, 2000).
3. Generates candidates from five lenses (intent, face/relation, omission/epistemic stance, attitude/non-literal force, sociocultural/genre frame; see Brown & Levinson, 1987), naming the alternative wording the author avoided, in the spirit of the Rational Speech Act framework (Frank & Goodman, 2012; Degen, 2023).
4. Screens decoys: literal restatements, lexical echoes, generic "being polite" stories, associative neighbours (Hu et al., 2023).
5. Scores each candidate (prior in frame, speaker likelihood, explanatory coverage, anchoring, inference cost), scores a literal-only control, and keeps up to 3 orthogonal readings.
6. Reports shares and evidence labels, with reliability caps for harder evidence types (for example Manner implicatures).

Built-in guards: every reading needs a verbatim quote; "nothing implied" is a valid answer; implied content is never called a lie or an admission.

## Known gaps

- No labelled evaluation set for real documents yet; weights are untuned.
- Cultural and genre priors come from the model, not from data.
- Open items are listed at the end of `sources.md`.

## References

Brown, P., & Levinson, S. C. (1987). *Politeness: Some universals in language usage*. Cambridge University Press.

Degen, J. (2019, June 21). *The Rational Speech Act framework: An integrative theory of the interaction between literal meaning, world knowledge, and context* [Conference presentation]. XPrag 2019, Edinburgh, United Kingdom.

Degen, J. (2023). The Rational Speech Act framework. *Annual Review of Linguistics, 9*(1), 519–540. https://doi.org/10.1146/annurev-linguistics-031220-010811

Eragamreddy, N. (2024). Exploring pragmatics: Uncovering the layers of language and meaning. *International Journal of Current Science Research and Review, 7*(3), 1886–1895. https://doi.org/10.47191/ijcsrr/V7-i3-50

Frank, M. C., & Goodman, N. D. (2012). Predicting pragmatic reasoning in language games. *Science, 336*(6084), 998. https://doi.org/10.1126/science.1218633

Grice, H. P. (1975). Logic and conversation. In P. Cole & J. L. Morgan (Eds.), *Syntax and semantics: Vol. 3. Speech acts* (pp. 41–58). Academic Press.

Hu, J., Floyd, S., Jouravlev, O., Fedorenko, E., & Gibson, E. (2023). A fine-grained comparison of pragmatic language understanding in humans and language models. In *Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)* (pp. 4194–4213). Association for Computational Linguistics. https://aclanthology.org/2023.acl-long.230/

Kang, D. (2026). Understanding conversational implicatures in humans and LLMs. In S. T.Y.S.S., J. D. Rodriguez, & O. de Gibert (Eds.), *Proceedings of the 64th Annual Meeting of the Association for Computational Linguistics (Volume 4: Student Research Workshop)* (pp. 741–753). Association for Computational Linguistics. https://doi.org/10.18653/v1/2026.acl-srw.66

Levinson, S. C. (2000). *Presumptive meanings: The theory of generalized conversational implicature*. MIT Press.

Sato, T., Kawano, S., & Yoshino, K. (2025). Pragmatic theories enhance understanding of implied meanings in LLMs. In K. Inui, S. Sakti, H. Wang, D. F. Wong, P. Bhattacharyya, B. Banerjee, A. Ekbal, T. Chakraborty, & D. P. Singh (Eds.), *Proceedings of the 14th International Joint Conference on Natural Language Processing and the 4th Conference of the Asia-Pacific Chapter of the Association for Computational Linguistics* (pp. 2458–2477). The Asian Federation of Natural Language Processing and the Association for Computational Linguistics. https://doi.org/10.18653/v1/2025.ijcnlp-long.132

Sperber, D., & Wilson, D. (1995). *Relevance: Communication and cognition* (2nd ed.). Blackwell.

Vignolo, M., Domaneschi, F., & D'Agruma, N. (2025). Semantics, lying, and additive particularised conversational implicatures. *Philosophical Studies*. Advance online publication. https://doi.org/10.1007/s11098-025-02371-3

Wilson, A. C., & Bishop, D. V. M. (2019). "If you catch my drift...": Ability to infer implied meaning is distinct from vocabulary and grammar skills. *Wellcome Open Research, 4*, Article 68. https://doi.org/10.12688/wellcomeopenres.15210.3

Yerukola, A., Vaduguru, S., Fried, D., & Sap, M. (2024). Is the Pope Catholic? Yes, the Pope is Catholic. Generative evaluation of non-literal intent resolution in LLMs. In L.-W. Ku, A. Martins, & V. Srikumar (Eds.), *Proceedings of the 62nd Annual Meeting of the Association for Computational Linguistics (Volume 2: Short Papers)* (pp. 265–275). Association for Computational Linguistics. https://doi.org/10.18653/v1/2024.acl-short.26

Quoted epigraph: Heller, B. (Creator). (2008–2015). *The Mentalist* [TV series]. Warner Bros. Television; CBS.
