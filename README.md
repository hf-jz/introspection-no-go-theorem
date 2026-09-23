# Introspective Capacity of Pretrained Generative Language Models
### A No-Go Theorem under Static Pretraining and Autoregressive Generation

**Author:** Sheng Cheng
**Affiliation:** Hefei Jiuzhai Big Data Technology Co., Ltd., Hefei, China
**Contact:** c.sheng@cumt.edu.cn
**Document:** [`introspection-no-go-theorem.pdf`](introspection-no-go-theorem.pdf), 42 pages
**Compiled with:** XeLaTeX (TeX Live 2021), A4, standard `article` class, 1.5 line spacing
**Status:** independent technical paper, September 2026

---

## TL;DR

Every frontier language model in use today is built on the same three pillars: a Transformer
architecture, static pretraining, and autoregressive generation. This paper formalizes that
paradigm as five explicit postulates, formalizes "introspection in the human sense" as five
necessary criteria, and then proves six theorems showing that the paradigm fails each of them.

The sharpest result is Theorem 1a. In the native generative regime, the self-report `R` and the
model's own hidden state `H` are conditionally independent given the public pair `(theta, c)`:

```
I(R ; H | theta, c) = 0
```

Everything a generated model says about its own inner state is therefore either a corpus
statistic or a response to an intervention performed by someone else. The paper's conclusion is
deliberately narrow: **no pretrained generative LLM possesses introspective capacity in the human
sense within this paradigm**, not "never" and not "for any conceivable machine".

---

## 1. Background: what this paper is responding to

### 1.1 The empirical result that started the discussion

In January 2026 the Anthropic research team published *Emergent Introspective Awareness in Large
Language Models* (arXiv:2601.01828, Lindsey et al.). The method is **concept injection**: an
activation vector for a specific concept is added to the residual stream, and the model is then
asked whether it "noticed" the injected "thought". The paper reports a set of genuinely
striking behaviours, among them:

- Claude Opus 4.1 and Opus 4 detect an injected thought about **20% of the time**, at the
  optimal injection layer and strength.
- Production models produce **0 false positives over 100 trials**; norm-matched **random vectors**
  elicit "noticing" only **9 out of 100 trials**.
- In the "bread" prefill experiment, a model whose output was forced denies that the output was
  intentional, but accepts it as intentional once the experimenter injects the "bread" concept
  vector into the residual stream before the response.
- **Base pretrained models** show a fairly high false-positive rate and **no greater-than-zero net
  task performance**, which points to post-training as the source of the behaviour.
- The authors state the limitations themselves: the abilities are "highly unreliable", "failures
  of introspection remain the norm", the injection protocol is "an unnatural setting unlike those
  they face in training or deployment", metacognitive representation is not demonstrated, and the
  work does not address human-like self-awareness or subjective experience.

### 1.2 The definitional debate that surrounds it

What counts as introspection is itself contested, and the paper takes that debate seriously:

- **Kammerer and Frankish (2023)** define introspection as a system representing its own current
  mental states in a form usable for online behavioural control. **Long (2023)** applies this
  functional definition to language models.
- **Comsa and Shanahan (2025)** make causal accuracy the test: a self-report is introspective only
  if it accurately describes an internal state through a causal process linking the two.
- **Song et al. (2025a, 2025b)** argue that these definitions miss the decisive feature,
  **privileged self-access**: an introspective process must yield information about internal
  states more reliably than any third party of equal or lower computational cost.
- **Block (1995)** supplies the standard access-versus-phenomenal distinction that the Anthropic
  paper itself invokes.

### 1.3 The recent move toward theorem-form statements

Alongside the empirical work, a small mathematical literature has begun to state LLM limitations
as theorems rather than measurements. **Kalai and Vempala (2023)** prove that calibrated language
models must hallucinate; **Skalse et al. (2022)** and **Manheim and Garrabrant (2018)**
characterize reward hacking and the variants of Goodhart's law; the mechanistic interpretability
line (**Elhage et al. 2021, 2022**; **Yang et al. 2018**) supplies the structural facts about
residual streams, softmax readouts, and feature superposition that this paper uses.

---

## 2. Why this paper was written

Three gaps in the literature motivated it. They correspond to Section 1.3 of the paper.

**1. No paradigm-level formalization.** The empirical result is bounded by what the experiment can
observe. It does not answer what the paradigm itself permits. A 20% success rate under injection is
compatible with at least two very different readings: an engineering-scale limitation that a
better training recipe would remove, or a structural ceiling that no in-paradigm recipe can cross.
Nothing in the published work decides between them.

**2. No logical relations among the criteria.** The four operational criteria of the Anthropic
paper are stated in parallel, without argument for their necessity, sufficiency, or their coupling
to the paradigm. "Internality" (their criterion 3) and "privileged access" (Song et al.) are
treated as separate requirements, although both follow from one fact: the report is a public
function of `(theta, c)`. Unifying them costs nothing and buys an additional criterion, closed-loop
plasticity, which the online-control definition of introspection requires and which no in-paradigm
system can satisfy.

**3. Correlation under intervention is not a structural guarantee.** Every causal link established
by concept injection is established by an external experimenter. The question the paper asks is
what remains when no one intervenes. The answer is Theorem 1a, and it is stated so that it can be
refuted: if a system satisfying the postulates stably produced `I(R ; H | theta, c) > 0` in the
unintervened regime, the theorem would be dead.

A fourth, practical motivation is that the debate needed a falsifiable target. "Models may be
introspecting" cannot be tested. "Within this paradigm, the report-state mutual information in the
native regime is zero, and the introspective risk admits no PAC-style guarantee" can be tested, and
can be wrong.

---

## 3. Where this paper differs from the published work

| Published position | This paper's position |
|---|---|
| **Lindsey et al. 2026**, empirical, four operational criteria (accuracy, grounding, internality, metacognitive representation) | Axiomatizes the paradigm as five postulates and the criteria as five (adding **A5 closed-loop plasticity**), then proves a paradigm-level no-go. The paper's data (20%, context dependence, zero net benefit for base models, unverifiable metacognitive representation, explicit caveats) are treated as **necessary corollaries of the theorems**, not as evidence of an in-paradigm capacity. |
| **Song et al. 2025b**, privileged self-access treated as a definitional requirement | Privileged access becomes **Theorem 5**, derived from public computability. Theorem 5c goes further: there exist state-description functions that an external probe reads exactly, at cost `O(d^2)`, while the model cannot report them above the random baseline. |
| **Comsa and Shanahan 2025**, introspection iff accurate causal self-description | Absorbed as criterion **A2** plus the first clause of **A3**. The causal test they introduce is kept, and its scope is made explicit: it holds under external intervention, not in the native regime. |
| **Kammerer and Frankish 2023**, **Long 2023**, functional definition built on online behavioural control | Supplies the online-control element that becomes **A5**. Where the definitional literature stops at "the state must be usable for control", this paper requires the control loop to be **private**, and Theorem 3 shows that in-paradigm influence is confined to the public text channel with an upper bound of `C log |Sigma|` bits. |
| **Concept-injection methodology** (representation engineering, activation addition, SelfIE, Patchscopes) | Treated as an **out-of-paradigm intervention**. The paper argues that an intervention performed by the experimenter is third-party access by construction, so injection experiments cannot certify a first-person channel, however striking their outputs are. |
| **Kalai and Vempala 2023**, calibrated models must hallucinate | Reused as a tool, not contested. Theorem 4c combines it with the softmax bottleneck to obtain a calibration-fidelity tension on open introspection queries. |
| **Goodhart / reward-hacking literature** (Manheim and Garrabrant; Skalse et al.) | Theorem 2 is a **strict instantiation** of regressional Goodhart. The novelty is the identity of the true objective, faithful reporting of the system's own state, and the proof that its orthogonality to the training signal (`O*` independent of `D` given `theta`) is structural rather than circumstantial. |
| **The "never" reading** that a negative result invites | Explicitly rejected. Chapter 5 is devoted to it: every impossibility theorem presupposes the postulates, and relaxing any one of them (online plasticity, a non-static corpus, a non-textual interface, an explicit verifier) dissolves the corresponding theorem. |

The difference is one of kind, not degree. The existing literature describes what models do under
observation; this paper proves what the paradigm can and cannot permit, and marks precisely where
the proof stops.

---

## 4. What the paper claims, and what it does not claim

**Proposition P (proved):** within the paradigm `Pi` given by postulates `Pi_1` to `Pi_5` (static
pretraining, frozen at inference, autoregressive decoding with bounded context, no explicit
verifier, absence of state labels), no system obtained by static pretraining plus autoregressive
generation satisfies the five necessary criteria `A1` to `A5` of human-significance introspection.

**Explicitly not claimed:** any universal negation. The paper does not claim that no machine can
ever introspect, and it makes no commitment about qualia or phenomenal consciousness. Relaxing a
single postulate removes the corresponding theorem, and Table 3 lists which theorem falls with
which postulate:

| Relaxed postulate | Theorems that fail |
|---|---|
| `Pi_1` online or continual learning | 1, 2, 6 |
| `Pi_2` plasticity at inference | 3, 6 |
| `Pi_3` non-textual interface or embodiment | 1, 5 |
| `Pi_4` explicit verifier or self-supervised objective | 4 |
| `Pi_5` corpus contains state labels | 1b, 2 |

The intended reading of the result is "impossible within the current paradigm", which is the
precise content of Proposition P.

---

## 5. Structure of the paper

| Part | Content |
|---|---|
| **Chapter 1, Introduction** | Statement of Proposition P, related work, the three gaps above, contributions, and the grading convention. |
| **Chapter 2, Formal framework** | The postulates `Pi_1` to `Pi_5`; the underlying mathematics of the paradigm (residual stream, fixed Lipschitz forward map, single unembedding readout, softmax bottleneck, superposition, factorization over text only); the state model; the five criteria `A1` to `A5`; measurement tools (grounding mutual information, closed-loop effect measure); proof logic, with the causal graph of the paradigm (Figure 1) and the Bayesian network of Theorem 1 (Figure 2). |
| **Chapter 3, Core theorems** | Theorem 1: absence of an introspective information channel (1a zero mutual information in the native regime; 1b no PAC-style guarantee, proved through the No-Free-Lunch theorem). Theorem 2: proxy-objective mismatch (higher text likelihood can strictly harm introspective fidelity). Theorem 3: closed-loop failure (no private channel; `C log |Sigma|` bit bound). Theorem 4: generation is not verification (no verifier node; truth undecidable in-paradigm; calibration-fidelity tension). Theorem 5: no privileged channel (public computability; probes read what reports cannot say). Theorem 6: no self-model evolution (frozen kernel, context-capped runtime self-knowledge). |
| **Chapter 4, Formal reinterpretation of the concept-injection experiments** | A falsifiability test of the theorems against the published data, and a table matching each of the Anthropic paper's self-reported limitations to the theorem that predicts it. |
| **Chapter 5, Boundary conditions and philosophical positioning** | Relaxation analysis, functional versus phenomenal introspection, relation to HOT, IIT, GWT, and biological naturalism, and the explicit statement on "never". |
| **Chapter 6, Conclusion** | Adjudication of Proposition P, theorem summary, and a provenance table separating what is cited from what is original. |
| **Appendix A** | Complete proofs of Theorems 1 to 6 and the assembled proof of Proposition P. |
| **Appendix B** | Notation, information theory, computational learning theory, Goodhart taxonomy, causal inference, and background from algorithmic information theory and recursion theory. |

---

## 6. Grading convention

Every assertion in the paper carries a grade, so that a reader can tell a theorem apart from a
judgment:

- **Grade A**: rigorously provable under the stated postulates. Theorems 1, 2, 3, 5.
- **Grade B / A-B**: structural assertions whose mathematical core is grade A but whose
  application to real systems needs additional interpretive assumptions. Theorem 4c and the
  "human introspection requires self-model updating" premise of Theorem 6.
- **Grade C**: architectural judgments, which take no part in proofs.
- **Grade D**: philosophical premises, chiefly the premise that the five criteria are necessary
  conditions for human introspection. Reject that premise and the conclusion becomes "the paradigm
  achieves at most weak functional self-report", an assertion that is itself grade A.

---

## 7. Data provenance

Every quantitative claim the paper attributes to arXiv:2601.01828 is quoted from the main text or
appendix of that paper: the 20% success rate at the optimal injection layer and strength, the 0
false positives over 100 trials for production models, the 9 out of 100 trials for norm-matched
random vectors, the two-thirds depth of the most sensitive layer against the shallower layer
favoured by prefill detection, the zero net performance of base pretrained models, and the
experimental results behind the "bread" experiment. The theorems themselves are derived from the
postulates and from standard results in information theory, learning theory, and causal inference.
They do not depend on any of the experimental data, which is why the data can serve as a test.

---

## 8. Repository contents

```
.
├── introspection-no-go-theorem.pdf   # the paper, 42 pages
└── README.md                         # this file
```

## 9. Selected references

- Lindsey, J. et al. (2026). *Emergent Introspective Awareness in Large Language Models.* arXiv:2601.01828
- Song, S., Lederman, H., Hu, J., Mahowald, K. (2025). *Privileged Self-Access Matters for Introspection in AI.* arXiv:2508.14802
- Comsa, I. M., Shanahan, M. (2025). *Does It Make Sense to Speak of Introspection in Large Language Models?* arXiv:2506.05068
- Kammerer, F., Frankish, K. (2023). *What Forms Could Introspective Systems Take?* Journal of Consciousness Studies 30(9-10)
- Kalai, A. T., Vempala, S. S. (2023). *Calibrated Language Models Must Hallucinate.* arXiv:2311.14648
- Manheim, D., Garrabrant, S. (2018). *Categorizing Variants of Goodhart's Law.* arXiv:1803.04585
- Skalse, J. et al. (2022). *Defining and Characterizing Reward Hacking.* arXiv:2209.13085
- Elhage, N. et al. (2021). *A Mathematical Framework for Transformer Circuits.* transformer-circuits.pub
- Elhage, N. et al. (2022). *Toy Models of Superposition.* arXiv:2209.10652
- Yang, Z. et al. (2018). *Breaking the Softmax Bottleneck.* ICLR 2018 (arXiv:1711.03953)
- Joudaki, A. et al. (2025). *Barriers for Learning in an Evolving World: Mathematical Understanding of Loss of Plasticity.* arXiv:2510.00304

The complete bibliography, 59 entries, is in the PDF.
