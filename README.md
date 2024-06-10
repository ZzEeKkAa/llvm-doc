# LLVM at VSCODE inside conda

This doc shows how to create llvm dev environment using conda and VSCode as an
IDE.

## Preretirement

- [Miniforge3](https://conda-forge.org/miniforge/)
- [VSCode](https://code.visualstudio.com)
- [CMake Tools VSCode Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)
- [MLIR VSCode Extension](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-mlir)

You need to have conda install available, preferably miniforge. Default may work
but not tested. It is also looks like build packages on host system are
required. For ubuntu it is `apt install build-essential`.

## Create conda environment

Open `llvm_dev.yaml` and edit it depending on your compiler and system
preferences. Then run:

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

### Enable tablegen and MLIR language support

Reference: [MLIR : Language Server Protocol](https://mlir.llvm.org/docs/Tools/MLIRLSP/#visual-studio-code)

You need [MLIR VSCode Extension](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-mlir)
and llvm build along with the mlir extension.

Then go to extensions settings and set paths to lsp servers and databases. You
can short cut it by opening `User's settings.json` and adding this to the
bottom (make sure to update it to your paths):

```json
{
    // ...
    "mlir.server_path": "/Users/zeka/Projects/Intel/llvm-project/build/Debug/bin/mlir-lsp-server",
    "mlir.pdll_server_path": "/Users/zeka/Projects/Intel/llvm-project/build/Debug/bin/mlir-pdll-lsp-server",
    "mlir.tablegen_server_path": "/Users/zeka/Projects/Intel/llvm-project/build/Debug/bin/tblgen-lsp-server",
    "mlir.tablegen_compilation_databases": [
        "/Users/zeka/Projects/Intel/llvm-project/build/Debug/tablegen_compile_commands.yml",
        "/Users/zeka/Projects/Intel/llvm-project/build/Debug/NATIVE/tablegen_compile_commands.yml"
    ],
    "mlir.pdll_compilation_databases": [
        "/Users/zeka/Projects/Intel/llvm-project/build/Debug/pdll_compile_commands.yml",
    ],
    "mlir.onSettingsChanged": "restart",
}
```

### Runing toy examples in-tree

Once you've set up everything:

- `> CMake: Set Build Target`
- `toyc-ch2`
- `> CMake: Build`

Now you can navigate to toy's examle at `./mlir/examples/toy/Ch2` in your VSCode
and all the definitinons in `cpp/tablegen/mlir` should work and you can easily
navigate (eg. navigate to MLIR definition definitions).
