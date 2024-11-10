class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])
        return self.parent[u]

    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            if self.rank[root_u] > self.rank[root_v]:
                self.parent[root_v] = root_u
            elif self.rank[root_u] < self.rank[root_v]:
                self.parent[root_u] = root_v
            else:
                self.parent[root_v] = root_u
                self.rank[root_u] += 1
            return True
        return False

def kruskal(m, edges):
    uf = UnionFind(m)
    mst_cost = 0
    for cost, u, v in edges:
        if uf.union(u, v):
            mst_cost += cost
    return mst_cost

def main():
    import sys
    input = sys.stdin.read
    data = input().splitlines()
    
    i = 0
    while i < len(data):
        m, n = map(int, data[i].split())
        if m == 0 and n == 0:
            break
        i += 1
        
        edges = []
        total_cost = 0
        
        for _ in range(n):
            x, y, z = map(int, data[i].split())
            i += 1
            edges.append((z, x, y))
            total_cost += z
        
        edges.sort()
        mst_cost = kruskal(m, edges)
        savings = total_cost - mst_cost
        print(savings)

if __name__ == "__main__":
    main()
