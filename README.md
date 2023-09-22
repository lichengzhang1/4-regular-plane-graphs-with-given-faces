# All 4-regular-planar graphs with given faces
For detailed information on the number of 4-regular plane graphs on n(<=60) vertices with all-faces-3-faces-or-4-faces, see 
[A111361](https://oeis.org/A111361).

```
A111361		The number of 4-regular plane graphs with all faces 3-gons or 4-gons.
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
```

# How to generate them

We use CaGe to generate them. See https://github.com/CaGe-graph/CaGe. If we don't want to use a Java-based frontend,  we can choose two files in CaGe named `quad_restrict.c` and   `plantri.c` to compile. 

```
cc -o quad_restrict -O '-DPLUGIN="quad_restrict.c"' -DALLTOGETHER plantri.c
```

then for example, use

```
./quad_restrict -q -F3F4  26 >quad_24_F3F4.plc
```
to  generate 24-vertex (with 26 faces)  4-regular plane graphs with with all faces 3-facesor 4-faces.

Similary,


```
./quad_restrict -q -F3F4  27 >quad_25_F3F4.plc
./quad_restrict -q -F3F4  29 >quad_27_F3F4.plc
```


# How to read it.

1. add an option -g for graph6 format and using showg, for `example, ./quad_restrict -q -F3F4  27 -g`  but it will miss embedding information. 
2. use *Mathematica* (This code should be due to Szabolcs Horvát）

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


```
data = Import[
   "~\\quad_24_F3F4.plc", "Byte"];
```




# Further...
If there exist two triangle faces in a given graph that share a common vertex, return `False`; otherwise, return `True`.

```
Findcomm[g_] :=
 Module[{AllF3}, AllF3 := Select[PlanarFaceList[g], Length[#] == 3 &];
   If[Length[
     Cases[AllF3, {___,
       SelectFirst[Tally[Join @@ AllF3], #[[2]] >= 2 &][[1]], ___}]] ==
     2, False, True]]
```


For example,

```
G1=Graph[{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24}, {Null, SparseArray[Automatic, {24, 24}, 0, {1, {{0, 4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64, 68, 72, 76, 80, 84, 88, 92, 96}, {{2}, {3}, {4}, {5}, {1}, {5}, {6}, {7}, {1}, {7}, {8}, {9}, {1}, {9}, {10}, {11}, {1}, {2}, {11}, {12}, {2}, {12}, {13}, {14}, {2}, {3}, {8}, {14}, {3}, {7}, {15}, {16}, {3}, {4}, {16}, {17}, {4}, {11}, {17}, {18}, {4}, {5}, {10}, {19}, {5}, {6}, {13}, {19}, {6}, {12}, {20}, {21}, {6}, {7}, {15}, {21}, {8}, {14}, {21}, {22}, {8}, {9}, {17}, {22}, {9}, {10}, {16}, {23}, {10}, {19}, {20}, {23}, {11}, {12}, {18}, {20}, {13}, {18}, {19}, {24}, {13}, {14}, {15}, {24}, {15}, {16}, {23}, {24}, {17}, {18}, {22}, {24}, {20}, {21}, {22}, {23}}}, Pattern}]}, {FormatType -> TraditionalForm, GraphLayout -> {"Dimension" -> 2}, ImageSize -> {100, 100}, PlotRange -> All, PlotTheme -> "Monochrome", VertexCoordinates -> {{0.7071067811865475, -0.7071067811865475}, {0.3743506488634663, -0.23570226039551573}, {0.7071067811865475, 0.7071067811865475}, {-0.7071067811865475, -0.7071067811865475}, {0.23570226039551578, -0.3743506488634662}, {0.18024290500833565, -0.09705387192756525}, {0.37435064886346636, 0.23570226039551584}, {0.23570226039551584, 0.3743506488634663}, {-0.7071067811865475, 0.7071067811865475}, {-0.37435064886346625, -0.23570226039551576}, {-0.23570226039551578, -0.37435064886346625}, {0.09705387192756534, -0.18024290500833556}, {0.06932419423397529, -0.06932419423397518}, {0.18024290500833567, 0.09705387192756536}, {0.09705387192756541, 0.18024290500833562}, {-0.2357022603955157, 0.37435064886346625}, {-0.37435064886346625, 0.23570226039551578}, {-0.18024290500833556, -0.0970538719275653}, {-0.0970538719275653, -0.18024290500833556}, {-0.06932419423397519, -0.06932419423397518}, {0.0693241942339753, 0.06932419423397525}, {-0.09705387192756523, 0.18024290500833556}, {-0.18024290500833554, 0.09705387192756532}, {-0.06932419423397516, 0.06932419423397523}}, VertexSize -> {{0.01, 0.01}}, VertexStyle -> {GrayLevel[0.6]}}]
```
![image](https://github.com/lichengzhang1/4-regular-plane-graphs-with-given-faces/assets/82444903/71b3108f-4bde-4fa1-8327-bce5e67854d2)

```
Findcomm[G1]

True.
```

```
G2=Graph[{1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24}, {Null, SparseArray[Automatic, {24, 24}, 0, {1, {{0, 4, 8, 12, 16, 20, 24, 28, 32, 36, 40, 44, 48, 52, 56, 60, 64, 68, 72, 76, 80, 84, 88, 92, 96}, {{2}, {3}, {4}, {5}, {1}, {5}, {6}, {7}, {1}, {4}, {7}, {8}, {1}, {3}, {9}, {10}, {1}, {2}, {10}, {11}, {2}, {11}, {12}, {13}, {2}, {3}, {13}, {14}, {3}, {9}, {14}, {15}, {4}, {8}, {10}, {16}, {4}, {5}, {9}, {17}, {5}, {6}, {17}, {18}, {6}, {18}, {19}, {20}, {6}, {7}, {20}, {21}, {7}, {8}, {21}, {22}, {8}, {16}, {22}, {23}, {9}, {15}, {17}, {19}, {10}, {11}, {16}, {18}, {11}, {12}, {17}, {19}, {12}, {16}, {18}, {23}, {12}, {13}, {23}, {24}, {13}, {14}, {22}, {24}, {14}, {15}, {21}, {24}, {15}, {19}, {20}, {24}, {20}, {21}, {22}, {23}}}, Pattern}]}, {FormatType -> TraditionalForm, GraphLayout -> {"Dimension" -> 2}, ImageSize -> {100, 100}, PlotRange -> All, PlotTheme -> "Monochrome", VertexCoordinates -> {{0.40394241242329176, -0.25945599573690153}, {0.7071067811865475, -0.7071067811865475}, {0.37907330901109937, 0.1313241832557828}, {0.2620631824699983, -0.0838523166788216}, {0.2675263770255219, -0.3781890683380199}, {-0.7071067811865475, -0.7071067811865475}, {0.7071067811865475, 0.7071067811865475}, {0.1431808599645599, 0.16149826425230693}, {0.1221576317492572, -0.03377681913464458}, {0.143079376696345, -0.17350063509952313}, {-0.18402306220409667, -0.37269286132910723}, {-0.3833586543598687, -0.187026754982597}, {-0.7071067811865475, 0.7071067811865475}, {0.13147477516274866, 0.4022338079342901}, {-0.05998227606486567, 0.14621188495379928}, {-0.05969289213387439, -0.03925258901254048}, {-0.07942968445939742, -0.19818433624660647}, {-0.21708216019596363, -0.2072912595452552}, {-0.2215172397604917, -0.07126108562271007}, {-0.38772843629647186, 0.2375521064241246}, {-0.22690672076222454, 0.42625458277242784}, {-0.09748181973788815, 0.3140756035258782}, {-0.22593525235226009, 0.1485262610495524}, {-0.2345130572872111, 0.28160213844299575}}, VertexSize -> {{0.01, 0.01}}, VertexStyle -> {GrayLevel[0.6]}}]
```

![image](https://github.com/lichengzhang1/4-regular-plane-graphs-with-given-faces/assets/82444903/800cfb34-ea06-4792-bab2-14fcba70c55c)

```
Findcomm[G2]
Flase
```









