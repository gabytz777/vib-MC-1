# AGENTS.md

## Project Overview

This repository contains vib-MC, a Java-based Minecraft server project with Mineflayer integration tests.

Before making changes, inspect the relevant source code, existing tests, build configuration, and project conventions. Understand the existing implementation before making assumptions.

## 1. Mandatory Testing Rules

**Tests must be run after every logical fix.**

### After every logical fix

- Immediately run the relevant Mineflayer integration tests after completing each logical fix.
- If a fix affects multiple features, test each affected feature.
- Investigate test failures before moving on.
- Add or update regression tests when practical.
- Do not accumulate unrelated fixes before testing them.

### Before every code commit

Run the full verification process:

1. Run every applicable Mineflayer test script.
2. Run `./gradlew test`.
3. Run `./gradlew build`.
4. Review the final diff for regressions, security issues, accidental files, and unintended changes.

Do not commit code with known test failures or unverified changes unless the situation has been explicitly reviewed and the limitation is clearly communicated.

### Mineflayer test commands

Mineflayer integration tests are located in `tools/`. Run these commands from that directory.

Available scripts include:

- `npm test`
- `npm run test:players`
- `npm run test:dimensions`
- `npm run test:chat`
- `npm run test:end`
- `npm run test:kick`
- `npm run test:player-persistence`
- `npm run test:spawn-flight`
- `npm run test:survival`
- `npm run test:versions`
- `npm run test:exploits`

Inspect `tools/package.json` to confirm the current scripts and their behavior.

**Important:** `npm test` alone does not mean the entire Mineflayer suite has passed. Run every applicable test script before a code commit.

### Test failures and unavailable environments

- Never claim a test passed unless it actually ran and passed.
- Never ignore a failing test or weaken tests just to make them pass.
- If a test fails, investigate the cause, fix the underlying issue when appropriate, and rerun the relevant tests.
- Start or configure a required Minecraft server when possible.
- If a test cannot run because of missing dependencies, environment limitations, server availability, or another blocker, explain the exact reason.
- Clearly report which checks passed, failed, or remain unverified.
- Never describe unverified changes as fully tested.

If verification fails or cannot be completed, stop and report the blocker before proceeding with a commit or pull request.

## 2. Understand Before Changing

Before modifying code:

- Inspect the relevant implementation and its callers.
- Review related tests and existing behavior.
- Understand protocol, version, threading, networking, and lifecycle implications where relevant.
- Reuse existing utilities and established patterns.
- Identify potentially affected features.

Avoid speculative changes unrelated to the task.

## 3. Code Quality

- Make the smallest clean change that fully solves the problem.
- Follow existing naming, formatting, architecture, and coding conventions.
- Avoid unnecessary refactoring while fixing a specific bug.
- Avoid duplicating logic when a suitable shared implementation exists.
- Remove unused imports, variables, and temporary debugging code introduced by your changes.
- Do not leave placeholder implementations in place of working solutions.
- Handle exceptions and failure conditions deliberately.
- Do not silently swallow exceptions or errors.
- Avoid unnecessary dependencies.
- Preserve compatibility with the project's Java target and build configuration.

## 4. Minecraft and Mineflayer Compatibility

Minecraft protocol behavior varies by version. Compatibility is a requirement.

- Understand which Minecraft versions are supported before changing protocol-sensitive code.
- Preserve existing version compatibility unless the task explicitly requires otherwise.
- Take particular care with packets, entity metadata, player visibility, dimensions, chat, movement, spawning, survival mechanics, and connection handling.
- Do not assume behavior in one Minecraft version applies to every supported version.
- Update compatibility tests when changing version-dependent behavior.
- Validate real client behavior using the existing Mineflayer setup whenever possible.
- Do not fix one version by breaking another.

## 5. Regression Prevention

For every bug fix:

1. Identify the underlying cause.
2. Add or update a regression test when practical.
3. Implement the smallest appropriate fix.
4. Immediately run relevant Mineflayer tests.
5. Test related behavior that may have been affected.
6. Review the diff for unintended side effects.

Do not delete, skip, or weaken existing tests merely because they expose a problem.

## 6. Security and Reliability

- Treat network packets and client-provided data as untrusted input.
- Validate input at appropriate boundaries.
- Avoid crashes, resource leaks, deadlocks, and unbounded resource consumption.
- Consider malformed packets, unexpected disconnects, invalid states, and repeated requests where relevant.
- Never expose secrets, credentials, tokens, or sensitive environment information in source code or logs.
- Do not disable security checks to make a test pass.
- Keep error handling informative without leaking sensitive information.

## 7. Git, Commits, and Pull Requests

- Follow the repository's existing branch, commit, and pull request conventions.
- Make focused commits with clear messages.
- Do not include unrelated changes in a commit.
- Never overwrite unrelated user changes.
- Review the diff before committing.
- Run all required verification before each code commit.
- When opening a pull request, include a clear summary, relevant test results, and any known limitations.
- Never claim changes were committed, pushed, or submitted in a pull request unless that actually happened.

## 8. Efficient Workflow

Follow this workflow for each task:

1. Inspect the repository and understand the requested change.
2. Identify affected components and relevant tests.
3. Implement one logical fix at a time.
4. Immediately run the relevant Mineflayer tests after each logical fix.
5. Investigate failures and repeat targeted testing as needed.
6. Review the overall diff for regressions.
7. Before each code commit, run the complete applicable Mineflayer suite and Java checks.
8. Report actual test results and remaining limitations.

Do not waste time rerunning unrelated tests after every tiny edit. However, never postpone relevant tests for a completed logical fix or skip full verification before a code commit.

## 9. Definition of Done

A task is complete when:

- The requested behavior is implemented.
- Changes follow existing project conventions.
- Regression tests have been added or updated where practical.
- Relevant Mineflayer tests have been run after every logical fix.
- Full applicable Mineflayer and Java verification has been completed before each code commit.
- The final diff contains no unintended changes.
- Remaining limitations and unverified behavior are clearly documented.
- Commit and pull request descriptions accurately represent the changes and test results.
