---
name: english-writing-review
description: This skill should be used when the user asks to "review my English writing", "check my paper sentences", "review this paragraph", "check my abstract", "help me improve this sentence", "review my slides text", "check for English errors", "polish my writing", "proofread my paper", or mentions reviewing English sentences for academic papers, technical reports, or presentation slides at WinLab (NYCU).
---

# English Writing Review for Academic Papers and Slides

Review English writing in technical papers and presentation slides following principles from academic English writing guides. All requirements use RFC 2119 keywords (MUST, SHOULD, MAY).

## RFC 2119 Keyword Definitions

- **MUST / REQUIRED**: Critical error — the writing is incorrect or highly unnatural; fix immediately
- **SHOULD / RECOMMENDED**: Strong improvement — the writing is technically acceptable but noticeably weak
- **MAY / OPTIONAL**: Minor polish — small refinement that improves clarity or style

---

## Review Process

When the user provides text to review:

1. Read the entire passage first before commenting
2. Apply the checklist below in order (grammar → readability → word choice → conciseness)
3. Quote the problematic phrase, label the issue type, and provide a corrected version
4. Summarize critical (MUST) issues first, then SHOULD improvements
5. For slides, also check that each bullet is ≤1 line and has clear hierarchy

---

## MUST Fix: Grammar Errors

### Articles (a / an / the)
- MUST use `the` for: previously mentioned nouns, uniquely defined nouns, superlatives, ordinals (`the first method`)
- MUST use `a/an` for: first introduction of a countable noun, any-one-of-a-kind reference
- MUST NOT add articles to uncountable nouns used generically: `information`, `research`, `software`, `evidence`, `feedback`, `terminology`, `equipment`
- MUST distinguish `most` (no article, refers to all) vs. `most of the` (refers to a specific group)

### Subject-Verb Agreement
- MUST use singular verb after: `more than one`, `each`, `every`, `the number of`
- MUST use plural verb with: `data`, `criteria`, `phenomena`, `a number of`
- MUST NOT write: `Many researches` → `Much research`; `two criterias` → `two criteria`

### Verb Tense
- MUST use present simple for: established facts, the paper's own methods/conclusions, figure descriptions (`Figure 3 **shows**…`)
- MUST use past simple for: specific experimental procedures (`The samples **were annealed**…`)
- MUST use present perfect for: linking past work to current relevance (`Several methods **have been proposed**…`)

### Sentence Completeness
- MUST NOT use comma splice: ~~`The samples were dipped, then they were washed`~~ → `…dipped, and then they were washed` / `…dipped. Then they were washed.` / `…dipped; then they were washed.`
- MUST NOT write `Because X, so Y` (choose one connective) or `Although X, but Y`
- MUST NOT place `then` after a comma as a connector: ~~`Since X, then Y`~~ → `Since X, Y`

### Pronoun Agreement
- MUST NOT use `they` to refer to singular antecedents like `each person`, `everyone`
- MUST clarify what `this` refers to: ~~`This was surprising`~~ → `This **result** was surprising`

### Subjunctive Mood
- MUST use bare infinitive after `suggest/recommend/require that`: `We suggest that the author **solve** (not *solves*) the problem`

---

## MUST Fix: Sentence Structure Errors

### Dangling / Misplaced Modifiers
- MUST ensure the subject of an introductory phrase matches the main clause subject
- ~~`Using this method, the results were excellent`~~ → `Using this method, **we obtained** excellent results`

### Parallel Structure
- MUST use identical grammatical form for logically parallel items:
  - ~~`The system is fast, reliable, and has low cost`~~ → `fast, reliable, and **low-cost**`
  - ~~`We develop a theory and testing it`~~ → `We **develop** a theory and **test** it`
- MUST maintain strict parallelism with correlative conjunctions: `both A and B`, `either A or B`, `not only A but also B`

### Double Connectives (Chinese Transfer Errors)
- MUST NOT use: ~~`Because X, so Y`~~, ~~`Although X, but Y`~~, ~~`Since X, then Y`~~

### Hyphenation of Compound Adjectives
- MUST hyphenate compound modifiers before nouns: `high-performance`, `so-called`, `right-hand side`, `well-known`

---

## SHOULD Apply: Readability Principles

### Old-Before-New (舊信息在前，新信息在後)
- SHOULD begin each sentence with information already known from prior context
- SHOULD place the key new information at the end of the sentence for emphasis
- ~~`A new control algorithm was developed by us. System stability is guaranteed by this algorithm.`~~ → `We developed a new control algorithm. **This algorithm** guarantees system stability.`

### Topic as Subject (主題作主詞)
- SHOULD make the paragraph's main topic the grammatical subject of most sentences
- SHOULD NOT switch subjects frequently within a paragraph about one topic

### Short Before Long
- SHOULD keep the main verb within the first ~9 words
- SHOULD place short phrases before long phrases
- SHOULD NOT bury the verb deep inside a long subject noun phrase

### Shorten Subject–Verb Distance
- SHOULD move intervening phrases/clauses out of the subject–verb gap
- ~~`The method, which was proposed by Smith in 1995 and modified by Jones, **is** widely used`~~ → `Smith's method (1995), later modified by Jones, **is** widely used`

### Active Voice Preference
- SHOULD use active voice ~75% of the time; passive ~25%
- Passive is justified ONLY when: (a) keeping old information at sentence start, or (b) keeping the paragraph topic as the subject
- ~~`It was found by us that…`~~ → `We found that…`

### Logical Connectives Must Be Accurate
- SHOULD use signal words to make logical relationships explicit: `because`, `however`, `therefore`, `although`, `in addition`, `in contrast`
- MUST NOT use `thus`/`therefore` unless the second sentence is a genuine logical consequence of the first

---

## SHOULD Apply: Conciseness

### Eliminate Nominalization (名詞化)
Replace weak-verb + noun constructions with strong verbs:

| Wordy | Concise |
|-------|---------|
| `perform an analysis of` | `analyze` |
| `make a comparison of` | `compare` |
| `give consideration to` | `consider` |
| `have an effect on` | `affect` |
| `make an improvement` | `improve` |
| `conduct a study of` | `study` |

### Remove Redundant Phrases
Replace with shorter equivalents:

| Wordy | Concise |
|-------|---------|
| `due to the fact that` | `because` |
| `owing to the fact that` | `because` / `since` |
| `in spite of the fact that` | `although` |
| `at the present time` | `now` / `currently` |
| `in the near future` | `soon` |
| `it should be noted that` | (delete; state the fact directly) |
| `it is interesting to note that` | (delete) |
| `for the purpose of` | `to` / `for` |
| `the reason is because` | `the reason is that` / `because` |
| `in order to` | `to` (most cases) |
| `phenomenon of X` | `X` |
| `functionality` | `function` |

### Avoid Vague Intensifiers
- SHOULD remove or replace: `very`, `quite`, `rather`, `some`, `a bit`
- SHOULD replace `some researchers` with `several researchers` or `many researchers`

---

## SHOULD Apply: Word Choice

### Overused / Misused Verbs

**Show** — overused; replace with precise verbs:
- Presenting results in figures/tables → `present`, `illustrate`
- Examining properties → `examine`, `investigate`, `analyze`
- Expressing equations → `express`, `write`
- Table comparing → `compare` (not `Table 4 shows the comparison of`)
- Experimental evidence → `demonstrate`, `indicate`, `confirm`, `suggest`
- Paper's own contribution → `propose`, `present` (not `show`)

**Perform** — overused: `perform experiments` → `conduct experiments` / `run experiments`

**Support** — overused by engineers; replace with precise verb:
- ~~`The system supports Windows`~~ → `The system **is compatible with** Windows` / `can run on Windows`

**Prove** — too strong for experimental results: use `confirm`, `verify`, `demonstrate`, `indicate`

**Propose vs. Present**:
- `propose` = recommend acceptance; use for the paper's own method (`the **proposed** method`)
- `present` = put forward for consideration; author may not endorse
- MUST NOT write ~~`our presented method`~~

### Common Preposition Errors

| Wrong | Right |
|-------|-------|
| `research **of** this problem` | `research **on** this problem` |
| `changes **of** thickness` | `changes **in** thickness` |
| `arrangements **of** the conference` | `arrangements **for** the conference` |
| `methods **of** solving` | `methods **for** solving` |
| `similar **as**` | `similar **to**` |
| `A is the same **with** B` | `A is the same **as** B` |
| `suffer several problems` | `suffer **from** several problems` |

### Connector Distinctions

| Connector | Correct Use |
|-----------|-------------|
| `On the contrary` | Negates the previous sentence's argument |
| `In contrast` | Introduces a strongly contrasting new idea |
| `On the other hand` | Introduces a new angle for comparison (requires a related prior topic) |
| `Similarly` | Points out resemblance — NOT for contrast or complement |
| `Conversely` | Use when relationship is opposite/complementary (not `similarly`) |
| `Thus` / `Therefore` | Second sentence must be a true logical result of the first |
| `However` | Requires a genuine contrast between the two sentences |

### Frequently Confused Words

| Word | Rule |
|------|------|
| `most` vs. `most of the` | `Most students` (all students, generally) vs. `Most of the students **in our lab**` (specific group) |
| `fewer` vs. `less` | `fewer` for countable nouns; `less` for uncountable |
| `few` vs. `a few` | `few` = almost none (negative); `a few` = some (positive) |
| `farther` vs. `further` | `further` for degree/additional (almost always in tech writing) |
| `e.g.` vs. `i.e.` | `e.g.` = for example; `i.e.` = that is (only one correct interpretation) |
| `ensure` vs. `insure` | `ensure` = guarantee; `insure` = take out insurance |
| `principal` vs. `principle` | `principal` = main/most important; `principle` = rule/law |
| `precise` vs. `accurate` | `precise` = repeatable; `accurate` = close to true value |
| `comprise` | The whole comprises the parts: ~~`is comprised of`~~ → `comprises` / `consists of` |
| `while` vs. `when` | `while` = during an ongoing period; `when` = at a moment or point in time |
| `modern` | Ambiguous (could mean decades ago); prefer `contemporary` or `recent` |
| `utilize` | Almost always replace with `use` |
| `firstly/secondly` | Use `first`, `second`, `third` (no `-ly`) |
| `respectively` | Only use when truly needed; avoid with >3 items — use a table instead |
| `totally` | Means "completely"; NEVER use for "in total" → use `a total of` / `altogether` |
| `that` vs. `which` | Restrictive clauses: `that` (no comma); non-restrictive: `which` (with comma) |
| `suppose` vs. `if` | `Suppose` is a verb (use period or semicolon after, not comma + `then`) |
| `occur` vs. `happen` | Formal writing prefers `occur`; avoid `there is/are` for events |
| `refer` | Use as active/imperative: `refer to Smith (1993)` — NEVER passive `is referred to in` |
| `so far` / `until now` | Replace with `to date` in research writing; present perfect often makes it redundant |
| `recently` | Vague; prefer `in recent years`, `in the past decade`, or a specific time frame |
| `novel` / `unique` | Only use if truly unprecedented/one-of-a-kind; otherwise use `new`, `proposed` |
| `performance` | Vague; specify: `speed`, `accuracy`, `throughput`, `efficiency` |
| `solution` | Only for genuine problem-solving; NEVER as a marketing synonym for `system`/`design` |
| `et al.` | Requires period: `Smith et al.` not `Smith et. al` |
| `vs.` | Correct abbreviation for *versus*; NOT `v.s.` |
| `so-called` | Must be hyphenated |
| `without loss of generality` | Fixed phrase; NOT `without the loss of generality` or `without losing the generality` |

---

## Review Checklist

Go through this checklist when reviewing submitted text:

**Grammar (MUST)**
- [ ] Articles (`a`/`an`/`the`) are correctly used; no article on generic uncountable nouns
- [ ] Subject-verb agreement is correct (`data are`, `more than one X has`, etc.)
- [ ] Verb tenses are appropriate for each sentence's context
- [ ] No comma splices, run-ons, or dangling modifiers
- [ ] No double connectives (`because...so`, `although...but`, `since...then`)
- [ ] Parallel structure is maintained in lists and correlative conjunctions
- [ ] Compound adjectives before nouns are hyphenated

**Readability (SHOULD)**
- [ ] Old information precedes new information within sentences
- [ ] The paragraph topic is consistently the grammatical subject
- [ ] Main verb appears within the first ~9 words
- [ ] Active voice is used unless passive is justified
- [ ] Logical connectives accurately reflect the actual relationship

**Conciseness (SHOULD)**
- [ ] Nominalizations replaced with verbs (`analyze` not `perform an analysis of`)
- [ ] Redundant phrases removed (`because` not `due to the fact that`)
- [ ] `show` replaced with precise verbs where possible
- [ ] `support`, `perform`, `utilize`, `modern`, `obviously`, `phenomenon of` reviewed
- [ ] Weak intensifiers (`very`, `quite`, `some`) removed or replaced

**Word Choice (SHOULD)**
- [ ] Prepositions are correct (especially `of` vs. `on`/`in`/`for`/`about`)
- [ ] `propose` vs. `present`, `prove` vs. `confirm/verify`, `while` vs. `when` correctly used
- [ ] `data`, `criteria`, `phenomena` treated as plural
- [ ] `research` treated as uncountable (no `researches`)
- [ ] Connectors (`however`, `thus`, `similarly`, `in contrast`) used correctly
- [ ] `respectively` used only when necessary and with comma separation
- [ ] `that` vs. `which` (restrictive vs. non-restrictive) correctly applied

---

## Output Format

When providing feedback, use this format:

**Critical Issues (MUST fix)**
> ~~`[original phrase]`~~ → `[corrected phrase]`
> **Reason:** [brief explanation]

**Improvements (SHOULD apply)**
> ~~`[original phrase]`~~ → `[suggested phrase]`
> **Reason:** [brief explanation]

**Overall Assessment:** [1–2 sentences summarizing the writing quality and main areas for improvement]
