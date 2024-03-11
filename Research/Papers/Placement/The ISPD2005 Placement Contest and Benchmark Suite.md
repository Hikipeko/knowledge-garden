### 2 ISPD 2005 Placement Contest

* Use GSRC Bookshelf placement format
* http://vlsicad.eecs.umich.edu/BK/ISPD06bench/BookshelfFormat.txt

##### File Descriptions

```
Assuming the benchmark name is zzz,
(1) zzz.aux : denotes the set of input files. Usually looks like
        RowBasedPlacement : zzz.nodes zzz.nets zzz.wts zzz.pl zzz.scl

(2) zzz.nodes: specifies the set of objects. For each object,

        object_name        width         height        terminal

     # keyword "terminal" appears only for fixed objects.

(3) zzz.nets: specifies the set of nets. For each net,

        NetDegree : k net_name
     # input pin  pin offset is measured from the center of corresponding object
     #            if pin offsets are unavailable, assumed to be (0,0)
                object_name        I : x_offset        y_offset          
     # output pin
                object_name        O : x_offset        y_offset          
       
(4) zzz.pl: specifies the location & orientation of objects. For each object,

     # x_location & y_location are BOTTOM LEFT coordinates of an object
     # orientation is one of N, S, E, W, FN, FS, FE, FW
     # and interpreted as in LEF/DEF (see Orientation.ppt in this
     # documentation for visual representations of each orientation)
        object_name        x_location        y_location        : orientation
     # fixed object
        object_name        x_location        y_location        : orientation    /FIXED

(5) zzz.scl: circuit row information. For each row,
        CoreRow Horizontal
           Coordinate :    www  # row starts at coordinate www
                                # (y-coordinate if horizontal, x-coordinate if vertical row)
           Height :        yyy  # row height is yyy
           Sitewidth :     1    
           Sitespacing :   1    # distance between the left edges of two adjacent sites
           Siteorient :    1
           Sitesymmetry :  1
       # You can specify subrows within a row. They following says the subrow spans from
       # aaa to (aaa + (bbb-1)*Sitespacing + Sitewidth) with yyy height.
           SubrowOrigin :  aaa  NumSites : bbb
       # Additional Subrows may be specified if there are breaks in the row
       # due to obstacles, but are not required. One subrow is always required.

(6) zzz.wts: specifies weights for objects and nets.

        object_name        weight
```

* Most of nodes' width are equal to the row height.

##### Placement

https://lume.ufrgs.br/bitstream/handle/10183/197078/001097380.pdf?sequence=1

![[Pasted image 20231108163127.png|400]]
![[Pasted image 20231108163147.png|500]]