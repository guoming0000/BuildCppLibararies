# Windows

1. 目录介绍
```
workspace\
  build\       - this is where the out-of-tree thrift cmake builds are generated
  dist\        - this is where the thrift build results end up
  thirdparty\  - this is where all third party binaries and libraries live
    build\       - this is where all third party out-of-tree builds are generated
                   (except for openssl, which only builds in-tree)
    dist\        - this is where all third party distributions end up
    src\         - this is where all third party source projects live
  scripts\     - batch files used to set environment variables for builds
  thrift\      - this is where the thrift source project lives
```

2. 下载winflexbison放到workspace\thirdparty\dist\winflexbison 下，注意是dist目录，和下面的不同。building the compiler时需要使用，因为是二进制的，不用编译了。

```
source: web site
location: https://sourceforge.net/projects/winflexbison/files/win_flex_bison-latest.zip/download
version: "latest"
directory: workspace\thirdparty\dist\winflexbison
```

3. 下载zlib放到workspace\thirdparty\src\zlib-1.2.9，下来要调用build-zlib.bat编译。

```
source: web site
location: http://zlib.net/
version: 1.2.9
directory: workspace\thirdparty\src\zlib-1.2.9
```

4. 下载openssl放到workspace\thirdparty\src\openssl-1.1.0c，注意要修改文件夹名字。由于zlib，libevent等都是动态库，所以要把openssl改成动态库，修改文件Configurations/10-main.conf 内容为（直接替换）：

```
"VC-noCE-common" => {
    inherit_from     => [ "VC-common" ],
    template         => 1,
    cflags           => add(picker(default => "-DUNICODE -D_UNICODE",
                                   debug   => "/MDd /Od -DDEBUG -D_DEBUG",
                                   release => "/MD /O2"
                                  )),
    bin_cflags       => add(picker(debug   => "/MDd",
                                   release => "/MD",
                                  )),
    bin_lflags       => add("/subsystem:console /opt:ref"),
    ex_libs          => add(sub {
        my @ex_libs = ();
        push @ex_libs, 'ws2_32.lib' unless $disabled{sock};
        push @ex_libs, 'gdi32.lib advapi32.lib crypt32.lib user32.lib';
        return join(" ", @ex_libs);
    }),
},
```

build-openssl.bat

5. 然后执行编译流程，生成thrift.exe（即thrift Compiler，这一步随便做不做，因为可以下载到）

```shell
%comspec% /k "D:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" x86
cd E:\LIBS\Thrift\thrift-0.11.0\build\wincpp\thirdparty\src
E:
set COMPILER=vc120
build-zlib.bat
build-openssl.bat
```

复制源码thrift-0.11.0.tar.gz到E:\LIBS\Thrift\thrift-0.11.0\build\wincpp目录下解压，重命名为thrift，再输入编译命令

```
cd E:\LIBS\Thrift\thrift-0.11.0\build\wincpp
E:
set COMPILER=vc120

;如果生成X64的请用下面2个
;set GENARCH= Win64
;set GENERATOR=Visual Studio 12 2013%GENARCH%

;如果生成X86的请用下面1个
set GENERATOR=Visual Studio 12 2013

build-thrift-compiler.bat
```

6. 编译C++运行库

```
cd E:\LIBS\Thrift\thrift-0.11.0\build\wincpp
E:
;这里要不要设置不清楚，估计沿用上面的设置
build-thrift.bat
```



