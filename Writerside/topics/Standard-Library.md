# Standard Library

The standard library in Niva doesn’t include any functions related to IO (such as file reading or console input), 
as future backends (platforms) beyond the JVM are planned, and such operations may not make sense on all of them.  
What happens when you compile code containing file reading in JS or WASM and run it in a browser? Nothing good.

Currently, you can access all necessary functions by adding a file with bindings to your project.  
A list of such files can be found here:
https://github.com/gavr123456789/bazar/tree/main/Bindings

- okio – for filesystem operations
- kotlinx.serialization.json – for JSON
- gtk – for GUI
