# Graphology

`graphology` is a specification and reference implementation for a robust & multipurpose JavaScript/TypeScript `Graph` object.

It aims at supporting various kinds of graphs with the same unified interface.

A `graphology` graph can therefore be directed, undirected or mixed and can be simple or support parallel edges.

Along with those specifications, one will also find a [standard library](#standard-library) full of graph theory algorithms and common utilities such as graph generators, layouts etc.

## Installation

To install the reference implementation:

```bash
npm install graphology
```

The source of the reference implementation can be found on [this](https://github.com/graphology/graphology) github repository.

Note that `graphology` also exposes type declaration so it can be comfortably used with [TypeScript](https://www.typescriptlang.org/).

You may need to install `graphology-types` along `graphology`, depending on your npm version (it is declared as a peer dependency to avoid common issues), so it works smoothly:

```bash
npm install graphology-types
```

## Quick Start

```js
const Graph = require('graphology');

const graph = new Graph();
graph.addNode('John');
graph.addNode('Martha');
graph.addEdge('John', 'Martha');

console.log('Number of nodes', graph.order);
console.log('Number of edges', graph.size);

graph.forEachNode(node => {
  graph.forEachNeighbor(node, neighbor => console.log(node, neighbor));
});
```

## Standard library

* [graphology-assertions](https://github.com/graphology/graphology-assertions#readme)<br>*Miscellaneous assertions (same nodes, same edges etc.).*
* [graphology-canvas](https://github.com/graphology/graphology-canvas#readme)<br>*Canvas rendering routines for graphs.*
* [graphology-communities-louvain](https://github.com/graphology/graphology-communities-louvain#readme)<br>*Louvain method for community detection.*
* [graphology-components](https://github.com/graphology/graphology-components#readme)<br>*Connected components (strong, weak etc.).*
* [graphology-generators](https://github.com/graphology/graphology-generators#readme)<br>*Graph generators (random graphs, complete graphs etc.).*
* [graphology-gexf](https://github.com/graphology/graphology-gexf#readme)<br>*Parsers & writers for the GEXF file format.*
* [graphology-graphml](https://github.com/graphology/graphology-graphml#readme)<br>*Parsers & writers for the GRAPHML file format.*
* [graphology-hits](https://github.com/graphology/graphology-hits#readme)<br>*HITS algorithm.*
* [graphology-layout](https://github.com/graphology/graphology-layout#readme)<br>*Basic graph layouts (random, circle etc.).*
* [graphology-layout-forceatlas2](https://github.com/graphology/graphology-layout-forceatlas2#readme)<br>*ForceAtlas2 layout algorithm.*
* [graphology-layout-noverlap](https://github.com/graphology/graphology-layout-noverlap#readme)<br>*Noverlap anti-collision layout algorithm.*
* [graphology-metrics](https://github.com/graphology/graphology-metrics#readme)<br>*Modularity, density, centrality etc.*
* [graphology-operators](https://github.com/graphology/graphology-operators#readme)<br>*Graph unary, binary & cast operators (reverse, union, intersection, conversion etc.)*
* [graphology-pagerank](https://github.com/graphology/graphology-pagerank#readme)<br>*Pagerank algorithm.*
* [graphology-simple-path](https://github.com/graphology/graphology-simple-path#readme)<br>*Simple path related functions (e.g. all paths between source & target)*
* [graphology-shortest-path](https://github.com/graphology/graphology-shortest-path#readme)<br>*Shortest path functions (Dijkstra, A&ast; etc.)*
* [graphology-svg](https://github.com/graphology/graphology-svg#readme)<br>*SVG export for graphs.*
* [graphology-types](https://github.com/graphology/graphology-types#readme)<br>*TypeScript declaration files.*
* [graphology-traversal](https://github.com/graphology/graphology-traversal#readme)<br>*Traversal functions (DFS, BFS, etc.)*
* [graphology-utils](https://github.com/graphology/graphology-utils#readme)<br>*Miscellaneous utils used by most of the other modules.*

## Changelog

A complete record describing changes from version to version can be found [here](https://github.com/graphology/graphology/blob/master/CHANGELOG.md) if required.

## Implementing graphology

`graphology` is only is a specification so that anyone can implement it however they like if necessary, while keeping the advantages of being able to use the [standard library](#standard-library).

Graphs are complex structures and, while we designed the reference implementation to handle most common cases with good performance, one will always be able to implement the present specifications in a more performant fashion for specific use cases.

It is therefore possible to test your custom implementation against the specifications' unit tests.

Directions about this can be found [here](unittests.md).
