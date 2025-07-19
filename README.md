# Kryon Examples Repository

A comprehensive collection of examples demonstrating the capabilities of the Kryon UI framework. From basic elements to advanced interactive applications, these examples cover every aspect of building modern cross-platform applications with Kryon.

## 📚 Table of Contents

- [Getting Started](#getting-started)
- [Repository Structure](#repository-structure)
- [Featured Examples](#featured-examples)
- [Learning Path](#learning-path)
- [Running Examples](#running-examples)

## 🚀 Getting Started

### Prerequisites
- Kryon framework installed
- Basic understanding of Kryon syntax
- Text editor or IDE with Kryon support

### Quick Start
```bash
# Run a basic example
kryon run 01_fundamentals/variables/variables_demo.kry

# Run an advanced example  
kryon run 07_advanced/calculator/calculator.kry
```

## 📂 Repository Structure

### 🔰 01_fundamentals/
**Basic concepts and syntax**
- **variables/** - Proper `@variables { }` syntax and usage patterns
- **hello_world/** - Getting started with Kryon
- **basic_syntax/** - Core language features
- **directives/** - Framework directives

### 🎨 02_elements/ 
**UI elements and components**
- **basic/** - Button showcase with all variants, sizes, and states
- **input/** - Comprehensive input showcase with validation examples
- **containers/** - Layout containers and organization
- **media/** - Basic media element examples

### 📐 03_layouts/ & 05_layouts/
**Layout systems and positioning**
- **flexbox/** - Complete flexbox system with alignment and properties  
- **absolute/** - Absolute positioning and overlays
- **responsive/** - Mobile-first and adaptive designs

### 🎭 04_styling/
**Visual design and theming**
- **colors/** - Color systems and gradients
- **typography/** - Font families and text styling
- **borders/** - Border styles and effects
- **shadows/** - Box and text shadows

### 🎬 06_multimedia/
**Rich media integration**
- **canvas/** - Interactive 2D drawing with tools and graphics
- **video/** - Native video player with custom controls  
- **image/** - Image gallery with grid layout and modal views
- **svg/** - Scalable vector graphics with interactivity

### 🚀 07_advanced/
**Complex applications and algorithms**
- **calculator/** - Scientific calculator with complex logic
- **data_visualization/** - Interactive charts and analytics dashboard

### ⚡ 08_interactions/
**User interactions and animations**
- **drag_drop/** - Multi-zone drag and drop interface
- **forms/** - Advanced multi-step forms with validation
- **animations/** - Comprehensive animation system with interactive playground

### 🔌 09_embedview/
**External content integration**
- **uxn/** - Complete UXN/Varvara virtual machine integration
- **webview/** - Full web browser with navigation and tabs
- **wasm/** - WebAssembly playground with multiple modules
- **native_renderer/** - Legacy application emulation

### 🎮 10_gaming/
**Interactive games and real-time applications**
- **interactive_game/** - Complete gaming showcase with multiple game types

### 📱 11_mobile/
**Mobile-specific examples**
- Touch-optimized interfaces
- Responsive design patterns
- Platform adaptation

### ♿ 12_accessibility/
**Accessibility and internationalization**
- Accessibility-compliant interfaces
- Screen reader support
- Multi-language applications

## 🌟 Featured Examples

### 🎨 Interactive Canvas Drawing
**Location**: `06_multimedia/canvas/interactive_drawing_app.kry`
A complete drawing application with tools, layers, and real-time collaboration features.

### 🎮 Gaming Showcase
**Location**: `10_gaming/interactive_game/game_showcase.kry`  
Multi-game platform featuring Pong, Snake, Tetris, and Platformer with high scores and achievements.

### 🌐 WebView Browser
**Location**: `09_embedview/webview/web_browser.kry`
Full-featured web browser with tabs, bookmarks, and modern browser features.

### ⚡ WASM Playground
**Location**: `09_embedview/wasm/wasm_playground.kry`
Interactive WebAssembly runtime with math engine, game modules, and performance monitoring.

### 🖥️ Native App Emulation
**Location**: `09_embedview/native_renderer/legacy_app_emulator.kry`
Legacy application emulation supporting Windows, Linux, and macOS applications.

## 📖 Learning Path

### 🎯 Beginner Track
1. **Hello World**: `01_fundamentals/hello_world/hello_world.kry`
2. **Variables**: `01_fundamentals/variables/variables_demo.kry`
3. **Basic Elements**: `02_elements/basic/button_showcase.kry`
4. **Simple Layout**: `03_layouts/flexbox/basic_flexbox.kry`

### 🚀 Intermediate Track
1. **Color Systems**: `04_styling/colors/color_systems.kry`
2. **Form Handling**: `08_interactions/forms/multi_step_form.kry`
3. **Media Integration**: `06_multimedia/video/video_player.kry`
4. **Responsive Design**: `03_layouts/responsive/mobile_first.kry`

### ⚡ Advanced Track
1. **WASM Integration**: `09_embedview/wasm/wasm_playground.kry`
2. **Game Development**: `10_gaming/interactive_game/game_showcase.kry`
3. **Data Visualization**: `07_advanced/data_visualization/analytics_dashboard.kry`
4. **Animation Systems**: `08_interactions/animations/animation_playground.kry`

## 🔧 Running Examples

### Quick Start
```bash
# Run any example
kryon run path/to/example.kry

# Run with specific renderer
kryon run --renderer=wgpu 06_multimedia/canvas/interactive_drawing_app.kry
```

### Available Renderers
```bash
# Terminal UI (great for testing)
kryon run --renderer=ratatui examples/01_fundamentals/hello_world/hello_world.kry

# High-performance graphics
kryon run --renderer=wgpu examples/10_gaming/interactive_game/game_showcase.kry

# Cross-platform native
kryon run --renderer=raylib examples/06_multimedia/video/video_player.kry

# Web browser deployment
kryon build --target=web examples/09_embedview/webview/web_browser.kry
```

### Development Workflow
```bash
# Run with hot reload
kryon dev examples/02_elements/basic/button_showcase.kry

# Run all tests
cargo test -p kryon-examples

# Build for production
kryon build --release examples/07_advanced/calculator/calculator.kry

# Generate documentation
kryon docs examples/
```

## 🎯 Quick Start Examples

### Simple Button with State
```kry
App {
    Container {
        Button {
            text: "Click me!"
            style: button_primary
            on_click: handle_click
        }
        
        Text {
            text: "Clicked: 0 times"
            style: counter_text
        }
    }
}

@variables {
    primary_color = "#3b82f6"
    text_color = "#1f2937"
    counter = 0
}

style button_primary {
    background_color: ${primary_color}
    color: white
    padding: 12px 24px
    border_radius: 6px
    cursor: pointer
}

style counter_text {
    color: ${text_color}
    font_size: 16px
    margin_top: 10px
}

@script lua {
function handle_click()
    counter = counter + 1
    update_counter_display()
    print("Button clicked " .. counter .. " times!")
end

function update_counter_display()
    -- Update UI with new counter value
end
}
```

### EmbedView Integration
```kry
App {
    Container {
        EmbedView {
            embed_view_type: "webview"
            embed_view_source: "https://example.com"
            width: 800px
            height: 600px
            
            embed_view_config: {
                allow_scripts: true
                security_level: "normal"
                user_agent: "Kryon Browser"
            }
            
            on_load: webview_loaded
            on_error: webview_error
        }
    }
}

@script lua {
function webview_loaded()
    print("WebView loaded successfully!")
end

function webview_error(error)
    print("WebView error: " .. error)
end
}
```

## 🎨 Example Categories

### 🔧 Essential Building Blocks
- **Variables & Logic**: `01_fundamentals/`
- **UI Components**: `02_elements/`
- **Layout Systems**: `03_layouts/` & `05_layouts/`
- **Visual Design**: `04_styling/`

### 🚀 Interactive Features  
- **User Input**: `08_interactions/`
- **Rich Media**: `06_multimedia/`
- **Complex Apps**: `07_advanced/`

### 🌐 Integration & Embedding
- **WebView**: Browser integration with full web compatibility
- **WASM**: High-performance WebAssembly modules
- **UXN**: Retro computing and virtual machines
- **Native Renderer**: Legacy application emulation

### 🎮 Real-World Applications
- **Games**: Complete game implementations
- **Tools**: Productivity applications
- **Demos**: Interactive showcases

## 🛠️ Development Tips

### Best Practices
- Start with `01_fundamentals/variables/` to understand Kryon syntax
- Use `@variables { }` blocks for dynamic configuration
- Follow the learning path for structured progression
- Test examples across different renderers

### Performance
- Use WGPU renderer for graphics-intensive examples
- Use Ratatui renderer for quick development iteration
- Profile WASM modules for optimal performance
- Implement proper state management for complex apps

### Debugging
```bash
# Enable debug output
kryon run --debug examples/path/to/example.kry

# Profile performance
kryon run --profile examples/10_gaming/interactive_game/game_showcase.kry

# Check syntax
kryon check examples/02_elements/basic/button_showcase.kry
```

## 🔗 Related Resources

- **[Kryon Framework](../kryon-compiler/)** - Core compiler and language
- **[Kryon Renderer](../kryon-renderer/)** - Multi-backend rendering system  
- **[VSCode Extension](../kry-vscode-extension/)** - Syntax highlighting and tools
- **[Community Examples](https://github.com/kryonlabs/community-examples)** - User contributions

## 📚 Documentation Links

- **[Language Guide](../docs/language-guide.md)** - Complete syntax reference
- **[Renderer API](../docs/renderer-api.md)** - Backend integration
- **[EmbedView Guide](../docs/embedview.md)** - External content integration
- **[Performance Guide](../docs/performance.md)** - Optimization techniques

## 🤝 Contributing

We welcome contributions! Check out specific areas:

- **New Examples**: Add examples for underrepresented use cases
- **Platform Testing**: Test examples on different operating systems
- **Performance**: Optimize existing examples
- **Documentation**: Improve example explanations

### Example Guidelines
- **Naming**: Use descriptive filenames with underscores
- **Comments**: Document complex logic and design decisions  
- **Compatibility**: Ensure examples work across all renderers
- **Progressive Complexity**: Build concepts incrementally
- **Real-World Relevance**: Create practical, useful examples

## 🎉 Getting Help

- **Issues**: Report bugs or request examples
- **Discussions**: Ask questions and share ideas
- **Discord**: Join our community chat
- **Documentation**: Check the comprehensive guides

---

**Ready to build amazing applications with Kryon?** Start with `01_fundamentals/hello_world/` and explore the endless possibilities! 🚀