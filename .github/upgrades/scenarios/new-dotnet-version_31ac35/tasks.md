# SwishApi .NET 10.0 Upgrade Tasks

## Overview

This document tracks the execution of the SwishApi solution upgrade from .NET 7.0 to .NET 10.0 LTS. All components will be upgraded simultaneously in a single atomic operation using the All-At-Once strategy.

**Progress**: 0/5 tasks complete (0%) ![0%](https://progress-bar.xyz/0)

---

## Tasks

### [ ] TASK-001: Verify prerequisites
**References**: #plan:Phase-0-Prerequisites

- [ ] (1) Verify .NET 10 SDK is installed per #plan:Phase-0-Prerequisites
- [ ] (2) .NET 10.0.x SDK present and active (Verify)
- [ ] (3) Check for global.json SDK version constraints (if present)
- [ ] (4) No SDK version conflicts (Verify)

### [ ] TASK-002: Atomic framework and dependency upgrade
**References**: #plan:Phase-1-Atomic-Upgrade, #plan:Project-by-Project-Details, #plan:Package-Update-Reference

- [ ] (1) Update TargetFramework to net10.0 in both project files per #plan:Phase-1-Atomic-Upgrade
- [ ] (2) Both projects updated to net10.0 (Verify)
- [ ] (3) Update Newtonsoft.Json package to version 13.0.4 per #plan:Package-Update-Reference
- [ ] (4) Package reference updated (Verify)
- [ ] (5) Restore all NuGet dependencies for the solution
- [ ] (6) All dependencies restored successfully (Verify)
- [ ] (7) Build solution and fix any compilation errors per #plan:Breaking-Changes-Catalog
- [ ] (8) Solution builds with 0 errors (Verify)

### [ ] TASK-003: Run console test application and validate functionality
**References**: #plan:Phase-2-Test-Validation, #plan:Testing-Strategy

- [ ] (1) Run SwishApiConsoleTest application per #plan:Phase-2-Test-Validation
- [ ] (2) Fix any runtime errors or test failures
- [ ] (3) Re-run application after fixes
- [ ] (4) Application runs without errors and all test scenarios pass (Verify)

### [ ] TASK-004: Final validation and quality checks
**References**: #plan:Phase-3-Final-Validation, #plan:Success-Criteria

- [ ] (1) Perform clean build of solution per #plan:Phase-3-Final-Validation
- [ ] (2) Solution builds cleanly with 0 errors and 0 warnings (Verify)
- [ ] (3) Run package security audit
- [ ] (4) No security vulnerabilities present (Verify)
- [ ] (5) Verify all success criteria met per #plan:Success-Criteria
- [ ] (6) All criteria satisfied (Verify)

### [ ] TASK-005: Commit all changes
**References**: #plan:Source-Control-Strategy

- [ ] (1) Commit all changes with message: "feat: Upgrade solution to .NET 10.0

- Update TargetFramework from net7.0 to net10.0 for both projects
- Upgrade Newtonsoft.Json from 13.0.3 to 13.0.4 in SwishApi
- Verify all builds and tests pass on .NET 10.0

BREAKING CHANGE: Requires .NET 10.0 SDK or higher"

---
