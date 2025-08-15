# CLAUDE.md - UnLua Core Plugin

This file provides guidance to Claude Code (claude.ai/code) when working with the UnLua core plugin.

## Plugin Overview

**UnLua** is the main runtime plugin providing core Lua scripting functionality for Unreal Engine. This plugin contains the essential components for Lua-UE integration.

**Location:** `Plugins/UnLua/`

## Architecture

### Modules

- **UnLua**: Core runtime module with Lua integration
  - Lua VM management and state handling
  - UE reflection system bindings (UCLASS, UPROPERTY, UFUNCTION, USTRUCT, UENUM)
  - Blueprint event overriding mechanism
  - Dynamic and static binding systems
  - Memory management and garbage collection

- **UnLuaEditor**: Editor extensions for development workflow
  - Lua template generation and customization
  - IntelliSense data export for IDE integration
  - Blueprint toolbar integration
  - Hot reload functionality
  - Console command support

- **UnLuaDefaultParamCollector**: Program module for collecting UFUNCTION default parameters
  - Automated parameter collection during compilation
  - Default value caching for Lua function calls

### Key Directories

- `Source/`: C++ source code for all modules
- `Content/Script/UnLua/`: Core Lua utility scripts
- `Config/LuaTemplates/`: Default Lua templates for different UE classes
- `Resources/`: Plugin icons and UI assets
- `Binaries/`: Compiled plugin binaries

### Important Files

- `UnLua.uplugin`: Plugin descriptor
- `Source/UnLua/UnLua.Build.cs`: Main module build configuration
- `Source/UnLuaEditor/UnLuaEditor.Build.cs`: Editor module build configuration
- `Content/Script/UnLua/`: Core Lua libraries (Input, HotReload, etc.)

## Development Guidelines

### C++ Development
- Main runtime code in `Source/UnLua/`
- Editor-specific code in `Source/UnLuaEditor/`
- Follow UE coding standards and naming conventions
- Use reflection macros appropriately for UE integration

### Lua Integration
- Blueprint binding through `UnLuaInterface`
- Lua scripts follow naming convention: `<BlueprintName>_C.lua`
- Runtime binding without Blueprint modifications via dynamic binding
- Template generation for rapid prototyping

### Editor Integration
- Toolbar integration in Blueprint editor
- Template generation system
- IntelliSense export for external IDEs
- Hot reload for development iteration

## Build System

### Dependencies
- Core UE modules: Engine, CoreUObject, UMG
- Editor dependencies: UnrealEd, ToolMenus, EditorStyle
- Lua 5.4.x (bundled in ThirdParty)

### Configuration Options
Available through `UnLua.Build.cs`:
- `AUTO_UNLUA_STARTUP`: Automatic UnLua environment startup
- `WITH_UE4_NAMESPACE`: Unified UE namespace access
- `SUPPORTS_RPC_CALL`: RPC method support
- `ENABLE_TYPE_CHECK`: Parameter type validation
- `UNLUA_ENABLE_DEBUG`: Detailed debug logging

## Plugin Features

### Core Functionality
- Zero-glue-code access to UE reflection system
- Blueprint event overriding (BlueprintImplementableEvent, BlueprintNativeEvent)
- Replication notification overrides
- Animation notification overrides
- Input event handling

### Performance Optimizations
- UFUNCTION invocation with persistent parameter caching
- Container access (TArray, TSet, TMap) without Lua table conversion
- Structure creation, access, and garbage collection optimizations
- Custom static export system for frequently used classes

### Developer Tools
- Lua template generation system
- IntelliSense data export
- Console commands for runtime debugging
- Hot reload for rapid iteration

## Testing

Tests for core functionality are located in the UnLuaTestSuite plugin. Core plugin functionality is validated through:
- API specification tests
- Blueprint binding tests
- Memory management tests
- Performance benchmarks

## Related Files

- Main project CLAUDE.md: `../../CLAUDE.md`
- UnLuaTestSuite plugin: `../UnLuaTestSuite/CLAUDE.md`
- UnLuaExtensions plugin: `../UnLuaExtensions/CLAUDE.md`