# Superpowers for Amplifier

A disciplined software development workflow system ported from [obra/superpowers](https://github.com/obra/superpowers) to the Amplifier framework.

## Credits

This project is a port of **[obra/superpowers](https://github.com/obra/superpowers)** by **Jesse Vincent** to the Amplifier agent framework. The original Superpowers project provides a comprehensive software development workflow for Claude Code, emphasizing TDD, systematic debugging, and structured planning.

**Original Project:** https://github.com/obra/superpowers  
**Original Author:** Jesse Vincent ([@obra](https://github.com/obra))  
**Original License:** MIT

This port adapts the Superpowers philosophy and workflows for the Amplifier ecosystem, converting markdown-based skills into Amplifier recipes with formal approval gates and checkpoint-based resumability.

---

## What is Superpowers?

Superpowers enforces disciplined development practices:

- **Test-Driven Development** - Write tests first, always
- **Systematic Debugging** - Root cause before fixes
- **Structured Planning** - Design → Plan → Execute
- **Human Checkpoints** - Approval gates at critical points
- **Evidence Over Claims** - Verify with proof

## Installation

### Standard Usage (Recommended)

Load the superpowers bundle which includes foundation:

```bash
amplifier bundle add git+https://github.com/ramparte/superpowers@main
```

### Custom Composition

Include just the behavior in your own bundle:

```yaml
# your-bundle.md
includes:
  - bundle: git+https://github.com/microsoft/amplifier-foundation@main
  - bundle: git+https://github.com/ramparte/superpowers@main#subdirectory=behaviors/superpowers.yaml
```

## Available Recipes

| Recipe | Purpose |
|--------|---------|
| **`superpowers-full-development-cycle.yaml`** | **Complete end-to-end workflow (recommended)** |
| `brainstorming.yaml` | Refine ideas into designs with approval gate |
| `writing-plans.yaml` | Create detailed TDD implementation plans |
| `executing-plans.yaml` | Batch execution with human checkpoints |
| `subagent-development.yaml` | Fresh agent per task with two-stage review |
| `git-worktree-setup.yaml` | Create isolated workspace for feature |
| `finish-branch.yaml` | Complete branch with merge/PR options |

### Full Development Cycle (Recommended)

The `superpowers-full-development-cycle.yaml` meta-recipe composes all workflows into one end-to-end experience:

```bash
amplifier run "execute superpowers:recipes/superpowers-full-development-cycle.yaml with feature_name='user-authentication'"
```

**Flow:**
```
Brainstorming → [APPROVE] → Planning → [APPROVE] → Worktree → Implementation → [APPROVE] → Finish
```

**Approval Gates (3):**
1. After design - approve before investing in planning
2. After plan - approve before writing code  
3. After implementation - choose: merge, PR, keep, or discard

**Error Handling:** Test failures trigger bug-hunter debugging (up to 3 retries), then halt for human decision.

### Example Usage

```bash
# Start a new feature
amplifier run "execute superpowers:recipes/brainstorming.yaml with topic='user authentication'"

# Create implementation plan
amplifier run "execute superpowers:recipes/writing-plans.yaml with design_path='docs/plans/2026-01-09-auth-design.md'"

# Set up isolated workspace
amplifier run "execute superpowers:recipes/git-worktree-setup.yaml with branch_name='feature/auth'"

# Execute the plan
amplifier run "execute superpowers:recipes/executing-plans.yaml with plan_path='docs/plans/2026-01-09-auth-plan.md'"

# Finish and merge
amplifier run "execute superpowers:recipes/finish-branch.yaml"
```

## Available Skills

| Skill | Purpose |
|-------|---------|
| `test-driven-development/SKILL.md` | TDD methodology - RED-GREEN-REFACTOR |
| `systematic-debugging/SKILL.md` | 4-phase debugging framework |
| `verification-before-completion/SKILL.md` | Evidence-based completion checklist |

## The Full Development Workflow

```
1. BRAINSTORM
   └─ Recipe: brainstorming.yaml
   └─ Output: Design document
   └─ Gate: Human approves design

2. SETUP WORKSPACE  
   └─ Recipe: git-worktree-setup.yaml
   └─ Output: Isolated worktree with clean baseline

3. WRITE PLAN
   └─ Recipe: writing-plans.yaml
   └─ Output: Detailed implementation plan
   └─ Gate: Human approves plan

4. EXECUTE (choose one)
   ├─ Option A: executing-plans.yaml
   │  └─ Batch execution, human checkpoints between batches
   └─ Option B: subagent-development.yaml
      └─ Fresh agent per task, two-stage reviews

5. FINISH
   └─ Recipe: finish-branch.yaml
   └─ Output: Merged code or PR
   └─ Gate: Human chooses action
```

## Superpowers Expert Agent

The bundle includes a `superpowers-expert` agent that:

- Guides users through workflows
- Recommends appropriate recipes
- Explains philosophy and principles
- Enforces TDD and systematic practices

Invoke when users mention "superpowers" or need workflow guidance.

## Directory Structure

```
superpowers/
├── bundle.md                    # Main bundle (includes foundation)
├── behaviors/
│   └── superpowers.yaml         # Reusable behavior for composition
├── agents/
│   └── superpowers-expert.md    # Expert agent
├── context/
│   ├── philosophy.md            # Core principles
│   └── instructions.md          # Usage guide
├── recipes/
│   ├── superpowers-full-development-cycle.yaml  # Meta-recipe (recommended)
│   ├── brainstorming.yaml
│   ├── writing-plans.yaml
│   ├── executing-plans.yaml
│   ├── subagent-development.yaml
│   ├── git-worktree-setup.yaml
│   └── finish-branch.yaml
├── skills/
│   ├── test-driven-development/
│   │   └── SKILL.md
│   ├── systematic-debugging/
│   │   └── SKILL.md
│   └── verification-before-completion/
│       └── SKILL.md
└── docs/
    └── ANALYSIS_SUMMARY.md      # Background and comparison
```

## Philosophy

### The Iron Laws

1. **TDD**: No production code without a failing test first
2. **Debugging**: No fixes without root cause investigation
3. **Verification**: No claiming success without evidence

### Anti-Patterns to Avoid

- Jumping to code without understanding requirements
- Skipping tests for "simple" changes
- Multiple fixes at once instead of isolated changes
- Claiming success without verification evidence
- Rationalizing shortcuts ("just this once")

## Comparison with Original

This port adapts obra/superpowers for Amplifier:

| Feature | Original Superpowers | Amplifier Port |
|---------|---------------------|----------------|
| Format | Markdown skills loaded into context | Skills + Recipes with formal structure |
| Workflow | Implicit skill chaining | Declarative YAML recipes |
| Provider | Claude Code only | Multi-provider support |
| Resumability | Plan file tracks task status | Checkpoint-based resumption |
| Two-stage review | Spec + quality loops until approved | Preserved from original |
| Human checkpoints | Implicit ("say 'Ready'") | Formal approval gates |

The original Superpowers already has effective resumability (via task status in plan files) and two-stage review loops. This port preserves those strengths while adding multi-provider support and formal approval gates.

See `docs/ANALYSIS_SUMMARY.md` for the full comparison.

## License

MIT License - following the original superpowers project.

## Acknowledgments

Special thanks to **Jesse Vincent** ([@obra](https://github.com/obra)) for creating the original [superpowers](https://github.com/obra/superpowers) project that inspired this port. The philosophy of disciplined development through TDD, systematic debugging, and structured planning is directly derived from his work.

If you find this useful, consider [sponsoring Jesse's open source work](https://github.com/sponsors/obra).
