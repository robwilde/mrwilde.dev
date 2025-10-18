---
title: "How I Failed a Technical Interview by Doing Too Much (And Built a Framework to Never Do It Again)"
date: 2025-10-17
draft: false
params:
  slug: "technical-interview-failure-framework"
layout: "post"
tags: [ "Technical Leadership", "Software Engineering", "Better Developer", "Guide" ]
authors: [ "Robert Wilde" ]
description: A post-mortem on how cognitive bias sabotaged my interview preparation, and the systematic approach I built to prevent it from happening again.
---

*A post-mortem on how cognitive bias sabotaged my preparation, and the systematic approach I built to prevent it from
happening again*

## The Setup

I was excited about the opportunity. It checked all my boxes: fully remote position, contract work with competitive
compensation, and genuine opportunities for growth. They sent me a technical challenge with a clear instruction right at
the top:

**"You are not required to code the full solution before the interview. Instead, prepare to walk through your design
decisions and, if desired, share code snippets or pseudocode to illustrate critical parts."**

I had a long weekend to prepare. I'm a Senior Software Engineer with deep PHP and Laravel expertise. I'd helped multiple
companies modernize legacy systems. This should have been straightforward.

So naturally, I completely missed the point and spent the entire weekend doing exactly what they told me not to do.

## The Moment of Realization

Monday, 12:45 PM. Fifteen minutes before the interview.

I stared at my screen, looking at the fully refactored Laravel 12 application I'd built, the comprehensive test suite
I'd written, the elaborate service layer architecture I'd implemented. My heart sank as I finally, *finally* read the
requirements properly:

- I had used `Http::pool()` for concurrent requests instead of designing an actual async solution.
- I was given oauth credentials and the challenge mentioned updating the basic to oauth. I focused on the external to
  internal client api and should have focused on the client to the external api.
- The data provided to connect to the internal API was not working, so I spent several hours building a mock internal
  API. My thinking was that this would make e2e testing more complete and ci/cd could utilise this for self-contained
  testing. I had reasons but again was not following the instructions.
- I had diagrams I had not updated, no flowcharts I had not revised
- I had a mess of context files I'd never cleaned up
- I had no clear presentation prepared

The interview was chaotic. I was just walking through each commit, like a guide pull request. I should have been going
over my plan of what to do and not showing then what I did. I should have been explaining the architecture and
presenting the trade-offs.

I knew within the first five minutes that I'd failed.

## The Painful Truth

Here's what hurt the most: I'm not a junior developer who doesn't know better. I've mentored other developers. I've led
technical decisions on production systems. I've given conference talks about software architecture. I recently helped a
recent uni graduate do their own technical challenge and land their first job.

And yet, when faced with a clear set of instructions, I did the exact opposite of what they asked.

Why?

## The Psychology of Why Smart People Miss Requirements

After the interview, I did what any engineer would do: I researched why this happened. Turns out, I wasn't experiencing
some unique personal failings. I was experiencing well-documented cognitive biases that affect everyone, especially
technically skilled people.

### Attentional Bias

My attention fixated on technical implementation, the comfortable, familiar territory where I feel competent, while
filtering out the presentation requirements, which felt ambiguous and uncomfortable. Research shows that our perception
is affected by our current train of thought, often causing us to focus on certain elements while ignoring
others [attentional-bias-decision-lab][attentional-bias-frontiers]. My
brain literally filtered out the parts of the requirements that made me uncomfortable.

### The Mere Urgency Effect

I prioritized tasks that *felt* urgent (getting the code working, making the refactoring "perfect") over tasks that were
actually *important* (diagrams, OAuth for external clients, presentation structure). Studies show that people tend to
prioritize tasks they perceive as time-sensitive over tasks that provide objectively greater rewards, even when those
urgent tasks aren't the ones that matter most [mere-urgency-effect][eisenhower-matrix].

### Activity Bias (or "Productive Procrastination")

Coding gave me immediate satisfaction. Every commit felt like progress. Creating diagrams and analyzing requirements
felt abstract and anxiety-inducing. My brain chose the dopamine hit of "look at all this code I wrote" over the
uncomfortable work of strategic thinking.

### Present Bias

The immediate reward of seeing working code outweighed the delayed reward of having a good presentation. I told
myself, "I'll do the diagrams later" knowing full well that "later" would never come.

### The Competence Trap

Here's the insidious part: I'm *so* skilled at coding that my brain defaults to it when faced with ambiguity. Coding is
my hammer, and this problem looked like a nail. When experts face uncertainty, they retreat to what they know best, even
when it's not what's needed.

The really painful part? While the Dunning-Kruger effect [dunning-kruger]
describes how unskilled people overestimate their abilities, there's a flip side; experts tend to underestimate their
own abilities at unfamiliar tasks. I assumed "anyone can make a diagram" but "not everyone can refactor code this well."
So I overinvested in the wrong thing.

## What I Should Have Done

Looking back with clear eyes, here's what a proper approach would have looked like:

**Day 1 (2 hours):**

- Extract all requirements into a checklist
- Highlight the phrase "not required to code the full solution"
- Create a presentation outline FIRST
- Sketch draft diagrams showing current vs. proposed architecture
- Identify 3-4 code snippets I'd need to illustrate key points

**Day 2-3 (8 hours):**

- Implement ONLY the strategy pattern as a proof of concept
- Implement OAuth 2.0 for external clients (the thing they explicitly asked about)
- Write a few key tests to demonstrate the testing approach
- Document my decisions and trade-offs

**Day 4 (3 hours):**

- Polish diagrams
- Practice presentation out loud
- Prepare talking points for discussion
- Clean up any demo code

**Day 5 (1 hour):**

- Final review of the requirement checklist
- One more dry run of presentation

Total time: ~14 hours spread across 5 days, with plenty of buffer.

Instead, I spent 20+ hours in a coding frenzy, finishing at midnight on Sunday, exhausted and with nothing presentable.

## The Framework I Built to Prevent This

I'm sharing this publicly because I know I'm not the only one who does this. So I built a systematic framework based on
requirements analysis, best practices and cognitive bias mitigation techniques. I call it the **Requirements-First
Framework**, and it's designed to interrupt the automatic patterns that lead us astray.

---

### Phase 0: Requirements Extraction (MANDATORY - DO THIS FIRST)

**Time Box:** 30-60 minutes
**When:** Immediately upon receiving any task, before touching code
**Rule:** You cannot start implementation until this phase is complete

Create a file called `REQUIREMENTS.md` in your project:

```markdown
# Requirements Document

Date Created: [DATE]
Task: [NAME]
Review By: [DATE - set 24 hours before deadline]

## EXPLICIT REQUIREMENTS

(Things they explicitly said you must do)

### Submission Format

- [ ] Requirement 1
- [ ] Requirement 2

### Deliverables

- [ ] Deliverable 1
- [ ] Deliverable 2

### Technical Requirements

- [ ] Technical requirement 1
- [ ] Technical requirement 2

## EVALUATION CRITERIA

(How will they judge success?)

1. Criterion 1
2. Criterion 2
3. Criterion 3

## EXPLICITLY EXCLUDED

(Things they said NOT to do)

- ❌ NOT doing full implementation - they said "not required"
- ❌ NOT perfecting code - they're evaluating architecture thinking

## RED FLAGS TO AVOID

(Common mistakes for this type of task)

- ⚠️ Getting lost in implementation details
- ⚠️ Ignoring presentation requirements
- ⚠️ Focusing on impressive work that wasn't asked for
```

#### Highlight Key Trigger Phrases

Go through the requirements and highlight these indicators:

**Presentation Indicators:**

- "discuss and present"
- "come prepared to explain"
- "use diagrams"
- "whiteboarding"
- "walk through your design"

**Exclusion Indicators:**

- "not required to..."
- "instead..."
- "rather than..."
- "focus on X not Y"

**Evaluation Indicators:**

- "will be evaluated based on"
- "we're looking for"
- "demonstrate"
- "show your understanding of"

#### Answer Two Critical Questions

Before starting any work:

**Question 1: What is the PRIMARY outcome they want?**

- Not "what can I build" but "what do they need to see"?
- Not "what shows my skills" but "what proves I meet their need"

**Question 2: If I only had 25% of the time, what would I focus on?**

- This reveals the true priorities
- Everything else is optional

For my interview, my honest answers would have been:

1. They want to see my architectural thinking and communication skills, not perfect code
2. With 25% time: Diagrams + discussing trade-offs + explaining a strategy pattern with one code example

That would have saved me.

---

### Phase 1: Presentation-First Planning (Do Before Implementation)

**Time Box:** 1-2 hours
**Rule:** Plan your presentation BEFORE writing code

Create your presentation outline first:

```markdown
# Presentation Outline

## Opening (2 minutes)

- What I'll cover today
- My approach to the problem

## Section 1: Current State Critique (5 minutes)

- Diagram: Current architecture
- Problems identified (3-5 key issues)

## Section 2: Proposed Solution (8 minutes)

- Diagram: Proposed architecture
- Key improvements with rationale
- Code snippet examples (3-4 snippets max)

## Section 3: Trade-offs & Priorities (5 minutes)

- What I would do first (MVP)
- What I would do next (Phase 2)
- What I deliberately deprioritized (and why)

## Materials Needed

- [ ] Diagram 1: Current architecture
- [ ] Diagram 2: Proposed architecture
- [ ] Code snippet 1: [Description]
- [ ] Code snippet 2: [Description]
```

**Why this works:** If you plan your presentation first, you'll only build what you need for the presentation. You
literally cannot overengineer if you've already decided what you're showing.

Create your diagram *sketches* before writing any code. Even rough boxes-and-arrows on paper. This forces architectural
thinking before implementation.

---

### Phase 2: Time-Boxed Implementation

**Rule:** Every task gets a time box. When time expires, you stop.

Create a time box matrix:

```markdown
| Task                          | Time Box | Status        |
|-------------------------------|----------|---------------|
| Requirements extraction       | 1 hour   | ✅ Done       |
| Create presentation outline   | 1 hour   | ✅ Done       |
| Create diagrams (draft)       | 2 hours  | ✅ Done       |
| Prototype core pattern        | 3 hours  | 🔄 In Progress |
| Write tests                   | 2 hours  | ⏳ Planned    |
| Polish diagrams               | 1 hour   | ⏳ Planned    |
| Practice presentation         | 1 hour   | ⏳ Planned    |
| BUFFER                        | 2 hours  | ⏳ Reserved   |
```

**Time box rules:**

1. Set a timer. Literally. Use your phone.
2. When timer goes off, stop immediately. Even mid-function.
3. Assess: Did I achieve the goal?
4. If no: Do I extend (with written justification) or move on?

**The Stopping Rule:**

If you're coding and losing track of time, STOP and ask:

1. "Will this specific code be shown in my presentation?"
2. "Does this code prove a concept I need to demonstrate?"
3. "Or am I just perfecting something that doesn't matter?"

If #3, stop immediately. Save and close the file.

---

### Phase 3: Regular Check-Ins (Mandatory)

Set calendar reminders to check against requirements.

#### Daily Check-In (5 minutes)

Every day at the same time:

```markdown
## Daily Check-In: [DATE]

### What I completed today:

- [Task 1]
- [Task 2]

### Against requirement checklist:

- ✅ Met requirement: [which one]
- ❌ Haven't addressed: [which one]

### Red flags check:

- Am I building something they said NOT to do? ❌ No
- Am I ignoring evaluation criteria? ❌ No
- Am I doing "impressive" work that wasn't asked for? ⚠️ YES - STOP

### Course correction needed?

[Yes/No + What to change]
```

#### The 48-Hour Warning Check-In (30 minutes)

48 hours before deadline, do a full review:

```markdown
## 48-Hour Pre-Mortem

If I submitted what I have RIGHT NOW:

### Evaluation Criteria Scoring (1-10)

1. [Criterion 1]: [Score]/10 - Reasoning: [Why]
2. [Criterion 2]: [Score]/10 - Reasoning: [Why]

### What's Missing That's Required?

- [ ] Missing item 1 - Impact: HIGH
- [ ] Missing item 2 - Impact: MEDIUM

### What Did I Build That Wasn't Asked For?

- [Item 1] - Time spent: [X hours] - Could have spent on: [Y]

### Honest Assessment:

Would this pass? YES / NO / MAYBE
If NO or MAYBE, what's the ONE thing that would move it to YES?
```

This 48-hour check-in would have saved me. I would have seen:

- Missing: OAuth implementation (HIGH impact)
- Missing: Diagrams (HIGH impact)
- Missing: Presentation practice (MEDIUM impact)
- Not asked for: Full Laravel 12 refactoring (12 hours wasted)

---

### Phase 4: The Presentation Dry Run (Mandatory)

**Time Box:** 1-2 hours
**When:** 24 hours before delivery
**Rule:** Do this even if it feels awkward

Practice your presentation out loud:

1. Set a timer for the presentation length (20 minutes)
2. Present to an empty room or record yourself
3. Follow your outline exactly
4. Note where you struggle:
    - Where did you not have materials ready?
    - Where couldn't you explain clearly?
    - Where did you go over time?

**The "Explain to a Rubber Duck" Test:**

Before the real presentation, explain your approach in one paragraph:

"The problem they gave me was [X]. The most important thing they're evaluating is [Y]. My approach was [Z]. The key
things I need to show are [A, B, C]."

If you can't clearly articulate this, you're not ready.

---

## Emergency Protocol: When You Realize You're Off Track

### The "Oh Shit" Moment Protocol

When you realize you've been focusing on the wrong thing:

**1. STOP immediately.** Close your IDE. Step away.

**2. Emergency triage (15 minutes):**

```markdown
## Emergency Triage

TIME REMAINING: [X hours]

CURRENT STATE:

- What I've built: [description]
- What I'm missing: [description]

REQUIREMENTS REVIEW:

- What's actually required: [list]
- What I've met: [list]
- What I'm missing: [list]

SALVAGE PLAN:

- Can I repurpose what I built? [Yes/No + How]
- What must I create from scratch? [List]
- What can I skip entirely? [List]
```

**3. Execute ruthlessly:**

- Cut everything that doesn't directly address requirements
- Accept "good enough" over "perfect"
- Focus on demonstration over completion

---

## Cognitive Bias Countermeasures

Beyond the process framework, here are mental tricks that help:

### The "What Would I Advise Someone Else" Technique

When you're deep in work, ask:

**"If a friend showed me this situation, what would I tell them?"**

We're much better at seeing other people's mistakes than our own. This creates psychological distance and clarity.

### The "Future Self" Check

Set a recurring reminder:

**"Will Future Me (in the interview) be glad I spent time on this specific thing?"**

If uncertain, the answer is probably "no."

### The "Uncomfortable Truth" Journal

Keep a document: `UNCOMFORTABLE_TRUTHS.md`

```markdown
## Things I Know But Don't Want to Admit

- [ ] I'm avoiding creating diagrams because I'm not confident
- [ ] I'm overengineering code because it feels safer than prep
- [ ] I haven't practiced presenting because it makes me anxious

## Action Plan

For each truth, what's the smallest step I can take TODAY?
```

Writing down what you're avoiding makes it conscious, which makes it addressable.

---

## How This Would Have Changed My Outcome

If I follow this framework for my interview:

**Phase 0 would have caught:**

- "Not required to code full solution" ← Red flag
- "Use diagrams and whiteboarding" ← Required deliverable
- "Discuss and present ideas" ← True evaluation focus
- External client OAuth vs. internal API ← Priority clarification

**Phase 1 would have forced:**

- Creating diagram placeholders before any code
- Outlining talking points first
- Identifying exactly 3-4 code snippets to show

**Phase 2 time boxes would have prevented:**

- 12+ hours on full refactoring
- Going down http::pool rabbit holes
- Perfecting code that wouldn't be shown

**Phase 3 check-ins would have revealed:**

- Missing OAuth on Day 2 (time to fix it)
- Missing diagrams on Day 3 (time to create them)
- Over-engineering on Day 4 (time to stop)

**Phase 4 dry run would have shown:**

- I couldn't walk through my solution clearly
- I didn't have visual aids
- My presentation was incoherent

The framework catches these issues BEFORE the interview, not moments before when it's too late.

---

## Implementation: Start Small

Don't try to use the entire framework at once. Build the habit gradually:

**Week 1:** Just do Phase 0 (requirement extraction) for every task
**Week 2:** Add daily check-ins
**Week 3:** Add presentation-first planning
**Week 4:** Add dry runs for presentations
**Week 5:** Add time boxing
**Week 6:** Full framework operational

By week 6, these practices become automatic.

---

## For Your Next Technical Challenge

Here's your immediate action plan:

### 1. Create Your Template Folder

Right now, create a folder called `interview-templates` with these files:

- `REQUIREMENTS.md` (empty template)
- `TIME_BOX_MATRIX.md` (empty template)
- `DAILY_CHECKIN.md` (empty template)
- `PRESENTATION_OUTLINE.md` (empty template)

When you get your next challenge, copy this folder and fill it out.

### 2. Set Up Your Pre-Task Checklist

Print this and put it somewhere visible:

```
□ I have created REQUIREMENTS.md
□ I have identified what they're evaluating
□ I have created a presentation outline
□ I have created time boxes for all tasks
□ I have set daily check-in reminders
□ I have scheduled a 48-hour warning
□ I have scheduled a dry run
□ I have identified what I'm NOT doing

ONLY PROCEED IF ALL BOXES ARE CHECKED
```

### 3. Find an Accountability Partner

Reach out to someone you trust. Ask them:

"I'm working on breaking a bad habit where I over-engineer and miss requirements. Can you be my check-in person for my
next interview prep? I just need you to ask me three questions at the midpoint: What was required? What did you build?
Do they match?"

External accountability catches blind spots.

---

## The Uncomfortable Truth About Senior Engineers

Here's what I've learned:

- this problem gets *worse* as you get better at engineering.
- Junior developers don't have this problem because they're not confident enough to go off-script. They follow
  instructions carefully because they're not sure of themselves.
- Senior developers have the opposite problem. We're so confident in our technical skills that we think we know better
  than the requirements. We see a problem and immediately think "I know how to solve this properly" and charge ahead.
- We confuse technical competence with reading comprehension.

I've mentored developers, led teams, modernised legacy systems, and given conference talks. And I still fell into this
trap. That's not despite my experience, it's *because* of it.

The framework exists because willpower and experience aren't enough. Our brains are wired to seek comfort, avoid
ambiguity, and jump to implementation. Senior engineers need *systems* to counteract these tendencies precisely because
we're good enough to build the wrong thing very efficiently.

---

## Why I'm Sharing This

I'm publishing this post-mortem because:

1. **This is common.** You're not alone if you've done this.

2. **It's not a character flaw.** These are well-documented cognitive patterns that affect everyone, especially skilled
   people facing ambiguous tasks.

3. **It's fixable.** Not through willpower, but through systems.

4. **Someone needs to say it out loud.** We all pretend we're perfectly rational engineers who follow requirements.
   We're not. We're humans with biases, and we need frameworks to compensate.

The research shows that awareness alone doesn't fix cognitive bias. But awareness *plus* systematic countermeasures
does.

So here's my systematic countermeasure, built from my failure, tested against the research, and ready for you to use.

## Resources

If you want to understand more about the cognitive science behind this:

- [**Thinking, Fast and Slow** by Daniel Kahneman](https://www.penguin.co.uk/books/56314/thinking-fast-and-slow-by-kahneman-daniel/9780141033570) - The definitive book on cognitive bias
- [**Judgment Under Uncertainty: Heuristics and Biases**](https://sites.socsci.uci.edu/~bskyrms/bio/readings/tversky_k_heuristics_biases.pdf) - The original research paper by Kahneman and
  Tversky
- [**The Checklist Manifesto** by Atul Gawande](https://atulgawande.com/book/the-checklist-manifesto/) - How checklists prevent experts from making
  avoidable mistakes
- [**Requirements Engineering**](https://www.sciencedirect.com/topics/computer-science/requirement-engineering) - Academic literature on requirements gathering best practices

If you want templates and tools:

- I've created a [GitHub repository](https://github.com/robwilde/requirement-first-framework) with all the templates from this post 

[attentional-bias-decision-lab]: https://thedecisionlab.com/biases/attentional-bias "Why do we focus more on some things than others?"

[mere-urgency-effect]: https://academic.oup.com/jcr/article-abstract/45/3/673/4847790 "The Mere Urgency Effect"

[eisenhower-matrix]: https://www.todoist.com/productivity-methods/eisenhower-matrix "The Eisenhower Matrix"

[dunning-kruger]: https://en.wikipedia.org/wiki/Dunning%E2%80%93Kruger_effect "Dunning-Kruger effect"

[thinking-fast-slow]: https://www.penguin.co.uk/books/56314/thinking-fast-and-slow-by-kahneman-daniel/9780141033570 "Thinking, Fast and Slow by Daniel Kahneman"

[linkedin-profile]: https://linkedin.com/in/robertewilde "Robert Wilde on LinkedIn"

[github-profile]: https://github.com/robwilde "Robert Wilde on GitHub"

---

## Final Thoughts

I failed that interview. That hurt. But failing taught me something valuable:

**Being a good engineer isn't enough. You also have to solve the right problem.**

And sometimes the right problem isn't technical at all. Sometimes it's communication, presentation, and understanding
what people actually need to see.

The Requirements-First Framework is my apology to Future Me, and my gift to anyone else who's ever spent a weekend
building the wrong thing because it felt right.

You're not broken. Your brain is just being a brain.

Build systems that compensate, and you'll be fine.

---

*Have you experienced something similar? I'd love to hear your stories and what frameworks you've built to prevent it.
Connect with me on [LinkedIn][linkedin-profile] or [GitHub][github-profile].*
