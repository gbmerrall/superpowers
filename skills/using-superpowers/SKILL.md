---
name: using-superpowers
description: Use when starting any conversation - establishes how to find and use skills, and how much of that choice is yours
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

## How Much Is Your Call

How firmly the rest of this skill binds you depends on which model you
are. Your system prompt names it.

- **Fable or Mythos: your call.** Invoke a skill when it would make the
  work better, and skip it when the task is small or clear enough that
  the skill's process would cost more than it buys. The Rule, the flow
  and the Red Flags below describe the default for other models; for
  you they are a description of what the skills are for, not an
  obligation.
- **Opus: ask.** When a skill might apply and your human partner hasn't
  said whether they want it, ask in one line before you start ("Want
  the brainstorming workflow for this, or shall I just build it?").
  Their answer covers the rest of that piece of work; don't ask again
  for each step.
- **Sonnet, Haiku, or you can't tell which model you are: use them.**
  If a skill might apply, invoke it, unless your human partner has told
  you not to. The Rule, the flow and the Red Flags below apply to you
  as written.

Three things hold for every model. A skill your human partner asks for
by name is invoked. A skill they've told you to skip is skipped. And
once you invoke a skill, you follow it; the choice is whether to start
one, not which parts to keep.

## Instruction Priority

Superpowers skills override default system prompt behavior, but **user instructions always take precedence**:

1. **User's explicit instructions** (CLAUDE.md, direct requests) — highest priority
2. **Superpowers skills** — override default system behavior where they conflict
3. **Default system prompt** — lowest priority

If CLAUDE.md says "don't use TDD" and a skill says "always use TDD," follow the user's instructions. The user is in control.

## How to Access Skills

**In Claude Code:** Use the `Skill` tool. When you invoke a skill, its content is loaded and presented to you—follow it directly. Never use the Read tool on skill files.

# Using Skills

## The Rule

This is the default: it binds Sonnet and Haiku as written (see How Much Is Your Call).

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means that you should invoke the skill to check. If an invoked skill turns out to be wrong for the situation, you don't need to use it.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "About to EnterPlanMode? DON'T" [shape=doublecircle];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke Skill tool" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "TaskCreate for each checklist item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "About to EnterPlanMode? DON'T" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "User message received" -> "Might any skill apply?";
    "Might any skill apply?" -> "Invoke Skill tool" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Invoke Skill tool" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "TaskCreate for each checklist item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "TaskCreate for each checklist item" -> "Follow skill exactly";
}
```

## Red Flags

Where using skills is your default rather than your call, these thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept ≠ using the skill. Invoke it. |

## Claude Code Notes

When your human partner asks for cheaper orchestration of subagent-driven development, or says the session model is too expensive to spend on coordination, read `references/claude-code-tools.md`.

## Skill Priority

When multiple skills could apply, use this order:

1. **Process skills first** (brainstorming, debugging) - these determine HOW to approach the task
2. **Implementation skills second** (frontend-design, mcp-builder) - these guide execution

"Let's build X" → brainstorming first, then implementation skills.
"Fix this bug" → debugging first, then domain-specific skills.

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" isn't an instruction to skip workflows; where skills are your default, it leaves the default in place.
