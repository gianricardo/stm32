# STM32 project template

This repository holds all the possible tools and templates for building and developing STM32 projects.  
Firmware is configured with CMake and can be built in multiple ways.  

I don't expect you to use all of the files found here or all the lines in the configuration files (Makefile, CMakeLists.txt, etc.). Make sure to read them and remove what you don't need.  

I will also try to make this repository as cross platform as possible. Currently I've tested it on Windows 10 and Linux (Fedora 35, Nixos, Ubuntu 20.04). Any problems regarding a specific distribution will be mentioned in the [workflows](#workflow).  

## Example

You can utilize everything in root of the repository. It holds an example project built around `STM32F407VG` on a discovery board. It blinks the onboard blue LED and prints "Hello, world" over SWO.  

## Workflow

There are a few options to build the CMake project. Approaches differ in dependency management and source control, which is important when you might work in a team or share your projects online. Pros and cons will be listed for each method in its guide. The following guides are available:  

[First way](docs/build-native.md) is to use the provided [Makefile](Makefile) for native development on your computer, You have to install the required dependencies on your computer, matching or exceeding the minimum required versions.  

[Second way](docs/build-container.md) is using a container tool like `docker` or `podman` with or without additional dependencies, like `docker-compose` or `make`.  

[Third way](docs/build-nix.md) is using nix with provided `flake.nix`.  

[Fourth way](docs/build-native-in-container.md) is to use the provided [Makefile](Makefile) for native development on your computer in a devcontainer, You have to install the required dependencies on your computer (VSCode, git, Docker) and, then, open the development folder (stm32) in a container (ONLY tested on linux).

## Code style

Before committing code, format all source files. I have provided a set of [rules](.clang-format) for `clang-format`, which you can apply to all source files.  

You can do it for a specific file: `clang-format -i <path to source file>`  

Or with a shell script:  

```shell
for file in $(find . -name '*.[ch]' -or -name '*.[ch]pp'); do clang-format -i $file; done
```

Or with provided makefile:

`make format`: formats all source files in root using host installed clang-format.  
`make format-container`: formats all source files in root using container installed clang-format.  

## Extra tool tips

I have provided a [document](docs/tools.md), where I write about tools and commands that you can use to aid your development.  

## Extra code tips

I have provided a [document](docs/code-tips.md), where I write about useful options for writing and debugging code.  
