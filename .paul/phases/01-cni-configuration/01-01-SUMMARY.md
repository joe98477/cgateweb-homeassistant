---
phase: 01-cni-configuration
plan: 01
subsystem: infra
tags: [cgate, cni, shell, s6-overlay, managed-mode]

requires: []
provides:
  - CNI/Wiser IP config option for managed mode
  - C-Gate project auto-start via project.start
  - cgate-setup s6 service for post-startup CNI configuration
affects: [02-managed-mode-networking]

tech-stack:
  added: []
  patterns: [C-Gate command protocol via netcat pipe, s6 one-shot-as-longrun pattern]

key-files:
  created: [cgateweb/rootfs/etc/services.d/cgate-setup/run]
  modified: [cgateweb/config.yaml, cgateweb/translations/en.yaml, cgateweb/rootfs/etc/services.d/cgate/run, cgateweb/rootfs/etc/cont-init.d/cgate-install.sh, cgateweb/package.json]

key-decisions:
  - "CNI IP configured via C-Gate commands (net create) after startup, not via C-GateConfig.txt"
  - "Shell-only implementation — no Node.js changes needed"
  - "netcat pipe pattern for sending C-Gate commands"

patterns-established:
  - "s6 one-shot service pattern: do work then exec sleep infinity"
  - "C-Gate command protocol interaction via piped netcat"

duration: ~15min
started: 2026-03-20T00:00:00Z
completed: 2026-03-20T00:00:00Z
---

# Phase 1 Plan 01: CNI Configuration for Managed Mode Summary

**Added cni_ip/cni_network config options and a cgate-setup s6 service that configures C-Gate's CNI connection via netcat commands after startup.**

## Performance

| Metric | Value |
|--------|-------|
| Duration | ~15min |
| Tasks | 2 completed |
| Files modified | 6 |

## Acceptance Criteria Results

| Criterion | Status | Notes |
|-----------|--------|-------|
| AC-1: CNI IP config option available | Pass | Optional string in config.yaml schema |
| AC-2: C-Gate auto-starts project | Pass | project.start added to both cgate/run and cgate-install.sh |
| AC-3: CNI interface configured after startup | Pass | cgate-setup service sends net create command |
| AC-4: Setup service idle when not needed | Pass | Sleeps when remote mode or no cni_ip |

## Accomplishments

- Added `cni_ip` and `cni_network` config options with translations
- Created `cgate-setup` s6 service that waits for C-Gate and configures CNI via command protocol
- Added `project.start` to C-GateConfig.txt so C-Gate auto-starts the project with its networks
- Version bumped to 1.5.0, tagged, and pushed to origin

## Task Commits

| Task | Commit | Type | Description |
|------|--------|------|-------------|
| Task 1 + Task 2 | `8ac665f` | feat | Add CNI/Wiser IP configuration for C-Gate managed mode |

## Files Created/Modified

| File | Change | Purpose |
|------|--------|---------|
| `cgateweb/config.yaml` | Modified | Added cni_ip, cni_network options + schema; version 1.5.0 |
| `cgateweb/package.json` | Modified | Version bump to 1.5.0 |
| `cgateweb/translations/en.yaml` | Modified | Added translations for cni_ip and cni_network |
| `cgateweb/rootfs/etc/services.d/cgate/run` | Modified | Added project.start to SETTINGS array |
| `cgateweb/rootfs/etc/cont-init.d/cgate-install.sh` | Modified | Added project.start to initial config |
| `cgateweb/rootfs/etc/services.d/cgate-setup/run` | Created | One-shot service: waits for C-Gate, sends net create CNI command |

## Decisions Made

| Decision | Rationale | Impact |
|----------|-----------|--------|
| CNI via C-Gate commands, not config file | CNI IP is project-level data, not a C-GateConfig.txt property | Requires service to send commands after startup |
| Shell-only, no Node.js changes | CNI config is infrastructure concern, not app concern | Clean separation; ConfigLoader untouched |
| netcat pipe for commands | Pattern used by homebridge-cbus and other integrations | Simple, proven approach |

## Deviations from Plan

### Summary

| Type | Count | Impact |
|------|-------|--------|
| Scope additions | 1 | Version bump (1.4.9 → 1.5.0) and package.json update — requested by user |

**Total impact:** Minor addition, no scope creep.

## Issues Encountered

None.

## Next Phase Readiness

**Ready:**
- CNI configuration functional for managed mode
- v1.5.0 tagged and pushed — HA addon updates will pick up

**Concerns:**
- netcat pipe for C-Gate commands is simple but fragile (no response parsing)
- No error handling if `net create` fails (e.g., CNI unreachable)

**Blockers:**
- None

---
*Phase: 01-cni-configuration, Plan: 01*
*Completed: 2026-03-20*
