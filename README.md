# Kryon Renderer Examples

This directory contains comprehensive examples that serve as both documentation and demonstrations of Kryon's capabilities.

## Structure

### Fundamentals
- `fundamentals/` - Basic concepts and getting started
  - `hello_world.kry` - Your first Kryon app
  - `image.kry` - Displaying images

### UI Elements  
- `ui-elements/` - Complete showcase of all UI components
  - `text_demo.kry` - Text elements with all properties
  - `button_showcase.kry` - Button types, states, and interactions
  - `button.kry` - Simple button example

### Layout System
- `layout/` - Layout and positioning demonstrations  
  - `flexbox_guide.kry` - Complete flexbox layout guide

### Styling
- `styling/` - Colors, themes, and visual design

### Advanced Features
- `advanced/` - Complex examples combining multiple features
  - `calculator.kry` - Interactive calculator app
  - `tab_bar.kry` - Tabbed interface component
  - `markdown.kry` - Markdown rendering example

## Running Examples

### New Unified CLI (Recommended)

The new CLI requires you to specify which renderer backend to use:

```bash
# GPU-accelerated desktop rendering
cargo run --bin kryon-renderer wgpu examples/fundamentals/hello_world.krb

# Terminal UI rendering  
cargo run --bin kryon-renderer ratatui examples/fundamentals/hello_world.krb

# Simple graphics rendering
cargo run --bin kryon-renderer raylib examples/fundamentals/hello_world.krb

# Debug mode - clean terminal node tree (no UI)
cargo run --bin kryon-renderer debug examples/fundamentals/hello_world.krb
```

### Debug Mode Features

The debug renderer outputs a clean terminal hierarchy without rendering UI:

```bash
# Basic tree view
cargo run --bin kryon-renderer debug examples/ui-elements/text_demo.krb

# Show layout information  
cargo run --bin kryon-renderer debug examples/layout/flexbox_guide.krb --show-layout

# Show all properties
cargo run --bin kryon-renderer debug examples/ui-elements/button_showcase.krb --show-properties

# Show colors in hex format
cargo run --bin kryon-renderer debug examples/styling/color_demo.krb --show-colors

# Complete analysis with all options
cargo run --bin kryon-renderer debug examples/advanced/calculator.krb --show-layout --show-properties --show-colors
```

Example debug output:
```
App pos:(0,0) size:(600,700) [bg:#F8F9FAFF]
├── Container pos:(30,30) size:(540,640)
│   ├── Text "Text Element Complete Demo" pos:(0,0) size:(540,24) [color:#212529FF]
│   ├── Container pos:(0,54) size:(540,180) [bg:#FFFFFFFF]
│   │   ├── Text "Font Sizes" pos:(20,20) size:(500,18) [color:#495057FF]
│   │   ├── Text "Small text (12px)" pos:(20,43) size:(500,12)
│   │   └── Text "Regular text (16px)" pos:(20,63) size:(500,16)
│   └── Container pos:(0,254) size:(540,120) [bg:#FFFFFFFF]
```

### Direct Backend Usage

You can still use the individual backend binaries directly:

```bash
cargo run --bin kryon-renderer-wgpu examples/fundamentals/hello_world.krb
cargo run --bin kryon-renderer-ratatui examples/fundamentals/hello_world.krb  
cargo run --bin kryon-renderer-raylib examples/fundamentals/hello_world.krb
cargo run --bin kryon-renderer-debug examples/fundamentals/hello_world.krb --format tree
```

## Example Categories Explained

### Fundamentals
Perfect starting point showing basic app structure and core concepts.

### UI Elements
Comprehensive documentation examples showing every property and state of each UI element type. These examples are designed to be copy-paste references.

### Layout System
Visual guides to understanding flexbox layouts, positioning, spacing, and responsive design patterns.

### Styling  
Color palettes, themes, borders, typography, and visual design systems.

### Advanced Features
Real-world examples combining multiple features - calculator apps, tabbed interfaces, data displays, and interactive components.

## Writing Documentation Examples

When creating new examples:

1. **Be comprehensive** - Show all properties and variations
2. **Use realistic content** - "Lorem ipsum" → "User Profile Settings"  
3. **Visual hierarchy** - Group related elements in containers
4. **Consistent styling** - Use standard color palettes and spacing
5. **Descriptive titles** - "Button Showcase" not "Button Test"
6. **Comments** - Explain complex interactions or patterns

Example structure:
```kry
// Element Type Documentation Example (using // comments)
# Complete demonstration of [element] capabilities (using # comments)

App {
    window_title: "[Element] Complete Demo/Showcase/Guide"
    window_width: 600
    window_height: 700
    background_color: "#F8F9FAFF"
    
    Container {
        layout: column start
        padding: 30
        gap: 25
        
        Text {
            text: "[Element] Complete Demo"
            font_size: 24
            text_alignment: center
            margin_bottom: 15
        }
        
        // Section 1: Basic usage
        Container {
            layout: column start
            background_color: "#FFFFFFFF"
            border_radius: 8
            padding: 20
            gap: 12
            
            Text {
                text: "Basic Usage"
                font_size: 18
                color: "#495057FF"
                margin_bottom: 8
            }
            
            # Examples here... (both comment styles work)
        }
        
        // Section 2: Variations
        # Section 3: States/Interactions  
        // Section 4: Advanced usage
    }
}
```

## Testing Examples

### Automated Testing

```bash
# Test all examples compile and render correctly
./test_examples.sh

# Test specific category
find examples/ui-elements -name "*.kry" -exec cargo run --bin kryon-renderer debug {} \;
```

### Manual Testing

1. **Compile**: Does the .kry file compile to .krb without errors?
2. **Debug tree**: Does the debug output show expected hierarchy?
3. **Visual**: Does each backend render the UI correctly?
4. **Interactive**: Do buttons, hovers, and other interactions work?

The debug mode is perfect for automated testing since it provides deterministic text output that can be compared against expected results.