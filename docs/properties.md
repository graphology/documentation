# Properties

All the following properties are read-only and may not be changed by the user.

## #.order

Number of nodes in the graph.

*Example*

```js
const graph = new Graph();

graph.addNode('John');
graph.addNode('Jack');

graph.order;
>>> 2
```

## #.size

Number of edges in the graph.

*Example*

```js
const graph = new Graph();

graph.addNode('John');
graph.addNode('Jack');

graph.addEdge('John', 'Jack');

graph.size;
>>> 1
```

## #.directedSize

Number of directed edges in the graph.

*Example*

```js
const graph = new Graph();

graph.addNode('John');
graph.addNode('Jack');

graph.addDirectedEdge('John', 'Jack');

graph.directedSize;
>>> 1
```

## #.undirectedSize

Number of undirected edges in the graph.

*Example*

```js
const graph = new Graph();

graph.addNode('John');
graph.addNode('Jack');

graph.addUndirectedEdge('John', 'Jack');

graph.undirectedSize;
>>> 1
```

## #.type

Type of the graph. One of `mixed`, `directed` or `undirected`.

*Example*

```js
const graph = new Graph();
graph.type;
>>> 'mixed'

const directedGraph = new DirectedGraph();
directedGraph.type;
>>> 'directed'
```

## #.multi

Whether the graph accepts parallel edges or not.

*Example*

```js
const graph = new Graph();
graph.multi;
>>> false

const multiGraph = new MultiGraph();
multiGraph.multi;
>>> true
```

## #.allowSelfLoops

Whether the graph accepts self loops or not.

```js
const graph = new Graph();
graph.allowSelfLoops;
>>> true

const otherGraph = new Graph({allowSelfLoops: false});
graph.allowSelfLoops;
>>> false
```

## #.implementation

A string containing the graph instance's implementation name.

```js
import Graph from 'graphology';

const graph = new Graph();
graph.implementation;
>>> 'graphology'
```

It can be useful if you ever need to optimize libraries based upon the knowledge that the given instance is from a specific implementation.
