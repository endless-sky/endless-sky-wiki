![](https://endless-sky.github.io/screenshots/blur.jpg)

Motion blur in a movie is a result of objects moving over the course of a single image exposure. An easy way to simulate motion blur is to apply a linear blur to each sprite, where the direction and length of the blur depend on the velocity of the sprite relative to the camera. This can be implemented by "stretching out" the bounding box of the sprite. Then, each pixel that is drawn within that box is sampled, not from a single texture pixel, but from a series of pixels in a line. The direction of that line is determined by projecting the velocity vector onto the texture coordinate space.

![](https://endless-sky.github.io/images/blur0.png)

Stretching out the texture rectangle results in eight vertices, instead of four, but two of those vertices are always on the inside of the region. So, the stretched-out shape will generally be a hexagon, with opposite sides parallel. Once the blur vector has been projected onto the texture coordinate space, the texture coordinates of the six vertices are:

![](https://endless-sky.github.io/images/blur1.png)

Of course, that diagram only applies to this example case, where blur.y is negative and blur.x is positive. For example, if the X and Y components were both positive, a different set of six vertices would need to be used:

![](https://endless-sky.github.io/images/blur2.png)

To draw a hexagon in OpenGL, it must be split into triangles. The most intuitive way to do that is to connect all vertices to the center point, which uses 6 triangles. The triangle count can be reduced to 4 by connecting the vertices differently, but in that case again there is complicated logic involved in deciding which of the vertices should be connected to which:

![](https://endless-sky.github.io/images/blur3.png)

Although the motion blur shown here is large, usually the blur will be relatively small compared to the size of the sprite. In other words, the number of extra pixels we have to draw to cover the whole blur area, will usually be a small fraction of the total. That suggests a simple optimization: extending four of the hexagon edges outward to create a rectangular region to be drawn, rather than a hexagon. All of the pixels in the extended regions will be completely transparent, but as long as they are a small fraction of the total area the performance penalty will be small:

![](https://endless-sky.github.io/images/blur4.png)

But, the real advantage of extending the hexagon out to a rectangle is that the texture coordinates of the four corners of the rectangle will be the same regardless of the sign of the X and Y components of the blur vector. That is, we can eliminate the need for four complex conditional cases that depend on whether the signs of the X and Y components are positive or negative:

![](https://endless-sky.github.io/images/blur5.png)

In the game, each sprite that is drawn is represented by an (X, Y) center point, plus a 2x2 transformation matrix mapping texture coordinate offsets from the center into screen coordinates. The red coordinates above give the offset in texture coordinates of each of the four corners relative to the center. Note that all four are exactly the same vector, just flipped in X or Y. So, the vertex shader for the four corners can calculate the screen coordinates quite easily, given a "vert" that is (&plusmn;.5, &plusmn;.5) depending on what corner it represents:

```c
vec2 blurOff = 2 * vec2(vert.x * abs(blur.x), vert.y * abs(blur.y));
gl_Position = vec4((transform * (vert + blurOff) + position) * scale, 0, 1);
```

For each pixel, the shader must sample several points in the texture, spread out on a line. In order to have the edges of the blur fade off, the "center" pixel should be assigned a higher weight:

```c
const int range = 5;
const float divisor = range * (range + 2) + 1;
vec4 color = vec4(0., 0., 0., 0.);
for(int i = -range; i <= range; ++i)
{
    float scale = (range + 1 - abs(i)) / divisor;
    vec2 coord = fragTexCoord + (blur * i) / range;
    color += scale * texture(tex0, coord);
}
```

That is, the weights are: [1 2 3 4 5 6 5 4 3 2 1], a triangular blur kernel. The sum of all those weights is:

> range * (range + 1) / 2 + (range + 1) * (range + 2) / 2 = (range + 1)^2

In practice the blur shader is slightly more complicated because it also handles [tweening](AnimationTweening) between two animation frames. Also, there is a special case in the shader for sprites with zero blur, to avoid having to sample 11 different texture points for them. This allows the same shader to still be used to draw large sprites like the UI panels. Because the blur is a uniform variable, every fragment of a given sprite will take the same path through that conditional, so it does not introduce any appreciable performance penalty compared to calling a simplified shader in those cases.