---
name: unknowns
description: Unknowns — the gap between what the user asked for and what the work actually needs. Use when the user says unknowns or blind spot pass, asks what they are missing, wants to be interviewed or quizzed on a change, wants several design directions to react to, or hands over a task still vague enough that building it means guessing.
---
 
The prompt is a **map**. The codebase, the domain, and the real constraints are the **territory**. An **unknown** is any place the map goes quiet and the agent has to guess.
 
Guessing is the failure. Every technique below converts a guess into something the user can react to, while reacting is still cheap.
 
Ask where the user is starting from — their experience with this codebase and this domain — and pitch every explanation to that level.
 
## Route by the unknown
 
Name which kind dominates before picking a move.
 
| Kind | Sounds like | Move |
|---|---|---|
| **Known knowns** — it's in the prompt | "here's exactly what I want" | [Implementation plan](#implementation-plan) |
| **Known unknowns** — an open question they can point at | "I haven't decided how X works yet" | [Interview](#interview) |
| **Unknown knowns** — obvious to them, unwritten, recognized on sight | "I'll know it when I see it" | [Prototype](#prototype), [Reference](#reference) |
| **Unknown unknowns** — never considered, no vocabulary for it yet | "I've never touched this part" | [Blind spot pass](#blind-spot-pass) |
 
More than one kind usually applies. Run the moves in the order above — an interview asks better questions after a blind spot pass has supplied the vocabulary.
 
An unknown found mid-implementation is a signal to go back up this table, not to push through on a guess.
 
## Pre-implementation
 
### Blind spot pass
 
Read the relevant code and search externally, then report what the user does not yet know exists:
 
- Prior art already in the codebase, and why it was built that way
- Conventions and constraints this work will inherit
- The potholes people hit here, and what "good" looks like in this domain
Write it as teaching, in the user's vocabulary rather than the domain's, defining each term the domain assumes.
 
Complete when the output ends with a rewritten version of the user's original prompt — the one they would have written with this knowledge — and they accept or amend it.
 
### Prototype
 
For unknown knowns, build the cheapest artifact that provokes a reaction: one self-contained HTML file, fake data, no backend, no state wired into the real app.
 
When the shape itself is open, produce several directions where each commits to a different premise — a genuinely different layout, interaction model, or scope — so the user's pick carries information. Variations on one idea teach nothing.
 
Complete when the user has named what works and what to discard. Their reaction is the deliverable; the artifact is scaffolding.
 
### Interview
 
One question at a time, ordered by how much the answer moves the architecture. Ask about data model, interfaces, and user-facing flow before anything cosmetic. Carry each answer into the next question.
 
Complete when every remaining ambiguity could be resolved either way without changing the shape of the code. Say so explicitly and stop.
 
### Reference
 
When the user cannot describe what they want, ask for source code — a folder, a library, a component they like — over a description or a screenshot. Source carries structure and edge-case handling that prose drops. A different language is fine.
 
Read it, then state which semantics carry over and which are deliberately left behind, and get that list confirmed before building.
 
### Implementation plan
 
Lead with the decisions most likely to change: data model changes, new type interfaces, anything user-facing. Put mechanical refactoring at the bottom.
 
Complete when every decision that would be expensive to reverse appears above the mechanical section, each with the alternative that was passed over.
 
Hand the accepted plan to a fresh session along with the prototype and any notes.
 
## During implementation
 
### Implementation notes
 
Keep an `implementation-notes.md` alongside the work. When an edge case forces a departure from the plan, take the conservative option, log it under `## Deviations` with the edge case that forced it, and keep going.
 
Complete when every departure from the plan is logged with its cause — the notes are the input to the next attempt, and to the explainer.
 
## Post-implementation
 
### Explainer
 
Package the prototype, the plan, and the deviations into one document aimed at a reviewer who starts with the unknowns the user started with. Lead with the demo. Show that the failure points an expert would ask about were considered.
 
### Quiz
 
After a long session the user has less understanding of the change than the diff suggests, because most behavior lives in the existing code paths the diff touches rather than the lines it adds.
 
Write a report on the change — what was done, why, and the intuition behind it — then quiz them on it. Target the behavior the diff does not show: what happens on the paths that were called into, what breaks under the edge cases handled, what a wrong answer here would cost.
 
Complete when the user has answered every question correctly. Explain each miss and re-ask it. State plainly that the change is ready to merge only after a clean pass.
 