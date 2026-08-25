# QUG: Questifying Uncertainty Game

QUG is an agent prompt for running short, game-structured roleplay sessions around unresolved uncertainty. It translates a recent stuck situation into a playable analogue, lets the player enact bounded choices, and returns to one manageable action without requiring the larger uncertainty to be resolved first.

This repository contains the QUG specification, a prompt-agent package for [`loglm`](https://github.com/ks91/loglm), and language-specific agent definitions for [`discord-agent-hub`](https://github.com/ks91/discord-agent-hub).

## Repository Contents

- `sg-qug-agent-master.md` - non-runtime master specification and research design source of truth. Edit this file first. It is intentionally disabled.
- `AGENT_INSTALL.md` - shared QUG prompt installed into coding-agent projects by `loglm`.
- `sg-qug-agent-en.md` - English runtime with Normal and Demo modes. Use this for the JCSG conference demonstration.
- `sg-qug-agent-ja.md` - Japanese runtime with Normal and Demo modes.
- `check_qug_runtime_sync.py` - checks source hashes, required mechanics, language separation, and runtime metadata.
- `LICENSE` - project license.

New changes should flow from the master specification to both runtime files.

## Interaction Architecture

Both language runtimes implement the same four-part loop:

1. **Extract** a provisional action-blocking structure from only what the participant stated.
2. **Transform** the representational surface while preserving the established relational pressure.
3. **Enact** bounded choices through causal quest-world play.
4. **Return** to one action unit that can be completed without resolving the larger uncertainty.

Both runtimes include Normal Mode and a 5-8 minute Demo Mode. Their mechanics, safety boundaries, state reconstruction, choice design, and Personal/Sample separation should remain equivalent.

## Runtime Strategy

QUG has one design source and two runtime instantiations:

```text
QUG Master Specification
        |
        +-- QUG English Runtime
        +-- QUG Japanese Runtime
```

The master retains the complete bilingual design rationale and state rules. Runtime prompts fix the output language and omit `SESSION_LANGUAGE`, removing language detection and switching as failure modes. `ACTIVE_MODE`, `ENTRY_TYPE`, and `CURRENT_STAGE` remain prompt-level session variables reconstructed silently from conversation history; they are not persisted by `discord-agent-hub`.

## Editing Workflow

1. Update `sg-qug-agent-master.md` first.
2. Apply the same behavioral change to `AGENT_INSTALL.md` and both language runtimes.
3. Update all three files' `source-sha256` markers to the current master SHA-256.
4. Run `python3 check_qug_runtime_sync.py`.
5. Test the loglm-installed prompt and the English and Japanese Normal/Demo paths before deployment.

The checker deliberately fails after any master edit until the install prompt and both runtime source markers are refreshed. It also rejects `SESSION_LANGUAGE` in a runtime and Japanese characters in the English runtime. It is a drift guard, not a substitute for behavioral testing.

The install prompt and each runtime store their `source-sha256` marker in an HTML comment near the top of the file. The marker is visible in GitHub's Code or Raw view but hidden in rendered Markdown.

## Install with loglm

From the project where you want to use QUG, install this repository as a prompt agent:

```bash
loglm agent install ringo8991b/QUG
```

For local development, install directly from a neighboring checkout:

```bash
loglm agent install ../QUG
```

`loglm` reads `AGENT_INSTALL.md`, writes the installed prompt into the target project, and adds its managed reference to the coding agent's instruction file. After the new agent context opens, send a short cue such as `QUGを始めよう。` to begin.

## Importing Into Discord Agent Hub

Import the desired runtime with the `/agent-import` slash command:

- Conference English agent: `sg-qug-agent-en.md`
- Japanese testing agent: `sg-qug-agent-ja.md`

After import, start a thread using `/chat` with the corresponding agent ID:

- `sg-qug-agent-en`
- `sg-qug-agent-ja`

When updating an existing agent, use `/agent-import` with `overwrite:true`. Do not import `sg-qug-agent-master.md`; its metadata has `enabled: false` because it is the specification rather than a deployment prompt.

## Design Principles

QUG should:

- Start from a recent concrete situation rather than an abstract life goal.
- Avoid inventing uncertainty, perfectionism, fear, or hidden motives not stated by the participant.
- Preserve the established pressure structure while changing the setting, roles, objects, and visible task.
- Use qualitatively different choices with local consequences and causal continuation.
- Keep Personal and Sample entry states separate throughout Return and Completion.
- Return to ordinary language before confirming one bounded side quest.

QUG should not:

- Act as therapy, counseling, crisis support, or high-stakes professional advice.
- Treat later agreement with an AI interpretation as proof that the interpretation existed in the original account.
- Copy the real-world decision into fantasy labels without meaningful transformation.
- Present a prepared sample as the participant's own situation.
- Promise behavioral change, distress reduction, or correct decisions.

## Status

QUG is an exploratory interaction-design prototype. The English runtime is intended for the JCSG conference demo, while both runtimes support continued formative testing of the same language-independent interaction architecture.
