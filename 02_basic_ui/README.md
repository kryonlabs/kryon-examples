# Basic UI Elements

Examples demonstrating fundamental UI components and interactions.

## Examples in this category:

### button.kry
Interactive button with Lua script integration demonstrating click events.

**Key concepts:**
- Button elements with hover and click states
- Lua script integration 
- Event handling (onClick)
- Automatic hover effects
- Print statements from Lua to console

**To run:**
```bash
# Compile and run
cargo run --bin kryon-compiler -- examples/02_basic_ui/button.kry examples/02_basic_ui/button.krb
cargo run --bin kryon-renderer-raylib examples/02_basic_ui/button.krb

# Watch console output when clicking the button!
```

### text_demo.kry
Comprehensive text rendering demonstration with different alignments and styles.

**Key concepts:**
- Text alignment options
- Text styling
- Font size control