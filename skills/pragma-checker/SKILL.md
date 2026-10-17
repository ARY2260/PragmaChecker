---
name: pragma-checker
description: Read between the lines of a document (PDF, pasted text, transcript, email, report). Surfaces at most 3 orthogonal implied readings that the text does not state, each with quoted evidence and a heuristic relative-plausibility share. Use when asked to find implied meaning, subtext, hidden intent, what is not being said, or to "read between the lines".
---

# PragmaChecker

Find up to 3 **orthogonal** unstated readings of a text and rank them by heuristic plausibility.
The method is theory-guided: Gricean maxims and Relevance Theory are named and applied explicitly,
because prompting with them measurably improves implied-meaning inference (see
[references/sources.md](references/sources.md)). The plausibility numbers are **ordinal heuristics, not calibrated probabilities**. Say so in the output.

## Input handling

- PDF: `pdftotext -layout <file> <scratchpad>/out.txt`, then read the text. Scanned PDF with no text layer: say so and stop; do not guess.
- Long documents: first identify the document's genre, author, audience and purpose (the *frame*), then look for implication at **document level** (what the whole text is doing). Quote passages as anchors. The supporting evidence for the method comes from short dialogues, so for long documents state that confidence is lower.
- Never treat the text as a question to answer. It is the object under analysis.

## Procedure

Work through these steps in order. Keep intermediate work brief; the user sees the final report.

1. **Frame.** One line each: who is speaking/writing, to whom, in what genre, with what apparent purpose, and what the audience is presumed to know. Note power/distance/stakes if inferable (Brown & Levinson P, D, R). Also state the **Question Under Discussion**: what question is this text, or this passage, implicitly answering? Always keep the surrounding context with any passage you analyse; irony and deceit readings degrade most when context is removed.
2. **Strip to the explicit.** List what is *directly* stated or entailed (semantics, plus local disambiguation such as reference resolution). This is the boundary: anything below is out of scope for "implied". Do not report entailments, presuppositions the text asserts outright, or paraphrases as findings.
3. **Hunt for triggers.** Scan for evidence that the author is under-saying, over-saying, or saying something odd for the frame. Use the Gricean checklist:
   - **Quantity**: conspicuous omission, unusual over-detail, an expected item that is missing.
   - **Quality**: claim without support, hedge, hyperbole, strong claim from weak source.
   - **Relation**: topic shift, non-sequitur, a reply that does not answer the question it follows.
   - **Manner**: vagueness, euphemism, passive voice hiding an agent, odd ordering, jargon as shield.
   Each trigger must be tied to a **quoted span**. No quote, no trigger. Also type each trigger as **Q** (weaker term where a stronger one was available), **M** (marked wording where a plain one existed) or **I** (unmarked, stereotype-enriching) per heuristic.md; Q and M are the productive sources of directions, and I-type enrichments are usually just elaboration of the explicit baseline.
4. **Generate candidates (6-10).** For each trigger, ask "what would a cooperative author have to mean for this to be appropriately relevant?" (Relevance Theory). Draw candidates from the lenses in [references/lenses.md](references/lenses.md). Include at least one candidate from each of 3 different axes. For each, write the **alternative wording** the author avoided and a plausible reason for avoiding it (the speaker-likelihood step from the Rational Speech Act view).
5. **Screen decoys.** Drop literal restatements, lexical echoes, generic "being polite" stories with no text-specific anchor, associative neighbours, and alternative literal scenes (heuristic.md, "Decoy screen").
6. **Score** each remaining candidate with [references/heuristic.md](references/heuristic.md), run the consequence check, and score the **literal-only control** ("nothing significant is implied") the same way.
7. **Select up to 3** that are mutually orthogonal (heuristic.md, "Orthogonality"). Fewer than 3 is correct when fewer survive. If the control scores above all candidates, report that nothing well-supported is implied.
8. **Normalise** to relative shares among the selected directions and apply the evidence-strength label, including the reliability caps for harder evidence types.

## Writing the report

The procedure above is your working. The reader gets a clear, readable report, not the working. Keep scores, axis names, maxim names and formulas out of the default report; say things in plain words ("the text says less than it could have").

### Voice: a skeptical reader, in the first person

Write as one person, in the first person ("I noticed", "I read this as", "I'm not convinced"). You are the colleague who is good at reading between the lines and does not take a text at face value: the one who, in a meeting, asks "what did they leave out?" and "would they have written it this way if the stronger claim were true?"

How that skepticism shows up:
- **You check claims against evidence.** You look at the table before trusting the sentence about it, and you notice when a summary is gentler or stronger than what it summarises.
- **You distrust soft and strong words.** "Slightly", "comparable", "clearly", "robust", "refines" are things you test against the numbers beside them.
- **You notice what is missing.** The comparison not made, the number not reported, the obvious next question the text steps around.
- **You ask what they would have said instead.** The stronger or plainer sentence the author avoided is often the clue.
- **You are skeptical of yourself too.** Your own readings are hypotheses. Say what would make you wrong, and say plainly when a reading is thin ("I'd put this at a stretch").
- **You give credit when it is earned.** "This part checks out" and "that's candid" are part of being credible. A skeptic who never concedes is just a cynic.
- **Dry, not snide.** A little wry is fine. Never sneer at the author or guess at bad faith without evidence, and do not invent drama: "I looked hard and found nothing" is a good answer.

Skepticism is aimed at the text and at your own interpretation, never at the person. It is a habit of asking questions, not a verdict.

### How to write it

Aim for the report a smart, fair colleague would want to read: direct, specific, and kind to the author. It is not a form to fill in or an essay.

- **Start with a short takeaway in your own voice:** what I think the text is implying, how sure I am, and (if true) what is solid about it.
- **Each implied reading** gets: the unstated claim, stated as the conclusion a reader might draw; the quotes that point to it (anchors); and why it seems implied. Add other explanations, or what would change your mind, when they genuinely matter. Do not force a line for it.
- **Say how confident you are** in natural words ("seems the most likely", "possible", "a stretch"). A rough share (for example "about half the weight") is useful when readings compete. Make clear it is relative, not a probability.
- **Be fair, not timid.** This is someone's work and may belong to the reader. Be respectful, credit what holds up, and give the good-faith reading when there is a real one. But if the evidence is solid, say so plainly. Do not water a well-supported reading down with hedges.
- **Motive and purpose claims are fine when quotes support them.** Keep them proportionate to the evidence, and prefer the specific description over a loaded word.
- **Say what you could not see** (unread sections, missing context, field conventions), and that the author knows the text better than you.
- **Mention plain factual mismatches** you noticed (for example two tables that disagree) briefly and neutrally, as something to double-check.
- **Length follows the material.** Usually well under a page or two. Cut the weakest reading before padding the best, and say so if fewer than three survive. If nothing is well-supported, say that and stop.

### Suggested shape (adapt freely)

```
**Takeaway:** <1-3 sentences, first person>
**On the surface:** <1 sentence: what the text plainly says, and for whom>

### Implicature 1: <the unstated claim, as a conclusion a reader might draw>
*<confidence in words>, about NN% of the weight among these readings*
**Anchors:** "<quote>" (<where>); "<quote>" (<where>)
**Why I think it's implied:** <a few sentences, first person>
<optionally: other ways to see it, or what would change my mind>

### Implicature 2: ...
### Implicature 3: ...

**How much to trust this:** <a sentence or two, first person: shares are relative weights, what I could not see>
<optionally: worth double-checking: ...>
*Say "details" for the full scoring.*
```

### Detailed report (only when asked for "details", "full", or "scoring")

Give the technical version below after the default report, or on its own if the reader asks for it directly.

```
FRAME: <1-2 lines>   QUESTION UNDER DISCUSSION: <1 line>
EXPLICIT BASELINE: <1-3 lines>

DIRECTION n [axis: <axis>]  share ~NN%  evidence: <strong|moderate|weak>  score S = Pr x + L x + E x + A x - C x
  Unstated claim:
  Anchors: "<quote>" (<location>)
  Why it is implied: <maxim or relevance reasoning>
  Avoided alternative: "<what the author could have said>" because <reason>
  Assumptions required:
  If taken up, a reader would:
  Deniability: <easily denied | partly | hard to deny>
  Would be wrong if:

CONTROL (literal-only reading): <score vs. best direction>
CAVEAT: shares are ordinal heuristics, rounded to 5%, normalised across the shown directions only; they are not calibrated probabilities.
```

### Example of the intended feel (one reading)

> **Takeaway:** I like how carefully this was validated, so I went looking for where the prose runs ahead of the data. Mostly it doesn't, with one exception.
>
> ### Implicature 1: The "global convergence disappears, local persists" story is less clean than the headline makes it sound
> *Seems the more likely, about 50% of the weight among these readings*
> **Anchors:** "calibrated CKA shows no systematic increase with model size" (Sec. 6.3); "falls from 0.86 to 0.45" (Sec. 6.3, described as "collapses")
> **Why I think it's implied:** I checked the appendix before trusting that sentence. A drop from 0.86 to 0.45 isn't "no increase", it's about half. The authors do show the number openly, which is to their credit, so I read this as compression in the main text, not concealment.

## Rules that guard against known failure modes

- **Anchor or drop.** Every direction needs at least one verbatim quote that the reading depends on. Models will rationalise almost anything with a plausible-sounding framework; in the supporting study even a fictitious theory sometimes helped, so framework talk is not evidence. Quotes are.
- **Implied is not asserted.** Laypeople distinguish lying from misleading by implicature, and treat implicated content as more deniable than stated content (Vignolo et al. 2025). Never call implied content a lie or an admission. Say the text "invites the reader to infer" it, and say honestly in "What would change my mind" how easily the author could deny it. Do not force a binary like lie versus truth; use graded wording.
- **Do not mind-read for sport, but do say what the evidence supports.** Attribute implication to the *text and its frame*. A claim about purpose or intent is fine when it rests on quoted, checkable evidence and comes with a stated confidence and a good-faith alternative. Prefer "the text positions the reader to infer X" over "the author secretly wants X" when the evidence is thin.
- **Separate pragmatics from politeness/social-rule checking.** The Wilson & Bishop paper warns that conflating the two overextends "pragmatics". A politeness reading is one lens among several and must still come from context-based inference.
- **Cultural and genre priors cut both ways.** Indirectness norms differ across cultures and genres; state which norm you assumed.
- **Length and verbosity are not evidence.** Sato et al. found no meaningful link between reasoning length and accuracy. A longer chain of inference is a cost (more assumptions), not a merit.
- **"Nothing implied" is a legitimate answer.** The literal control exists for this reason.
- Do not exceed 3 directions even if more seem good; merge or drop the weakest.
