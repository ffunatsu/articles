---
tags:
  - bevy
  - this-week-in-bevy
---
今日の[This Week in Bevy](https://www.youtube.com/watch?v=g8t-LRr3M2A&t=342s)で取り上げられていたやつ。GizmoをECS気にすることなく使えるようにするやつで、シンプルに便利そう。

https://github.com/atlv24/glizmo

```rust
use bevy::{color::palettes::css::*, prelude::*};

fn main() {
    App::new()
        .add_plugins((DefaultPlugins, glizmo::GlizmoPlugin))
        .add_systems(Update, draw)
        .run();
}

fn draw() {
    glizmo::sphere(Vec3::splat(10.0), 1.0, PURPLE);
}
```

ふと思ったけど、[odin-cc](https://github.com/cc4v/odin-cc)等のいわゆる即時描画系も、こうやって使えるとBevyにも組み込めそうで、Bevy上の即時描画をいろいろ調べてみてもいいかなという気がしてきた。