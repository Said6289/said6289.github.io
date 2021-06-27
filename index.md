# 101 tricks I learned while making a game or sth

This is a list of some programming "tricks" that are probably
obvious to anyone with half a brain, but that's not me.

- [2D Camera zoom](#2d-camera-zoom)
- [Pixel coordinates to UV coordinate mapping](#pixel-coordinates-to-uv-coordinate-mapping)
- [Incrementing dereferenced pointers](#incrementing-dereferenced-pointers)
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


## incrementing dereferenced pointers

This small mistake has caused me more headache than I'm proud to admit. So here's what it is and how to deal with it:

Say you have an integer `int index = 3` and a pointer to the integer:
 ```int *pointer = &index```
And now you're halfway down your code, you want to increment your original integer. You will have to dereference it (since it's a pointer) and add to it. 

You know that in order to add by one you can either:    
```index = index + 1;```    
Or    
```index += 1;```    
Or    
```index++;```    

And in order to increment using the pointer we defined:    

```*pointer = *pointer + 1;```    
Or     
```*pointer += 1;```      
Or      
```*pointer++;```     

This should work right? Nope. The first two work, it's the third one that breaks the program.

You see, dereferencing, like arithmetic operations, has its order amongst these operations as well. The compiler decides which to do first. So in the last case, what the compiler does here is increment first, deterrence second. So it increments a local copy of the pointer instead of what the pointer is pointing to. 

To fix this simply force the compiler to do the dereferencing first:    
```(*pointer)++;```

That's it.
