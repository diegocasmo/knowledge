# Depth First Search In Undirected Graphs

Chapter 3 and 4 of the book Algorithms by S. Dasgupta, C. H. Papadimitriou, and U. V. Vazirani focus on graphs. A graph is specified as a set of vertices ``V`` (or nodes) and edges ``E`` between selected pairs of vertices. One of the big advantages of using graphs is that graphs provide clarity when solving problems, since they are not cluttered with irrelevant information, just the mathematical object.

### Graph Representation
A graph with ``n=|V|`` vertices ``v1,...,vn`` can be represented as a matrix (an array of ``n x n``), whose ``(i, j)th`` entry is:

- 1 if there is an edge from ``vi`` to ``vj``
- 0 otherwise

In a matrix representation of a graph, the presence of a particular edge can be inspected in constant time, but it requires ``O(n^2)`` of memory space, which can be wasteful if the graph does not have many edges. Another representation of a graph is an adjacency list. It consists of ``|V|`` linked lists (one per vertex). Then, the linked list for vertex ``u`` holds the names of vertices which ``u`` has and outgoing edge. In contrast to the matrix representation, verifying the presence of a particular edge is now linear (by running down the corresponding linked list), but the memory required to store the graph is ``O(|E|)``.

A graph can be classified by the number of edges it has as:

  - A dense graph: the number of edges is close to the maximum number of edges the graph can have
  - A sparse graph: the number of edges is close to the minimal number of edges the graph can have

This is important since it plays a big role when selecting a proper data structure and algorithm to use when working with graphs. For instance, for storing the World Wide Web as graph (a Web page is a vertex, and it has edges to all the other Web pages it has a hyper-link to), it is more convenient to use an adjacency list because the World Wide Web graph is very sparse (the average Web page has hyper-links to only about half a dozen other pages, out of billions of possibilities).

### Depth First Search in Undirected Graphs
The first algorithm the author examines in Chapter 3 is depth first search in undirected graphs. In a undirected graph, vertices that are connected together have bidirectional edges.

Depth first search is a linear time algorithm which essentially answers the following question:

- What parts of the graph are reachable from a given vertex?

The following implementation of the depth first search algorithm uses an adjacency list and returns all vertices of a graph which are reachable from the specified vertex.

### Depth First Search Algorithm

#### [Pseudocode:](https://en.wikipedia.org/wiki/Depth-first_search)

```
procedure DFS(G,v):
  label v as discovered
  for all edges from v to w in G.adjacentEdges(v) do
    if vertex w is not labeled as discovered then
      recursively call DFS(G,w)
```

#### Implementation:

``` ruby
class DFS

  def initialize(graph={})
    @graph = graph
    @visited = {}
  end

  # This implementation of depth-first search visits
  # only the portion of the graph reachable from the
  # starting vertex
  # Input: A vertex element of the graph
  # Output: All reachable vertices from vertex
  def dfs(vertex)
    raise RuntimeError if @graph[vertex].nil?
    @visited = {}
    explore(@graph[vertex], vertex)
    return @visited.keys - [vertex]
  end

  private

  # Find all reachable vertices from a particular vertex
  def explore(graph_of_vertex, current_vertex)
    @visited[current_vertex] = true
    graph_of_vertex.each do |vertex|
      if not @visited[vertex]
        explore(@graph[vertex], vertex)
      end
    end
  end
end
```

#### Tests:

``` ruby
require 'minitest/autorun'
require './depth_first_search_undirected'

describe DFS do

  before do
    graph = {
      :a => [:b, :e],
      :b => [:a],
      :c => [:d, :g, :h],
      :d => [:h, :c],
      :e => [:j, :i],
      :f => [],
      :g => [:c, :h, :k],
      :h => [:d, :l, :c, :g, :k],
      :i => [:e, :j],
      :j => [:e, :i],
      :k => [:g, :h],
      :l => [:h]
    }
    @dfs = DFS.new(graph)
  end

  describe "#dfs" do
    it "should return all reachable vertices from vertex 'a'" do
      assert_equal(@dfs.dfs(:a), [:b, :e, :j, :i])
    end

    it "should return all reachable vertices from vertex 'c'" do
      assert_equal(@dfs.dfs(:c), [:d, :h, :l, :g, :k])
    end

    it "should return all reachable vertices from vertex 'f'" do
      assert_equal(@dfs.dfs(:f), [])
    end

    it "should raise an error if vertex is not defined in graph" do
      assert_raises(RuntimeError) { @dfs.dfs(:foo) }
    end
  end
end
```

### Conclusion
Depth first search is an interesting algorithm, and as you might suspect, it is particularly well suited for inspecting if a graph is connected; if the tree returned by depth first search contains all vertices in the graph, it is connected, otherwise, it is not.
