# Scientific Calculator – WASM Components

WebAssembly components built in Rust and TypeScript, following the [Bytecode Alliance guides](https://component-model.bytecodealliance.org/language-support/).

## Components

### `arithmetic-calculator` (Rust, wasm32-wasip2)
Exports an `arithmetic` interface with:
- `add(x: f64, y: f64) -> f64`
- `subtract(x: f64, y: f64) -> f64`
- `multiply(x: f64, y: f64) -> f64`
- `divide(x: f64, y: f64) -> result<f64, string>` — returns an error on division by zero

### `trigonometric-calculator` (Rust, wasm32-wasip2)
Exports a `trigonometric` interface with (angles in degrees):
- `sin(degrees: f64) -> f64`
- `cos(degrees: f64) -> f64`
- `tan(degrees: f64) -> f64`
- `arctan(value: f64) -> f64` — returns degrees

### `moddiv` (TypeScript → WASM via jco)
Exports a `moddiv` interface with:
- `mod(x: f64, y: f64) -> f64` — remainder of x divided by y
- `div(x: f64, y: f64) -> f64` — quotient of x divided by y

## Prerequisites

```sh
rustup target add wasm32-wasip2
cargo install --locked wasm-tools
```

## Build

From the repo root (the Cargo workspace builds both components):

```sh
cargo build --target wasm32-wasip2 --release
```

Or build individually:

```sh
cargo build -p arithmetic-calculator --target wasm32-wasip2 --release
cargo build -p trigonometric-calculator --target wasm32-wasip2 --release
```

Output binaries (shared workspace `target/` at repo root):

```
target/wasm32-wasip2/release/arithmetic_calculator.wasm
target/wasm32-wasip2/release/trigonometric_calculator.wasm
```

### moddiv (TypeScript)

```sh
cd moddiv
npm install
npm run build   # compiles TypeScript → JS, then componentizes → moddiv.wasm
```

## Inspect

```sh
wasm-tools component wit target/wasm32-wasip2/release/arithmetic_calculator.wasm
wasm-tools component wit target/wasm32-wasip2/release/trigonometric_calculator.wasm
wasm-tools component wit moddiv/moddiv.wasm
```
