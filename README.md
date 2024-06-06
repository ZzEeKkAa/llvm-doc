# LLVM at VSCODE inside conda

This doc shows how to create llvm dev environment using conda and VSCode as an
IDE.

## Preretirement

You need to have conda install available, preferably miniforge. Default may work
but not tested. It is also looks like build packages on host system are
required. For ubuntu it is `apt install build-essential`.

## Create conda environment

```bash
conda env create -n llvm-dev -f llvm_dev.yaml 
```

## Set up vscode

Copy `.vscode` folder to `llvm-project` folder and open it in code.

Make sure to update `cmakePath` at `settings.json` and conda path at
`activate.sh`.

## Useful tips

`.vscode` is set to configure cmake project at `./build/Debug` folder, that
means you can configure it from vscode and build it from terminal (handy to set
task to build on remote machine without being connected there).

## CMake commands

### Build
```shell
cmake --build ./build
```

### Run tests

To run all tests use one of the commands:
```shell
ninja check-llvm -C ./build/Debug
ninja check-mlir -C ./build/Debug
```

Note: if you changed config to use `make` instead of `ninja` - adjust commands
above.

To run individual test through lit:
```shell
./build/Debug/bin/llvm-lit ./mlir/test/Dialect/Linalg/
```
