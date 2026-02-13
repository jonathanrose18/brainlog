## KISS — Keep It Simple, Stupid

* **Description:** Documentation should be short, clear, and easy to scan.
* **Why:** Simple docs are read, understood, and maintained more often.
* **Do:** Use short sentences, clear headings, and diagrams where they explain faster than text.
* **Example:** Replace a long setup paragraph with a `Quick Start` section and a small architecture diagram.

## DRY — Don’t Repeat Yourself

* **Description:** Every piece of documentation knowledge should exist in exactly one canonical place.
* **Why:** Duplicate explanations drift over time and create contradictions.
* **Do:** Link to the source section instead of copying text into multiple files.
* **Example:** Keep API field definitions in OpenAPI and reference them from guides.

## SRD — Single Responsibility per Document

* **Description:** Each document should have one primary purpose.
* **Why:** Mixing architecture, operations, and tutorials in one file confuses readers.
* **Do:** Split by intent, for example: `architecture.md`, `runbook.md`, and `how-to.md`.
* **Example:** A deployment runbook should not also explain domain modeling decisions.

## SSOT — Single Source of Truth

* **Description:** Documentation should live with the code and be versioned together.
* **Why:** Versioned docs stay aligned with releases, branches, and changes.
* **Do:** Store docs in the repository and update them in the same pull request as code changes.
* **Example:** Update `/docs/authentication.md` in the same commit that changes auth middleware behavior.

## EYODF — Eat Your Own Dog Food

* **Description:** The team should rely on its own documentation for onboarding and daily work.
* **Why:** Real usage quickly reveals gaps, ambiguity, and outdated steps.
* **Do:** Follow docs exactly during onboarding, incidents, and handovers; fix issues immediately.
* **Example:** New engineers set up the project using only the README and log every missing step.

## BSR — Boy Scout Rule

* **Description:** Leave documentation a little better every time you touch related code.
* **Why:** Small continuous improvements prevent documentation debt.
* **Do:** Correct one outdated line, add one clarification, or improve one example in every relevant PR.
* **Example:** While changing a config flag, also update the docs with defaults and a usage snippet.

## Summary

Good software documentation is concise, unambiguous, versioned with code, and actively used.
