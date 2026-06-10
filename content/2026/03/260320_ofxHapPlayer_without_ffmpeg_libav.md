---
title: 260320 ofxHapPlayer without ffmpeg/libav
tags:
  - openFrameworks
  - cpp
---
https://github.com/ffunatsu/ofxHapPlayer/tree/feat/remove_ffmpeg

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=funatsufumiya/ofxHapPlayer" ></iframe>

以前から気になって[Issue](https://github.com/bangnoise/ofxHapPlayer/issues/84)も立ててたけど誰も反応がなかった、ofxHapPlayerのLGPL/GPL問題（ffmpeg/libavのstatic libを使っている問題）をようやく解決させた。

具体的には、[TrussC](https://github.com/TrussC-org/TrussC)の[`tcxMovParser`](https://github.com/TrussC-org/TrussC/blob/94c98cf79ab68038e86168d1042a2e9cbb35ba2f/addons/tcxHap/src/tcxMovParser.h)がlibav/ffmpegに非依存なように作られていたことに気づいて、それを移植した形。先人いてくれて感謝。

その代わり、音声再生は非対応とした。一応出来はしたものの、AAC再生に対応できなかったり制約があったし、元々ofxHapPlayerはWindowsでは音が出なかったっぽいので、まぁいっかと思って切り捨てた。（必要なら同尺の音声ファイル作って時間を同期すれば良いと思う。）