# VapourSynth-JincResize

[![Build Status](https://api.travis-ci.org/Kiyamou/VapourSynth-JincResize.svg?branch=master)](https://travis-ci.org/github/Kiyamou/VapourSynth-JincResize)

## Description

JincResize is a resizer plugin for VapourSynth, works by Jinc function and elliptical weighted averaging (EWA). Support 8-16 bit and 32 bit sample type. Support YUV color family.

### master branch

The master branch (Release r7-RC1) is ported from [AviSynth plugin](https://github.com/AviSynth/jinc-resize) and based on [EWA-Resampling-VS](https://github.com/Lypheo/EWA-Resampling-VS).

The master branch is in developing. It's much faster than "r6.1" branch and adds parameters. But it has bug when there are texts on screen for 8bit and 16 bit input, while it's normal for 32bit input and 9-15bit input.

### "r6.1" branch

The "r6.1" branch (Release r1 - r6.1) is old version, based on [EWA-Resampling-VS](https://github.com/Lypheo/EWA-Resampling-VS).

### More

If want to learn more about Jinc, you can read the [post](https://zhuanlan.zhihu.com/p/103910606) (Simplified Chinese / 简体中文).

## Usage

```python
core.jinc.JincResize(clip clip, int width, int height[, int tap, float blur])
```

* ***clip***
    * Required parameter.
    * Clip to process.
    * Integer sample type of 8-16 bit depth and float sample type of 32 bit depth is supported.
* ***width***
    * Required parameter.
    * The width of output.
* ***height***
    * Required parameter.
    * The height of output.
* ***tap***
    * Optional parameter. Range: 1–16. *Default: 3*.
    * Corresponding to different zero points of Jinc function.
    * The recommended value is 3, 4, 6, 8, which is similar to the [AviSynth plugin](https://github.com/AviSynth/jinc-resize),  ` Jinc36Resize `, ` Jinc64Resize `, ` Jinc128Resize `, ` Jinc256Resize `.
* ***blur***
    * Optional parameter. *Default: 0.9812505644269356*.
    * Blur processing, it can reduce side effects.
    * To achieve blur, the value should less than 1.
    * If don't have relevant knowledge or experience, had better not modify the parameter.

**The added parameters in Release r7-RC1**

```python
core.jinc.JincResize(clip clip, int width, int height[, float crop_left, float crop_top,
                     float crop_width, float crop_height, int tap, float blur])
```

* ***crop_left***
  * Optional parameter. *Default: 0.0*.
  * Same as `src_left` in [AviSynth](http://avisynth.nl/index.php/Resize#Common_Parameters).
* ***crop_top***
  * Optional parameter. *Default: 0.0*.
  * Same as `src_top` in [AviSynth](http://avisynth.nl/index.php/Resize#Common_Parameters).
* ***crop_width***
  * Optional parameter. *Default: the width of input*.
  * Same as `src_width` in [AviSynth](http://avisynth.nl/index.php/Resize#Common_Parameters).
* ***crop_height***
  * Optional parameter. *Default: the height of input*.
  * Same as `src_height` in [AviSynth](http://avisynth.nl/index.php/Resize#Common_Parameters).

## Tips

JincResize will lead to ringing. A solution is to use de-ringing as post-processing, such as `HQDeringmod()` in [havsfunc](https://github.com/HomeOfVapourSynthEvolution/havsfunc). A simple example as follows.

```python
from vapoursynth import core
import havfunc as haf

# src is a 720p clip
dst = core.jinc.JincResize(src, 1920, 1080)
dst = haf.HQDeringmod(dst)
```

## Compilation

### Windows

```bash
x86_64-w64-mingw32-g++ -shared -static -std=c++17 -O3 -march=native JincResize.cpp -o JincResize.dll
```

`VapourSynth.h` and `VSHelper.h` is need. You can get them from [here](https://github.com/vapoursynth/vapoursynth/tree/master/include) or your VapourSynth installation directory (`VapourSynth/sdk/include/vapoursynth`).

### Linux

```bash
meson build
ninja -C build
```
or dircetly

```bash
g++ -shared -fPIC -std=c++17 -O3 -march=native JincResize.cpp -o JincResize.so
```

## Acknowledgement

EWA-Resampling-VS: https://github.com/Lypheo/EWA-Resampling-VS

AviSynth plugin: https://github.com/AviSynth/jinc-resize
