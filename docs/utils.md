# Utils

## #.copy

Returns a copy of the current instance.

*Example*

```js
graph.addNodesFrom(['Thomas', 'Eric']);
graph.addEdgeWithKey('T->E', 'Thomas', 'Eric', {type: 'KNOWS'});

const newGraph = graph.copy();
newGraph.hasNode('Eric');
>>> true
newGraph.order
>>> 2
newGraph.size
>>> 1
graph.type === newGraph.type
>>> true
```

## #.nullCopy

Returns a null copy, i.e. a copy of the graph without nodes nor edges, of the current instance while retaining the type & the options of the graph.

*Example*

```js
graph.addNodesFrom(['Thomas', 'Eric']);
graph.addEdgeWithKey('T->E', 'Thomas', 'Eric', {type: 'KNOWS'});

const newGraph = graph.nullCopy();
newGraph.hasNode('Eric');
>>> false
newGraph.order
>>> 0
newGraph.size
>>> 0
graph.type === newGraph.type
>>> true
```

*Arguments*

* **options** <span class="code">[object]</span>: options to merge to create a slightly different graph.

## #.emptyCopy

Returns an empty copy, i.e. a copy of the graph containing only nodes, of the current instance while retaining the type & the options of the graph.

This is useful to functions needing to return subgraphs or near identical copies of a graph such as reversed graph or graph converted to another type altogether.

*Example*

```js
graph.addNodesFrom(['Thomas', 'Eric']);
graph.addEdgeWithKey('T->E', 'Thomas', 'Eric', {type: 'KNOWS'});

const newGraph = graph.emptyCopy();
newGraph.hasNode('Eric');
>>> true
newGraph.order
>>> 2
newGraph.size
>>> 0
newGraph.hasEdge('Thomas', 'Eric');
>>> false
graph.type === newGraph.type
>>> true
```

*Arguments*

* **options** <span class="code">[object]</span>: options to merge to create a slightly different graph.

## #.upgradeToMixed

Upgrade the graph to a mixed one.

*Example*

```js
const graph = new UndirectedGraph();
graph.addNodesFrom([1, 2]);

// Let's upgrade the graph
graph.upgradeToMixed();

// We can now add directed edges to this graph
graph.addDirectedEdge(1, 2);
```

## #.upgradeToMulti

Upgrade the graph to a multi one.

*Example*

```js
const graph = new Graph();
graph.addNodesFrom([1, 2]);
graph.addEdgeWithKey('A', 1, 2);

// This will throw
graph.addEdgeWithKey('B', 1, 2);

// Let's upgrade the graph
graph.upgradeToMulti();

// We can now add multiple edges to this graph
graph.addEdgeWithKey('B', 1, 2);
graph.edges(1, 2);
>>> ['A', 'B']
```

