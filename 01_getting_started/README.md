# Getting Started Examples

These examples demonstrate the basics of KryonLabs UI development.

## Examples in this category:

### hello_world.kry
Your first KryonLabs application - displays "Hello, World!" text in a simple window.

**Key concepts:**
- Basic App structure
- Text rendering
- Window configuration

**To run:**
```bash
# Compile KRY to KRB
cargo run --bin kryon-compiler -- examples/01_getting_started/hello_world.kry examples/01_getting_started/hello_world.krb

# Run with different backends
cargo run --bin kryon-renderer-raylib examples/01_getting_started/hello_world.krb
cargo run --bin kryon-renderer-ratatui examples/01_getting_started/hello_world.krb
```