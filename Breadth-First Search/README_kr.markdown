# Breadth-First Search

> 튜토리얼은 여기서 진행해볼 수 있습니다. [here](https://www.raywenderlich.com/155801/swift-algorithm-club-swift-breadth-first-search)

BFS는 트리 혹은 그래프를 탐색하는 방법 중 하나 입니다. 다음 이웃 노드로 움직이기 전에 시작 노드에서부터 연결되어있는 주변 노드들을 탐색하는 방식입니다.

BFS는 방향 그래프와 무방향그래프에 상관없이 사용될 수 있습니다.

## Animated example

Here's how breadth-first search works on a graph:
지금부터 BFS가 어떻게 작동하는지 설명하겠습니다 :

![Animated example of a breadth-first search](Images/AnimatedExample.gif)

방문할 노드는 검정색으로 표시하겠습니다. 현재 노드와 연결된 이웃 노드들을 모두 큐에 넣습니다. 에니메이션에서 노드는 큐에 들어가지만 아직 방문되지는 않았기 때문에 회색으로 표시합니다.

Let's follow the animated example. We start with the source node `A` and add it to a queue. In the animation this is shown as node `A` becoming gray.

```swift
queue.enqueue(A)
```

The queue is now `[ A ]`. The idea is that, as long as there are nodes in the queue, we visit the node that's at the front of the queue, and enqueue its immediate neighbor nodes if they have not been visited yet.

To start traversing the graph, we pull the first node off the queue, `A`, and color it black. Then we enqueue its two neighbor nodes `B` and `C`. This colors them gray.

```swift
queue.dequeue()   // A
queue.enqueue(B)
queue.enqueue(C)
```

The queue is now `[ B, C ]`. We dequeue `B`, and enqueue `B`'s neighbor nodes `D` and `E`.

```swift
queue.dequeue()   // B
queue.enqueue(D)
queue.enqueue(E)
```

The queue is now `[ C, D, E ]`. Dequeue `C`, and enqueue `C`'s neighbor nodes `F` and `G`.

```swift
queue.dequeue()   // C
queue.enqueue(F)
queue.enqueue(G)
```

The queue is now `[ D, E, F, G ]`. Dequeue `D`, which has no neighbor nodes.

```swift
queue.dequeue()   // D
```

The queue is now `[ E, F, G ]`. Dequeue `E` and enqueue its single neighbor node `H`. Note that `B` is also a neighbor for `E` but we've already visited `B`, so we're not adding it to the queue again.

```swift
queue.dequeue()   // E
queue.enqueue(H)
```

The queue is now `[ F, G, H ]`. Dequeue `F`, which has no unvisited neighbor nodes.

```swift
queue.dequeue()   // F
```

The queue is now `[ G, H ]`. Dequeue `G`, which has no unvisited neighbor nodes.

```swift
queue.dequeue()   // G
```

The queue is now `[ H ]`. Dequeue `H`, which has no unvisited neighbor nodes.

```swift
queue.dequeue()   // H
```

The queue is now empty, meaning that all nodes have been explored. The order in which the nodes were explored is `A`, `B`, `C`, `D`, `E`, `F`, `G`, `H`.

We can show this as a tree:

![The BFS tree](Images/TraversalTree.png)

The parent of a node is the one that "discovered" that node. The root of the tree is the node you started the breadth-first search from.

For an unweighted graph, this tree defines a shortest path from the starting node to every other node in the tree. So breadth-first search is one way to find the shortest path between two nodes in a graph.

## The code

Simple implementation of breadth-first search using a queue:

```swift
func breadthFirstSearch(_ graph: Graph, source: Node) -> [String] {
  var queue = Queue<Node>()
  queue.enqueue(source)

  var nodesExplored = [source.label]
  source.visited = true

  while let node = queue.dequeue() {
    for edge in node.neighbors {
      let neighborNode = edge.neighbor
      if !neighborNode.visited {
        queue.enqueue(neighborNode)
        neighborNode.visited = true
        nodesExplored.append(neighborNode.label)
      }
    }
  }

  return nodesExplored
}
```

While there are nodes in the queue, we visit the first one and then enqueue its immediate neighbors if they haven't been visited yet.

Put this code in a playground and test it like so:

```swift
let graph = Graph()

let nodeA = graph.addNode("a")
let nodeB = graph.addNode("b")
let nodeC = graph.addNode("c")
let nodeD = graph.addNode("d")
let nodeE = graph.addNode("e")
let nodeF = graph.addNode("f")
let nodeG = graph.addNode("g")
let nodeH = graph.addNode("h")

graph.addEdge(nodeA, neighbor: nodeB)
graph.addEdge(nodeA, neighbor: nodeC)
graph.addEdge(nodeB, neighbor: nodeD)
graph.addEdge(nodeB, neighbor: nodeE)
graph.addEdge(nodeC, neighbor: nodeF)
graph.addEdge(nodeC, neighbor: nodeG)
graph.addEdge(nodeE, neighbor: nodeH)
graph.addEdge(nodeE, neighbor: nodeF)
graph.addEdge(nodeF, neighbor: nodeG)

let nodesExplored = breadthFirstSearch(graph, source: nodeA)
print(nodesExplored)
```

This will output: `["a", "b", "c", "d", "e", "f", "g", "h"]`
   
## What is BFS good for?

Breadth-first search can be used to solve many problems. A small selection:

* Computing the [shortest path](../Shortest%20Path%20(Unweighted)/) between a source node and each of the other nodes (only for unweighted graphs).
* Calculating the [minimum spanning tree](../Minimum%20Spanning%20Tree%20(Unweighted)/) on an unweighted graph.

*Written by [Chris Pilcher](https://github.com/chris-pilcher) and Matthijs Hollemans*
