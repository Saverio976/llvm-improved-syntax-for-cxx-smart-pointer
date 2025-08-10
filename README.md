> [!NOTE]
> This is a fork of the [llvm-project](https://github.com/llvm/llvm-project)

> [!NOTE]
> This fork is part of my dissertation at [Heriot Watt University](https://www.hw.ac.uk/), [School of Mathematical and Computer Science](https://www.hw.ac.uk/about/our-schools/mathematical-and-computer-sciences).

# LLVM - An Improved Syntax for C++ Smart Pointers

## Usage

### 1. Code with the syntax

`test.cpp`
```c
#include <memory>

int some_func(int %value) { return *value; }

int main() {
    int %value = std::make_unique<int>(1);
    some_func(std::move(value));
}
```

> [!NOTE]
> Caution with the tokens. It can be interpreted as a trigraph in case of templates
> For examples: `vector<int %>`. `%>` trigraph means `}`. To accept this syntax a space must be insert between the 2 characters.

### 2. Command Line Argument

Specify what smart pointer to use for a specific token.

```bash
clang++   '-smart-pointer=%,std::unique_ptr'  test.cpp
```

## Documentation

You can specify multiple token for different smart pointers

```bash
clang++ \
    -smart-pointer=%,std::shared_ptr   \
    '-smart-pointer=|,std::unique_ptr' \
    a.c
clang++   -smart-pointer=%,std::weak_ptr      a.c
clang++   -smart-pointer=|,std::shared_ptr    a.c
```

## Build

```
# if you have not clone the project:
git clone https://github.com/Saverio976/llvm-improved-syntax-for-cxx-smart-pointer.git llvm-smart-pointer-syntax
cd llvm-smart-pointer-syntax
# in the repository folder
mkdir build
cd build
# in the build folder
cmake -DCMAKE_INSTALL_PREFIX=$(pwd)/install -DLLVM_ENABLE_PROJECTS="clang;clang-tools-extra" -DCMAKE_BUILD_TYPE=Release ../llvm
make # you can specify how much core on your cpu to use. for example with 4 cores: -j4
# make -j4
```

- The `clang++` binary will be at `./bin/clang++`
- The `clangd` binary will be at `./bin/clangd`
