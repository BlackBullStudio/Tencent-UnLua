# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **UnLua**, a highly optimized Lua scripting solution for Unreal Engine by Tencent. UnLua allows direct access to all UE reflection system types (UCLASS, UPROPERTY, UFUNCTION, USTRUCT, UENUM) from Lua without glue code, and enables overriding Blueprint events, replication notifications, animation notifications, and input events.

**Current Engine Version:** Unreal Engine 5.6  
**Supported Platforms:** Windows, Android, iOS, Linux, OSX  
**Supported Engine Versions:** UE 4.17.x - UE 5.x

## Architecture

### Core Components

- **UnLua Plugin** (`Plugins/UnLua/`): Main runtime plugin containing three modules:
  - `UnLua`: Core runtime module with Lua integration
  - `UnLuaEditor`: Editor extensions for Lua template generation and IntelliSense
  - `UnLuaDefaultParamCollector`: Program module for collecting UFUNCTION default parameters

- **UnLuaTestSuite Plugin** (`Plugins/UnLuaTestSuite/`): Comprehensive test suite with spec-based tests and regression tests

- **UnLuaExtensions Plugin** (`Plugins/UnLuaExtensions/`): Extension examples including:
  - `LuaSocket`: Socket library integration
  - `LuaRapidjson`: JSON processing library
  - `LuaProtobuf`: Protocol buffer support

- **TPSProject**: Example third-person shooter project demonstrating UnLua integration

### Lua Script Organization

All Lua scripts are located in `Content/Script/` with the following structure:
- `Content/Script/Tutorials/`: Tutorial examples (01-13 covering core features)
- `Content/Script/Tests/`: Test scripts including benchmarks, bindings, and regression tests
- Blueprint-specific Lua files follow naming convention: `<BlueprintName>_C.lua`

### Key Integration Points

- **Static Binding**: Blueprints implement `UnLuaInterface` and return Lua module path in `GetModuleName`
- **Dynamic Binding**: Runtime binding without Blueprint modifications
- **Template Generation**: UnLua toolbar in Blueprint editor provides "Create Lua Template" functionality
- **Console Commands**: Runtime debugging with `lua.do`, `lua.dofile`, `lua.gc` commands

## Development Commands

### Building
```bash
# Generate Visual Studio project files
# Open TPSProject.uproject in UE Editor to build
```

### Testing
```bash
# Run automated tests via UE Editor's Automation Tool:
# Window -> Developer Tools -> Automation
# Search for "UnLua" to run all UnLua-related tests
```

### Console Commands (In-Game)
- `lua.do <code>`: Execute Lua code in default environment
- `lua.dofile <module path>`: Execute specified Lua file
- `lua.gc`: Force garbage collection

### Editor Workflow
1. Create Blueprint and implement `UnLuaInterface`
2. Return Lua file path in `GetModuleName` (relative to `Content/Script/`)
3. Use UnLua toolbar "Create Lua Template" to generate boilerplate
4. Edit generated `.lua` file in `Content/Script/`

## Testing Structure

The project uses UE's Automation Testing framework with two main approaches:

1. **Spec Tests** (`Plugins/UnLuaTestSuite/Source/UnLuaTestSuite/Private/Specs/`):
   - Feature-based tests using `BEGIN_DEFINE_SPEC`/`END_DEFINE_SPEC`
   - Test individual API components (LuaLib_Array, LuaLib_Delegate, etc.)
   - Chinese language support in test descriptions

2. **Regression Tests** (`Content/Script/Tests/Regression/`):
   - Issue-specific tests named by GitHub issue numbers (Issue276, Issue279, etc.)
   - Both C++ test classes and corresponding Lua scripts

## Module Dependencies

Key dependency relationships:
- Main project depends on `UnLua` and `Lua` modules
- Test suite depends on `UnLua`, `Lua`, and `UMG` modules
- Editor-specific features require `UnLuaEditor` module
- Extensions are optional and can be disabled individually

## Important File Patterns

- `*.Build.cs`: Unreal Build Tool configuration files
- `*.uplugin`: Plugin descriptor files
- `*.spec.cpp`: Automation test specifications
- `*_C.lua`: Blueprint-bound Lua implementation files
- `Content/Script/Tutorials/*.lua`: Feature demonstration scripts

## Localization

The project includes comprehensive localization support:
- `Content/Localization/UnLua/`: Main localization assets
- `Plugins/UnLua/Content/Localization/UnLua/`: Plugin-specific localization
- Supports Chinese (zh-Hans) and English

## Performance Considerations

UnLua is highly optimized for:
- UFUNCTION invocation with persistent parameter caching
- Container access (TArray, TSet, TMap) without Lua table conversion
- Structure creation, access, and garbage collection
- Custom static export of classes, functions, and enums