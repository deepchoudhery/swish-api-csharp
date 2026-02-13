# SwishApi .NET 10.0 Upgrade Tasks

## Overview

This document tracks the execution of the SwishApi solution upgrade from .NET 7.0 to .NET 10.0. Both projects will be upgraded simultaneously using the All-At-Once strategy, combining project updates, dependency restoration, and compilation in a single coordinated operation.

**Progress**: 0/3 tasks complete (0%) ![0%](https://progress-bar.xyz/0)

---

## Tasks

### [ ] TASK-001: Atomic framework and dependency upgrade
**References**: #plan:Phase-1, #plan:Phase-2, #plan:Phase-3, #plan:Project-by-Project-Plans

- [ ] (1) Update target framework to net10.0 in both project files per #plan:1.1 and #plan:2.1
- [ ] (2) All project files updated to net10.0 (Verify)
- [ ] (3) Update Newtonsoft.Json to 13.0.4 in SwishApi.csproj per #plan:1.2
- [ ] (4) Package reference updated (Verify)
- [ ] (5) Restore all NuGet dependencies for solution
- [ ] (6) All dependencies restored successfully with no conflicts (Verify)
- [ ] (7) Build solution and fix any compilation errors per #plan:Breaking-Changes-Catalog
- [ ] (8) Solution builds with 0 errors and 0 warnings (Verify)

### [ ] TASK-002: Functional testing and validation
**References**: #plan:Phase-4, #plan:Testing-Strategy

- [ ] (1) Run SwishApiConsoleTest application to verify functionality
- [ ] (2) Verify RestSharp HTTP operations work correctly in .NET 10
- [ ] (3) Verify Newtonsoft.Json serialization/deserialization functions properly
- [ ] (4) Application executes without runtime exceptions (Verify)

### [ ] TASK-003: Final validation and commit
**References**: #plan:Phase-5, #plan:Success-Criteria

- [ ] (1) Verify both projects target net10.0
- [ ] (2) Verify Newtonsoft.Json is at version 13.0.4
- [ ] (3) Run final solution build to confirm stability
- [ ] (4) Solution builds cleanly with 0 errors and 0 warnings (Verify)
- [ ] (5) Commit all changes with message: "Complete upgrade to .NET 10.0"

---
