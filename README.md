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
