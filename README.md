# VulkanPorQue4K
4K yee  
We out here, vibin'

# Building

## Build Windows

`setup_windows.bat`
```
$ mkdir buildWinVS2017
$ cd buildWinVS2017
$ cmake -G "Visual Studio 15 2017 Win64" -DDXC_PATH="C:\src\sc\dxc\d0e9147a\bin\dxc.exe" ..
```

Then build from generated Visual Studio solution `buildWinVS2017\VulkanPorQue4K.sln`

## Build GGP

### Deploy Assets

Deploy assets to GGP instance (from repo root folder):  
`deploy_assets.bat`|`deploy_assets.sh`
```
> ggp ssh put -r ./assets
```

### Build GGP + Ninja

`setup_ggp_ninja.sh`
```
$ mkdir buildGGP
$ cd buildGGP
$ cmake -G Ninja .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE="$GGP_SDK_PATH/cmake/ggp.cmake" -DCMAKE_BUILD_WITH_INSTALL_RPATH=TRUE -DDXC_PATH="C:\src\sc\dxc\d0e9147a\bin\dxc.exe"
$ ninja
```

Then deploy the 4KApp binary to gamelet
```
> ggp ssh put ./buildGGP/src/app/4KApp
> ggp run --cmd "4KApp"
```

### Build GGP + Visual Studio
Right now, it's not entirely clear how to use CMake to directly generate a GGP-compatible
Visual Studio solution. However, we can get pretty close, and fix up the last deltas with
a custom script. Obviously, this is fragile, but it works ok for now.

`setup_ggp_vs.bat`
```
$ mkdir buildGGPVS
$ cd buildGGPVS
$ cmake -G "Visual Studio 15 Win64" -DDXC_PATH="C:\src\sc\dxc\fdd4e3b0\bin\dxc.exe" .. -DCMAKE_TOOLCHAIN_FILE="$GGP_SDK_PATH/cmake/ggp.cmake"
$ python3 ../ggpvs_conversion.py3 .
```

Then open `buildGGPVS/VulkanPorQue4K.sln` with Visual Studio 2017. Building should work, though
getting the debugging working is still TBD (not deploying binary correctly yet). 

## Build Linux
TBD - Help!

# Upscaling Techniques
See the [Techniques Hub document](docs/TECHNIQUES.md).

# Contributing
[Contribution guidelines for this project](docs/CONTRIBUTING.md).

# DirectXShaderCompiler Info
This project uses [DXC](https://github.com/microsoft/DirectXShaderCompiler) to compile HLSL shaders to SPIR-V.

Currently, the project has been verified against [9c89a1c2](https://github.com/microsoft/DirectXShaderCompiler/commit/9c89a1c2c6baa76dabc154f126408973848b0069).