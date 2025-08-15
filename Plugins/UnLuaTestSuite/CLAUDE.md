# CLAUDE.md - UnLua Test Suite Plugin

This file provides guidance to Claude Code (claude.ai/code) when working with the UnLua test suite plugin.

## Plugin Overview

**UnLuaTestSuite** is a comprehensive testing plugin that validates UnLua functionality through automated tests, specifications, and regression testing.

**Location:** `Plugins/UnLuaTestSuite/`

## Architecture

### Test Categories

#### 1. Specification Tests (`Source/UnLuaTestSuite/Private/Specs/`)
Feature-based tests using UE's `BEGIN_DEFINE_SPEC`/`END_DEFINE_SPEC` framework:
- `LuaEnv.spec.cpp`: Lua environment management
- `LuaLib_*.spec.cpp`: Individual API component tests
  - Array, Set, Map container operations
  - Delegate and MulticastDelegate functionality
  - Math types (FVector, FQuat, FRotator, FTransform, etc.)
  - Color types (FColor, FLinearColor)
  - Key input handling
  - Object and World access
- `UnLuaBase.spec.cpp`: Core UnLua functionality
- `UnLuaEnum.spec.cpp`: Enumeration handling
- `UELib.spec.cpp`: UE library integration

#### 2. Regression Tests (`Source/UnLuaTestSuite/Private/Tests/`)
Issue-specific tests named by GitHub issue numbers:
- `Issue276Test.cpp` through `Issue668Test.cpp`: Specific bug fixes and feature additions
- Each test corresponds to a GitHub issue and validates the fix
- Tests include both C++ test classes and corresponding Lua scripts

#### 3. Performance Tests (`Source/UnLuaTestSuite/Private/Perfs/`)
- `UnLuaBenchmarkFunctionLibrary.cpp`: Performance comparison utilities
- `UnLuaBenchmarkProxy.cpp`: Proxy objects for benchmarking

### Test Assets

#### Blueprint Test Assets (`Content/Tests/`)
- `Benchmark/`: Performance comparison blueprints
- `Binding/`: Static and dynamic binding test blueprints
- `Misc/`: Utility test assets (data tables, enums, structs)
- `Regression/Issue*/`: Assets for specific issue regression tests

#### Lua Test Scripts
Located alongside blueprint assets, following naming convention:
- Blueprint-specific: `<BlueprintName>_C.lua`
- Issue-specific: `Issue*/TestActor.lua`, `Issue*/TestUMG.lua`, etc.

## Test Framework Integration

### UE Automation System
Tests integrate with UE's built-in automation testing framework:
- Run via `Window -> Developer Tools -> Automation`
- Search for "UnLua" to run all UnLua-related tests
- Automated CI/CD integration capability

### Test Execution
```bash
# Via UE Editor Automation Tool
# 1. Open UE Editor
# 2. Window -> Developer Tools -> Automation
# 3. Search "UnLua"
# 4. Run selected tests
```

### Test Categories by Type

#### API Validation Tests
- Verify correct parameter passing and return values
- Test memory management and object lifetime
- Validate type conversion between Lua and UE

#### Binding Tests
- Static binding through UnLuaInterface
- Dynamic binding for runtime-spawned objects
- Blueprint event overriding functionality

#### Regression Tests
- Ensure previously fixed issues remain resolved
- Cover edge cases and specific use patterns
- Validate compatibility across UE versions

## Development Guidelines

### Adding New Tests

#### For New Features
1. Create specification test in `Specs/` directory
2. Follow naming convention: `LuaLib_[FeatureName].spec.cpp`
3. Use UE's spec framework macros
4. Include both positive and negative test cases

#### For Bug Fixes
1. Create issue-specific test in `Tests/` directory
2. Name after GitHub issue: `Issue[Number]Test.cpp`
3. Create corresponding test assets if needed
4. Include Lua scripts to reproduce the issue

### Test Asset Organization
- Group related tests in appropriate subdirectories
- Use descriptive naming for easy identification
- Include both C++ and Blueprint test variants when applicable
- Provide Lua scripts that demonstrate the issue/feature

### Performance Testing
- Use benchmark utilities for performance-critical features
- Compare UnLua performance against native Blueprint/C++
- Measure memory usage and execution time
- Document performance characteristics

## Test Coverage

### Core Functionality
- ✅ Lua environment initialization and cleanup
- ✅ Blueprint event overriding
- ✅ Static and dynamic binding
- ✅ Container operations (TArray, TSet, TMap)
- ✅ Delegate and multicast delegate handling
- ✅ Math type operations
- ✅ Memory management and garbage collection

### Integration Points
- ✅ UE reflection system access
- ✅ Blueprint compilation integration
- ✅ Editor workflow integration
- ✅ Console command functionality
- ✅ Hot reload capabilities

### Platform Coverage
- ✅ Windows development environment
- ✅ Editor and runtime functionality
- 🔲 Mobile platform testing (manual)
- 🔲 Console platform testing (manual)

## Running Tests

### Prerequisites
- UnLua plugin must be enabled
- UnLuaTestSuite plugin must be enabled
- UE Editor with Developer Tools access

### Execution Methods

#### Full Test Suite
1. Open Automation window
2. Search for "UnLua"
3. Select all tests
4. Click "Run Selected Tests"

#### Specific Test Categories
- Search "UnLua.Spec" for specification tests
- Search "UnLua.Issue" for regression tests
- Search "UnLua.Benchmark" for performance tests

#### Individual Tests
- Navigate to specific test in automation tree
- Double-click to run individual test
- View results in automation log

## Continuous Integration

The test suite is designed to integrate with CI/CD pipelines:
- Automation tests can be run via command line
- Test results can be exported in standard formats
- Performance benchmarks track regression over time

## Related Files

- Main project CLAUDE.md: `../../CLAUDE.md`
- UnLua core plugin: `../UnLua/CLAUDE.md`
- UnLuaExtensions plugin: `../UnLuaExtensions/CLAUDE.md`