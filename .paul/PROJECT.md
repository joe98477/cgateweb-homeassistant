# Project: cgateweb-homeassistant

## Description
A Home Assistant add-on for connecting to and controlling a C-Bus system via C-Gate and MQTT.

## Core Value
Home Assistant users can seamlessly control and monitor their C-Bus lighting and automation devices without manual configuration.

## Requirements

### Validated
- CNI/Wiser IP configuration for managed mode — Phase 1

### Must Have
- Managed mode works end-to-end (CNI connection, device control)
- Error handling for CNI connection failures

### Should Have
- Documentation for managed mode setup

### Nice to Have
- access.txt configuration for remote C-Gate access

## Constraints
- C-Gate only accepts `-c` (config) and `-s` (servermode) CLI args
- CNI IP is project-level data in C-Gate, configured via commands not config file
- netcat-openbsd used for C-Gate command protocol interaction

## Key Decisions

| Decision | Phase | Rationale |
|----------|-------|-----------|
| CNI via C-Gate commands after startup | Phase 1 | CNI IP is project-level, not C-GateConfig.txt property |
| Shell-only CNI implementation | Phase 1 | Infrastructure concern, not app-level |
| netcat pipe for commands | Phase 1 | Proven pattern from other C-Gate integrations |

## Success Criteria
- Home Assistant users can seamlessly control and monitor their C-Bus lighting and automation devices without manual configuration
- Managed mode connects to user's CNI/Wiser device at configured IP

---
*Created: 2026-03-20*
*Last updated: 2026-03-20 after Phase 1*
