# Installation

## Compile from sources
P.S. u can find binary releases in the releases  
After running compile.sh you will get niva_compiler folder that contains jvm or native binary.

### JVM
1) `sh compile.sh jvm`
2) run compiler from bin folder
### Native
1) install graalvm `yay -S jdk21-graalvm-bin` and set it default: `sudo archlinux-java set java-21-graalvm` on Arch, `nix shell nixpkgs#graalvm-ce` on nix
2) `sh compile.sh bin`
### Usage

Niva can eat .niva and .scala files, because Scala highlight fits well for Niva :3
`niva main.niva` - compile and run  
`niva main.niva -с` - compile only, will create binary for native target and fat-jar for jvm
`niva main.niva -i > info.md` - will generate info about all code base of the projects, `-iu` - only user defined files
`niva run` - run all files in current folder with main.niva as entry point
`niva build` - produce binary

[VS Code plugin](https://github.com/gavr123456789/niva-vscode-bundle) for syntax highlighting

## Get binaries from releases
TODO
