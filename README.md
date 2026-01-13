# Spec-Driven AI (Workflow for Claude Code)

> **A markdown-based state management system to prevent "Context Rot" in large LLM-generated projects.**

[![Claude Code](https://img.shields.io/badge/Made%20for-Claude%20Code-f0462c)](https://anthropic.com/claude)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 🛑 The Problem: "Context Rot"

When building large projects with AI coding assistants (like Claude Code), you typically hit a wall after 3-4 days of development:

1.  **Context Loss:** The LLM forgets *why* a file was created or what the architectural goal was.
2.  **Looping:** It starts rewriting the same code because it lost the execution plan.
3.  **Ghost Code:** You end up with orphaned files and don't know if they are still needed.
4.  **Traditional SDLC:** In traditional SDLC, there is no defined workflow for development with an AI coding assistant, meaning you just prompt the requirement vaguely and get the task done but if a bug or change request comes, then AI has no idea why this was developed or the initial requirement of the feature. 

![Standard SDLC - The Problem](BE%20followed%20SDLC.png)

**The cause?** Chat sessions are ephemeral. Your project state is not.

## ⚡ The Solution: Spec-Driven State Management

This repository implements a **Spec-Driven Workflow** that forces Claude to track its own state using markdown files directly in your project. It moves every feature through three strict phases:

1.  **Planning (`.claude/specs/plans/`)**: High-level architectural decisions and "Why we are building this."
2.  **In-Progress (`.claude/specs/in-progress/`)**: The active "Context Anchor" for the current session.
3.  **Executed (`.claude/specs/plans-executed/`)**: A permanent audit trail. **6 months later**, you can read exactly *why* code was written.

> **Why this works:** Markdown files are "context-cheap" for LLMs to read, whereas re-reading your entire codebase to guess the state is "context-expensive" and error-prone.

![Spec-Driven SDLC - The Solution](BE%20proposed%20SDLC%20with%20claude.png)

---

## 🚀 How to Use (The Protocol)
This isn't just a concept—it's a strict workflow. The diagram below outlines the exact steps and CLI commands (like `/spec`, `java-code-review`) used to enforce quality.

![Detailed Workflow Steps](generic-workflow.png)

### Step 1: Install the Rules
Copy `CLAUDE.md` to your project root. Create the folder structure:
```bash
mkdir -p .claude/specs/{plans,in-progress,plans-executed}
```

### Step 2: Generate a Spec
Use the provided prompts or the GPTs to create your first feature spec.
* Provide all the required inputs for generating the architect specification and then the development specification
* Save the development specification to `.claude/specs/user-api.md`.

### Step 3: Execute with State
Ask Claude:
> *"Read the spec in `.claude/specs/user-api.md` and create an execution plan."*

Claude will now:
1.  Create a plan in `.claude/specs/plans/`.
2.  Move items to `.claude/specs/in-progress/` as it works.
3.  Move the final log to `.claude/specs/plans-executed/` when done.

---

## 📂 Free Resources in this Repo
We have open-sourced the prompt engineering files so you can implement this workflow manually.

### 1. Spec Generators (Prompt Engineering)
* **[`prompt/architect-spec-generator-prompt.md`](./prompt/architect-spec-generator-prompt.md)**: Generates high-level system design (API Contracts, Security, Data Flow) or use my custom GPT [Architect API Specification Generator](https://chatgpt.com/g/g-69464d55b1608191887671e189c7ef24-architect-api-specification-generator) for generating the high-level specification.
* **[`prompt/development-spec-generator-prompt.md`](./prompt/development-spec-generator-prompt.md)**: Converts the architect spec into actionable, step-by-step coding tasks or use my custom GPT [Development Specification Generator](https://chatgpt.com/g/g-6946691f808081919ae448904de05cbd-development-specification-generator) for generating the development specification.

---

## 🛠 The Spec-Driven Toolkit (Optional Automation)

While you can use the methodology above manually, we have built a **CLI Toolkit** to automate the friction.

**The Full Toolkit ($49) includes:**

* ⚡ **Session Commands:** `/session-start`, `/session-update`, `/session-end` (auto-generates git summaries and todo lists).
* 🤖 **Auto-Reviewers:** The full suite of Agents (`java-code-reviewer`, `api-test-reviewer`) configured to auto-activate on file save.
* 🧪 **Test Generator:** The complete `spring-boot-unit-test` skill + 6 reference guides for different testing scenarios.
* 🛠 **Git Helpers:** Conventional commit generators and branch managers.

[**Get the Full Toolkit on Gumroad →**](https://hathwar.gumroad.com/l/spec-driven-ai)

---

## License
The files in this repository are available under the MIT License.
