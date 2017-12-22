#安装grpc--windows端

https://github.com/grpc/grpc/blob/master/INSTALL.md

- Install Visual Studio 2015 or 2017 (Visual C++ compiler will be used).

- Install [Git](https://git-scm.com/).

- Install [CMake](https://cmake.org/download/).

- Install [Active State Perl](https://www.activestate.com/activeperl/) (`choco install activeperl`) - *required by boringssl*

- Install [Go](https://golang.org/dl/) (`choco install golang`) - *required by boringssl*

- Install [yasm](http://yasm.tortall.net/) and add it to `PATH` (`choco install yasm`) - *required by boringssl*

- (Optional) Install [Ninja](https://ninja-build.org/) (`choco install ninja`)

  ​

1.首先要安装安装choco.exe（现在开始都需要翻墙，打开ishadowsocks的全局模式），使用管理员权限打开cmd.exe或者powershell

https://chocolatey.org/install

```
;如果是cmd输入命令：
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

;如果是powershell
Set-ExecutionPolicy AllSigned
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

2.安装active state perl

```
choco install activeperl
```

3.安装go

```
choco install golang
```

4.安装yasm

```
choco install yasm
```

5.安装Ninja

```
choco install ninja
```

6.编译源码。cmd.exe创建gRPC文件夹，进入文件夹输入以下命令

```shell
::下载
::原来的下载指令是这个,但是由于boringssl-with-bazel无法下载，我重新fork了删掉了 powershell git clone --recursive -b ((New-Object System.Net.WebClient).DownloadString(\"https://grpc.io/release\").Trim()) https://github.com/grpc/grpc

powershell git clone --recursive -b master https://github.com/guoming0000/grpc

::上面的也失败了，只能分别下载
git clone https://github.com/madler/zlib
git clone https://github.com/google/protobuf.git -b 3.0.x
git clone https://github.com/gflags/gflags.git
git clone https://github.com/google/googletest.git
::https://github.com/google/boringssl.git
git clone https://github.com/google/benchmark
::git clone https://boringssl.googlesource.com/boringssl
git clone https://github.com/c-ares/c-ares.git -b cares-1_13_0
git clone https://github.com/google/bloaty.git
git clone https://github.com/abseil/abseil-cpp
::注意cares放的路径特别点，是third_party/cares/cares里

::进入目录后进行使用cmake生成sln
%comspec% /k "D:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Auxiliary\Build\vcvarsall.bat" x86

::或者

%comspec% /k "D:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" x86

cd E:\LIBS\gRPC\grpc
E:
mkdir .build
cd .build
::cmake .. -G "Visual Studio 15 2017" -DCMAKE_BUILD_TYPE=Debug
cmake .. -G "Visual Studio 12 2013" -DCMAKE_BUILD_TYPE=Debug
cmake --build .
```



```
::选取最新的release版本
git clone --branch v1.8.2 https://github.com/grpc/grpc.git
cd grpc
::把.gitmodules中的https://boringssl.googlesource.com/boringssl改成https://github.com/airtimemedia/boringssl.git
::修改WORKSPACE中的url = "https://boringssl.googlesource.com/boringssl/+archive/886e7d75368e3f4fab3f4d0d3584e4abfc557755.tar.gz",为url = "https://github.com/airtimemedia/boringssl/archive/f9f31ae338e204b9b4df2c4894bd3f4065e0bb04.tar.gz"----这个值通过commit页面的copy the full SHA得到


::更新依赖
git submodule update --init
```

