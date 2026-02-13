# SwishApi .NET 10.0 Upgrade Tasks

## Overview

This document tracks the execution of the SwishApi solution upgrade from .NET 7.0 to .NET 10.0 (LTS). All projects will be upgraded simultaneously in a single atomic operation, followed by comprehensive testing.

**Progress**: 0/3 tasks complete (0%) ![0%](https://progress-bar.xyz/0)

---

## Tasks

### [ ] TASK-001: Verify prerequisites and environment
**References**: #plan:Prerequisites-Checklist, #plan:Execution-Sequence

- [ ] (1) Verify .NET 10.0 SDK is installed per #plan:Environment-Requirements
- [ ] (2) .NET 10.0 SDK version verified (Verify)
- [ ] (3) Ensure clean working tree and on correct branch (copilot/upgrade-swishapi-to-net10-again)
- [ ] (4) Working tree clean and on upgrade branch (Verify)

### [ ] TASK-002: Atomic framework and dependency upgrade
**References**: #plan:Execution-Sequence, #plan:Project-by-Project-Plans, #plan:Package-Update-Reference

- [ ] (1) Update target framework to net10.0 in both project files per #plan:Phase-1
- [ ] (2) Both projects updated to net10.0 (Verify)
- [ ] (3) Update Newtonsoft.Json package to 13.0.4 per #plan:Package-Update-Reference
- [ ] (4) Package reference updated (Verify)
- [ ] (5) Clean and restore all NuGet dependencies
- [ ] (6) All dependencies restored successfully (Verify)
- [ ] (7) Build solution and fix any compilation errors per #plan:Breaking-Changes-Catalog
- [ ] (8) Solution builds with 0 errors (Verify)

### [ ] TASK-003: Run comprehensive testing and validation
**References**: #plan:Testing-Strategy, #plan:Success-Criteria

- [ ] (1) Run SwishApiConsoleTest to verify library functionality
- [ ] (2) Console test executes successfully with no errors (Verify)
- [ ] (3) Verify no security vulnerabilities in packages
- [ ] (4) No security vulnerabilities found (Verify)
- [ ] (5) Commit all changes with message: "Upgrade solution from .NET 7.0 to .NET 10.0"

---
