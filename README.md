# Scientific Calculator – WASM Components

WebAssembly components built in Rust, TypeScript, and C#, following the [Bytecode Alliance guides](https://component-model.bytecodealliance.org/language-support/).

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

### `logaritmic-calculater` (C# / .NET 10, componentize-dotnet)
Exports a `logaritmic` interface with:
- `e() -> f64` — Euler's number (≈ 2.71828…)
- `ln(x: f64) -> f64` — natural logarithm of x

### `statistics-calculator` (Python, componentize-py)
Exports a `statistics` interface with:
- `sum(numbers: list<f64>) -> f64` — sum of a list of numbers
- `avg(numbers: list<f64>) -> f64` — arithmetic mean (returns 0.0 for empty list)

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

### `moddiv` (TypeScript)

```sh
cd moddiv
npm install
npm run build   # compiles TypeScript → JS, then componentizes → moddiv.wasm
```

### `logaritmic-calculater` (C# / .NET 10)

Requires [.NET 10 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/10.0).

```sh
cd logaritmic-calculater
dotnet build
# output: bin/Debug/net10.0/wasi-wasm/native/logaritmic-calculater.wasm
```

### `statistics-calculator` (Python)

Requires Python 3.10+ and `componentize-py`.

```sh
pip install componentize-py
cd statistics-calculator
componentize-py --wit-path wit/component.wit --world statistics-calculator componentize app -o statistics-calculator.wasm
```

## Inspect

```sh
wasm-tools component wit target/wasm32-wasip2/release/arithmetic_calculator.wasm
wasm-tools component wit target/wasm32-wasip2/release/trigonometric_calculator.wasm
wasm-tools component wit moddiv/moddiv.wasm
wasm-tools component wit logaritmic-calculater/bin/Debug/net10.0/wasi-wasm/native/logaritmic-calculater.wasm
wasm-tools component wit statistics-calculator/statistics-calculator.wasm
```
