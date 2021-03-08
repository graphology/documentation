# Mutation

## #.addNode

Adds a single node to the graph with optional attributes and returns the node.

Will throw if the node already exists in the graph.

*Example*

```js
// Adding a simple node:
const node = graph.addNode('John');

// Adding a node with attributes:
const node = graph.addNode('John', {
  age: 24,
  eyes: 'blue'
});
```

*Arguments*

* **node** <span class="code">any</span>: the key referencing the node.
* **attributes** <span class="code">[object]</span>: optional attributes.

## #.mergeNode

Adds a node only if the node does not exist in the graph yet. Else it will merge the provided attributes with the already existing ones.

*Example*

```js
// Since 'John' does not exist in the graph, a node will be added
graph.mergeNode('John');

// Since 'John' already exists in the graph, this will do nothing
graph.mergeNode('John');

// Note that if the node already exists, attributes are merged
graph.mergeNode('John', {eyes: 'blue'});
graph.getNodeAttributes('John');
>>> {
  eyes: 'blue'
}
```

*Arguments*

* **node** <span class="code">any</span>: the node's key.
* **attributes** <span class="code">object</span>: the node's initial attributes or attributes to merge with the already existing ones.


## #.updateNode

Adds a node only if the node does not exist in the graph yet. Else it will update the already existing node's attributes using the provided function.

If the node does not yet exist, the updated function must still return the initial attributes and will be given an empty object to do so.

*Example*

```js
// Since 'John' does not exist in the graph, a node will be added
graph.updateNode('John', attr => {
  return {
    ...attr,
    count: (attr.count || 0) + 1
  };
});

// Since 'John' already exist in the graph, this time its count will get incremented
graph.updateNode('John', attr => {
  return {
    ...attr,
    count: (attr.count || 0) + 1
  };
});

graph.getNodeAttribute('John', 'count');
>>> 2
```

*Arguments*

* **node** <span class="code">any</span>: the node's key.
* **updater** <span class="code">[function]</span>: a function tasked to update target node's attributes.

## #.addEdge

Adds a single directed edge if the graph is `directed` or `mixed`, or an undirected edge if the graph is `undirected` and returns the edge.

*Example*

```js
graph.addNode('John');
graph.addNode('Jack');

// Adding a simple edge between John & Jack:
const edge = graph.addEdge('John', 'Jack');

// Adding an edge with attributes between John & Jack;
const edge = graph.addEdge('John', 'Jack', {
  type: 'KNOWS',
  weight: 0
});
```

*Arguments*

* **source** <span class="code">any</span>: the source node.
* **target** <span class="code">any</span>: the target node.
* **attributes** <span class="code">[object]</span>: optional attributes.

*Important*

This method is a convenience built on top of the [`#.addEdgeWithKey`](#addedgewithkey) method so that the user may add an edge in the graph without having to create a specific key for it.

Note that internally, because this key is still needed, the graph will generate one for you using a unique identifier. You remain free to customize the way those keys are generated through the [`edgeKeyGenerator`](./instantiation.md#arguments) option.

*Variants*

* `#.addDirectedEdge`
* `#.addUndirectedEdge`

## #.addEdgeWithKey

Adds a single directed edge if the graph is `directed` or `mixed`, or an undirected edge if the graph is `undirected`, using the provided key, and returns the edge.

This is quite useful when dealing with a `MultiGraph` if you need to retrieve precise edges since polling the graph using both the source & the target node might return several edges rather than only one.

*Example*

```js
graph.addNode('John');
graph.addNode('Jack');

// Adding a simple edge between John & Jack:
const edge = graph.addEdgeWithKey('John->Jack', 'John', 'Jack');

// Adding an edge with attributes between John & Jack;
const edge = graph.addEdgeWithKey('John->Jack', 'John', 'Jack', {
  type: 'KNOWS',
  weight: 0
});
```

*Arguments*

* **edge** <span class="code">any</span>: the key referencing the edge.
* **source** <span class="code">any</span>: the source node.
* **target** <span class="code">any</span>: the target node.
* **attributes** <span class="code">[object]</span>: optional attributes.

*Variants*

* `#.addDirectedEdgeWithKey`
* `#.addUndirectedEdgeWithKey`

## #.mergeEdge

Adds an edge with the given key only if the edge does not exist in the graph yet. Else it will merge the provided attributes with the already existing ones.

Furthermore, this method will also add the source and/or target nodes to the graph if not found.

Note that an edge is deemed to already exist in a simple graph if the graph can find one edge of same type going from the same source to the same target.

In a multi graph, this method will therefore always add a new edge.

```js
const graph = new UndirectedGraph();

// Since the edge does not exist, it will be added
graph.mergeEdge('John', 'Martha');

// Now, since the edge already exists, this will do nothing
graph.mergeEdge('John', 'Martha');

// Note that if the edge already exists, attributes are merged
graph.mergeEdge('John', 'Martha', {type: 'KNOWS'});
graph.getEdgeAttributes('John', 'Martha');
>>> {
  type: 'KNOWS'
}
```

*Arguments*

* **source** <span class="code">any</span>: the source node.
* **target** <span class="code">any</span>: the target node.
* **attributes** <span class="code">[object]</span>: optional attributes to merge.

*Variants*

* `#.mergeDirectedEdge`
* `#.mergeUndirectedEdge`

## #.mergeEdgeWithKey

Adds an edge with the given key only if the edge does not exist in the graph yet. Else it will merge the provided attributes with the already existing ones.

Furthermore, this method will also add the source and/or target nodes to the graph if not found.

Note that in this case, an edge is deemed to already exist in the graph if an edge with the same key, same type and same source & target is found in the graph.

If one tries to add an edge with the given key and if the graph has an edge with the same key but a different source & target, the method will throw to notify of the inconsistency.

```js
const graph = new UndirectedGraph();
graph.addNodesFrom(['John', 'Martha', 'Thomas']);

// Since the edge does not exist, it will be added
graph.mergeEdgeWithKey('J->M', 'John', 'Martha');

// Now, since the edge already exists, this will do nothing
graph.mergeEdgeWithKey('J->M', 'John', 'Martha');

// Note that if the edge already exists, attributes are merged
graph.mergeEdgeWithKey('J->M', 'John', 'Martha', {type: 'KNOWS'});
graph.getEdgeAttributes('J->M');
>>> {
  type: 'KNOWS'
}

// However, the following will throw an error
graph.mergeEdgeWithKey('J->M', 'Thomas', 'Martha');
```

*Arguments*

* **edge** <span class="code">any</span>: the edge's key.
* **source** <span class="code">any</span>: the source node.
* **target** <span class="code">any</span>: the target node.
* **attributes** <span class="code">[object]</span>: optional attributes to merge.

*Variants*

* `#.mergeDirectedEdgeWithKey`
* `#.mergeUndirectedEdgeWithKey`

## #.updateEdge

Adds an edge with the given key only if the edge does not exist in the graph yet. Else it will update the already existing edge's attributes using the provided function.

If the edge does not yet exist, the updated function must still return the initial attributes and will be given an empty object to do so.

Furthermore, this method will also add the source and/or target nodes to the graph if not found.

Note that an edge is deemed to already exist in a simple graph if the graph can find one edge of same type going from the same source to the same target.

In a multi graph, this method will therefore always add a new edge.

```js
const graph = new UndirectedGraph();

// Since the edge does not exist, it will be added
graph.updateEdgeWithKey('John', 'Martha', attr => {
  return {
    ...attr,
    weight: (attr.weight || 0) + 1
  };
});

// Since the edge already exist, its weight will get incremented this time
graph.updateEdge('John', 'Martha', attr => {
  return {
    ...attr,
    weight: (attr.weight || 0) + 1
  };
});

graph.getEdgeAttribute('John', 'Martha', 'weight');
>>> 2
```

*Arguments*

* **edge**
* **source** <span class="code">any</span>: the source node.
* **target** <span class="code">any</span>: the target node.
* **updater** <span class="code">[function]</span>: optional function tasked to update the edge's attributes.

*Variants*

* `#.updateDirectedEdge`
* `#.updateUndirectedEdge`

## #.updateEdgeWithKey

Adds an edge with the given key only if the edge does not exist in the graph yet. Else it will update the already existing edge's attributes using the provided function.

If the edge does not yet exist, the updated function must still return the initial attributes and will be given an empty object to do so.

Furthermore, this method will also add the source and/or target nodes to the graph if not found.

Note that in this case, an edge is deemed to already exist in the graph if an edge with the same key, same type and same source & target is found in the graph.

If one tries to add an edge with the given key and if the graph has an edge with the same key but a different source & target, the method will throw to notify of the inconsistency.

```js
const graph = new UndirectedGraph();

// Since the edge does not exist, it will be added
graph.updateEdgeWithKey('J->M', 'John', 'Martha', attr => {
  return {
    ...attr,
    weight: (attr.weight || 0) + 1
  };
});

// Since the edge already exist, its weight will get incremented this time
graph.updateEdgeWithKey('J->M', 'John', 'Martha', attr => {
  return {
    ...attr,
    weight: (attr.weight || 0) + 1
  };
});

graph.getEdgeAttribute('J->M', 'weight');
>>> 2
```

*Arguments*

* **edge** <span class="code">any</span>: the edge's key.
* **source** <span class="code">any</span>: the source node.
* **target** <span class="code">any</span>: the target node.
* **updater** <span class="code">[function]</span>: optional function tasked to update the edge's attributes.

*Variants*

* `#.updateDirectedEdgeWithKey`
* `#.updateUndirectedEdgeWithKey`

## #.dropNode

Drops a single node & all its attached edges from the graph.

*Example*

```js
graph.addNode('John');
graph.dropNode('John');

graph.dropNode('Martha');
>>> Error "Martha not in the graph"
```

*Arguments*

* **node** <span class="code">any</span>: the node to drop.

## #.dropEdge

Drops a single edge from the graph.

*Example*

```js
graph.addNode('John');
graph.addNode('Martha');

const edge = graph.addEdge('John', 'Martha');

// Dropping the edge using its key:
graph.dropEdge(edge);

// Dropping the first matching edge between John & Martha
graph.dropEdge('John', 'Martha');
```

*Arguments*

1. Using the key:
  * **edge** <span class="code">any</span>: the edge to drop.
2. Using the source & target:
  * **source** <span class="code">any</span>: source node of the edge to drop.
  * **target** <span class="code">any</span>: target node of the edge to drop.

## #.clear

Drop every node & every edge from the graph, leaving it empty.

*Example*

```js
graph.addNode('John');
graph.addNode('Jack');
graph.addEdge('John', 'Jack');

console.log(graph.order, graph.size);
>>> 2, 1

graph.clear();

console.log(graph.order, graph.size);
>>> 0, 0

graph.hasNode('John');
>>> false
```

## #.clearEdges

Drop every every edge from the graph, keeping only nodes.

*Example*

```js
graph.addNode('John');
graph.addNode('Jack');
graph.addEdge('John', 'Jack');

console.log(graph.order, graph.size);
>>> 2, 1

graph.clearEdges();

console.log(graph.order, graph.size);
>>> 2, 0

graph.hasEdge('John', 'Jack');
>>> false
```
