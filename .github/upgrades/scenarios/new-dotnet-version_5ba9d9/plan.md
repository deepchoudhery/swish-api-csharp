# .NET 10.0 Upgrade Plan for SwishApi Solution

**Date**: 2026-02-13  
**Target Framework**: .NET 10.0 (Long Term Support)  
**Current Framework**: .NET 7.0  
**Strategy**: All-At-Once

---

## Executive Summary

This plan outlines the upgrade of the SwishApi solution from .NET 7.0 to .NET 10.0. The solution consists of 2 projects with a simple dependency structure (SwishApiConsoleTest depends on SwishApi). The assessment identified minimal complexity with only 1 NuGet package requiring an update and no API compatibility issues.

**Key Metrics:**
- Total Projects: 2
- Total NuGet Packages: 2 (1 requires update)
- Total Code Files: 28
- Total Lines of Code: 2,177
- Estimated LOC to Modify: 0+ (minimal changes expected)
- Overall Difficulty: 🟢 Low

**Critical Findings:**
- Both projects are already SDK-style projects
- No security vulnerabilities identified
- No API compatibility issues detected
- Single package update recommended (Newtonsoft.Json 13.0.3 → 13.0.4)
- RestSharp 112.0.0 is already compatible

---

## Upgrade Strategy: All-At-Once

**Rationale for All-At-Once Approach:**

1. **Small Solution Size**: Only 2 projects with clear dependency relationship
2. **Low Complexity**: No complex dependency chains or circular dependencies
3. **Modern Codebase**: Both projects already on .NET 7.0 and SDK-style
4. **Minimal Risk**: Low difficulty rating for both projects, no API breaking changes
5. **Simple Dependencies**: Only 2 NuGet packages, 1 needing minor version bump
6. **Quick Execution**: Can complete upgrade in single coordinated operation

This solution is an ideal candidate for All-At-Once strategy, allowing us to upgrade both projects simultaneously and test the entire solution as a unit.

---

## Dependency Analysis

### Project Relationship Graph

```
SwishApiConsoleTest (DotNetCoreApp)
    └─> SwishApi (ClassLibrary)
```

**Dependency Order:**
- **Tier 1 (Leaf)**: SwishApi - No project dependencies
- **Tier 2 (Root)**: SwishApiConsoleTest - Depends on SwishApi

Since we're using All-At-Once strategy, both projects will be upgraded simultaneously, but validation should follow the dependency order (SwishApi first, then SwishApiConsoleTest).

---

## Project-by-Project Plans

### 1. SwishApi (ClassLibrary)

**Current State:**
- Target Framework: net7.0
- Type: ClassLibrary, SDK-style
- Lines of Code: ~2,000 (estimated from project metrics)
- Difficulty: 🟢 Low
- Package Issues: 1
- API Issues: 0

**Target State:**
- Target Framework: net10.0

**Changes Required:**

#### 1.1 Update Target Framework
- File: `SwishApi/SwishApi.csproj`
- Change: `<TargetFramework>net7.0</TargetFramework>` → `<TargetFramework>net10.0</TargetFramework>`

#### 1.2 Update NuGet Packages
- **Newtonsoft.Json**: 13.0.3 → 13.0.4
  - Reason: NuGet package upgrade recommended
  - Impact: Minor version bump, no breaking changes expected
  - Action: Update PackageReference version in SwishApi.csproj

#### 1.3 Restore Dependencies
- Run `dotnet restore SwishApi/SwishApi.csproj`
- Verify no package conflicts

#### 1.4 Build Verification
- Run `dotnet build SwishApi/SwishApi.csproj`
- Verify zero errors
- Verify zero warnings

**Risk Level**: 🟢 Low
- Small package update with no known breaking changes
- No API compatibility issues identified
- Modern, maintainable codebase

---

### 2. SwishApiConsoleTest (Console Application)

**Current State:**
- Target Framework: net7.0
- Type: DotNetCoreApp, SDK-style
- Lines of Code: ~177 (estimated from project metrics)
- Difficulty: 🟢 Low
- Package Issues: 0
- API Issues: 0

**Target State:**
- Target Framework: net10.0

**Changes Required:**

#### 2.1 Update Target Framework
- File: `SwishApiConsoleTest/SwishApiConsoleTest.csproj`
- Change: `<TargetFramework>net7.0</TargetFramework>` → `<TargetFramework>net10.0</TargetFramework>`

#### 2.2 Verify Dependencies
- This project depends on SwishApi (already upgraded)
- RestSharp 112.0.0 is compatible, no changes needed

#### 2.3 Restore Dependencies
- Run `dotnet restore SwishApiConsoleTest/SwishApiConsoleTest.csproj`
- Verify no package conflicts

#### 2.4 Build Verification
- Run `dotnet build SwishApiConsoleTest/SwishApiConsoleTest.csproj`
- Verify zero errors
- Verify zero warnings

**Risk Level**: 🟢 Low
- No package updates required
- Depends on upgraded SwishApi library
- Console application with straightforward dependencies

---

## Package Update Reference

| Package | Current Version | Target Version | Projects | Priority | Notes |
|---------|----------------|----------------|----------|----------|-------|
| Newtonsoft.Json | 13.0.3 | 13.0.4 | SwishApi.csproj | Normal | Minor version bump, recommended upgrade |
| RestSharp | 112.0.0 | - | SwishApi.csproj | N/A | ✅ Already compatible, no update needed |

---

## Breaking Changes Catalog

### .NET 7.0 → .NET 10.0 Breaking Changes

Based on the assessment, **no API compatibility issues were detected** in this codebase. However, here are general categories to monitor during execution:

#### Potential Areas (None Currently Detected)
1. **API Changes**: No binary, source, or behavioral incompatibilities found
2. **Package Compatibility**: All packages compatible or have clear upgrade paths
3. **Runtime Behavior**: No known behavioral changes affecting this codebase

#### Known .NET 10 Changes (Reference)
While none affect this codebase currently, developers should be aware of:
- Obsolete API warnings may appear (SYSLIB series)
- Default security configurations may be stricter
- Performance optimizations may change runtime behavior slightly

**Action**: Monitor compiler warnings during build phase and address any that appear.

---

## Testing Strategy

### Multi-Level Testing Approach

#### Level 1: Per-Project Testing

**SwishApi Project:**
- [ ] Project file updated to net10.0
- [ ] Newtonsoft.Json updated to 13.0.4
- [ ] `dotnet restore` succeeds without errors
- [ ] `dotnet build` succeeds without errors
- [ ] `dotnet build` produces zero warnings
- [ ] No package dependency conflicts

**SwishApiConsoleTest Project:**
- [ ] Project file updated to net10.0
- [ ] Correctly references upgraded SwishApi
- [ ] `dotnet restore` succeeds without errors
- [ ] `dotnet build` succeeds without errors
- [ ] `dotnet build` produces zero warnings

#### Level 2: Solution-Wide Testing

- [ ] `dotnet restore SwishApi.sln` succeeds
- [ ] `dotnet build SwishApi.sln` succeeds with zero errors
- [ ] No dependency conflicts across projects
- [ ] All projects targeting net10.0
- [ ] Console application runs successfully

#### Level 3: Functional Validation

- [ ] SwishApiConsoleTest application executes without runtime errors
- [ ] API calls through SwishApi library function correctly
- [ ] RestSharp HTTP client operations work as expected
- [ ] JSON serialization/deserialization via Newtonsoft.Json works correctly
- [ ] No unexpected exceptions or warnings at runtime

---

## Risk Management

### Risk Assessment

**Overall Risk**: 🟢 Low

#### Identified Risks

1. **Package Update Risk** - 🟢 Low
   - **Description**: Newtonsoft.Json 13.0.3 → 13.0.4 is minor version bump
   - **Likelihood**: Low
   - **Impact**: Low
   - **Mitigation**: Version 13.0.4 is a patch/minor release, typically only bug fixes
   - **Rollback**: Revert csproj change if issues arise

2. **Framework Compatibility Risk** - 🟢 Low
   - **Description**: .NET 7.0 → .NET 10.0 is major version jump (3 versions)
   - **Likelihood**: Low (assessment found no API issues)
   - **Impact**: Low to Medium
   - **Mitigation**: No API breaking changes detected; both projects are modern SDK-style
   - **Rollback**: Git revert to restore net7.0

3. **Runtime Behavior Risk** - 🟢 Low
   - **Description**: Subtle runtime behavior changes in .NET 10
   - **Likelihood**: Low
   - **Impact**: Low
   - **Mitigation**: Thorough functional testing of console application
   - **Rollback**: Git revert if unexpected behavior

### Mitigation Strategies

1. **Build Validation First**
   - Ensure clean builds before functional testing
   - Address any compiler warnings immediately
   - Verify package restore succeeds

2. **Incremental Verification**
   - Test SwishApi build first
   - Then test SwishApiConsoleTest build
   - Finally test solution-wide build
   - Run functional tests last

3. **Rollback Plan**
   - All changes in version control
   - Clean git history for easy revert
   - Document any issues encountered
   - Can revert individual projects if needed

---

## Execution Sequence

### Phase 1: Project File Updates (Simultaneous)

**Actions:**
1. Update `SwishApi/SwishApi.csproj`:
   - Change TargetFramework to net10.0
   - Update Newtonsoft.Json to 13.0.4
   
2. Update `SwishApiConsoleTest/SwishApiConsoleTest.csproj`:
   - Change TargetFramework to net10.0

**Duration**: 5 minutes  
**Validation**: Visual inspection of changes

---

### Phase 2: Dependency Restoration

**Actions:**
1. Run `dotnet restore SwishApi.sln`
2. Verify no package resolution errors
3. Verify Newtonsoft.Json 13.0.4 restored
4. Verify RestSharp 112.0.0 compatible

**Duration**: 2 minutes  
**Validation**: Successful restore, no errors

---

### Phase 3: Build Verification (Dependency Order)

**Actions:**
1. Build SwishApi: `dotnet build SwishApi/SwishApi.csproj`
   - Verify zero errors
   - Verify zero warnings
   
2. Build SwishApiConsoleTest: `dotnet build SwishApiConsoleTest/SwishApiConsoleTest.csproj`
   - Verify zero errors
   - Verify zero warnings
   
3. Build entire solution: `dotnet build SwishApi.sln`
   - Verify zero errors
   - Verify zero warnings

**Duration**: 5 minutes  
**Validation**: All builds successful, clean output

---

### Phase 4: Functional Testing

**Actions:**
1. Run SwishApiConsoleTest application
2. Verify no runtime exceptions
3. Test API interactions through SwishApi
4. Verify HTTP operations via RestSharp
5. Verify JSON operations via Newtonsoft.Json
6. Check for any obsolete API warnings

**Duration**: 10 minutes  
**Validation**: Application runs successfully, no unexpected behavior

---

### Phase 5: Final Validation and Commit

**Actions:**
1. Review all changes in git
2. Verify both projects targeting net10.0
3. Verify package versions correct
4. Run final solution build
5. Commit changes with descriptive message
6. Document any observations or warnings

**Duration**: 5 minutes  
**Validation**: Clean git commit, all tests passing

---

## Success Criteria

The upgrade will be considered successful when ALL of the following are met:

### Technical Criteria
- [ ] SwishApi.csproj targets `net10.0`
- [ ] SwishApiConsoleTest.csproj targets `net10.0`
- [ ] Newtonsoft.Json updated to 13.0.4
- [ ] `dotnet restore SwishApi.sln` succeeds without errors
- [ ] `dotnet build SwishApi.sln` succeeds without errors
- [ ] `dotnet build SwishApi.sln` produces zero warnings
- [ ] No NuGet package conflicts
- [ ] No dependency resolution issues

### Functional Criteria
- [ ] SwishApiConsoleTest application runs without exceptions
- [ ] API operations function correctly
- [ ] HTTP client operations work as expected
- [ ] JSON serialization/deserialization functions properly
- [ ] No unexpected runtime warnings or errors

### Quality Criteria
- [ ] All changes committed to version control
- [ ] Clean git history with descriptive commit messages
- [ ] No security vulnerabilities remain (none found in assessment)
- [ ] Code builds cleanly in CI/CD environment (if applicable)

---

## Timeline Estimate

| Phase | Duration | Dependencies |
|-------|----------|--------------|
| Project File Updates | 5 min | None |
| Dependency Restoration | 2 min | Phase 1 |
| Build Verification | 5 min | Phase 2 |
| Functional Testing | 10 min | Phase 3 |
| Final Validation | 5 min | Phase 4 |
| **Total** | **~30 min** | Sequential execution |

**Notes:**
- Timeline assumes no unexpected issues
- Add buffer time for investigation if warnings appear
- Functional testing may take longer depending on application complexity

---

## Rollback Strategy

### Immediate Rollback (If Major Issues Found)

**Trigger Conditions:**
- Build failures that cannot be quickly resolved
- Critical runtime errors
- Data corruption or loss
- Incompatible package versions causing conflicts

**Rollback Steps:**
1. `git status` - Review uncommitted changes
2. `git checkout SwishApi/SwishApi.csproj SwishApiConsoleTest/SwishApiConsoleTest.csproj` - Revert project files
3. `dotnet restore SwishApi.sln` - Restore original packages
4. `dotnet build SwishApi.sln` - Verify rollback successful
5. Document issues encountered

**Duration**: 5 minutes

### Partial Rollback (If Single Project Issues)

If one project has issues but the other is stable:
1. Revert only the problematic project's .csproj file
2. Investigate and address the specific issue
3. Retry upgrade for that project once resolved

---

## Post-Upgrade Monitoring

### Items to Monitor

1. **Build Performance**
   - Note any significant build time changes
   - Monitor for new warnings over time

2. **Runtime Performance**
   - Application startup time
   - Memory usage patterns
   - HTTP operation latency

3. **Security**
   - Monitor for new security advisories for .NET 10
   - Watch for new vulnerability reports in dependencies

4. **Compatibility**
   - Track any reports of issues from users
   - Monitor for .NET 10-specific bugs in issue trackers

---

## Notes and Considerations

### Positive Factors
- ✅ Already on modern .NET (7.0)
- ✅ Both projects SDK-style (no conversion needed)
- ✅ Small, focused solution
- ✅ No API compatibility issues
- ✅ No security vulnerabilities
- ✅ Simple dependency structure
- ✅ Minimal package updates required

### Areas of Attention
- ⚠️ Monitor for any .NET 10-specific warnings during build
- ⚠️ Verify RestSharp HTTP operations work correctly in .NET 10
- ⚠️ Test Newtonsoft.Json serialization patterns thoroughly
- ⚠️ Check for any obsolete API warnings (SYSLIB series)

### Future Considerations
- Consider migrating from Newtonsoft.Json to System.Text.Json (native .NET JSON library) for better performance
- Keep dependencies up to date with .NET 10 lifecycle
- Monitor .NET 11 preview releases for future upgrade planning

---

## Conclusion

This upgrade plan provides a comprehensive roadmap for migrating the SwishApi solution from .NET 7.0 to .NET 10.0. The low-risk nature of this upgrade, combined with the simple project structure and minimal dependencies, makes this an ideal candidate for the All-At-Once strategy.

**Key Success Factors:**
1. Clear dependency structure
2. No API breaking changes detected
3. Minimal package updates required
4. Modern SDK-style projects
5. Comprehensive testing strategy

**Next Steps:**
- Review and approve this plan
- Proceed to Execution stage
- Follow phases sequentially
- Validate at each checkpoint
- Monitor for any unexpected issues

---

*This plan supports the Execution stage of the .NET 10.0 upgrade workflow.*
