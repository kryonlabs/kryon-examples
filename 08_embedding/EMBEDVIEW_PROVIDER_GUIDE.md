# EmbedView Provider Development Guide

This guide explains how to create new ViewProviders for the Kryon EmbedView system, allowing you to embed custom content types into Kryon applications.

## Overview

The EmbedView system provides a unified way to embed external content into Kryon applications. ViewProviders handle specific content types like WebView, WASM modules, native renderers, emulators, and development tools.

## Supported Provider Types

The following EmbedView types are currently supported:

```rust
pub enum EmbedViewType {
    WebView = 0x00,        // Web content via WebView
    NativeRenderer = 0x01, // Native rendering (Raylib, etc.)
    WasmView = 0x02,       // WebAssembly modules
    UxnView = 0x03,        // UXN virtual machine
    GbaView = 0x04,        // Game Boy Advance emulator
    DosView = 0x05,        // DOS emulator
    CodeEditor = 0x06,     // Code editor component
    Terminal = 0x07,       // Terminal emulator
    ModelViewer = 0x08,    // 3D model viewer
}
```

## Basic EmbedView Usage

In your `.kry` files, use EmbedView elements like this:

```kry
EmbedView {
    id: "my_embed"
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

## Creating a New ViewProvider

### Step 1: Add Your Type to the Enum

First, add your new provider type to the `EmbedViewType` enum in `kryon-compiler/src/core/types.rs`:

```rust
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
#[repr(u8)]
pub enum EmbedViewType {
    // ... existing types ...
    YourNewType = 0x09,  // Use next available number
}
```

### Step 2: Update the Parser

Add parsing support in `kryon-compiler/src/compiler/frontend/semantic.rs`:

```rust
impl EmbedViewType {
    pub fn from_str(s: &str) -> Result<Self, String> {
        match s {
            // ... existing matches ...
            "your_new_type" => Ok(EmbedViewType::YourNewType),
            _ => Err(format!("Invalid EmbedView type: '{}'", s)),
        }
    }
}
```

### Step 3: Implement the ViewProvider

Create your provider implementation in the runtime. Here's a template:

```rust
use kryon_runtime::providers::ViewProvider;
use std::collections::HashMap;

pub struct YourNewTypeProvider {
    config: HashMap<String, String>,
    // Add your provider-specific fields
}

impl YourNewTypeProvider {
    pub fn new() -> Self {
        Self {
            config: HashMap::new(),
        }
    }
}

impl ViewProvider for YourNewTypeProvider {
    fn initialize(&mut self, config: HashMap<String, String>) -> Result<(), String> {
        self.config = config;
        // Initialize your provider
        Ok(())
    }
    
    fn load_content(&mut self, source: &str) -> Result<(), String> {
        // Load and prepare your content
        // The source parameter contains the URL, file path, or content string
        Ok(())
    }
    
    fn render(&mut self, width: u32, height: u32) -> Result<Vec<u8>, String> {
        // Render your content and return RGBA pixel data
        // Return a Vec<u8> with width * height * 4 bytes (RGBA)
        let pixels = vec![0u8; (width * height * 4) as usize];
        Ok(pixels)
    }
    
    fn handle_event(&mut self, event: ViewEvent) -> Result<(), String> {
        // Handle mouse, keyboard, and other events
        match event {
            ViewEvent::MouseClick { x, y, button } => {
                // Handle mouse click
            }
            ViewEvent::KeyPress { key, modifiers } => {
                // Handle key press
            }
            // ... handle other events
        }
        Ok(())
    }
    
    fn get_preferred_size(&self) -> (u32, u32) {
        // Return the preferred size for your content
        (400, 300)
    }
    
    fn cleanup(&mut self) {
        // Clean up resources when the provider is destroyed
    }
}
```

### Step 4: Register the Provider

Add your provider to the provider registry in `kryon-runtime/src/providers/mod.rs`:

```rust
use your_module::YourNewTypeProvider;

pub fn create_provider(provider_type: EmbedViewType) -> Box<dyn ViewProvider> {
    match provider_type {
        // ... existing cases ...
        EmbedViewType::YourNewType => Box::new(YourNewTypeProvider::new()),
    }
}
```

## Configuration Options

Providers can accept configuration through the `config` property:

```kry
EmbedView {
    type: "your_new_type"
    source: "content_source"
    
    config: {
        "option1": "value1"
        "option2": "value2"
        "enable_feature": true
    }
}
```

Access these in your provider's `initialize` method:

```rust
fn initialize(&mut self, config: HashMap<String, String>) -> Result<(), String> {
    if let Some(option1) = config.get("option1") {
        // Use the configuration option
    }
    
    let enable_feature = config.get("enable_feature")
        .and_then(|v| v.parse::<bool>().ok())
        .unwrap_or(false);
    
    Ok(())
}
```

## Event Handling

Handle user input events in your provider:

```rust
fn handle_event(&mut self, event: ViewEvent) -> Result<(), String> {
    match event {
        ViewEvent::MouseClick { x, y, button } => {
            println!("Mouse clicked at ({}, {}) with button {:?}", x, y, button);
        }
        ViewEvent::MouseMove { x, y } => {
            // Handle mouse movement
        }
        ViewEvent::KeyPress { key, modifiers } => {
            println!("Key pressed: {:?} with modifiers: {:?}", key, modifiers);
        }
        ViewEvent::Resize { width, height } => {
            // Handle resize events
        }
    }
    Ok(())
}
```

## Best Practices

### 1. Resource Management
- Always clean up resources in the `cleanup` method
- Use RAII patterns where possible
- Handle errors gracefully

### 2. Performance
- Cache rendered content when possible
- Use incremental updates for large content
- Consider async operations for I/O

### 3. Security
- Validate all input sources
- Sandbox untrusted content
- Implement proper access controls

### 4. Error Handling
- Return descriptive error messages
- Handle edge cases gracefully
- Log important events for debugging

## Example: Simple Image Viewer Provider

Here's a complete example of a simple image viewer provider:

```rust
use image::{ImageBuffer, Rgba};
use kryon_runtime::providers::{ViewProvider, ViewEvent};
use std::collections::HashMap;

pub struct ImageViewerProvider {
    image_data: Option<ImageBuffer<Rgba<u8>, Vec<u8>>>,
    scale_factor: f32,
}

impl ImageViewerProvider {
    pub fn new() -> Self {
        Self {
            image_data: None,
            scale_factor: 1.0,
        }
    }
}

impl ViewProvider for ImageViewerProvider {
    fn initialize(&mut self, config: HashMap<String, String>) -> Result<(), String> {
        self.scale_factor = config.get("scale")
            .and_then(|s| s.parse().ok())
            .unwrap_or(1.0);
        Ok(())
    }
    
    fn load_content(&mut self, source: &str) -> Result<(), String> {
        let img = image::open(source)
            .map_err(|e| format!("Failed to load image: {}", e))?;
        
        self.image_data = Some(img.to_rgba8());
        Ok(())
    }
    
    fn render(&mut self, width: u32, height: u32) -> Result<Vec<u8>, String> {
        if let Some(ref img) = self.image_data {
            let mut output = vec![0u8; (width * height * 4) as usize];
            
            // Simple scaling and blitting logic
            let (img_width, img_height) = img.dimensions();
            let scaled_width = (img_width as f32 * self.scale_factor) as u32;
            let scaled_height = (img_height as f32 * self.scale_factor) as u32;
            
            // Center the image
            let offset_x = (width.saturating_sub(scaled_width)) / 2;
            let offset_y = (height.saturating_sub(scaled_height)) / 2;
            
            // Blit scaled image (simplified - you'd want proper scaling)
            for y in 0..scaled_height.min(height - offset_y) {
                for x in 0..scaled_width.min(width - offset_x) {
                    let src_x = (x as f32 / self.scale_factor) as u32;
                    let src_y = (y as f32 / self.scale_factor) as u32;
                    
                    if src_x < img_width && src_y < img_height {
                        let src_pixel = img.get_pixel(src_x, src_y);
                        let dst_idx = (((y + offset_y) * width + (x + offset_x)) * 4) as usize;
                        
                        if dst_idx + 3 < output.len() {
                            output[dst_idx] = src_pixel[0];     // R
                            output[dst_idx + 1] = src_pixel[1]; // G
                            output[dst_idx + 2] = src_pixel[2]; // B
                            output[dst_idx + 3] = src_pixel[3]; // A
                        }
                    }
                }
            }
            
            Ok(output)
        } else {
            // Return transparent pixels if no image loaded
            Ok(vec![0u8; (width * height * 4) as usize])
        }
    }
    
    fn handle_event(&mut self, event: ViewEvent) -> Result<(), String> {
        match event {
            ViewEvent::MouseClick { x, y, button } => {
                println!("Image clicked at ({}, {})", x, y);
            }
            ViewEvent::Scroll { delta_x, delta_y } => {
                // Zoom in/out with scroll wheel
                self.scale_factor *= if delta_y > 0.0 { 1.1 } else { 0.9 };
                self.scale_factor = self.scale_factor.clamp(0.1, 5.0);
            }
            _ => {}
        }
        Ok(())
    }
    
    fn get_preferred_size(&self) -> (u32, u32) {
        if let Some(ref img) = self.image_data {
            let (w, h) = img.dimensions();
            ((w as f32 * self.scale_factor) as u32, (h as f32 * self.scale_factor) as u32)
        } else {
            (400, 300)
        }
    }
    
    fn cleanup(&mut self) {
        self.image_data = None;
    }
}
```

## Testing Your Provider

Create test applications to verify your provider works correctly:

```kry
App {
    window_width: 800
    window_height: 600
    window_title: "Test Your Provider"
    
    Container {
        padding: 20
        
        EmbedView {
            id: "test_view"
            type: "your_new_type"
            source: "test_content"
            width: 400
            height: 300
            
            config: {
                "test_option": "test_value"
            }
        }
    }
}
```

## Bundle Optimization

When creating providers, consider bundle size impact:

- Only include dependencies that are actually used
- Use feature flags to make components optional
- Implement lazy loading for heavy resources
- Consider splitting large providers into separate bundles

The Kryon bundler will automatically exclude unused providers from the final application bundle, so users only pay for what they use.

## Contributing

When contributing new providers to the Kryon project:

1. Follow the existing code style and patterns
2. Add comprehensive tests for your provider
3. Document all configuration options
4. Consider cross-platform compatibility
5. Optimize for both performance and memory usage

For more information, see the main Kryon documentation and existing provider implementations in the `kryon-runtime/src/providers/` directory.