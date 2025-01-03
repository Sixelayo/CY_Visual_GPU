# cuda template CY Visual
 
starter kit for GPU programming. This repo is made primarily for windows / VScode but can be used anywhere with some tweaking
This template can replace the old one using freeglut.
- uses opengl / GLFW
- has the same functionnalities than the previous one
- support for window resizing added
- added ImGui - docking branch 

# Requierement

## Compiler

While this repo is made for VSCode, it is **highly recommended** (*and probably mandatory*) to install [VSCommunity](https://visualstudio.microsoft.com/fr/vs/community/)  with c/c++ desktop dev before installing cuda, because the nvcc compiler relies on MSVC toolchains which is installed by Microsoft Visual Studio.

## CUDA toolkit 12.5

Download and execute the version matching your OS (may take a few minutes):
https://developer.nvidia.com/cuda-12-5-0-download-archive

At this point you can easily check that your cuda install is working by cloning the [cuda samples repo](https://github.com/NVIDIA/cuda-samples). 

**FIXME** (replace with exact string)
then in VSCommunity > open solution > select cuda samples .sln (may take a few minutes)
on any project : right clic > set as start up project
Project to check :
- param
- bandwidth
- fluid


## Recommended VS Code extension

- C/C++ : for intellisense
- Nsight Visual Studio Code extension : for advanced debugging. for more details see https://www.youtube.com/watch?v=gN3XeFwZ4ng


## GLFW 

This repo already contains `/lib/glfw3_mt.lib` which is a pre-compilled glfw runtime binaries for windows 10  / Visual C++ 2022.

if you're using a different OS, you need to replace it with the expected binaries :
- glfw binaries : https://www.glfw.org/download.html


# Compiling

## Instruction

1. add a few things to path. The location that you need to add should look like this :
```
C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\bin\Hostx64\x64
```

2. create `/exe/` diretory 

3. If you're using VSCode You can simply run the provided task (`ctrl+shift+p > Run Task > build nvcc`)

It will simply run the following command :
```shell
nvcc template.cu ./imgui/*.cpp -o ./exe/template.exe -I./include/ -L./lib/ -lglfw3_mt -lopengl32 -luser32 -lgdi32 -lshell32
```
The wildcard `*.cpp` might not expand, in that case you'll need to use a script or list file individually
```shell
nvcc template.cu (Get-ChildItem -Path .\\imgui\\ -Filter *.cpp | ForEach-Object { $_.FullName }) -o ./exe/template.exe -I./include/ -L./lib/ -lglfw3_mt -lopengl32 -luser32 -lgdi32 -lshell32
```
```shell
nvcc template.cu ./imgui/imgui_draw.cpp ./imgui/imgui_impl_glfw.cpp ./imgui/imgui_impl_opengl3.cpp ./imgui/imgui_tables.cpp ./imgui/imgui_widgets.cpp ./imgui/imgui.cpp -o ./exe/template.exe -I./include/ -L./lib/ -lglfw3_mt -lopengl32 -luser32 -lgdi32 -lshell32
```

## Static vs dynamic compilation for GLFW

The VScode task will compile statically. (as it's obviously recommended).
If for some reason you want to compile dynamically, check the readme of glfw pre-compiled binairies
At the very least you'll need to add glfw3.dll to your .exe directory (or path) and link with glfw3dll.lib. Note that you can get rid of `-luser32`, `-lgdi32`, `-lshell32` if you're compiling dynamically

# Intellisense for librairies :

Simply update the include path in `c_cpp_properties.json`. For instance :

```json
"includePath": [
                "${workspaceFolder}/**",
                "O:/vcpkg/installed/x64-windows/include/**"
            ],
```



# Using ImGui

You can learn more about ImGui here : [Original repo](https://github.com/ocornut/imgui)

The easiest way to learn about ImGui is by running the example file and fooling arround. Then just ctrl+f in `imgui_demo.cpp` to find out how things are done

**FIXME** (commmand to be confirmed)
**FIXME** (add the option to remove debug compilation option for imgui)

If you MSYS2 - mingw64 then you can install GLFW in mingw64 shell with 
```shell
pacman -S mingw-w64-x86_64-glfw
```

And compile `imgui_example.cpp` with :
```shell
g++ imgui_example.cpp ./imgui/*.cpp -o ./exe/example.exe -lopengl32 -lglfw3
```

Otherwise, you need to to precise include path to g++.
If you're using windows, you'll also need to make glfw3.dll visible from you're OS. either add `glfw3.dll` in the same directory than your executable or add `glfw-3.4.bin.WIN64\lib-vc2022` to PATH environnement variable
```shell
g++ imgui_example.cpp ./imgui/*.cpp -o ./exe/example.exe -I./include/ -L./lib/ -lopengl32 -lglfw3
```

But please **DO NOT** manually add dll to `/system32/`
