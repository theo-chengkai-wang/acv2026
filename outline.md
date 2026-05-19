# ACV 2026 Submission — Detailed Outline

**Working title:** *Full abstraction as one instance of abstract–concrete matching: a parameterised algebraic theory for quantum I/O*

**Author:** Theo Wang (Oxford); Sam Staton acknowledged as supervisor only (he co-organises ACV — see COI footnote).

**Target length:** 1–2 pages (workshop format). Aim ~2 pages.

**Workshop fit (one sentence):**
We present a worked example of the kind of abstract↔concrete matching ACV is convened to study: a parameterised algebraic theory $\iotq$ for classically controlled quantum I/O, matched by two concrete models — a stream-based operational semantics and a free monad on $\CPinf$ — which an adequacy + full abstraction theorem connects.

**Narrative device:**
A *triangle diagram* (Figure~1 in the LaTeX) whose three vertices are **Algebraic Theory**, **Operational Semantics**, **Denotational Semantics**, with edges *soundness* (alg → op sem), *soundness* (alg → den sem), and *adequacy + full abstraction* (op sem ↔ den sem). The figure is introduced once in §1 and referred back to where relevant; we do *not* let it dictate the section structure of the prose.

---

## §1. Introduction (~ ¼ page)
- **What.** Classically controlled quantum communication as a programming-language primitive — sending/receiving qubits in which sending may be conditional on a classical measurement outcome. One imperative code snippet.
- **Why.** Two reasons: distributed quantum computing needs the primitive; textbook quantum protocols (teleportation, BB84) implicitly use it. Prior accounts treat the topic mostly inside quantum process calculi, where it is entangled with concurrency.
- **How.** Algebraic theory + two concrete models + adequacy/FA. Introduce the triangle figure (Figure~1). State the slogan: this is offered as a worked instance of the abstract–concrete matchings ACV is convened to study.

## §2. Background: PATs and Staton's $\quantum$ (~ ¼ page)
- One-paragraph introduction to parameterised algebraic theories: linear parameter contexts, the type discipline reading, sums and tensors.
- Staton's $\quantum$: operations `new`, `apply_U`, `measure`; equations including deferred measurement; sound and complete with respect to its operator-algebra interpretation in $\CP$. **Important note:** this is *completeness*, not *full completeness* (a stronger and different notion). 
- The analogous triangle is already settled in the I/O-free case; the contribution is to fill it in with communication.

## §3. The algebraic theory $\iotq$ (~ ⅙ page)
- Add `in : (0|1)` and `out : (1|0)` to $\quantum$. Tensor (not sum) with a standard $\io$ theory.
- Two design points: (i) `in` and `out` do not commute with each other (order of I/O events is observable); (ii) they commute with all quantum operations, motivated by the Bell-pair-and-discard ≡ coin-toss-and-prepare equivalence (one displayed equation: `out`/`measure` commutativity).
- Eckmann–Hilton risk acknowledged as an *analogy* (the original result is about unital binary operations, not all tensors of PATs). Non-collapse is checked *a posteriori* via the two models that follow.

## §4. Operational semantics: quantum streams (~ ¼ page)
- **Quantum streams.** A $\traceset$-indexed family of CP carriers with CPTP transitions updating a hidden environment state at each I/O event. Coalgebraic in flavour (revisited in §7).
- **Configurations and reductions.** $\langle\rho, \sigma, c\rangle$; measurement reduces probabilistically; other ops deterministically; continuation call terminates.
- **Program equivalence $\quequiv$.** Same probability-weighted distribution over (final state, continuation called, final I/O trace) triples on every stream and initial state.
- **Soundness.** Terms modulo $\quequiv$ form a model of $\iotq$.

## §5. Denotational semantics: a free I/O monad on $\CPinf$ (~ ¼ page)
- **Why $\CPinf$ and not $\CP$.** Finite biproducts in $\CP$ vs the infinite sum over I/O traces needed below. Following Pagani–Selinger–Valiron, $\CPinf$ is the infinite-biproduct, dcpo-enriched completion of $\CPM$; compact closure inherited.
- **The monad.** `T(X) ≜ μY. (qbit^in ⊗ Y) ⊕ (qbit^out ⊗ Y) ⊕ X ≅ ⊕_σ Aux(σ) ⊗ X`. For every trace σ, the corresponding summand tensors the qubits exchanged along σ with a payload of type `X`.
- **Wire-bending: two distinct steps.** Compact closure of $\CPinf$ gives `qbit ⊸ Y ≅ qbit* ⊗ Y` for any compact-closed object; self-duality of `qbit` in $\CPM$ then gives `qbit* ≅ qbit`. Reconciled by the unnormalised maximally entangled state as a cup. (We are careful: compact closure ≠ self-duality — the rewriting needs both.)
- **Monadic denotation.** Each `Γ | Δ ⊢ c` denotes a Kleisli morphism indexed (after rearrangement) by (final I/O trace, continuation called).
- **Soundness.** $\complex$ is a model of $\iotq$ in $\Kl{T}^{op}$.

## §6. Adequacy and full abstraction (~ ⅙ page)
- **Theorem (adequacy + full abstraction).** For all `Γ | Δ ⊢ c_1, c_2`: `⟦c_1⟧ = ⟦c_2⟧` iff `c_1 ≃_qu c_2`.
- **Proof technique.** A sum-of-paths characterisation matching the operational sum over reduction sequences to the categorical sum over biproduct components.
- **What this is.** An instance of the abstract–concrete correspondence the workshop addresses; the abstract theory $\iotq$ is the common interface each concrete model refines.
- **What this isn't.** It does not establish *completeness* of $\iotq$ for $\quequiv$ — that is conjectured and open.

## §7. Discussion and open directions (~ ⅙ page)
- **The pattern, hedged.** PAT + two concrete models + adequacy/FA echoes themes in the algebraic-effects programme of Plotkin–Power, and is instantiated for $\quantum$ alone in Staton's POPL'15 paper. The quantum I/O case is technically non-trivial because compact closure, infinite biproducts, and the possibility of a tensor-of-theories collapse each constrain the construction. (We do *not* claim this is a complete methodological recipe — only that the pattern recurs.)
- **Three open directions.**
  - (a) **Completeness** of $\iotq$ for $\quequiv$. What would a suitable axiomatic logic look like?
  - (b) **Coalgebraic reading.** The stream is coalgebraic; relate to Ceragioli et al.'s coalgebraic model of quantum bisimulation (the cited paper, NOT effect-LTSs — those belong to the lqCCS line of work) and to Kissinger–Uijlen's $\mathsf{Caus}$ construction.
  - (c) **Algorithmic verification.** A decidable fragment? A probabilistic / quantum Hennessy–Milner logic for $\quequiv$?

---

## Style notes
- Match the PLanQC submission's `acmart` + `theo-pkgs.tex` setup; reuse macros verbatim.
- Include the TikZ triangle diagram (Figure~1) as a visual organising device introduced in §1, referred back to where relevant. Do *not* let the figure dictate the prose section structure: section titles are content-based, not "Top vertex / Bottom-left / Bottom-right / Bottom edge".
- Equations: the `out`/`measure` commutativity equation; the monad fixed point; the adequacy + FA biconditional.
- One Kleisli display showing the indexing alignment.

## Anti-overclaim checklist (applied during drafting)
- "Sound and complete", not "fully complete". (Staton proves completeness; full completeness is a stronger and distinct property.)
- "Motivated by quantum theory", not "forced by quantum theory" — one equation motivates the design, it does not prove it is the only choice.
- "To our knowledge has not been treated in this framework before", not "the first".
- "The pattern echoes themes in", not "Plotkin–Power execute this recipe" — they provide the top-vertex + denotational methodology, not the full triangle with both operational and denotational sides plus adequacy/FA.
- "Ceragioli et al.'s coalgebraic model of quantum bisimulation", **not** "effect-LTSs" — the cited paper is about bisimulation; effect-LTSs belong to the separate lqCCS line of work.
- Compact closure and self-duality of $\qbit$ are explicitly distinguished as two separate facts.
- "An instance of the abstract–concrete correspondence the workshop addresses", not "*the* matching ACV asks for" / "ACV invites".
- "Echoes themes in", "non-trivial because", not "a recipe" or "force every step to be earned".
- Figure caption no longer says the abstract theory "certifies" agreement — the bottom-edge theorem does.
- Completeness of $\iotq$ is flagged as open; not implied by adequacy/FA.

## What is different from the PLanQC submission
- Lead with the ACV slogan (full abstraction as abstract↔concrete matching).
- Triangle figure is the organising visual.
- Discussion (§7) reframed around the *pattern* plus three ACV-relevant open directions, rather than the bare future-work list of the PLanQC abstract.
- COI handling: `\thanks` footnote noting Staton's supervision and recusal as ACV organiser.
