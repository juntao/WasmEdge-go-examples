# WasmEdge-Go Tensorflow Extension MobileNet-Plants example

## Build

Before building this project, please ensure the dependency of [WasmEdge-tensorflow extension](https://github.com/second-state/WasmEdge-go#wasmedge-tensorflow-extension) has been installed.

```bash
# In the current directory.
$ go get -u github.com/second-state/WasmEdge-go/wasmedge
$ go build --tags tensorflow
```

## (Optional) Build the example WASM from rust

The pre-built WASM from rust is provided as "rust_mobilenet_plants_lib_bg.wasm".
The pre-built compiled-WASM from rust is provided as "rust_mobilenet_plants_lib_bg.wasm.so".

For building the WASM from the rust source, the following steps are required:

* Install the [rustc and cargo](https://www.rust-lang.org/tools/install).
* Set the default `rustup` version to `1.50.0` or lower.
  * `$ rustup default 1.50.0`
* Install the [rustwasmc](https://github.com/second-state/rustwasmc)
  * `$ curl https://raw.githubusercontent.com/second-state/rustwasmc/master/installer/init.sh -sSf | sh`

```bash
$ cd rust_mobilenet_plants
$ rustwasmc build
# The output WASM will be `pkg/rust_mobilenet_plants_lib_bg.wasm`.
```

For compiling the WASM to SO for the AOT mode, please follow the tools of [WasmEdge](https://github.com/WasmEdge/WasmEdge):

```bash
$ wget https://github.com/WasmEdge/WasmEdge/releases/download/0.8.0/WasmEdge-0.8.0-manylinux2014_x86_64.tar.gz
$ tar -xzf WasmEdge-0.8.0-manylinux2014_x86_64.tar.gz
$ ./WasmEdge-0.8.0-Linux/bin/wasmedgec rust_mobilenet_plants_lib_bg.wasm rust_mobilenet_plants_lib_bg.wasm.so
# The output compiled-WASM will be at `rust_mobilenet_plants_lib_bg.wasm.so`.
```

Or follow the [example](https://github.com/second-state/WasmEdge-go-examples/tree/master/go_WasmAOT) for compiling the WASM to SO:

```bash
# In the `go_WasmAOT` directory
$ go get -u github.com/second-state/WasmEdge-go/wasmedge
$ go build
# Prepare the input WASM file
$ ./wasmAOT input.wasm output.wasm.so
```

## Run

```bash
# For interpreter mode:
$ ./mobilenet_plants rust_mobilenet_plants_lib_bg.wasm sunflower.jpg
# For AOT mode:
$ ./mobilenet_plants rust_mobilenet_plants_lib_bg.wasm.so sunflower.jpg
```

The standard output of this example will be the following:

```bash
Go: Args: [./mobilenet_plants rust_mobilenet_plants_lib_bg.wasm.so sunflower.jpg]
RUST: Loaded image in ... 12.267229ms
RUST: Resized image in ... 14.206377ms
RUST: Parsed output in ... 294.487086ms
RUST: index 1680, prob 0.9295
RUST: Finished post-processing in ... 294.701533ms
GO: Run bindgen -- infer: It is very likely a <a href='https://www.google.com/search?q=Helianthus annuus'>Helianthus annuus</a> in the picture
```
