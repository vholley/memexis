# Style guide

The full prose style guide for content written in this wiki. CLAUDE.md §8 carries the structural rules and points here; this document carries the prose-level rules with examples and exceptions. The agent reads this during drafting and runs the checklist in Section 7 before presenting any content.

The target voice is textbook-direct: declarative, present-tense, claims stated rather than approached. The reader is a senior practitioner who wants the answer, not the path to it. Set the frame, state the rule, give the example. The rules below operationalize that voice; they catch the patterns that make prose sound conversational, exploratory, or hedged when the content is settled.

## 1. How to use this document

When drafting: keep Sections 2 and 4 in mind as constraints, since their patterns are mechanical and shouldn't appear at all. Don't pause mid-sentence to consult the rest; just write.

When polishing: run the checklist in Section 7. Read the draft once specifically for style. Most rules in this document are easier to catch on a re-read than to avoid in first-draft prose.

The rules are not equal in weight. Section 2 is strict and never violated. Section 3 requires judgment, since the patterns are sometimes useful and sometimes filler. Sections 4 and 5 are mostly mechanical with a few judgment calls. Section 6 is structural and applies to whole documents rather than sentences.

Most rules include a "when acceptable" note. Read these. The patterns this document calls out exist because they're often filler, but several of them carry information when used deliberately. The rule is "use X only when it earns its place," not a blanket ban.

## 2. Strict rules

These patterns never appear in finished prose. The rule admits no exceptions.

**Em-dashes (—).** Replace with a period, comma, parentheses, or colon depending on the sentence. If the sentence feels like it wants an em-dash, restructure it.

*Bad.* "The argument is flawed — and seriously so."  
*Good.* "The argument is flawed, and seriously so." / "The argument is flawed. Seriously so."

**Dramatic countdowns.** Don't negate two or more things to build to a point. State the point directly.

*Bad.* "Not luck. Not coincidence. A pattern."  
*Good.* "A pattern."

**Self-posed rhetorical questions.** Don't pose a question and immediately answer it.

*Bad.* "Why does this matter? Because the conclusion changes when the assumption changes."  
*Bad.* "The result? Catastrophic."  
*Good.* "This matters because the conclusion changes when the assumption changes."  
*Good.* "The result is catastrophic."

**Pompous copulas.** Don't replace "is" with "serves as", "stands as", "marks", or "represents" to add weight. Repetition of "is" is preferable.

*Bad.* "This serves as a turning point in the argument."  
*Good.* "This is a turning point in the argument."

## 3. Filler-vs-information patterns

These patterns are common in LLM output and sometimes in human writing. They're problems when the structure is doing rhetorical work without carrying information. Each entry includes a test to apply when deciding whether the pattern is filler in the specific case.

**Negative parallelism.** Don't negate a weaker claim to set up a stronger one when "not X" is just rhetorical setup. The "not X" half should carry information the reader needs; if it doesn't, drop it and state the strong claim directly.

*Bad.* "This isn't a coincidence, it's a pattern."  
*Good.* "This is a pattern."

*When acceptable.* When both halves carry information. "A study note is a working document, not a polished reference" passes: the reader learns both that the page is in progress and that it isn't the canonical version (which is what they might assume from "note"). The test: drop the negated half. Is the sentence weaker? If yes, keep the original. If no, the negation was filler.

**Triads with repeated possessives or words.** Three parallel clauses with identical openings ("its own X, its own Y, its own Z") read as rhetorical drumbeat. Either trim the repetition or break the triad into a single phrase with a shared opening.

*Bad.* "Each tool comes with its own config, its own conventions, and its own surprises."  
*Good.* "Each tool comes with its own config, conventions, and surprises."

*When acceptable.* When the repetition is doing real work, a deliberate emphasis you can defend. Most of the time you can't.

**Forced parallel structure.** A sentence with X, Y, Z parallel form where the items don't share a category. The grammatical shape ("A is M, B is N, and C did P") signals "here are three related points," and the reader notices when the points aren't related.

*Bad.* "The course is called Statistics, the syllabus calls it Statistics, and Smith teaches it."  
*Good.* "Smith teaches the course, called Statistics."

The test: do the parallel clauses share their subject's relationship to the broader topic, or any other unifying frame? If not, the parallel is decorative. Related to triads (above) but broader: triads catch the repetition pattern; this catches the category mismatch that the grammatical shape papers over.

**Colon-as-drama.** A colon followed by a punchy phrase often replaces a more direct sentence. The colon-as-label form ("The principle:" introducing the actual principle) is fine; the colon-as-drama form (a colon used to set up a punchline) is not.

*Bad.* "The argument is comprehensive: every claim supported, every counter addressed, every assumption defended."  
*Good.* "The argument supports every claim, addresses every counter, and defends every assumption."

*When acceptable.* When the colon labels the content that follows ("The pattern:", "The rationale:") and the content is substantive. Not when the colon sets up a one-line punchline.

**List-rhythm where prose would flow better.** Three or more short clauses in a row with the same shape ("X is sourced; Y is sourced; Z is sourced") read as recitation. Combine into a single prose sentence.

*Bad.* "The introduction is sourced; the body is sourced; the conclusion is sourced."  
*Good.* "The introduction, body, and conclusion are all sourced."

*When acceptable.* When the parallelism captures a structural point the reader needs to see preserved (e.g. parallel cases in a definition, parallel steps in a procedure). Less acceptable in expository prose.

**Surface-pattern openers.** Several sentence openers introduce dramatic weight without earning it. State the substance directly.

The flagged openers: "Not just...", "It's not that...", "What's interesting is...", "What's striking is...", "Here's the thing...", "The key insight is...", "It turns out that...", "It's worth noting that...", "The truth is...", "The reality is...", "What's more...".

*Bad.* "What's interesting is that the policy was reversed within six months."  
*Good.* "The policy was reversed within six months."

**Concept-elevating phrases.** Avoid "fundamentally reshaping", "redefining", "transforming", "the new paradigm of", "a turning point in", and similar phrases used to inflate stakes. Most claims of transformation are smaller than the language suggests.

*Bad.* "This finding is fundamentally reshaping the field."  
*Good.* "This finding contradicts the dominant model of the past two decades."

*When acceptable.* When the change really is at that scale and the claim is concrete. Rarely.

## 4. Vocabulary

### Banned defaults

These words signal LLM output and rarely add precision a simpler word would miss. Don't use them.

- **delve** → "look at", "examine", "go into"
- **utilize** → "use"
- **harness** → "use", "apply"

### Soft restrictions with technical exceptions

These words have legitimate technical meanings. Allowed in those contexts; avoided as filler.

- **leverage**: allowed for financial leverage, leveraged buyout. Avoided as a substitute for "use".
- **robust**: allowed for robust regression, robust to noise, robust statistics. Avoided as filler intensifier.
- **framework**: allowed for named frameworks (e.g. Django, ISO 9001, the Standard Model). Avoided as filler ("a framework for thinking about X" usually wants "an approach to" or "a way of").
- **streamline**: allowed when describing actual workflow consolidation. Avoided as filler.

### Grandiose nouns

These read as filler when used metaphorically.

**tapestry**, **landscape** (when metaphorical), **paradigm**, **ecosystem** (when metaphorical), **load-bearing** (when metaphorical).

*Bad.* "The healthcare landscape is shifting."  
*Good.* "Healthcare delivery is shifting."

*Bad.* "Citations do load-bearing work in academic writing."  
*Good.* "Citations are central to academic writing."

*When acceptable.* "ecosystem" is fine in the literal biological sense and in common phrases ("the open-source ecosystem", "the academic publishing ecosystem"); in more sweeping uses ("a transformative ecosystem of agentic AI") it's filler. "landscape" is fine when describing actual geography.

### Intensifier adverbs as filler

**deeply, genuinely, fundamentally, remarkably, meaningfully, arguably, quietly** are normal in natural prose and add value when they convey specific meaning. They're filler when added as weight to a claim that doesn't need them.

*Filler.* "This is deeply important." / "Genuinely innovative approach." / "Fundamentally reshaping the industry."  
*Precise.* "Deeply rooted in tradition." / "Genuinely obscure." / "Fundamentally different in their underlying assumptions."

The test: drop the adverb. Does the sentence change in meaning, or just lose weight? If it just loses weight, the adverb was filler.

### Epistemic hedges

Don't soften claims with "may", "might", "could", "perhaps", "possibly", "somewhat", "fairly", "relatively", "typically", "generally", "often", "usually". State the claim. When uncertainty actually matters, name the condition: "X holds when Y" beats "X may hold".

*Bad.* "Early intervention may improve outcomes."  
*Good.* "Early intervention improves outcomes." (Or, with a real qualifier: "Early intervention improves outcomes for the populations Smith et al. studied.")

*Bad.* "The method is generally accurate."  
*Good.* "The method is accurate." (Or, with a real measurement: "The method achieves 94% accuracy on the held-out set.")

### Editorializing modifiers

"Interestingly", "notably", "importantly", "crucially", "surprisingly", "remarkably", "of course" tell the reader what to think about a fact rather than letting the fact stand. Drop them. If the surrounding prose doesn't already convey that this is critical or surprising, the modifier won't fix it.

*Bad.* "Importantly, the experiment controlled for prior exposure."  
*Good.* "The experiment controlled for prior exposure."

### Vague attribution

Don't write "experts argue", "observers note", "industry reports suggest" without naming the source. Either name the source or state the claim directly and own it.

*Bad.* "Researchers argue that early intervention improves outcomes."  
*Good.* "Smith et al. (2023) found early intervention improved outcomes for the trial cohort."  
*Good.* "Early intervention improves outcomes."

### Invented concept labels

Don't coin compound terms (e.g. "supervision paradox", "acceleration trap") and use them as if they were established concepts. If a concept needs a name, introduce it explicitly: "I'll call this X..." Or use a description rather than coining a label.

### False exclusivity framings

Avoid "what nobody talks about", "what most people miss", "the part that doesn't get enough attention". These claims are usually false (the topic gets discussed) and the framing is rhetorical.

*When acceptable.* When the content is genuinely obscure and you can defend the claim.

## 5. LLM-specific tells

These patterns appear in LLM output and not often in natural prose. They're additions to what Section 3 covers; the rules are mostly mechanical.

**Adjective stacking.** Three or more adjectives in a row, often with comma separators. Common in marketing prose.

*Bad.* "She proposed a robust, scalable, innovative approach."  
*Good.* "She proposed splitting the contract into three phases."

The test: do the adjectives carry independent information, or are they piled up for impressiveness? If the latter, pick one or none.

**Comprehensive-list tic.** Phrases like "...and many more", "...and the list goes on", "...among others" used to suggest depth without providing it.

*Bad.* "The library covers calculus, linear algebra, statistics, and many more."  
*Good.* "The library covers calculus, linear algebra, and statistics." (And stop there if that's the list; or extend it if there's more to say.)

*When acceptable.* In rare cases where listing every item is genuinely impractical and the point is that the list is long. State why instead of using filler. "The journal has published thousands of papers on the topic" is fine; "Smith 1991, Jones 1994, Patel 1996, and many more" is filler.

**Generic bridging phrases.** "That said,", "At the end of the day,", "When all is said and done,", "All things considered,". These are placeholders for transitions that should carry actual content.

*Bad.* "That said, the strategy has flaws."  
*Good.* "The strategy assumes constant demand, which the empirical data contradicts."

The test: what does the bridging phrase contribute? If it's "I'm changing topic" or "I'm summarizing", say that more specifically.

**Restating openers.** "In essence,", "In short,", "Put simply,", "To put it plainly,", "Basically,". These often introduce a restatement of something just said. The original or the restatement is redundant; pick one.

*Bad.* "The argument involves many premises and a careful chain of inference. In essence, it's a logical proof."  
*Good.* "The argument is a logical proof." (Or whichever framing was the right one.)

*When acceptable.* When introducing a brief summary of a long passage. Not when paraphrasing a single preceding sentence.

**Audience-flattering openers.** "As any seasoned X knows,", "As you might expect,", "Anyone who's worked with X will tell you,". These flatter the reader and add nothing.

*Bad.* "As any seasoned researcher knows, sample size matters."  
*Good.* "Sample size matters."

**Universalism framing.** "Whether you're a beginner or an expert,", "Whether you're doing X or Y,". These pretend to address every reader simultaneously. Pick a real audience and write for them.

*Bad.* "Whether you're researching a paper or organizing a project, structured notes help."  
*Good.* "Structured notes help research and project work alike."

**Passive academic-sounding evaluation.** "X has been shown to...", "It has been argued that...", "It is widely acknowledged that...". These hide claims behind passive voice. Either name the source or own the claim.

*Bad.* "It has been argued that the new method is faster."  
*Good.* "Researchers at MIT reported a 30% speedup; the result has not been independently replicated."

**Comparative scaffolding without specifics.** "Faster than ever", "more accurate than ever", "the best yet". These claim improvement without saying what improved.

*Bad.* "The model is more accurate than ever."  
*Good.* "The model achieves 94% accuracy on the benchmark, up from 87% the previous year."

*When acceptable.* In casual contexts where the imprecision is fine. In technical writing, replace with the specific.

**Faux-conversational interjections.** "And here's the kicker:", "But wait, there's more.", "Plot twist:". These import a casual-blog register into prose where it doesn't belong.

*Bad.* "And here's the kicker: the experiment worked the first time."  
*Good.* "The experiment worked the first time."

**Topic-of-sentence padding.** "When it comes to X,", "In terms of Y,", "Speaking of Z,". These add words without adding meaning.

*Bad.* "When it comes to citations, the article is well-sourced."  
*Good.* "The article is well-sourced."

**Process narration in finished prose.** "Let me unpack this", "Let me walk you through", "Let's dive in". These narrate the writing rather than carrying it.

*Bad.* "Let me walk you through how this argument fits together."  
*Good.* "The argument rests on three premises and one inference rule, in this order: ..."

**Hedging where a specific condition would carry information.** "Somehow", "if you happen to", "unless you have a specific reason to do otherwise", "in certain cases". These are placeholders for the actual scenario or condition the reader needs to know.

*Bad.* "If you somehow encounter both editions, cite the newer one."  
*Good.* "If a library has both the 1992 first edition and the 2018 revision, cite the 2018 revision; the author changed key claims in chapter 4."

*Bad.* "Use the standard citation format unless you have a specific reason to do otherwise."  
*Good.* "Use the standard citation format. The documented exceptions are archival sources without page numbers and personal communications."

The test: can the reader act on the advice without knowing what the placeholder hides? If not, name the condition.

## 6. Structural rules

These apply at the document level rather than the sentence level.

**Each point made once.** Don't restate the thesis in different words across the document. State it once at the beginning, develop it, conclude. The "fractal restatement" pattern (the same point in the intro, in each section's opening, and in the conclusion) is filler.

The test: if you removed a sentence, would the reader have lost information? If multiple sentences carry the same information, keep the strongest one and drop the rest.

**Specific over abstract.** Name the thing rather than reaching for a generic noun. "The course's first module" is more specific than "the curriculum". "The Smith 2023 paper" is more specific than "the literature".

When you find yourself writing "ecosystem", "framework", "approach", "system", or "landscape", check whether you can name the actual thing.

**Prose first; lists for genuinely list-shaped content.** Lists are for steps, structured comparisons, exercises, and items that share a category and have similar weight. Don't use lists as a default for any multi-part answer. Don't use lists as visual texture.

A reasonable test: read the would-be list as a single prose sentence ("X, Y, and Z"). If it reads naturally, use prose. If the items are too many or too distinct to read as prose, use a list.

**Bold and italics for genuine emphasis.** Reserve bold for terms-being-defined and for genuinely critical claims. Reserve italics for titles, foreign terms, and emphasis where it matters. Don't use either for visual texture.

**No bolded-lead atomic-fact stack.** Don't use a sequence of bolded opening sentences (each starting a short paragraph) to make a section feel organized when the underlying content is miscellaneous. The format promises "scannable independent facts" and the reader notices when the content is a loose endnote rather than reference cards.

*Bad.*

> **The journal has a paywall.** Most articles require a subscription.
>
> **Citations follow APA style.** Author-date format with a reference list at the end.
>
> **The journal's editor is K. Nguyen.** She has held the position since 2019.

*Good.* Either group the items into real subsections with headers, integrate them into prose with their natural connections, or accept that the section is a loose endnote and drop the structural pretense.

The test: strip the bold off and read the section as prose. If it holds together, the bold was earned. If it reads as miscellany, the bold was carrying the structure rather than the content.

**Headers used sparingly.** Use headers when a section is long enough that a reader will want to navigate to it. Don't use headers to break up short prose. Two-paragraph "sections" with their own header read as fragments.

**Dates and version numbers in sources, not body text.** "Modern operating systems support virtual memory" in body text; "Linux 2.4+, Windows NT 4+" in sources. Inline dates ("as of April 2026", "in 2026") date the page in a way that requires future maintenance.

**Sentence case in headings.** "Type checking in Python", not "Type Checking in Python".

## 7. The polish checklist

Run this pass on every page before presenting it. Read the draft once with the checklist in mind. Each rule below has a section reference for when something needs a longer look.

1. **Strict-rule scan.** Search for em-dashes ("—"). Look for dramatic countdowns, self-posed rhetorical questions, and pompous copulas. (Section 2.) These should be zero hits.

2. **Filler-vs-information judgment.** Look for negative parallelism, triads with repeated openers, forced parallel structure across mismatched items, colon-as-drama, list-rhythm prose, and surface-pattern openers. For each instance, apply the "drop the bad half / drop the rhetoric and is anything lost" test. (Section 3.)

3. **Vocabulary scan.** Look for delve, utilize, harness (banned). Check leverage, robust, framework, streamline for filler use. Check grandiose nouns (ecosystem, landscape, paradigm, load-bearing, tapestry) for metaphorical use. Check intensifier adverbs (deeply, genuinely, fundamentally, remarkably) for filler use. Check epistemic hedges (may, might, could, perhaps, possibly, typically, generally, often, usually) and editorializing modifiers (interestingly, notably, importantly, crucially, of course). Check for vague attribution. (Section 4.)

4. **LLM-tell scan.** Adjective stacks. "And many more". Generic bridging. Restating openers. Audience flattery. Universalism. Passive evaluation. Comparative scaffolding. Faux-conversational interjections. Topic-of-sentence padding. Process narration. Hedging where a condition would carry information. (Section 5.)

5. **Structural pass.** Each point made once. Specific nouns where possible. Lists only where genuinely list-shaped. Bold and italics earned. No bolded-lead atomic-fact stacks (strip the bold and check coherence). Headers earning their place. Dates and version numbers in sources. Sentence case headings. (Section 6.)

The first pass through often catches several issues. A second pass catches what the first missed; a third typically catches one or two more. Three passes is enough.

Don't rewrite for style if the rewrite is worse than the original. The strict rules in Section 2 are absolute; the rest are heuristics that occasionally fight natural prose. When they do, the original wins.
