# CLAUDE.md - UnLua Extensions Plugin

This file provides guidance to Claude Code (claude.ai/code) when working with the UnLua extensions plugin.

## Plugin Overview

**UnLuaExtensions** provides extension examples and third-party library integrations for UnLua, demonstrating how to extend UnLua functionality with external libraries.

**Location:** `Plugins/UnLuaExtensions/`

## Architecture

### Extension Modules

#### 1. LuaSocket (`LuaSocket/`)
Socket library integration for network communication:
- **Purpose**: Provides TCP/UDP socket functionality to Lua scripts
- **Use Cases**: Network debugging, HTTP requests, custom protocols
- **Dependencies**: Cross-platform socket implementation
- **Lua Scripts**: `Content/Script/socket/`, `ltn12.lua`, `mime.lua`

**Key Features:**
- TCP client/server sockets
- UDP datagram sockets
- HTTP client functionality
- MIME encoding/decoding
- Non-blocking I/O operations

#### 2. LuaRapidjson (`LuaRapidjson/`)
High-performance JSON processing library:
- **Purpose**: Fast JSON parsing and generation in Lua
- **Use Cases**: API communication, configuration files, data serialization
- **Dependencies**: RapidJSON C++ library
- **Performance**: Optimized for speed and memory efficiency

**Key Features:**
- JSON document parsing and generation
- Schema validation
- DOM and SAX parsing modes
- UTF-8 encoding support
- Memory-efficient operations

#### 3. LuaProtobuf (`LuaProtobuf/`)
Protocol buffer support for structured data:
- **Purpose**: Protocol buffer serialization/deserialization
- **Use Cases**: Efficient binary data exchange, API communication
- **Dependencies**: Protocol buffer library
- **Lua Scripts**: `Content/Script/protoc.lua`, `serpent.lua`

**Key Features:**
- Protocol buffer message encoding/decoding
- Dynamic message creation
- Schema compilation
- Binary serialization format

## Directory Structure

### Per-Extension Layout
Each extension follows a consistent structure:
```
ExtensionName/
├── ExtensionName.uplugin          # Plugin descriptor
├── Source/                        # C++ source code
│   ├── ExtensionName.Build.cs    # Build configuration
│   ├── Private/                   # Implementation files
│   ├── Public/                    # Header files
│   └── src/                       # Third-party source (if included)
├── Content/Script/                # Lua utility scripts
├── Binaries/                      # Compiled binaries
└── Intermediate/                  # Build artifacts
```

### Build System Integration
- Each extension is a separate UE plugin
- Can be enabled/disabled independently
- Optional dependencies for UnLua core functionality

## Development Guidelines

### Creating New Extensions

#### 1. Plugin Setup
- Copy existing extension as template
- Update `.uplugin` file with new metadata
- Modify `Build.cs` file for dependencies
- Update module names and includes

#### 2. C++ Integration
- Implement module initialization/cleanup
- Register Lua functions and types
- Handle memory management properly
- Follow UE coding standards

#### 3. Lua Integration
- Provide Lua wrapper scripts if needed
- Include usage examples and documentation
- Ensure proper error handling
- Test with various data sizes and types

### Third-Party Library Integration

#### Library Selection Criteria
- Cross-platform compatibility
- Performance characteristics
- License compatibility
- Maintenance status
- Community support

#### Integration Process
1. **Source Integration**: Include source in `src/` directory
2. **Build Configuration**: Update `.Build.cs` with include paths
3. **API Wrapping**: Create C++ wrapper functions
4. **Lua Binding**: Register functions with Lua state
5. **Testing**: Validate functionality across platforms

## Extension Usage

### Prerequisites
- UnLua core plugin must be enabled
- Individual extensions can be enabled as needed
- Some extensions may require additional setup

### In Lua Scripts

#### LuaSocket Example
```lua
local socket = require("socket")
local client = socket.tcp()
client:connect("www.example.com", 80)
client:send("GET / HTTP/1.1\r\nHost: www.example.com\r\n\r\n")
local response = client:receive("*a")
client:close()
```

#### LuaRapidjson Example
```lua
local rapidjson = require("rapidjson")
local data = {name = "test", value = 42}
local json_str = rapidjson.encode(data)
local parsed = rapidjson.decode(json_str)
```

#### LuaProtobuf Example
```lua
local pb = require("pb")
-- Load schema and encode/decode messages
```

## Performance Considerations

### Memory Management
- Extensions handle their own memory allocation
- Proper cleanup on Lua state destruction
- Avoid memory leaks in C++ wrapper code

### Performance Optimization
- Use efficient data structures
- Minimize Lua-C++ boundary crossings
- Cache frequently used objects
- Profile critical code paths

## Platform Support

### Supported Platforms
- **Windows**: Full support for all extensions
- **Android**: Limited to compatible libraries
- **iOS**: May require additional configuration
- **Linux**: Generally supported
- **macOS**: Platform-specific considerations

### Platform-Specific Notes
- Socket operations may require platform permissions
- Some libraries may have platform limitations
- Mobile platforms may have restricted network access

## Debugging Extensions

### Common Issues
- Missing library dependencies
- Incorrect Lua module paths
- Memory management errors
- Platform compatibility problems

### Debugging Tools
- UE logging system for C++ code
- Lua print statements and error handling
- Platform-specific debugging tools
- Memory leak detection

### Testing
- Unit tests for individual functions
- Integration tests with UnLua core
- Performance benchmarks
- Cross-platform validation

## Extension Examples

### Network Communication
```lua
-- HTTP request using LuaSocket
local http = require("socket.http")
local body, code = http.request("http://api.example.com/data")
```

### JSON Processing
```lua
-- Parse game configuration
local config_json = LoadTextFromFile("config.json")
local config = rapidjson.decode(config_json)
UpdateGameSettings(config.graphics, config.audio)
```

### Binary Data Exchange
```lua
-- Serialize player data
local player_data = {level = 10, score = 1500}
local binary_data = pb.encode("PlayerData", player_data)
SendToServer(binary_data)
```

## Future Extensions

### Potential Additions
- Database connectivity (SQLite, etc.)
- Cryptography libraries
- Image processing
- Audio processing
- File compression

### Community Contributions
- Extension template for developers
- Documentation guidelines
- Testing requirements
- License considerations

## Related Files

- Main project CLAUDE.md: `../../CLAUDE.md`
- UnLua core plugin: `../UnLua/CLAUDE.md`
- UnLuaTestSuite plugin: `../UnLuaTestSuite/CLAUDE.md`