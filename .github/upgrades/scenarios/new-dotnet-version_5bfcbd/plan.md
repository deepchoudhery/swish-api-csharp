# .NET 10.0 Upgrade Plan for SwishApi Solution

## Executive Summary

This plan outlines the upgrade of the SwishApi solution from .NET 7.0 to .NET 10.0 (LTS). The solution consists of 2 projects with a simple dependency structure, making it an ideal candidate for an all-at-once upgrade approach. The upgrade includes updating target frameworks and one NuGet package.

**Key Facts:**
- **Current Framework:** .NET 7.0
- **Target Framework:** .NET 10.0 (Long Term Support)
- **Total Projects:** 2
- **Total LOC:** 2,177
- **Upgrade Complexity:** 🟢 Low
- **Estimated Effort:** 2-3 hours (including testing)
- **Package Updates Required:** 1

**Risk Level:** Low - Small codebase, simple dependencies, minimal API issues, and well-supported upgrade path.

---

## Upgrade Strategy

### Selected Strategy: All-At-Once

**Rationale:**
The all-at-once strategy is optimal for this solution because:

1. **Small Project Count:** Only 2 projects in the solution
2. **Simple Dependencies:** Clear dependency structure (SwishApiConsoleTest → SwishApi)
3. **Low Complexity:** Both projects are SDK-style with minimal dependencies
4. **Low Risk:** Assessment shows no API compatibility issues (0 binary incompatible, 0 source incompatible)
5. **Minimal Package Updates:** Only 1 package needs updating (Newtonsoft.Json)
6. **Modern Starting Point:** Already on .NET 7.0, close to target .NET 10.0

**Approach:**
All projects will be updated to .NET 10.0 simultaneously in a single operation, with package updates applied atomically.

**Advantages:**
- Fastest completion time (2-3 hours vs 4-6 hours incremental)
- No multi-targeting complexity
- Single testing phase
- Clean dependency resolution
- Both projects benefit simultaneously

---

## Dependency Analysis

### Project Relationship Graph

```
SwishApiConsoleTest (DotNetCoreApp)
        ↓
    SwishApi (ClassLibrary)
```

### Dependency Order

Given the simple structure, we'll upgrade both projects simultaneously:

**Phase 1: Atomic Update**
1. SwishApi.csproj (library)
2. SwishApiConsoleTest.csproj (console app)

**Note:** While SwishApiConsoleTest depends on SwishApi, the all-at-once strategy updates both together, so dependency order is less critical than in incremental upgrades.

---

## Project-by-Project Plans

### Project 1: SwishApi (Class Library)

**Current State:**
- Target Framework: net7.0
- Project Type: ClassLibrary (SDK-style)
- LOC: 1,884
- Files: 27
- Dependencies: 2 NuGet packages

**Target State:**
- Target Framework: net10.0

**Changes Required:**

1. **Project File Update**
   - File: `SwishApi/SwishApi.csproj`
   - Change: Update `<TargetFramework>net7.0</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`

2. **Package Updates**
   - Newtonsoft.Json: 13.0.3 → 13.0.4
   - RestSharp: 112.0.0 (no update required - compatible)

**Risk Assessment:** 🟢 Low
- No API compatibility issues detected
- Small codebase
- Simple package dependencies
- Well-tested upgrade path from .NET 7.0 to .NET 10.0

**Validation Steps:**
- [ ] Project builds without errors
- [ ] Project builds without warnings
- [ ] No package restore errors
- [ ] No dependency conflicts

---

### Project 2: SwishApiConsoleTest (Console Application)

**Current State:**
- Target Framework: net7.0
- Project Type: DotNetCoreApp (SDK-style)
- LOC: 293
- Files: 2 (1 code file)
- Dependencies: 1 project reference (SwishApi)

**Target State:**
- Target Framework: net10.0

**Changes Required:**

1. **Project File Update**
   - File: `SwishApiConsoleTest/SwishApiConsoleTest.csproj`
   - Change: Update `<TargetFramework>net7.0</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`

2. **Package Updates**
   - None required (inherits from SwishApi)

**Risk Assessment:** 🟢 Low
- No API compatibility issues detected
- Very small codebase (293 LOC)
- Single project dependency
- Test/example project

**Validation Steps:**
- [ ] Project builds without errors
- [ ] Project builds without warnings
- [ ] Console app runs successfully
- [ ] No dependency conflicts with SwishApi

---

## Package Update Reference

### Packages Requiring Updates

| Package | Current Version | Target Version | Affected Projects | Reason |
|---------|----------------|----------------|-------------------|--------|
| Newtonsoft.Json | 13.0.3 | 13.0.4 | SwishApi | Package update recommended for .NET 10.0 compatibility |

### Compatible Packages (No Update Required)

| Package | Current Version | Affected Projects | Status |
|---------|----------------|-------------------|--------|
| RestSharp | 112.0.0 | SwishApi | ✅ Compatible with .NET 10.0 |

### Package Update Details

#### Newtonsoft.Json (13.0.3 → 13.0.4)
- **Type:** Patch update
- **Breaking Changes:** None expected (patch version)
- **Reason:** Recommended for optimal .NET 10.0 compatibility
- **Implementation:** Update PackageReference in SwishApi.csproj
- **Testing:** Verify JSON serialization/deserialization functionality

---

## Breaking Changes Catalog

### .NET 7.0 → .NET 10.0 Migration

**Expected Breaking Changes:** Minimal

Based on the assessment analysis:
- **Binary Incompatible APIs:** 0
- **Source Incompatible APIs:** 0
- **Behavioral Changes:** 0
- **Compatible APIs:** 6 (all analyzed APIs are compatible)

### Areas to Monitor

Even with no detected breaking changes, the following areas should be monitored during testing:

1. **JSON Serialization**
   - Verify Newtonsoft.Json behavior with updated version
   - Test complex object serialization/deserialization
   - Validate custom converters (if any)

2. **HTTP Client (RestSharp)**
   - Verify REST API calls function correctly
   - Test error handling
   - Validate response parsing

3. **Runtime Behavior**
   - Monitor any performance differences
   - Check for logging changes
   - Verify exception handling patterns

4. **Build and Deployment**
   - Ensure .NET 10.0 SDK is available in build environments
   - Update CI/CD pipelines to use .NET 10.0
   - Verify deployment targets support .NET 10.0

### Known .NET 10.0 Changes to Be Aware Of

While the assessment shows no breaking changes for this codebase, .NET 10.0 includes:
- Performance improvements across the runtime
- New language features (C# 13)
- Enhanced diagnostics and tooling
- Updated default settings for HTTP clients
- Improved nullability annotations

**Recommendation:** Review the official [.NET 10.0 breaking changes documentation](https://learn.microsoft.com/dotnet/core/compatibility/10.0) after upgrade if any unexpected issues arise.

---

## Testing Strategy

### Multi-Level Testing Approach

#### Level 1: Build Verification
**Objective:** Ensure all projects compile successfully

**Steps:**
1. Clean the solution: `dotnet clean SwishApi.sln`
2. Restore packages: `dotnet restore SwishApi.sln`
3. Build solution: `dotnet build SwishApi.sln --no-restore`
4. Verify zero build errors
5. Verify zero build warnings

**Success Criteria:**
- All projects build successfully
- No compiler errors
- No compiler warnings
- No package restore failures

#### Level 2: Project-Level Validation
**Objective:** Verify each project independently

**SwishApi Project:**
- [ ] Builds without errors
- [ ] Builds without warnings
- [ ] No package conflicts (check `dotnet list package` output)
- [ ] No security vulnerabilities
- [ ] All public APIs accessible

**SwishApiConsoleTest Project:**
- [ ] Builds without errors
- [ ] Builds without warnings
- [ ] References SwishApi correctly
- [ ] Console app executes without runtime errors

#### Level 3: Functional Testing
**Objective:** Ensure runtime behavior is correct

**SwishApi Library:**
- [ ] JSON serialization/deserialization works correctly
- [ ] RestSharp HTTP calls function properly
- [ ] All public methods execute without errors
- [ ] Error handling behaves as expected

**SwishApiConsoleTest:**
- [ ] Application starts successfully
- [ ] All test scenarios complete
- [ ] Console output is correct
- [ ] No unhandled exceptions

#### Level 4: Integration Testing
**Objective:** Verify the solution works as a complete system

- [ ] SwishApiConsoleTest successfully uses SwishApi library
- [ ] API calls through RestSharp complete successfully
- [ ] JSON processing via Newtonsoft.Json works correctly
- [ ] End-to-end scenarios pass

### Testing Checklist

```markdown
## Build Phase
- [ ] Solution cleans successfully
- [ ] Packages restore without errors
- [ ] Solution builds without errors
- [ ] Solution builds without warnings
- [ ] Output binaries target .NET 10.0

## Runtime Phase
- [ ] SwishApiConsoleTest executes successfully
- [ ] All API calls complete without errors
- [ ] JSON serialization works correctly
- [ ] No runtime exceptions

## Verification Phase
- [ ] Package dependencies resolve correctly
- [ ] No security vulnerabilities (run: dotnet list package --vulnerable)
- [ ] No outdated packages (run: dotnet list package --outdated)
- [ ] Framework version confirmed: net10.0
```

---

## Risk Management

### Risk Assessment Matrix

| Risk Area | Likelihood | Impact | Risk Level | Mitigation Strategy |
|-----------|-----------|--------|------------|-------------------|
| Build failures | Low | Medium | 🟢 Low | Simple rollback via git; small codebase |
| Package incompatibilities | Very Low | Medium | 🟢 Low | Only 1 package update; patch version |
| API breaking changes | Very Low | Low | 🟢 Low | Assessment shows 0 breaking changes |
| Runtime issues | Low | Medium | 🟢 Low | Comprehensive testing plan |
| Deployment issues | Low | Low | 🟢 Low | .NET 10.0 widely available |

### Overall Risk Level: 🟢 LOW

**Justification:**
- Small, simple solution
- Modern starting point (.NET 7.0)
- Minimal dependencies
- No detected API issues
- Clear rollback path

### Risk Mitigation Strategies

1. **Version Control**
   - All changes on dedicated branch: `copilot/upgrade-swishapi-to-net10-again`
   - Easy rollback to source branch if needed
   - Commit after each major step

2. **Incremental Validation**
   - Build verification after each project update
   - Test immediately after package updates
   - Validate at each checkpoint

3. **Documentation**
   - Document any unexpected issues
   - Record solutions for future reference
   - Update team on changes

### Rollback Plan

**If Critical Issues Occur:**

1. **Immediate Rollback:**
   ```bash
   git checkout <source-branch>
   git branch -D copilot/upgrade-swishapi-to-net10-again
   ```

2. **Partial Rollback (if on feature branch):**
   ```bash
   git reset --hard HEAD~N  # where N is number of commits to undo
   ```

3. **Post-Rollback:**
   - Analyze root cause of failure
   - Address blockers
   - Retry upgrade with mitigations

**Rollback Decision Criteria:**
- Build failures that cannot be resolved within 1 hour
- Critical runtime issues affecting core functionality
- Package conflicts that cannot be resolved
- Deadline constraints requiring stable codebase

---

## Execution Sequence

### Prerequisites Checklist

Before starting the upgrade:

- [x] Git repository in clean state
- [x] Working on dedicated upgrade branch: `copilot/upgrade-swishapi-to-net10-again`
- [x] .NET 10.0 SDK installed and verified
- [ ] Latest packages restored (`dotnet restore`)
- [ ] Baseline build successful (`dotnet build`)
- [ ] Team notified of upgrade in progress

### Step-by-Step Execution

#### Phase 1: Framework Update (Atomic)

**Step 1.1: Update SwishApi Project**
```bash
# Edit: SwishApi/SwishApi.csproj
# Change: <TargetFramework>net7.0</TargetFramework>
# To:     <TargetFramework>net10.0</TargetFramework>
```

**Step 1.2: Update SwishApiConsoleTest Project**
```bash
# Edit: SwishApiConsoleTest/SwishApiConsoleTest.csproj
# Change: <TargetFramework>net7.0</TargetFramework>
# To:     <TargetFramework>net10.0</TargetFramework>
```

**Step 1.3: Update Newtonsoft.Json Package**
```bash
# Edit: SwishApi/SwishApi.csproj
# Change: <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
# To:     <PackageReference Include="Newtonsoft.Json" Version="13.0.4" />
```

#### Phase 2: Build Verification

**Step 2.1: Clean and Restore**
```bash
dotnet clean SwishApi.sln
dotnet restore SwishApi.sln
```

**Step 2.2: Build Solution**
```bash
dotnet build SwishApi.sln --configuration Release
```

**Expected Outcome:** 
- Build succeeds with 0 errors
- Build produces 0 warnings
- Output: `Build succeeded.`

#### Phase 3: Testing

**Step 3.1: Build Verification**
- Verify both projects build successfully
- Check for any warnings
- Validate package restore

**Step 3.2: Runtime Testing**
```bash
dotnet run --project SwishApiConsoleTest/SwishApiConsoleTest.csproj
```
- Verify console app runs without errors
- Check output for correctness
- Monitor for exceptions

**Step 3.3: Package Audit**
```bash
dotnet list package --vulnerable
dotnet list package --outdated
```
- Ensure no security vulnerabilities
- Confirm all packages are up to date

#### Phase 4: Finalization

**Step 4.1: Commit Changes**
```bash
git add .
git commit -m "Upgrade solution from .NET 7.0 to .NET 10.0

- Updated SwishApi project to target net10.0
- Updated SwishApiConsoleTest project to target net10.0
- Updated Newtonsoft.Json from 13.0.3 to 13.0.4
- All builds successful, no breaking changes"
```

**Step 4.2: Final Verification**
```bash
# Full clean build from scratch
dotnet clean SwishApi.sln
dotnet restore SwishApi.sln
dotnet build SwishApi.sln --configuration Release
dotnet run --project SwishApiConsoleTest/SwishApiConsoleTest.csproj
```

**Step 4.3: Documentation Update**
- Update README.md if it specifies .NET version requirements
- Update CI/CD pipeline to use .NET 10.0 SDK
- Document any observations or notes

---

## Timeline Estimate

### Detailed Time Breakdown

| Phase | Task | Estimated Time | Cumulative |
|-------|------|---------------|------------|
| **Phase 1** | Framework updates | 15 minutes | 0:15 |
| **Phase 2** | Build verification | 15 minutes | 0:30 |
| **Phase 3** | Testing and validation | 45-60 minutes | 1:30 |
| **Phase 4** | Finalization and commit | 15 minutes | 1:45 |
| **Buffer** | Unexpected issues | 30-45 minutes | 2:30 |
| **Total** | **Complete upgrade** | **2-3 hours** | **2-3 hours** |

### Timeline Notes

- **Best Case:** 1.5 hours (if everything goes perfectly)
- **Expected Case:** 2-3 hours (includes normal testing)
- **Worst Case:** 4 hours (if minor issues require investigation)

**Factors that could extend timeline:**
- Discovering unexpected API issues during testing
- Package conflicts not detected in assessment
- Need for code changes due to behavioral differences
- Environment setup issues (.NET 10.0 SDK)

---

## Success Criteria

### The upgrade is complete and successful when ALL of the following are true:

#### Technical Criteria
- [x] All projects target net10.0
- [x] Newtonsoft.Json updated to version 13.0.4
- [x] Solution builds without errors
- [x] Solution builds without warnings
- [x] All package dependencies resolve correctly
- [x] No package version conflicts
- [x] No security vulnerabilities (`dotnet list package --vulnerable` returns clean)

#### Functional Criteria
- [x] SwishApiConsoleTest runs successfully
- [x] All console test scenarios pass
- [x] SwishApi library functions correctly
- [x] JSON serialization/deserialization works as expected
- [x] RestSharp API calls complete successfully
- [x] No runtime exceptions or errors

#### Quality Criteria
- [x] Code builds in Release configuration
- [x] Output binaries target correct framework (net10.0)
- [x] No deprecated API warnings
- [x] Git history shows clean commit

#### Documentation Criteria
- [x] Changes committed to git with descriptive message
- [x] Any issues encountered are documented
- [x] Team notified of completed upgrade
- [x] CI/CD pipeline updated (if applicable)

### Verification Commands

```bash
# Verify target framework
dotnet list SwishApi/SwishApi.csproj package | grep "TargetFramework"
dotnet list SwishApiConsoleTest/SwishApiConsoleTest.csproj package | grep "TargetFramework"

# Verify package versions
dotnet list package

# Verify security
dotnet list package --vulnerable

# Verify build
dotnet build SwishApi.sln --configuration Release

# Verify runtime
dotnet run --project SwishApiConsoleTest/SwishApiConsoleTest.csproj
```

---

## Appendix

### A. Reference Links

- [.NET 10.0 Release Notes](https://learn.microsoft.com/dotnet/core/whats-new/dotnet-10)
- [.NET 10.0 Breaking Changes](https://learn.microsoft.com/dotnet/core/compatibility/10.0)
- [Migrate from .NET 7 to .NET 10](https://learn.microsoft.com/dotnet/core/porting/)
- [Newtonsoft.Json Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm)
- [RestSharp Documentation](https://restsharp.dev/)

### B. Project File Diffs

#### SwishApi.csproj
```diff
- <TargetFramework>net7.0</TargetFramework>
+ <TargetFramework>net10.0</TargetFramework>

- <PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
+ <PackageReference Include="Newtonsoft.Json" Version="13.0.4" />
```

#### SwishApiConsoleTest.csproj
```diff
- <TargetFramework>net7.0</TargetFramework>
+ <TargetFramework>net10.0</TargetFramework>
```

### C. Environment Requirements

**Required Software:**
- .NET 10.0 SDK or later
- Git (for version control)
- IDE: Visual Studio 2025, VS Code, or Rider (optional)

**Verification:**
```bash
dotnet --version  # Should show 10.0.x or higher
git --version     # Any recent version
```

### D. Troubleshooting Guide

**Common Issues and Solutions:**

1. **Issue:** `.NET 10.0 SDK not found`
   - **Solution:** Install .NET 10.0 SDK from https://dotnet.microsoft.com/download/dotnet/10.0

2. **Issue:** `Package restore fails`
   - **Solution:** Clear NuGet cache: `dotnet nuget locals all --clear`, then retry

3. **Issue:** `Build warnings about deprecated APIs`
   - **Solution:** Review warnings, update code to use recommended alternatives

4. **Issue:** `Runtime errors with Newtonsoft.Json`
   - **Solution:** Check for custom serialization settings, verify JSON structure

5. **Issue:** `RestSharp HTTP calls fail`
   - **Solution:** Verify SSL/TLS settings, check for HTTP client configuration changes

---

## Plan Approval

This plan is ready for execution. The upgrade is straightforward with minimal risk.

**Recommended Next Steps:**
1. Review this plan with the team
2. Ensure .NET 10.0 SDK is installed
3. Verify baseline build is successful
4. Proceed with execution when ready

**Questions or Concerns:**
- Any additional testing requirements?
- Any specific deployment timeline constraints?
- Any additional environments to test?

---

*Plan created: February 13, 2026*  
*Assessment basis: assessment.md (analyzed 2 projects, 2,177 LOC)*  
*Target framework: .NET 10.0 (Long Term Support)*  
*Strategy: All-At-Once*  
*Risk level: 🟢 Low*
