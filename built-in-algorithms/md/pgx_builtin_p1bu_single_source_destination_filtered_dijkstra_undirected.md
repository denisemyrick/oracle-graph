# Filtered Dijkstra Algorithm (ignoring edge directions)

- **Category:** path finding
- **Algorithm ID:** pgx_builtin_p1bu_single_source_single_destination_filtered_dijkstra_undirected
- **Time Complexity:** O(E + V log V) with V = number of vertices, E = number of edges
- **Space Requirement:** O(4 * V) with V = number of vertices
- **Javadoc:**
  - [Analyst#shortestPathFilteredDijkstra(PgxGraph graph, PgxVertex<ID> src, PgxVertex<ID> dst, EdgeProperty<java.lang.Double> cost, GraphFilter filterExpr, boolean ignoreEdgeDirection)](https://docs.oracle.com/en/database/oracle/property-graph/24.3/spgjv/oracle/pgx/api/Analyst.html#shortestPathFilteredDijkstra_oracle_pgx_api_PgxGraph_oracle_pgx_api_PgxVertex_oracle_pgx_api_PgxVertex_oracle_pgx_api_EdgeProperty_oracle_pgx_api_filter_GraphFilter_boolean_)
  - [Analyst#shortestPathFilteredDijkstra(PgxGraph graph, PgxVertex<ID> src, PgxVertex<ID> dst, EdgeProperty<java.lang.Double> cost, GraphFilter filterExpr, VertexProperty<ID,​PgxVertex<ID>> parent, VertexProperty<ID,​PgxEdge> parentEdge, boolean ignoreEdgeDirection)](https://docs.oracle.com/en/database/oracle/property-graph/24.3/spgjv/oracle/pgx/api/Analyst.html#shortestPathFilteredDijkstra_oracle_pgx_api_PgxGraph_oracle_pgx_api_PgxVertex_oracle_pgx_api_PgxVertex_oracle_pgx_api_EdgeProperty_oracle_pgx_api_filter_GraphFilter_oracle_pgx_api_VertexProperty_oracle_pgx_api_VertexProperty_boolean_)

This variant of the Dijkstra's algorithm tries to find the shortest path ignoring edges directions for directed graphs while also taking into account a filter expression, which will add restrictions over the potential edges when looking for the shortest path between the source and destination vertices.

## Signature

| Input Argument | Type | Comment |
| :--- | :--- | :--- |
| `G` | graph | the graph. |
| `weight` | edgeProp<double> | edge property holding the (positive) weight of each edge in the graph. |
| `root` | node | the source vertex from the graph for the path. |
| `dest` | node | the destination vertex from the graph for the path. |
| `filter` | edgeFilter | filter expression with conditions to be satisfied by the shortest path. If the expression is targeted to edges, it will be evaluated straight away. If the expression targets vertices, then it will be automatically translated into an equivalent edge expression by using the sources and/or the destinations of the edges from the current evaluated vertex, with exception of the source and destination vertices. |

| Output Argument | Type | Comment |
| :--- | :--- | :--- |
| `parent` | vertexProp<node> | vertex property holding the parent vertex of the each vertex in the shortest path. |
| `parent_edge` | vertexProp<edge> | vertex property holding the edge ID linking the current vertex in the path with the previous vertex in the path. |

| Return Value | Type | Comment |
| :--- | :--- | :--- |
| | bool | true if there is a path connecting source and destination vertices, false otherwise |

## Code

```java
/*
 * Copyright (C) 2013 - 2024 Oracle and/or its affiliates. All rights reserved.
 */
package oracle.pgx.algorithms;

import oracle.pgx.algorithm.EdgeProperty;
import oracle.pgx.algorithm.PgxEdge;
import oracle.pgx.algorithm.PgxGraph;
import oracle.pgx.algorithm.PgxMap;
import oracle.pgx.algorithm.PgxVertex;
import oracle.pgx.algorithm.VertexProperty;
import oracle.pgx.algorithm.annotations.GraphAlgorithm;
import oracle.pgx.algorithm.annotations.Out;
import oracle.pgx.algorithm.filter.EdgeFilter;

@GraphAlgorithm
public class DijkstraFiltredUndirected {
  public boolean dijkstraFiltredUndirected(PgxGraph g, EdgeProperty<Double> weight, PgxVertex root, PgxVertex dest,
      EdgeFilter filter, @Out VertexProperty<PgxVertex> parent, @Out VertexProperty<PgxEdge> parentEdge) {
    if (g.getNumVertices() == 0) {
      return false;
    }

    VertexProperty<Boolean> reached = VertexProperty.create();

    // sequentially initialize, otherwise compiler flags this algorithm as
    //parallel in nature
    g.getVertices().forSequential(n -> {
      parent.set(n, PgxVertex.NONE);
      parentEdge.set(n, PgxEdge.NONE);
      reached.set(n, false);
    });

    //-------------------------------
    // look up the vertex
    //-------------------------------
    PgxMap<PgxVertex, Double> reachable = PgxMap.create();
    reachable.set(root, 0d);

    //-------------------------------
    // look up the vertex
    //-------------------------------
    boolean found = false;

    while (!found && reachable.size() > 0) {
      PgxVertex next = reachable.getKeyForMinValue();
      if (next == dest) {
        found = true;
      } else {
        reached.set(next, true);
        double dist = reachable.get(next);
        reachable.remove(next);
        next.getNeighbors().filter(v -> !reached.get(v) && filter.evaluate(v.edge())).forSequential(v -> {
          PgxEdge e = v.edge();
          if (!reachable.containsKey(v) || reachable.get(v) > dist + weight.get(e)) {
            reachable.set(v, dist + weight.get(e));
            parent.set(v, next);
            parentEdge.set(v, e);
          }
        });
      }
    }

    // return false if not reachable
    return found;
  }
}
```
