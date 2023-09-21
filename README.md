# All 4-regular-planar graphs on 24, 25 and 27 vertices with all faces 3-faces or 4-faces
For detailed information on the number of 4-regular plane graphs on n(<=60) vertices with all-faces-3-faces-or-4-faces, see the link below. We use CaGe to generate them. See https://github.com/CaGe-graph/CaGe. Or we choose two files in CaGe named `quad_restrict.c` and    `plantri.c`  to compile. 

```
cc -o quad_restrict -O '-DPLUGIN="quad_restrict.c"' -DALLTOGETHER plantri.c
./quad_restrict -q -F3F4  26 >quad_24_F3F4.plc
./quad_restrict -q -F3F4  27 >quad_25_F3F4.plc
./quad_restrict -q -F3F4  29 >quad_27_F3F4.plc
```

# How to read it. 
1. add an option -g for graph6 format and using showg, for `example, ./quad_restrict -q -F3F4  27 -g`  but it will miss embedding information. 
2. use mathematica

```
decode[data_] := 
 Module[{d = data, head, vc, g},(*skip header if it exists*)
  head = ToCharacterCode[">>planar_code le<<"];
  If[Take[d, Min[Length[d], Length[head]]] === head, 
   d = Drop[d, Length[head]];];
  (*split data at zero separators*)d = Most /@ Split[d, #1 =!= 0 &];
  First@Last@
    Reap@While[Length[d] > 0, 
      vc = d[[1, 
        1]];(*vertex count*)(*get as many neighbour lists as the \
vertex count*){g, d} = TakeDrop[d, vc];
      (*drop vertex count from first neighbour list*)
      g = MapAt[Rest, g, {1}];
      (*build association for combinatorial embedding*)
      Sow@AssociationThread[Range[vc], 
        Reverse /@ g (*reverse from clockwise to counterclockwise*)]]]
```
This code should be due to Szabolcs Horvát. 

```
data = Import[
   "~\\quad_24_F3F4.plc", "Byte"];
```



https://oeis.org/search?q=4-regular+plane+graphs+&sort=&language=&go=Search

A111361		The number of 4-regular plane graphs with all faces 3-gons or 4-gons.

```
n		a(n)
2		0
3		0
4		0
5		0
6		1
7		0
8		1
9		1
10		2
11		1
12		5
13		2
14		8
15		5
16		12
17		8
18		25
19		13
20		30
21		23
22		51
23		33
24		76
25		51
26		109
27		78
28		144
29		106
30		218
31		150
32		274
33		212
34		382
35		279
36		499
37		366
38		650
39		493
40		815
41		623
42		1083
43		800
44		1305
45		1020
46		1653
47		1261
48		2045
49		1554
50		2505
51		1946
52		3008
53		2322
54		3713
55		2829
56		4354
57		3418
58		5233
59		4063
60		6234
```
