---
name: winlab-slides-guidelines
description: This skill should be used when the user asks to "review my WinLab slides", "check WinLab slides formatting", "create WinLab presentation slides", "help me with WinLab slide design", or mentions WinLab slide guidelines, presentation standards, or PowerPoint formatting for WinLab.
---

# WinLab Slides Guidelines

Review and guide WinLab presentation slides creation following WinLab guidelines. All requirements use RFC 2119 keywords (MUST, MUST NOT, REQUIRED, SHALL, SHALL NOT, SHOULD, SHOULD NOT, RECOMMENDED, MAY, OPTIONAL).

## RFC 2119 Keyword Definitions

When reviewing slides, interpret these keywords as defined in RFC 2119:

- **MUST / REQUIRED / SHALL**: Absolute requirement - the slide MUST comply or it is non-compliant
- **MUST NOT / SHALL NOT**: Absolute prohibition - the slide MUST NOT include this
- **SHOULD / RECOMMENDED**: Strong recommendation - valid reasons may exist to ignore, but implications must be understood
- **SHOULD NOT / NOT RECOMMENDED**: Strong recommendation against - may be acceptable in specific circumstances, but implications should be understood
- **MAY / OPTIONAL**: Truly optional - one may choose to include or omit

## Slide Title Requirements

- **MUST** have slide titles that clearly indicate what the slide intends to express
- **MUST** have unique slide titles (no duplicate titles)
- **MUST** have titles that directly address the slide's main topic
- **SHOULD** use suffix notation like "(1/2)", "(2/2)" when content spans multiple slides and cannot fit on a single slide

## Content Structure and Organization

- **SHOULD** organize overall structure from high-level concepts to detailed information
- **SHOULD** group related content together on the same slide or adjacent slides
- **SHOULD** reorganize content for presentation format (presentations cannot be read like articles with back-and-forth referencing)

## Context Before Detail

Audience needs the *why* before they care about the *what* and *how*.

- **MUST** establish context (background, motivation, the problem being solved) before diving into details, methods, or results — set up cause and effect first
- **MUST NOT** jump straight into implementation, numbers, or results without explaining what triggered the work or what question it answers
- **SHOULD** lead each topic with: what was the situation → what was the problem → what was decided/done → what was the outcome
- **SHOULD** make the connection between sequential slides explicit so the audience can follow the chain of reasoning

## Make the Point Obvious

The audience reads each slide for a few seconds. The key point must land in those seconds.

- **MUST** make the main takeaway of each slide visually or structurally obvious — bold, color, callout box, position, or a one-line summary at the top
- **MUST NOT** bury the key conclusion inside dense paragraphs, inside a table cell, or at the end of a long bullet list
- **MUST** make sure that anyone glancing at the slide can identify the main point without reading every word
- **SHOULD** state the conclusion of the slide explicitly, not leave the audience to infer it from the data
- **SHOULD** use one slide = one point — if the audience cannot tell you in one sentence what the slide said, the slide failed

## One Topic, One Slide

The goal is topic cohesion — not slide minimization, not slide maximization.

- **MUST NOT** split a single topic across multiple slides when the description and conclusion are the same content (e.g., one slide introducing topic X, another slide concluding topic X with the same points)
- **MUST** put the introduction and conclusion of the same topic on the same slide — if they share the subject, they share the slide
- **MUST NOT** cram unrelated topics onto one slide just to reduce slide count — fewer slides is not the goal
- **MUST NOT** repeat the same theme across multiple slides under different headings — if it is the same topic, merge it
- **SHOULD** treat each topic as one cohesive unit; use "(1/2)", "(2/2)" notation only when a single topic's content genuinely cannot fit on one slide, not as an excuse to restate the same idea

## Bullet Points and Lists

- **MUST** make the hierarchical relationships between bullet list items clear
- **SHOULD** keep each item's description concise but clear
- **SHOULD** keep each item's text within one line (do not exceed one line per bullet point)

## Abbreviations and Terminology

- **MUST** provide full names for all English abbreviations
- **SHOULD** provide full names at least in the earlier slides of the presentation

## Diagrams and Process Flows

- **MUST** include step descriptions for flowcharts, pipelines, and any content related to step sequences or timing

## Review Checklist

When reviewing WinLab slides, check:

- [ ] Each slide title clearly indicates what the slide expresses
- [ ] All slide titles are unique (no duplicates)
- [ ] Titles directly address the slide's main topic
- [ ] Multi-slide content uses "(1/2)", "(2/2)" notation when needed
- [ ] Bullet list hierarchical relationships are clear
- [ ] Each bullet point is concise, clear, and within one line
- [ ] Overall structure flows from high-level to detailed
- [ ] Related content is grouped together
- [ ] Same topic's introduction and conclusion are on the same slide (not split with repeated content)
- [ ] No unrelated topics crammed onto one slide
- [ ] No repeated theme across multiple slides under different headings
- [ ] Context (background / motivation / problem) is established before details and results
- [ ] Each slide's main takeaway is visually or structurally obvious at a glance
- [ ] Key conclusions are not buried in dense paragraphs or long bullet lists
- [ ] Each slide can be summarized in one sentence — one slide, one point
- [ ] All English abbreviations have full names provided
- [ ] Flowcharts and pipelines include step descriptions
- [ ] Content is reorganized for presentation format (not article format)

## Usage

When reviewing or creating WinLab slides:
1. Check against all MUST/REQUIRED/SHALL requirements first
2. Review SHOULD/RECOMMENDED items and note any deviations
3. Verify that titles are descriptive and unique
4. Ensure content structure supports presentation format
5. Provide feedback using RFC 2119 keyword context
