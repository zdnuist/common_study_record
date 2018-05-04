#### TinkerApplication
```
//1
Class<?> delegateClass = Class.forName(delegateClassName, false, getClassLoader());

```

#### dex2oat
```
// dex2oat == dex file to oat file, 就是新旧两种运行时文件的转换，一个可执行ELF文件，在手机Terminal中可以直接调用

Usage: dex2oat [options]...
--dex-file=<dex-file>: specifies a .dex file to compile.
    Example: --dex-file=/system/framework/core.jar
--zip-fd=<file-descriptor>: specifies a file descriptor of a zip file
    containing a classes.dex file to compile.
    Example: --zip-fd=5
--zip-location=<zip-location>: specifies a symbolic name for the file
    corresponding to the file descriptor specified by --zip-fd.
    Example: --zip-location=/system/app/Calculator.apk
--oat-file=<file.oat>: specifies the oat output destination via a filename.
    Example: --oat-file=/system/framework/boot.oat
--oat-fd=<number>: specifies the oat output destination via a file descriptor.
    Example: --oat-fd=6
--oat-location=<oat-name>: specifies a symbolic name for the file corresponding
    to the file descriptor specified by --oat-fd.
    Example: --oat-location=/data/dalvik-cache/system@app@Calculator.apk.oat
--oat-symbols=<file.oat>: specifies the oat output destination with full symbols.
    Example: --oat-symbols=/symbols/system/framework/boot.oat
--bitcode=<file.bc>: specifies the optional bitcode filename.
    Example: --bitcode=/system/framework/boot.bc
--image=<file.art>: specifies the output image filename.
    Example: --image=/system/framework/boot.art
--image-classes=<classname-file>: specifies classes to include in an image.
    Example: --image=frameworks/base/preloaded-classes
--base=<hex-address>: specifies the base address when creating a boot image.
    Example: --base=0x50000000
--boot-image=<file.art>: provide the image file for the boot class path.
    Example: --boot-image=/system/framework/boot.art
    Default: $ANDROID_ROOT/system/framework/boot.art
--android-root=<path>: used to locate libraries for portable linking.
    Example: --android-root=out/host/linux-x86
    Default: $ANDROID_ROOT
--instruction-set=(arm|arm64|mips|x86|x86_64): compile for a particular
    instruction set.
    Example: --instruction-set=x86
    Default: arm
--instruction-set-features=...,: Specify instruction set features
    Example: --instruction-set-features=div
    Default: default
--compiler-backend=(Quick|Optimizing|Portable): select compiler backend
    set.
    Example: --compiler-backend=Portable
    Default: Quick
--compiler-filter=(verify-none|interpret-only|space|balanced|speed|everything):
    select compiler filter.
    Example: --compiler-filter=everything
    Default: speed
--huge-method-max=<method-instruction-count>: the threshold size for a huge
    method for compiler filter tuning.
    Example: --huge-method-max=10000
    Default: 10000
--huge-method-max=<method-instruction-count>: threshold size for a huge
    method for compiler filter tuning.
    Example: --huge-method-max=10000
    Default: 10000
--large-method-max=<method-instruction-count>: threshold size for a large
    method for compiler filter tuning.
    Example: --large-method-max=600
    Default: 600
--small-method-max=<method-instruction-count>: threshold size for a small
    method for compiler filter tuning.
    Example: --small-method-max=60
    Default: 60
--tiny-method-max=<method-instruction-count>: threshold size for a tiny
    method for compiler filter tuning.
    Example: --tiny-method-max=20
    Default: 20
--num-dex-methods=<method-count>: threshold size for a small dex file for
    compiler filter tuning. If the input has fewer than this many methods
    and the filter is not interpret-only or verify-none, overrides the
    filter to use speed
    Example: --num-dex-method=900
    Default: 900
--host: used with Portable backend to link against host runtime libraries
--dump-timing: display a breakdown of where time was spent
--include-patch-information: Include patching information so the generated code
    can have its base address moved without full recompilation.
--no-include-patch-information: Do not include patching information.
--include-debug-symbols: Include ELF symbols in this oat file
--no-include-debug-symbols: Do not include ELF symbols in this oat file
--runtime-arg <argument>: used to specify various arguments for the runtime,
    such as initial heap size, maximum heap size, and verbose output.
    Use a separate --runtime-arg switch for each argument.
    Example: --runtime-arg -Xms256m
--profile-file=<filename>: specify profiler output file to use for compilation.
--print-pass-names: print a list of pass names
--disable-passes=<pass-names>:  disable one or more passes separated by comma.
    Example: --disable-passes=UseCount,BBOptimizations
```
参考
[1]https://yq.aliyun.com/articles/8944
