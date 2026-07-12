# project-eleven
A native PC recompilation of the Xbox 360 version of 11eyes CrossOver PC recompilation using ReXGlue.

## Building

If you want to build from source you'll need:

* ReXGlue SDK
* CMake
* Visual Studio / MSVC / GCC / LLVM
* Clang 20+
* Your legally owned game files

## Setup
* Get the `Nightly 0.8.1.68-dev.g8dadea6` or newer release of the ReXGlue SDK.
* Put your your own legally owned game's XEX file into `extracted`.
* Copy the path to `rexglue` or `rexglue.exe` from the downloaded SDK, then in the downloaded `project-eleven` archive, run `rexglue --verbose --log-file ../logs/recomp.log codegen`
* After codegen is successful, run `cmake --preset linux-amd64-relwithdebinfo -DCMAKE_PREFIX_PATH=/path/to/rexglue-sdk` to configure (be sure to select your platform), if not sure, check `CMakePresets.json`)
* Build the code with `cmake --build out/build/linux-amd64-relwithdebinfo` (Again, be sure to select your platform)
* The compiled executable will be inside `out/build/linux-amd64-relwithdebinfo` (or what your platform was). Note that you still need to copy over `librexruntimerd.so`, `librexruntimerd.so`, `libTracyClientrd.so` on Linux if you don't compile using `cmake --build . --target project_eleven_codegen`.

## What's working
* Likely everything that does on Xenia. Even the OP/ED videos likely work.
* To launch, use `./project_eleven --game_data_root="extracted/" --user_data_root="userdata/" --cache_path="cache/" --gpu_plugin=xenos --no-fullscreen`. Make sure to put your legally owned game files inside `extracted`.
* You'll need to use a controller.

## Troubleshooting
* If you see something like 
```[critical] [core] [t550982] [FATAL] Call to invalid or unregistered function at guest address 0xADDRESS``` 
in the log (at `compiled program/logs/program_001.log`), take note of the address in question, then in `program_manifest.toml`, go to 
```[entrypoint.functions]
0xADDRESS = {}
```
Specify the address in here, and if you don't specify an end or a size, the codegen process will map it out.

* `[warning] [krnl] [t521854] [NtCreateFile] FAILED: path='D:\data\system.dat' -> 0xc000000f` -> see [this article](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-erref/596a1078-e883-4972-9bbc-49e60bebca55) for explanations
## Legal

This repository does not contain any game assets, game code, ROMs, XEX files, textures, audio, or other copyrighted material.

You must provide your own game files.
