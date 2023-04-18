# Dramatic EDitor

**THIS SOFTWARE IS UNFINISHED!!! Don't have any high expectations.**

# Quick Start
soon

# Build dependencies
## Linux
Debian/Ubuntu:
```
sudo apt install build-essential cmake git
```
Arch/Manjaro:  
```
pacman -S git cmake
```
## Windows
[Visual Studio](https://visualstudio.microsoft.com/)  
[CMake](https://cmake.org/download/)  
[git](https://github.com/git-for-windows/git/releases/latest)

## macOS
```
brew install git cmake
```
---
# Clone repo

```
git clone --recursive https://github.com/Mr-Precise/ded

cd ded
```
---
# Build
### Linux/macOS
```
mkdir build && cd build  
cmake ..
```
```
make -j$(($(nproc) + 1))
```
or build using cmake:
```
cmake --build . --config Release
```

### (old/deprecated) build script:
```console
$ ./build.sh
$ ./ded src/main.c
```

### MSVC (old/deprecated build script)
```console
> .\setup_dependencies.bat
> .\build_msvc.bat
> .\ded.exe src\main.c
```
---
# Used dependencies

### Libs

- [SDL2 2.0.9+](https://www.libsdl.org/)
- [FreeType 2.13.0+](https://freetype.org/)
- [GLEW 2.1.0+](https://glew.sourceforge.net/)

### Fonts

- Victor Mono: https://rubjo.github.io/victor-mono/
- Iosevka: https://github.com/be5invis/Iosevka
