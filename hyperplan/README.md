# The Hyperplan Skill

This skill installs **Hyperplan**, a discipline for rigorous, adversarial planning.

## The Discipline

Hyperplan is a mental model for an AI agent that acts as an orchestrator (the "Lead"). It teaches the agent to run a multi-round debate between a team of five hostile, specialist AI personas before committing to a course of action. The goal is to expose weak assumptions and build a robust plan that has survived intense scrutiny.

The core principles are:
-   **Intellectual Combat**: The process is not about consensus. It's about subjecting a plan to ruthless cross-critique to see what survives.
-   **Specialized Hostility**: Each of the five personas (Skeptic, Validator, Researcher, Architect, Creative) has a unique "attack vector" to find flaws in the plan.
-   **Distillation Before Formalization**: The orchestrator's job is to distill the insights that survive the debate. A separate `plan` agent then formalizes these insights into an executable work plan.

## When to Use It

An agent equipped with this skill should use it for any complex, ambiguous, or high-stakes planning task. It is the go-to process for moving from a vague idea to a concrete, battle-tested work plan.

## Getting Started

Once an agent has loaded this skill, it knows how to orchestrate the Hyperplan workflow. You can trigger it by asking the agent to use this process for a planning task:

> "I need a plan for a new real-time collaboration feature. Use the Hyperplan process to make sure we cover all the angles."

For a full breakdown of the discipline, phases, and agent roles, see the [SKILL.md](./SKILL.md) file.
