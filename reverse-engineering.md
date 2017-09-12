# Reverse Engineering

## .Net

If dealing with a .net binary then the easiest way to decompile is to use [dotPeek](https://www.jetbrains.com/decompiler/).  This can decompile and then export the decompiled binary as a project, making it incredibly easy to debug and work out behaviour.  Of course this is assuming no obfuscation has taken place.

## Classic C Binaries

### ltrace

This will show all function calls made by a program.

### strace

This will show all syscalls made by a program.



