# Claude Code canary

[![Claude Code canary](https://github.com/osamastro7-droid/claude-code-canary/actions/workflows/canary.yml/badge.svg)](https://github.com/osamastro7-droid/claude-code-canary/actions/workflows/canary.yml)

Every day, this runs the whole test suite of the Temporal Claude Agent SDK plugin
(`python/claude_agent_sdk`, [temporalio/ai-integrations#33](https://github.com/temporalio/ai-integrations/pull/33))
with the newest [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code) release,
on Linux, macOS and Windows.

The plugin runs Claude Code as its engine. A new Claude Code release can change how hooks,
sessions or tool calls behave. So the plugin's real-engine tests run with each new release,
within a day of it shipping.

This is a personal project. It is not made by Anthropic or Temporal.

## How it works

1. **pick** (20:37 UTC every day): the newest version on npm, and the plugin commit (the pull
   request's head). It waits until all three platform packages are on npm. A version and
   plugin commit that passed before are not run again.
2. **test** (Linux, macOS, Windows): installs the plugin's locked dependencies, puts that
   Claude Code binary in place of the one the Python SDK bundles, checks `claude -v`, and
   runs the whole suite.
3. **record**: remembers a pair that passed on all three systems.

A failed run is red, and GitHub notifies the repo owner. The test report is kept with the run.

## Run it by hand

Actions, then **Claude Code canary**, then **Run workflow**. All inputs are optional: a
Claude Code version (like `2.1.288`), a ref of temporalio/ai-integrations (like
`refs/heads/main`), and **force** (run a pair that passed before).

## After the plugin is merged

In `.github/workflows/canary.yml`, set `PLUGIN_REF: refs/heads/main`.

## Good to know

- GitHub turns off scheduled workflows in a public repo after 60 days with no activity in
  the repo. If that happens, click **Enable workflow** on the Actions page.
- The canary uses the same pinned actions as the plugin's own CI.
