# 1

```
$ wget https://github.com/llvm/llvm-project/releases/download/llvmorg-16.0.0/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04.tar.xz
$ tar xf compiler-rt-16.0.0.src.tar.xz
```

```
$ git clone --recursive https://github.com/sonydevworld/spresense.git
$ git log
commit 81b78d810f6f01cc1493fc9b7b96577f0490e43f (HEAD -> master, tag: v3.0.0, origin/master, origin/HEAD)
Merge: 556c2969 833f1456
Author: SPRESENSE <41312067+SPRESENSE@users.noreply.github.com>
Date:   Mon Mar 13 19:38:42 2023 +0900

    Merge pull request #622 from sonydevworld/develop

    Merge develop into master for v3.0.0 release
```

```
$ source spresenseenv/setup
```

```
$ export PATH=~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin:$PATH
```



```
$ tools/config.py -m
```

![image](https://user-images.githubusercontent.com/424371/229501987-23ea619b-6ade-409c-8619-674c0dd1844d.png)


```
$ make
clang-16: error: configuration file 'armv7em_hard_fpv4_sp_d16_nosys' cannot be found
clang-16: note: was searched for in the directory: ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin
make[1]: Entering directory '~/spresense-clang/nuttx'
clang-16: error: configuration file 'armv7em_hard_fpv4_sp_d16_nosys' cannot be found
clang-16: note: was searched for in the directory: ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin
clang-16: error: configuration file 'armv7em_hard_fpv4_sp_d16_nosys' cannot be found
clang-16: note: was searched for in the directory: ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin
make[2]: Entering directory '~/spresense-clang/sdk/apps'
clang-16: error: configuration file 'armv7em_hard_fpv4_sp_d16_nosys' cannot be found
clang-16: note: was searched for in the directory: ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin
make[3]: Entering directory '~/spresense-clang/sdk/apps/builtin'
clang-16: error: configuration file 'armv7em_hard_fpv4_sp_d16_nosys' cannot be found
clang-16: note: was searched for in the directory: ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin
clang-16: error: configuration file 'armv7em_hard_fpv4_sp_d16_nosys' cannot be found
clang-16: note: was searched for in the directory: ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin
ERROR: clang failed: 1
```


```
$ cat armv7em_hard_fpv4_sp_d16_nosys
-target armv7e-m-none-none-eabi

-mthumb

-nostdinc

-I ~/spresense/sdk/apps/include
-I ~/spresense/nuttx/include/
-I ~/spresense/nuttx/../sdk/include/

-I ~/spresense/nuttx/include/nuttx/lib/
-I ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/include/c++/v1/
-I ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/include/c++/v1/experimental/
-I ~/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/include/x86_64-unknown-linux-gnu/c++/v1/
```


build compiler-rt

```
cmake ../compiler-rt \
-G Ninja \
-DCMAKE_AR=$HOME/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin/llvm-ar \
-DCMAKE_ASM_COMPILER_TARGET="arm-linux-gnueabihf" \
-DCMAKE_ASM_FLAGS="-march=armv7a -mthumb --gcc-toolchain=$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/bin --sysroot=$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/libc" \
-DCMAKE_C_COMPILER=$HOME/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin/clang-16 -DCMAKE_C_COMPILER_TARGET="arm-linux-gnueabihf" \
-DCMAKE_C_FLAGS="-march=armv7a -mthumb -Qunused-arguments --gcc-toolchain=$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/bin --sysroot=$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/libc -B $HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/lib/gcc/arm-none-linux-gnueabihf/10.3.1/ -L$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/lib/gcc/arm-none-linux-gnueabihf/10.3.1/ -L$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/libc/usr/lib/ -I $HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/include/c++/10.3.1/" \
-DCMAKE_EXE_LINKER_FLAGS="-fuse-ld=lld" \
-DCMAKE_CXX_COMPILER_TARGET="arm-linux-gnueabihf" \
-DCMAKE_CXX_FLAGS=" -Werror=unknown-argument --gcc-toolchain=$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/bin --sysroot=$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/libc -B $HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/lib/gcc/arm-none-linux-gnueabihf/10.3.1/ -L$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/lib/gcc/arm-none-linux-gnueabihf/10.3.1/ -L$HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/libc/usr/lib/ -I $HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/include/c++/10.3.1/ -I $HOME/armgcc/gcc-arm-10.3-2021.07-x86_64-arm-none-linux-gnueabihf/arm-none-linux-gnueabihf/include/c++/10.3.1/arm-none-linux-gnueabihf/" \
-DCMAKE_NM=$HOME/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin/llvm-nm \
-DCMAKE_RANLIB=$HOME/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin/llvm-ranlib \
-DCOMPILER_RT_BUILD_BUILTINS=ON \
-DCOMPILER_RT_BUILD_LIBFUZZER=OFF \
-DCOMPILER_RT_BUILD_MEMPROF=OFF \
-DCOMPILER_RT_BUILD_PROFILE=OFF \
-DCOMPILER_RT_BUILD_SANITIZERS=OFF \
-DCOMPILER_RT_BUILD_XRAY=OFF \
-DCOMPILER_RT_DEFAULT_TARGET_ONLY=ON \
-DLLVM_CMAKE_DIR=$HOME/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/lib/cmake/ \
-DLLVM_CONFIG_PATH=$HOME/clang+llvm-16.0.0-x86_64-linux-gnu-ubuntu-18.04/bin/llvm-config
```


```
$ make EXTRA_LIBS=/home/linuxyutaka/llvm-project/build-compiler-rt/lib/linux/libclang_rt.builtins-armhf.a
```
