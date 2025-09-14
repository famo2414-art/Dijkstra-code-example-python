# Dijkstra-code-example-python




# Language: Python 3
# Input: Weighted graph as adjacency list, source node
# Output: distances to all nodes and shortest paths

import heapq

def dijkstra(graph, source):
    # Initialize distances and predecessors
    distances = {node: float('inf') for node in graph}
    distances[source] = 0
    prev = {node: None for node in graph}
    
    # Priority queue: (distance, node)
    heap = [(0, source)]
    
    while heap:
        curr_dist, u = heapq.heappop(heap)
        
        # Skip if we already found a better path
        if curr_dist > distances[u]:
            continue
        
        # Relax neighbors
        for v, weight in graph[u]:
            if distances[u] + weight < distances[v]:
                distances[v] = distances[u] + weight
                prev[v] = u
                heapq.heappush(heap, (distances[v], v))
    
    return distances, prev

# Helper function to reconstruct path
def reconstruct_path(prev, target):
    path = []
    while target is not None:
        path.append(target)
        target = prev[target]
    return path[::-1]  # Reverse to get source → target

# Sample graph: adjacency list
# Format: node: [(neighbor, weight), ...]
graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('D', 5)],
    'C': [('B', 1), ('D', 8)],
    'D': []
}

source_node = 'A'
distances, prev = dijkstra(graph, source_node)

print("Distances from source:", distances)
print("Shortest path to D:", reconstruct_path(prev, 'D'))
