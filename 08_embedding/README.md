# Kryon EmbedView Examples

This directory contains examples demonstrating the EmbedView system in Kryon, which provides a unified way to embed external content and viewers into Kryon applications.

## What is EmbedView?

EmbedView is Kryon's extensible system for embedding external content types. Unlike Canvas (which provides a universal drawing API), EmbedView is designed for optional and specialized content providers like:

- **WebView**: Web content via browser engines
- **WASM**: WebAssembly modules
- **Native Renderers**: Custom native rendering engines (Raylib, OpenGL, etc.)
- **Emulators**: Game console emulators (GBA, DOS, UXN)
- **Development Tools**: Code editors, terminals, model viewers

## Examples Overview

### Basic Examples

- **`basic_embed_view.kry`** - Simple demonstration of WebView, WASM, and Native renderer
- **`simple_native_demo.kry`** - Native rendering with Raylib via EmbedView

### Advanced Examples  

- **`advanced_embed_view.kry`** - Multiple provider types with different configurations
- **`full_bundle_demo.kry`** - Comprehensive example using many provider types
- **`unified_native_renderer.kry`** - Examples of unified native rendering approaches

### Bundle Optimization Examples

- **`bundle_optimization_demo.kry`** - Shows selective provider inclusion (WebView + Canvas only)
- **`minimal_bundle_demo.kry`** - Minimal app with no EmbedView or Canvas for smallest bundle

### Development Resources

- **`EMBEDVIEW_PROVIDER_GUIDE.md`** - Complete guide for creating new ViewProviders
- **`templates/`** - Template files for creating new providers
  - `simple_provider_template.kry` - Basic .kry file template
  - `provider_rust_template.rs` - Complete Rust provider implementation template

## Quick Start

### Using an Existing Provider

```kry
EmbedView {
    id: "my_content"
    type: "webview"  // or any supported type
    source: "https://example.com"
    width: 400
    height: 300
    
    config: {
        "javascript_enabled": true
        "allow_navigation": false
    }
}
```

### Supported Provider Types

- `webview` - Web content via WebView
- `native_renderer` - Native rendering engines
- `wasm_view` - WebAssembly modules  
- `uxn_view` - UXN virtual machine
- `gba_view` - Game Boy Advance emulator
- `dos_view` - DOS emulator
- `code_editor` - Code editor component
- `terminal` - Terminal emulator
- `model_viewer` - 3D model viewer

## Bundle Optimization

The Kryon bundler automatically performs tree-shaking to include only the providers your application actually uses:

- **Full bundle**: ~6.9MB (all providers included)
- **Selective bundle**: ~1.5MB (only used providers)
- **Minimal bundle**: ~230KB (no EmbedView/Canvas)

This means you only pay for what you use!

## Creating New Providers

See `EMBEDVIEW_PROVIDER_GUIDE.md` for a complete guide on creating new ViewProviders. The process involves:

1. Adding your type to the `EmbedViewType` enum
2. Implementing the `ViewProvider` trait
3. Registering your provider in the provider registry
4. Testing with example applications

## Compiling Examples

From the project root:

```bash
# Compile a single example
./kryon-compiler/target/release/kryc kryon-examples/08_embedding/basic_embed_view.kry kryon-examples/08_embedding/basic_embed_view.krb

# Compile all examples
find kryon-examples/08_embedding -name "*.kry" -not -path "*/templates/*" -exec ./kryon-compiler/target/release/kryc {} {}.krb \;
```

## Architecture Notes

### EmbedView vs Canvas

- **Canvas**: Universal drawing API for 2D/3D graphics, built into core runtime
- **EmbedView**: Extensible system for specialized content providers, optional and tree-shaken

### Provider Lifecycle

1. **Initialize**: Provider receives configuration options
2. **Load Content**: Provider loads/prepares content from source
3. **Render**: Provider renders content to RGBA pixel buffer
4. **Handle Events**: Provider processes user input events
5. **Cleanup**: Provider releases resources when destroyed

### Performance Considerations

- Providers render to pixel buffers that are composited by the main renderer
- Event handling is routed through the main event system
- Providers can implement caching and incremental updates
- Large providers are automatically excluded from bundles when unused

## Contributing

When contributing new providers or examples:

1. Follow the existing code patterns and style
2. Add comprehensive examples demonstrating your provider
3. Update this README with new provider types
4. Consider bundle size impact and optimization opportunities
5. Test across different platforms and configurations

For more details, see the main Kryon documentation and the provider development guide.