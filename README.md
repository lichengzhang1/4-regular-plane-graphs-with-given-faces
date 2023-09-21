# All 4-regular-planar graphs on 24, 25 and 27 vertices with all faces 3-faces or 4-faces
For detailed information on the number of 4-regular plane graphs on n(<=60) vertices with all-faces-3-faces-or-4-faces, see the link below. We use CaGe to generate them. See https://github.com/CaGe-graph/CaGe. Or we choose two files in CaGe named `quad_restrict.c` and    `plantri.c`  to compile. 

```
cc -o quad_restrict -O '-DPLUGIN="quad_restrict.c"' -DALLTOGETHER plantri.c
./quad_restrict -q -F3F4 -d 26 
./quad_restrict -q -F3F4 -d 27
./quad_restrict -q -F3F4 -d 29
```



https://oeis.org/search?q=4-regular+plane+graphs+&sort=&language=&go=Search

A111361		The number of 4-regular plane graphs with all faces 3-gons or 4-gons.
