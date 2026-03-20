# Roadmap: cgateweb-homeassistant

## Overview
A Home Assistant add-on for connecting to and controlling a C-Bus system via the C-Gate protocol server and MQTT.

## Current Milestone
**v0.1 Initial Release** (v0.1.0)
Status: In progress
Phases: 1 of 2 complete

## Phases

| Phase | Name | Plans | Status | Completed |
|-------|------|-------|--------|-----------|
| 1 | CNI Configuration for Managed Mode | 1/1 | Complete | 2026-03-20 |
| 2 | Managed Mode Networking & Polish | TBD | Not started | - |

## Phase Details

### Phase 1: CNI Configuration for Managed Mode
**Goal:** Allow users to specify their CNI/Wiser IP address so C-Gate in managed mode can connect to the physical C-Bus network.

**Scope:**
- Add `cni_ip` and `cni_network` config options to `config.yaml`
- Create a one-shot service that sends `net create` commands to C-Gate after startup
- Add `project.start` to C-GateConfig.txt so the project auto-starts with its networks
- Update translations for new options

**Key finding:** CNI IP is project-level data in C-Gate, not a C-GateConfig.txt property. Must be configured via C-Gate commands (`net create <network> cni <ip>`) after the server starts.

### Phase 2: Managed Mode Networking & Polish
**Goal:** Ensure managed mode works reliably end-to-end, handle edge cases, improve documentation.

**Scope:**
- `access.txt` configuration for remote access (if needed)
- Error handling for CNI connection failures
- Documentation updates for managed mode setup
- Version bump and release

---
*Roadmap created: 2026-03-20*
*Updated: 2026-03-20 — Phases defined after CNI research*
