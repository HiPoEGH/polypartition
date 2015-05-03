PolyPartition is a lightweight C++ library for polygon partition and triangulation. PolyPartition implements multiple algorithms for both convex partitioning and triangulation. Different algorithms produce different quality of results (and their complexity varies accordingly). The implemented methods/algorithms with their advantages and disadvantages are outlined in the table below.

For input parameters and return values see method declarations in `polypartition.h`. All methods require that the input polygons are not self-intersecting, and are defined in the correct vertex order (conter-clockwise for non-holes, clockwise for holes). Polygon vertices can easily be ordered correctly by calling `TPPLPoly::SetOrientation` method.

| **Algorithm** | **Method** | **Time/Space complexity** | **Supports holes** | **Quality of solution** | **ExampleImage** |
|:--------------|:-----------|:--------------------------|:-------------------|:------------------------|:-----------------|
| Triangulation by ear clipping | `TPPLPartition::Triangulate_EC` | `O(n^2)/O(n)` | Yes, by calling `TPPLPartition::RemoveHoles` | Satisfactory in most cases | ![http://polypartition.googlecode.com/svn/trunk/images/tri_ec.png](https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/tri_ec.png) |
| Optimal triangulation in terms of edge length using dynamic programming algorithm | `TPPLPartition::Triangulate_OPT` | `O(n^3)/O(n^2)` | No. You could call `TPPLPartition::RemoveHoles` prior to calling `TPPLPartition::Triangulate_OPT`, but the solution would no longer be optimal, thus defeating the purpose | Optimal in terms of minimal edge length | ![https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/tri_opt.png](https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/tri_opt.png) |
| Triangulation by partition into monotone polygons | `TPPLPartition::Triangulate_MONO` | `O(n*log(n))/O(n)` | Yes, by design | Poor. Many thin triangles are created in most cases | ![https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/tri_mono.png](https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/tri_mono.png) |
| Convex partition using Hertel-Mehlhorn algorithm | `TPPLPartition::ConvexPartition_HM` | `O(n^2)/O(n)` | Yes, by calling `TPPLPartition::RemoveHoles` | At most four times the minimum number of convex polygons is created. However, in practice it works much better than that and often gives optimal partition | ![https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/conv_hm.png](https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/conv_hm.png) |
| Optimal convex partition using dynamic programming algorithm by Keil and Snoeyink | `TPPLPartition::ConvexPartition_OPT` | `O(n^3)/O(n^3)` | No. You could call `TPPLPartition::RemoveHoles` prior to calling `TPPLPartition::Triangulate_OPT`, but the solution would no longer be optimal, thus defeating the purpose | Optimal. A minimum number of convex polygons is produced | ![https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/conv_opt.png](https://raw.githubusercontent.com/ivanfratric/polypartition/master/images/conv_opt.png) |

