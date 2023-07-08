# 编译问题处理说明

1. Found no assembler Minimum version is nasm-2.13 If you really want to compile without asm, configure with --disable-asm. （缺少nasm 通过brew安装 ）
brew install nasm

2. No working C compiler found.（编译x264时报的错， 如果fdk-aac编译时也报这个也这样处理）
CFLAGS="$CFLAGS -mios-simulator-version-min=7.0" 和 5.0 都改为8.0 就好了，我的是这样

3. no member named “encoderDelay” （ffmpeg编译集成fdk-aac报的错误）
ffmpeg针对fdk-aac,存在如下patch解决此问题。大家可以参考下：https://github.com/libav/libav/commit/141c960e21d2860e354f9b90df136184dd00a9a8.patch 这是由于使用的fdk-aac版本太新，数据结构有所改变。修改文件目录ffmpeg-4.0.2/libavcodec/libfdk-aacenc.c 。另外一种变通的修改方式，降低fdk-aac的版本，也可以解决问题

4. GNU assembler not found, install/update gas-preprocessor
是因为 gas-preprocessor.pl 版本太老了，去这个地址 https://github.com/libav/gas-preprocessor 下载最新的，下载完放在/usr/local/lib/下，增加权限即可。

5. 从 https://www.linuxfromscratch.org/blfs/view/svn/multimedia/fdk-aac.html 下载源码，并解压放入根目录，然后修改脚本脚本（build-fdk-aac.sh）中的源码目录位置，再执行脚本编译即可。

# fdk-aac build script for iOS

See https://github.com/kewlbear/FFmpeg-iOS for .xcframework support, integration with FFmpeg and more.

Shell script to build fdk-aac for use in iOS apps.

## Preparation

```
brew install automake libtool
fdk-aac-X.X.X/autogen.sh
```

## Usage

* Build all:

```
build-fdk-aac.sh
```

* Build for some architectures:

```
build-fdk-aac.sh armv7s x86_64
```

* Build universal library from separately built architectures:

```
build-fdk-aac.sh lipo
```
