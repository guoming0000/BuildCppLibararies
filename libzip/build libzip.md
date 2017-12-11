%comspec% /k "D:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\vcvarsall.bat" x86
cd E:\LIBS\LibZip\libzip-1.2.0\vstudio
E:
vsbuild clean
vsbuild build "Visual Studio 12" v120_xp