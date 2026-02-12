# .NET 10 Upgrade Plan for SwishApi Solution

**Date**: 2026-02-12  
**Target Framework**: .NET 10.0 (LTS)  
**Current Framework**: .NET 7.0  
**Assessment Mode**: Scenario-Guided

---

## Executive Summary

### Selected Strategy
**All-At-Once Strategy** - All projects upgraded simultaneously in a single coordinated operation.

**Rationale**: 
- 2 projects (small solution)
- All currently on .NET 7.0
- Simple dependency structure (console app depends on library)
- All packages have target framework versions available or are compatible
- Low complexity upgrade (🟢 Low risk for both projects)
- 2,177 total lines of code
- No API breaking changes identified

### Scope Overview

| Metric | Value |
| :--- | :--- |
| Projects to Upgrade | 2 |
| Target Framework | net10.0 |
| Package Updates Required | 1 (Newtonsoft.Json) |
| API Breaking Changes | 0 |
| Estimated Risk | Low |
| Expected Duration | 1-2 hours |

### High-Level Approach

1. **Preparation**: Verify .NET 10 SDK is installed
2. **Atomic Upgrade**: Update both projects and packages simultaneously
3. **Build Verification**: Compile solution and resolve any issues
4. **Test Validation**: Run console test application
5. **Final Validation**: Verify no warnings or errors

---

## Upgrade Strategy

### Why All-At-Once?

This solution is an ideal candidate for the All-At-Once strategy:

✅ **Small Solution**: Only 2 projects  
✅ **Simple Dependencies**: Linear dependency (SwishApiConsoleTest → SwishApi)  
✅ **Modern Base**: Both projects already on .NET 7.0 (SDK-style)  
✅ **Low Complexity**: 2,177 LOC total, no API incompatibilities  
✅ **Package Compatibility**: All packages either compatible or have clear upgrade paths  

### Alternative Considered

**Incremental Upgrade**: Not needed for this solution. The overhead of maintaining intermediate states would exceed the benefit for only 2 projects with such a simple dependency graph.

---

## Dependency Analysis

### Project Dependency Graph

```
SwishApiConsoleTest (Console Application)
    └── SwishApi (Class Library)
```

### Upgrade Order

Since we're using All-At-Once strategy, both projects will be upgraded simultaneously. However, the natural dependency order is:

1. **SwishApi** (leaf node - no project dependencies)
   - Type: Class Library
   - Current: net7.0
   - Target: net10.0
   - Package Issues: 1 (Newtonsoft.Json upgrade)

2. **SwishApiConsoleTest** (depends on SwishApi)
   - Type: Console Application
   - Current: net7.0
   - Target: net10.0
   - Package Issues: 0

This order ensures that if any intermediate validation is needed, the foundational library is ready first.

---

## Project-by-Project Details

### Project 1: SwishApi (Class Library)

**Path**: `SwishApi/SwishApi.csproj`  
**Type**: Class Library  
**Current Framework**: net7.0  
**Target Framework**: net10.0  
**Risk Level**: 🟢 Low  
**Lines of Code**: ~2,000 (estimated from total)

#### Changes Required

1. **TargetFramework Update**
   ```xml
   <!-- Before -->
   <TargetFramework>net7.0</TargetFramework>
   
   <!-- After -->
   <TargetFramework>net10.0</TargetFramework>
   ```

2. **Package Updates**
   - **Newtonsoft.Json**: 13.0.3 → 13.0.4
     - Reason: Recommended package upgrade
     - Breaking Changes: None expected (patch version)
   - **RestSharp**: 112.0.0 (no change - compatible with .NET 10)

3. **Code Changes**
   - No API breaking changes identified
   - No code modifications expected

4. **Configuration Changes**
   - No configuration changes expected
   - Package metadata may need version bump (optional)

#### Validation Steps

- [ ] Project builds without errors
- [ ] Project builds without warnings
- [ ] No dependency conflicts
- [ ] Package references resolve correctly

---

### Project 2: SwishApiConsoleTest (Console Application)

**Path**: `SwishApiConsoleTest/SwishApiConsoleTest.csproj`  
**Type**: Console Application  
**Current Framework**: net7.0  
**Target Framework**: net10.0  
**Risk Level**: 🟢 Low  
**Lines of Code**: ~177 (estimated from total)

#### Changes Required

1. **TargetFramework Update**
   ```xml
   <!-- Before -->
   <TargetFramework>net7.0</TargetFramework>
   
   <!-- After -->
   <TargetFramework>net10.0</TargetFramework>
   ```

2. **Package Updates**
   - None required (only has project reference to SwishApi)

3. **Code Changes**
   - No API breaking changes identified
   - No code modifications expected

4. **Configuration Changes**
   - No configuration changes expected

#### Validation Steps

- [ ] Project builds without errors
- [ ] Project builds without warnings
- [ ] Application runs successfully
- [ ] All test scenarios pass

---

## Package Update Reference

### Consolidated Package Updates

| Package | Current Version | Target Version | Projects Affected | Reason | Breaking Changes |
| :--- | :---: | :---: | :--- | :--- | :--- |
| Newtonsoft.Json | 13.0.3 | 13.0.4 | SwishApi | Recommended upgrade | None (patch release) |
| RestSharp | 112.0.0 | (no change) | SwishApi | Compatible with .NET 10 | N/A |

### Package Update Details

#### Newtonsoft.Json (13.0.3 → 13.0.4)

**Impact**: Patch version update, minimal risk

**Projects**: SwishApi

**Reason**: Recommended package upgrade for better compatibility and bug fixes

**Breaking Changes**: None expected (semantic versioning - patch release)

**Migration Steps**:
1. Update PackageReference in SwishApi.csproj
2. Run `dotnet restore` to fetch new version
3. Verify no compilation errors

---

## Breaking Changes Catalog

### Framework Breaking Changes (NET 7.0 → NET 10.0)

Based on Microsoft documentation for .NET 8, 9, and 10 breaking changes:

#### Identified Categories
- None specifically affecting this codebase

#### Potential Areas to Monitor
1. **Obsolete APIs**: Some APIs deprecated in .NET 8-10 may generate warnings
2. **Behavioral Changes**: Minor runtime behavior changes in BCL
3. **Trimming**: If using trimming, some APIs may have new annotations

### Package Breaking Changes

#### Newtonsoft.Json (13.0.3 → 13.0.4)
- **Type**: Patch version update
- **Expected Impact**: None
- **Known Issues**: None for this minor version bump

#### RestSharp (112.0.0)
- **Status**: No update required
- **Compatibility**: Fully compatible with .NET 10

### No Code Changes Expected

The assessment identified **zero API compatibility issues**, meaning:
- No binary incompatible APIs detected
- No source incompatible APIs detected
- No behavioral changes affecting the codebase
- All 6 APIs analyzed are compatible

---

## Implementation Timeline

### Phase 0: Prerequisites (5 minutes)

**Goal**: Ensure environment is ready for .NET 10 development

**Tasks**:
1. Verify .NET 10 SDK installation
   - Run `dotnet --list-sdks`
   - Confirm .NET 10.0.x is present
   - If missing, download from https://dot.net
2. Check for global.json constraints
   - Verify no global.json files restrict SDK version
   - If present, update to allow .NET 10 SDK

**Validation**: `dotnet --version` shows .NET 10.x SDK

---

### Phase 1: Atomic Upgrade (15-30 minutes)

**Goal**: Update all projects and packages in a single coordinated operation

**Operations** (performed as single batch):

#### 1.1 Update Project Files
Update TargetFramework property in both projects:
- `SwishApi/SwishApi.csproj`
- `SwishApiConsoleTest/SwishApiConsoleTest.csproj`

Change:
```xml
<TargetFramework>net7.0</TargetFramework>
```
To:
```xml
<TargetFramework>net10.0</TargetFramework>
```

#### 1.2 Update Package References
Update Newtonsoft.Json in `SwishApi/SwishApi.csproj`:

Change:
```xml
<PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
```
To:
```xml
<PackageReference Include="Newtonsoft.Json" Version="13.0.4" />
```

#### 1.3 Restore Dependencies
```bash
dotnet restore SwishApi.sln
```

#### 1.4 Build Solution
```bash
dotnet build SwishApi.sln --configuration Release
```

#### 1.5 Address Compilation Errors (if any)
- Review build output for errors
- Address any framework-related API issues
- Fix any package compatibility issues
- Iterate until clean build

**Deliverables**: 
- ✅ Solution builds with 0 errors
- ✅ No package dependency conflicts
- ⚠️ Document any warnings for review

---

### Phase 2: Test Validation (15-30 minutes)

**Goal**: Verify application functionality after upgrade

**Operations**:

#### 2.1 Run Console Test Application
```bash
dotnet run --project SwishApiConsoleTest/SwishApiConsoleTest.csproj
```

#### 2.2 Functional Testing
- Execute test scenarios defined in the console app
- Verify Swish API integration works correctly
- Test certificate handling and authentication
- Validate JSON serialization/deserialization

#### 2.3 Address Test Failures (if any)
- Investigate any runtime errors
- Review behavioral changes from framework upgrade
- Fix issues and re-test

**Deliverables**: 
- ✅ Console application runs without errors
- ✅ All test scenarios pass
- ✅ No runtime exceptions

---

### Phase 3: Final Validation (10-15 minutes)

**Goal**: Ensure upgrade completeness and quality

**Operations**:

#### 3.1 Clean Build
```bash
dotnet clean SwishApi.sln
dotnet build SwishApi.sln --configuration Release
```

#### 3.2 Warning Review
- Review all compiler warnings
- Address any obsolete API warnings
- Document warnings that can be deferred

#### 3.3 Package Audit
```bash
dotnet list package --vulnerable
dotnet list package --outdated
```

#### 3.4 Verification Checklist
- [ ] Both projects target net10.0
- [ ] Newtonsoft.Json updated to 13.0.4
- [ ] Solution builds without errors
- [ ] Zero dependency conflicts
- [ ] Console application runs successfully
- [ ] No security vulnerabilities reported

**Deliverables**: 
- ✅ Clean build with zero errors
- ✅ All validation checks passed
- ✅ Ready for commit

---

## Testing Strategy

### Multi-Level Testing Approach

Since this is a class library with a console test application, testing will be primarily functional rather than unit test-based.

#### Level 1: Build Verification
**When**: After each file change  
**What**: Ensure projects compile  
**How**: `dotnet build`  
**Success Criteria**: Zero build errors

#### Level 2: Application Testing
**When**: After all upgrades complete  
**What**: Run console test application  
**How**: `dotnet run --project SwishApiConsoleTest`  
**Success Criteria**: 
- Application starts without errors
- Can load test certificates
- Can initialize Swish API client
- No runtime exceptions

#### Level 3: Integration Verification
**When**: After application testing  
**What**: Verify library works correctly when consumed  
**How**: Test scenarios in console app  
**Success Criteria**:
- JSON serialization works
- HTTP client operations function
- Certificate handling operates correctly

### Test Scenarios

Based on the repository structure, the console test application should verify:

1. **Certificate Loading**: Verify test certificates load correctly
2. **API Client Creation**: Ensure SwishApi client initializes
3. **JSON Operations**: Test Newtonsoft.Json serialization/deserialization
4. **HTTP Operations**: Verify RestSharp client functionality (if tested)

### Testing Checklist

- [ ] SwishApi project builds successfully
- [ ] SwishApiConsoleTest project builds successfully
- [ ] Console application executes without errors
- [ ] Test certificates load correctly
- [ ] API client initialization succeeds
- [ ] No package version conflicts
- [ ] No runtime exceptions during testing

---

## Risk Management

### Risk Assessment

#### Overall Risk: 🟢 LOW

**Factors Supporting Low Risk**:
- Small codebase (2,177 LOC)
- Only 2 projects
- Simple dependency graph
- Both projects already modern (.NET 7.0)
- SDK-style projects (no legacy format conversion)
- No API breaking changes identified
- Minimal package updates (1 package)
- No security vulnerabilities in current packages

### Identified Risks

#### Risk 1: .NET 10 SDK Not Available
**Likelihood**: Low  
**Impact**: High (blocks entire upgrade)  
**Mitigation**:
- Verify SDK installation before starting
- Download .NET 10 SDK from official sources
- Document SDK version used
**Contingency**: Download and install SDK as first step

#### Risk 2: Package Compatibility Issues
**Likelihood**: Very Low  
**Impact**: Medium  
**Mitigation**:
- Assessment confirmed all packages compatible
- Newtonsoft.Json 13.0.4 explicitly tested with .NET 10
- RestSharp 112.0.0 confirmed compatible
**Contingency**: Roll back to .NET 7.0 if critical issues found

#### Risk 3: Hidden Breaking Changes
**Likelihood**: Very Low  
**Impact**: Medium  
**Mitigation**:
- Assessment performed comprehensive API analysis
- Zero breaking changes detected
- Thorough testing phase included
**Contingency**: Review .NET 10 breaking changes documentation, apply targeted fixes

#### Risk 4: Test Certificate Compatibility
**Likelihood**: Very Low  
**Impact**: Low (test-only impact)  
**Mitigation**:
- Test certificates are .NET version agnostic (p12 format)
- Certificate handling APIs stable across .NET versions
**Contingency**: Verify certificate loading in upgraded console app

### Risk Mitigation Strategies

1. **Incremental Validation**: Build after each change to catch issues early
2. **Comprehensive Testing**: Run full application test scenarios
3. **Git Branching**: All work on dedicated `upgrade-to-NET10` branch
4. **Documentation**: Document any warnings or minor issues for future reference
5. **Rollback Plan**: Can revert changes via Git if critical issues arise

### Rollback Plan

If critical issues are discovered that cannot be quickly resolved:

1. **Immediate Rollback**:
   ```bash
   git checkout <previous-commit>
   git branch -D upgrade-to-NET10
   ```

2. **Assess Issues**: Document what went wrong

3. **Research Solutions**: Investigate breaking changes or compatibility issues

4. **Retry**: Create new branch and apply fixes before re-attempting

**Rollback Criteria**: Upgrade should be rolled back if:
- Critical functionality broken with no clear fix
- Security vulnerabilities introduced by new packages
- Performance degradation >20%
- Cannot build solution after 2+ hours of troubleshooting

---

## Source Control Strategy

### Branching Strategy

**Branch**: `upgrade-to-NET10` (already created)  
**Base**: `copilot/upgrade-swishapi-to-dotnet-10`

### Commit Strategy

**Recommended Approach**: Single commit for atomic upgrade

Since this is an All-At-Once upgrade with minimal changes, a single commit is appropriate:

```
feat: Upgrade solution to .NET 10.0

- Update TargetFramework from net7.0 to net10.0 for both projects
- Upgrade Newtonsoft.Json from 13.0.3 to 13.0.4 in SwishApi
- Verify all builds and tests pass on .NET 10.0

BREAKING CHANGE: Requires .NET 10.0 SDK or higher
```

**Alternative Approach**: Separate commits (if preferred)

If finer-grained history is desired:

1. **Commit 1**: Update TargetFramework properties
   ```
   chore: Update target framework to net10.0
   ```

2. **Commit 2**: Update package versions
   ```
   chore: Update Newtonsoft.Json to 13.0.4
   ```

3. **Commit 3**: Address any build issues (if needed)
   ```
   fix: Resolve .NET 10 compatibility issues
   ```

### Pull Request Strategy

**Title**: `Upgrade SwishApi solution to .NET 10.0`

**Description Template**:
```markdown
## Overview
Upgrades SwishApi solution from .NET 7.0 to .NET 10.0 LTS

## Changes
- ✅ Updated TargetFramework to net10.0 for both projects
- ✅ Upgraded Newtonsoft.Json from 13.0.3 to 13.0.4
- ✅ Verified clean build with zero errors
- ✅ Tested console application functionality

## Testing
- [x] Solution builds successfully
- [x] Console application runs without errors
- [x] No package dependency conflicts
- [x] No security vulnerabilities

## Risk Assessment
**Risk Level**: Low
- Small solution (2 projects)
- No API breaking changes
- All packages compatible
- Comprehensive testing completed

## Deployment Notes
**Required**: .NET 10.0 SDK or higher

## Rollback Plan
Revert this PR if critical issues discovered post-deployment
```

---

## Success Criteria

### Technical Criteria

The upgrade is considered **complete** when:

#### Build Requirements
- ✅ Both projects target `net10.0`
- ✅ Solution builds with **zero errors**
- ✅ Solution builds with **zero warnings** (or warnings documented as acceptable)
- ✅ All package references resolve without conflicts

#### Package Requirements
- ✅ Newtonsoft.Json updated to version 13.0.4
- ✅ RestSharp remains at 112.0.0 (compatible)
- ✅ No security vulnerabilities in dependencies
- ✅ No outdated packages with known issues

#### Functional Requirements
- ✅ SwishApiConsoleTest application runs successfully
- ✅ Test certificates load correctly
- ✅ API client initialization works
- ✅ No runtime exceptions during testing

#### Quality Requirements
- ✅ No new compiler warnings introduced
- ✅ No new static analysis issues
- ✅ Code style and formatting maintained
- ✅ All existing functionality preserved

### Acceptance Checklist

Before marking upgrade complete:

- [ ] .NET 10 SDK verified installed
- [ ] Both project files updated to net10.0
- [ ] Newtonsoft.Json updated to 13.0.4
- [ ] `dotnet restore` completes successfully
- [ ] `dotnet build` produces zero errors
- [ ] Console application runs without exceptions
- [ ] Test scenarios execute successfully
- [ ] No dependency conflicts reported
- [ ] No security vulnerabilities detected
- [ ] Changes committed to upgrade branch
- [ ] Documentation updated (if needed)

### Definition of Done

1. **Code Complete**: All changes implemented and committed
2. **Build Verified**: Clean build on local machine
3. **Tests Passed**: Functional testing completed successfully
4. **Security Validated**: No vulnerabilities in upgraded packages
5. **Documentation Current**: README or docs updated if necessary
6. **Ready for Review**: PR created with comprehensive description
7. **Rollback Ready**: Can quickly revert if issues found

---

## Post-Upgrade Considerations

### Optional Enhancements

After successful upgrade, consider these optional improvements:

1. **SDK Version Pinning**
   - Add `global.json` to specify minimum .NET 10 SDK version
   - Ensures consistent builds across team

2. **Package Version Update**
   - Consider updating SwishApi library version number
   - Reflect .NET 10 support in release notes

3. **Documentation Updates**
   - Update README.md with .NET 10 requirement
   - Update package description if published to NuGet
   - Document any behavioral changes observed

4. **CI/CD Pipeline Updates**
   - Update build pipelines to use .NET 10 SDK
   - Update deployment targets if applicable
   - Update Docker images if containerized

5. **Dependency Modernization**
   - Consider System.Text.Json as Newtonsoft.Json alternative (future)
   - Review RestSharp for any newer versions
   - Audit all transitive dependencies

### Monitoring & Validation

After deployment to production (if applicable):

1. **Performance Monitoring**
   - Compare performance metrics to .NET 7.0 baseline
   - Monitor memory usage patterns
   - Track startup time and throughput

2. **Error Tracking**
   - Monitor for any new exceptions or errors
   - Watch for API deprecation warnings
   - Track package compatibility issues

3. **User Feedback**
   - Gather feedback from library consumers
   - Monitor issue reports for upgrade-related problems
   - Document any discovered edge cases

---

## Appendix

### A. Reference Documentation

**Official Microsoft Documentation**:
- [What's new in .NET 10](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10)
- [Breaking changes in .NET 10](https://learn.microsoft.com/en-us/dotnet/core/compatibility/10.0)
- [Breaking changes in .NET 9](https://learn.microsoft.com/en-us/dotnet/core/compatibility/9.0)
- [Breaking changes in .NET 8](https://learn.microsoft.com/en-us/dotnet/core/compatibility/8.0)
- [Upgrade from .NET 7 to .NET 8](https://learn.microsoft.com/en-us/dotnet/core/migration/80)

**Package Documentation**:
- [Newtonsoft.Json Release Notes](https://github.com/JamesNK/Newtonsoft.Json/releases)
- [RestSharp Documentation](https://restsharp.dev/)

### B. Assessment Summary

**Key Findings from Assessment**:
- Total Projects: 2
- Total LOC: 2,177
- Package Updates: 1
- API Issues: 0
- Security Issues: 0
- Risk Level: Low

**Projects**:
1. SwishApi - Class Library (net7.0 → net10.0)
2. SwishApiConsoleTest - Console App (net7.0 → net10.0)

**Dependencies**:
- Newtonsoft.Json: 13.0.3 → 13.0.4
- RestSharp: 112.0.0 (no change)

### C. Command Reference

Quick reference for common commands during upgrade:

```bash
# Verify SDK installation
dotnet --list-sdks
dotnet --version

# Restore packages
dotnet restore SwishApi.sln

# Build solution
dotnet build SwishApi.sln
dotnet build SwishApi.sln --configuration Release

# Clean build artifacts
dotnet clean SwishApi.sln

# Run console application
dotnet run --project SwishApiConsoleTest/SwishApiConsoleTest.csproj

# List packages
dotnet list package
dotnet list package --outdated
dotnet list package --vulnerable

# Package operations (if needed)
dotnet add SwishApi/SwishApi.csproj package Newtonsoft.Json --version 13.0.4
```

### D. File Locations

**Project Files**:
- `SwishApi/SwishApi.csproj` - Main library project
- `SwishApiConsoleTest/SwishApiConsoleTest.csproj` - Console test application

**Test Certificates**:
- `SwishApiConsoleTest/TestCert/Swish_Merchant_TestCertificate2_1234679304.p12`
- `SwishApiConsoleTest/TestCert/Swish_Merchant_TestCertificate_1234679304.p12`
- `SwishApiConsoleTest/TestCert/Swish_Merchant_TestSigningCertificate_1234679304.p12`

**Upgrade Artifacts**:
- `.github/upgrades/scenarios/new-dotnet-version_31ac35/assessment.md` - Assessment report
- `.github/upgrades/scenarios/new-dotnet-version_31ac35/plan.md` - This planning document

---

## Summary

This plan provides a comprehensive, low-risk path to upgrade the SwishApi solution from .NET 7.0 to .NET 10.0 LTS. The All-At-Once strategy is appropriate given the small solution size, simple dependencies, and lack of breaking changes.

**Key Points**:
- ✅ Both projects upgraded simultaneously
- ✅ Minimal package updates (1 package)
- ✅ No API breaking changes
- ✅ Low risk, high confidence
- ✅ Clear validation criteria
- ✅ Simple rollback plan

**Estimated Timeline**: 1-2 hours total
- Prerequisites: 5 minutes
- Atomic Upgrade: 15-30 minutes
- Test Validation: 15-30 minutes
- Final Validation: 10-15 minutes

**Next Step**: Proceed to Execution stage to implement this plan systematically.

---

*This plan was created based on the assessment analysis and follows Microsoft's best practices for .NET upgrades. It will serve as the blueprint for the Execution stage.*
