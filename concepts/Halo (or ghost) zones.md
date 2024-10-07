During the [[Stencil]] algorithm each [[CUDA thread block]] is assigned a one 3D block of points. As the computed point need to access the data of its neighbours the halo (or ghost) region is created i.e. the area of values around the core which are needed in order to compute the next iteration.

```

.............
.+---------+.
.|         |.
.|         |.
.+---------+.
.............
 
-- dots are the halo
```
