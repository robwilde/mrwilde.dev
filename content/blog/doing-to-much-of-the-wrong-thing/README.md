# Requirements-First Framework

A systematic approach to preventing over-engineering and scope creep in technical challenges, interviews, and projects.

## Overview

The Requirements-First Framework is a structured methodology designed to help engineers—especially senior developers—avoid the common pitfall of building impressive solutions to the wrong problem. Born from a failed technical interview, this framework addresses cognitive biases that lead skilled engineers to over-engineer, misread requirements, and focus on code over communication.

**Read the full story:** [How I Failed a Technical Interview by Doing Too Much (And Built a Framework to Never Do It Again)](https://mrwilde.dev/blog/2025/10/17/how-i-failed-a-technical-interview-by-doing-too-much-and-built-a-framework-to-never-do-it-again/)

## The Problem

Senior engineers often fall into these traps:

- **Attentional Bias**: Focusing on comfortable technical implementation while filtering out presentation requirements
- **Mere Urgency Effect**: Prioritizing tasks that feel urgent over what's actually important
- **Activity Bias**: Choosing immediate coding satisfaction over strategic planning
- **Present Bias**: Seeking immediate rewards of working code over delayed rewards of good preparation
- **Competence Trap**: Defaulting to what we're best at (coding) even when it's not what's needed

The result? Spending 20+ hours building the wrong thing while ignoring what was actually asked for.

## The Solution

A four-phase framework with mandatory checkpoints to interrupt automatic patterns that lead engineers astray.

### Phase 0: Requirements Extraction (MANDATORY)

**Time Box:** 30-60 minutes
**When:** Immediately upon receiving any task, before touching code
**Rule:** You cannot start implementation until this phase is complete

- Create `REQUIREMENTS.md` documenting:
  - Explicit requirements (what they said you must do)
  - Evaluation criteria (how success will be judged)
  - Explicitly excluded items (what they said NOT to do)
  - Red flags to avoid (common mistakes)
- Highlight trigger phrases:
  - Presentation indicators: "discuss and present", "walk through your design"
  - Exclusion indicators: "not required to...", "instead...", "rather than..."
  - Evaluation indicators: "will be evaluated based on", "we're looking for"
- Answer two critical questions:
  1. What is the PRIMARY outcome they want?
  2. If I only had 25% of the time, what would I focus on?

### Phase 1: Presentation-First Planning

**Time Box:** 1-2 hours
**Rule:** Plan your presentation BEFORE writing code

- Create presentation outline first (opening, current state, proposed solution, trade-offs)
- Identify required materials (diagrams, code snippets)
- Create diagram sketches before any implementation
- **Why this works**: If you plan your presentation first, you'll only build what you need for the presentation

### Phase 2: Time-Boxed Implementation

**Rule:** Every task gets a time box. When time expires, you stop.

- Create a time box matrix for all tasks
- Set actual timers (use your phone)
- When timer expires, assess and decide: extend with justification or move on
- Apply The Stopping Rule:
  1. Will this code be shown in my presentation?
  2. Does this prove a concept I need to demonstrate?
  3. Or am I just perfecting something that doesn't matter?

### Phase 3: Regular Check-Ins (Mandatory)

**Daily Check-In (5 minutes):**
- What I completed today
- Progress against requirement checklist
- Red flags check (building what they said NOT to do?)
- Course correction needed?

**48-Hour Warning Check-In (30 minutes):**
- Full pre-mortem: "If I submitted right now..."
- Score against evaluation criteria
- Identify what's missing that's required
- Identify what you built that wasn't asked for
- Honest assessment: Would this pass?

### Phase 4: Presentation Dry Run (Mandatory)

**Time Box:** 1-2 hours
**When:** 24 hours before delivery

- Set timer for presentation length
- Present to empty room or record yourself
- Follow outline exactly
- Note where you struggle
- Apply "Explain to a Rubber Duck" test

## Quick Start

### 1. Create Your Template Folder

Create a folder called `interview-templates` with these template files:
- `REQUIREMENTS.md`
- `TIME_BOX_MATRIX.md`
- `DAILY_CHECKIN.md`
- `PRESENTATION_OUTLINE.md`

When you get your next challenge, copy this folder and fill it out.

### 2. Set Up Pre-Task Checklist

Before starting ANY implementation:

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

### 3. Build the Habit Gradually

Don't try to use the entire framework at once:

- **Week 1:** Just do Phase 0 (requirement extraction) for every task
- **Week 2:** Add daily check-ins
- **Week 3:** Add presentation-first planning
- **Week 4:** Add dry runs for presentations
- **Week 5:** Add time boxing
- **Week 6:** Full framework operational

## Emergency Protocol

### When You Realize You're Off Track

**1. STOP immediately.** Close your IDE. Step away.

**2. Emergency triage (15 minutes):**
- Current state: What I've built vs. what I'm missing
- Requirements review: What's required vs. what I've met
- Salvage plan: What can I repurpose, create from scratch, or skip

**3. Execute ruthlessly:**
- Cut everything that doesn't directly address requirements
- Accept "good enough" over "perfect"
- Focus on demonstration over completion

## Cognitive Bias Countermeasures

### Mental Tricks That Help

1. **"What Would I Advise Someone Else"**: Ask what you'd tell a friend in the same situation
2. **"Future Self" Check**: Will Future Me (in the interview) be glad I spent time on this?
3. **"Uncomfortable Truth" Journal**: Write down what you're avoiding to make it addressable

## Templates

All templates are available in the `/templates` directory:

- [Requirements Document Template](templates/REQUIREMENTS.md)
- [Time Box Matrix Template](templates/TIME_BOX_MATRIX.md)
- [Daily Check-In Template](templates/DAILY_CHECKIN.md)
- [Presentation Outline Template](templates/PRESENTATION_OUTLINE.md)
- [48-Hour Warning Template](templates/48_HOUR_WARNING.md)
- [Emergency Triage Template](templates/EMERGENCY_TRIAGE.md)

## Who This Is For

This framework is especially valuable for:

- Senior engineers preparing for technical interviews
- Developers who tend to over-engineer solutions
- Anyone who has ever spent a weekend building the wrong thing
- Technical leads who need to balance implementation with communication
- Engineers who struggle with presentation and architectural discussions

## The Uncomfortable Truth

This problem gets **worse** as you get better at engineering:

- Junior developers follow instructions carefully because they're uncertain
- Senior developers think they know better than requirements and charge ahead
- We confuse technical competence with reading comprehension
- Our brains are wired to seek comfort and jump to implementation
- Senior engineers need **systems** to counteract these tendencies

## Research Background

This framework is built on well-documented cognitive science research:

- **Attentional Bias**: Our perception is affected by our current train of thought
- **Mere Urgency Effect**: People prioritize perceived time-sensitive tasks over objectively important ones
- **Activity Bias**: Immediate satisfaction outweighs strategic thinking
- **Dunning-Kruger Effect**: Experts underestimate their abilities at unfamiliar tasks

Awareness alone doesn't fix cognitive bias. But awareness **plus** systematic countermeasures does.

## Resources

Further reading on the cognitive science behind this framework:

- **Thinking, Fast and Slow** by Daniel Kahneman - The definitive book on cognitive bias
- **Judgment Under Uncertainty: Heuristics and Biases** - The original research by Kahneman and Tversky
- **The Checklist Manifesto** by Atul Gawande - How checklists prevent expert mistakes
- **Requirements Engineering** - Academic literature on requirements gathering

## Contributing

Have you experienced similar issues? Built your own framework? We'd love to hear about it!

1. Fork this repository
2. Share your own templates or modifications
3. Submit a pull request with improvements
4. Share your story in the discussions

## Author

Created by [Robert Wilde](https://linkedin.com/in/robertewilde) after a painful but educational technical interview failure.

Connect:
- [LinkedIn](https://linkedin.com/in/robertewilde)
- [GitHub](https://github.com/robwilde)

## License

This framework and all templates are released under the MIT License. Use them, modify them, share them.

## Final Thoughts

You're not broken. Your brain is just being a brain.

Build systems that compensate, and you'll be fine.

---

*Being a good engineer isn't enough. You also have to solve the right problem.*
