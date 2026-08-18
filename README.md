# Gym Skill

A Claude Code skill that plans real strength-training programs and pushes
them straight to your Suunto watch. It adapts on two axes: before training,
your recovery data (HRV, sleep) gates how hard today should be; after
training, what actually happened — weights and reps hit, effort (RPE), PRs,
even lap-inferred completion from the watch — decides whether next week
progresses, holds, or deloads.

Not a generic workout generator — it writes real progressive-overload
programming (specific exercises, weights, sets, reps, and a progression rule
per lift), gates intensity against your HRV/sleep, and keeps training history
unified with your broader health record via
[health-skill](https://github.com/googlarz/health-skill).

## What it needs

- [suunto-mcp](https://github.com/googlarz/suunto-mcp) — connects your Suunto
  watch to Claude, and provides `push_workout_guide` (puts your plan on your
  wrist) and `get_recovery`/`get_sleep` (the recovery-gating data)
- [health-skill](https://github.com/googlarz/health-skill) — the person
  workspace this skill stores its data in, so training stays unified with
  injuries, conditions, and the rest of your health record

## Install

```bash
git clone https://github.com/googlarz/gym-skill.git ~/.claude/skills/gym
```

Then in Claude Code:

```
/gym setup
```

## Commands

| Command | What it does |
|---------|-------------|
| `/gym setup` | Interviews you once (goal, equipment, experience, injuries not already in health-skill), writes a real Upper/Mid/Legs program |
| `/gym plan` | Refreshes the week and pushes all three sessions to your watch |
| `/gym today` | Checks your recovery, gates intensity if it's poor, shows today's session |
| `/gym log` | Logs what happened including RPE, detects PRs, flags recurring pain to health-skill instead of guessing at it |
| `/gym review` | Weekly progression check — which lifts are moving, which stalled, next week's adjustment |

## Dashboard

[`docs/dashboard-demo.html`](docs/dashboard-demo.html) is a demo of what a
`/gym dashboard` command would show once real data exists — this week's
sessions with deviations and reasons, eight-week progression per lift,
bodyweight and active constraints from health-skill, and the daily
recovery-gate decision. Sample data throughout, clearly marked; download and
open it locally to view. Not yet a real command — needs `/gym setup` to have
actually run first.

## Watch sync

Plan → watch is a real, working loop through the SuuntoPlus Guide API — see
suunto-mcp's `push_workout_guide` docs for exactly how, and its one honest
limitation (no live push; delivery rides your phone's normal Suunto sync).
Watch → Claude comes back via lap timestamps in the synced workout, which
`/gym log` cross-checks against what was planned.

## Boundaries

Training programming only. Persistent pain, injury, or anything medical
routes to health-skill's triage — this skill never plays physio.

## License

MIT
