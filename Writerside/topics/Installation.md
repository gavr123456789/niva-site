# Installation
There are 2 way to install it, the JVM and Native.  
JVM is much easier but will compile 10-30% slower.

## Compile from sources
### Gradle
If you have graalvm in your JAVA_HOME then inside /Niva/Niva/Niva folder run:
`./gradlew buildJvmNiva` this will create jvm based binary in ~/.niva/niva/bin
`./gradlew buildNativeNiva` this will create native binary in ~/.niva/bin if you have GraalVM as ur default JVM.


#### How to install GraalVM
Arch: `yay -S jdk22-graalvm-bin`  
`archlinux-java status`  
`archlinux-java set <JAVA_ENV_NAME>`  
[select java on arch](https://wiki.archlinux.org/title/Java#Switching_between_JVM)

macOS: `brew install --cask graalvm-jdk`  
`/usr/libexec/java_home -V`  
`export JAVA_HOME='/usr/libexec/java_home -v 22.0.2'`  
[select java on mac os](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-macos)  
If you have `expanded from macro 'NS_FORMAT_ARGUMENT'` problem with buildNativeNiva on macOS then [update XCode](https://wails.io/docs/guides/troubleshooting/#my-mac-app-gives-me-weird-compilation-errors)
`xcode-select -p && sudo xcode-select --switch /Library/Developer/CommandLineTools`


## Get binaries from releases
TODO



