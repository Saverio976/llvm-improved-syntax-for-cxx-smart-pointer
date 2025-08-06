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
