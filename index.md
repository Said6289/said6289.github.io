# 101 tricks I learned while making a game or sth

This is a list of some programming "tricks" that are probably
obvious to anyone with half a brain, but that's not me.

- [2D Camera zoom](#2d-camera-zoom)
- [Pixel coordinates to UV coordinate mapping](#pixel-coordinates-to-uv-coordinate-mapping)

## 2D Camera zoom

For a long time when programming camera zoom I used to do something like this:

```
camera_scale += scroll_amount;
```

Here `scroll_amount` is a signed value (integer or float) I get from the underlying
platform layer. It can be transformed to adjust sensitivity or whatever, but
that's the main idea.

The problem with that is it results in non-uniforms zoom "speed". If you're
already zoomed in on something, then any additional scrolling will only slightly
enlarge/shrink the inspected scene. Likewise, the zoom "speed" increases dramatically 
the more you zoom out.

The solution to get a uniform speed is to _multiply_ `scroll_amount` into
`camera_scale` instead:

```
camera_scale *= 1 + scroll_amount;
```

## Pixel coordinates to UV coordinate mapping

To map an integer pixel coordinate `(x, y)` to a pixel center in uv coordinates,
use this formula:

```
u = (x + 0.5) / width;
v = (y + 0.5) / height;
```

If you use this:

```
u = x / width;
v = y / height;
```

then you'll map `(x, y)` to the _left edge_ of the pixel.
