---
tags:
  - cpp
  - openFrameworks
  - godot
---
https://github.com/kyr0/libsharedmemory

<iframe width="426" height="162" scrolling="no" frameborder="0" src="https://matsubara0507.github.io/github-card/?target=kyr0/libsharedmemory" ></iframe>

少し前に通知が来ていたのだけれど、自分がIssueを立ててた、プロセス間で共有メモリがうまく共有されないなどのバグが解消され、C++20に対応した新しいバージョンが公開された。

作者がサードパーティの例としてofxSharedMemoryを挙げてくれていたので、遅ればせながらv2.0に対応させた。

https://github.com/funatsufumiya/ofxSharedMemory

https://github.com/funatsufumiya/godot_sharedmemory

Godotでも使えるし、例えば動画共有をプロセス間などで行う際に便利と思う。去年その目的で作っていたのだけれど、前述のバグでうまくいかなかったので、その際は別の方法をとってお茶を濁していたので、v2.0になって使っていけるように実用するのがとても楽しみ。