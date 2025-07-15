# KryonLabs Examples

Welcome to the KryonLabs examples collection! These examples are organized by complexity and topic to help you learn the framework progressively.

## 📁 Directory Structure

### 🚀 [01_getting_started/](01_getting_started/)
Your first steps with KryonLabs - basic app structure and simple rendering.

### 🎛️ [02_basic_ui/](02_basic_ui/) 
Fundamental UI elements like buttons, text, and basic interactions.

### 📐 [03_layouts/](03_layouts/)
Layout systems including flexbox, centering, and responsive design.

### 🎨 [04_styling/](04_styling/)
CSS-like styling, themes, colors, and visual design patterns.

### 🖱️ [05_interactions/](05_interactions/)
User input handling, events, and interactive behaviors.

### 🎬 [06_multimedia/](06_multimedia/)
Images, media, animations, and graphics primitives.

### 🔧 [07_advanced/](07_advanced/)
Complex applications with advanced patterns and multi-script integration.

### 🎮 [98_demos/](98_demos/)
Complete, polished applications showcasing KryonLabs capabilities.

### 🧪 [99_tests/](99_tests/)
Testing examples and validation scenarios.

## 🚀 Quick Start

1. **Compile an example:**
   ```bash
   cargo run --bin kryon-compiler -- examples/01_getting_started/hello_world.kry examples/01_getting_started/hello_world.krb
   ```

2. **Run with your preferred backend:**
   ```bash
   # GPU-accelerated (recommended for desktop)
   cargo run --bin kryon-renderer-raylib examples/01_getting_started/hello_world.krb
   
   # Terminal-based (great for testing)
   cargo run --bin kryon-renderer-ratatui examples/01_getting_started/hello_world.krb
   ```

3. **Try the interactive button:**
   ```bash
   cargo run --bin kryon-compiler -- examples/02_basic_ui/button.kry examples/02_basic_ui/button.krb
   cargo run --bin kryon-renderer-raylib examples/02_basic_ui/button.krb
   # Click the button and watch the console output!
   ```

## 📖 Learning Path

**Beginner:** Start with `01_getting_started` → `02_basic_ui` → `03_layouts`

**Intermediate:** Explore `04_styling` → `05_interactions` → `06_multimedia`

**Advanced:** Dive into `07_advanced` and study `98_demos`

## 🎯 Featured Examples

- **Interactive Button** (`02_basic_ui/button.kry`) - Demonstrates Lua script integration with console output
- **Hello World** (`01_getting_started/hello_world.kry`) - Your first KryonLabs application
- **Flexbox Guide** (`03_layouts/flexbox_guide.kry`) - Complete layout system tutorial

## 🚨 Key Features Demonstrated

- ✅ **Automatic hover effects** - Buttons intelligently lighten/darken on hover
- ✅ **Multi-backend rendering** - Same code runs on GPU (Raylib) and Terminal (Ratatui)  
- ✅ **Script integration** - Lua, JavaScript, Python, and Wren script support
- ✅ **Responsive layouts** - Flexbox-based layout engine
- ✅ **CSS-like styling** - Familiar styling with inheritance and pseudo-states

## 🔧 Running Examples

### Backend Options

```bash
# GPU-accelerated desktop rendering (Raylib)
cargo run --bin kryon-renderer-raylib examples/02_basic_ui/button.krb

# Terminal UI rendering (great for CI/testing)
cargo run --bin kryon-renderer-ratatui examples/02_basic_ui/button.krb

# Debug mode - clean terminal tree output
cargo run --bin kryon-renderer-debug examples/02_basic_ui/button.krb
```

### Testing All Examples

```bash
# Test compilation of all examples
./test_examples.sh

# Test specific category  
find examples/02_basic_ui -name "*.kry" -exec cargo run --bin kryon-compiler -- {} {}.krb \;
```

## 📝 Writing New Examples

When creating examples, follow these patterns:

1. **Clear purpose** - Each example demonstrates specific features
2. **Realistic content** - Use meaningful text and interactions
3. **Progressive complexity** - Build on previous concepts
4. **Comprehensive coverage** - Show all properties and states
5. **Script integration** - Include Lua scripts for interactivity

## 🔤 Variable Syntax

KryonLabs uses different variable syntax depending on the context:

- **Regular usage**: Use `$variable_name` (e.g., `text_color: $primary_color`)
- **Inside string concatenation/functions**: Use `${variable_name}` (e.g., `style: "button_${position}"`)
- **Component properties**: Use `${property_name}` inside Define blocks

Example:
```kry
@variables {
    primary_color: "#007BFF"
    button_size: 60
}

style "my_button" {
    background_color: $primary_color    # Regular usage
    width: $button_size                 # Regular usage
}

Define MyComponent {
    Properties {
        position: String = "left"
    }
    
    Button {
        style: "button_${position}"     # String concatenation needs ${}
        text: ${text}                   # Component property needs ${}
    }
}
```

Happy coding with KryonLabs! 🎉