## Desafio E - Otimização de Iluminação em Byteland 🌃💡

### 📝 Descrição do Problema

Byteland é uma terra onde todas as estradas são iluminadas durante a noite para garantir a segurança de seus habitantes. No entanto, manter todas as estradas iluminadas custa 1 Dólar Byteland por metro a cada dia. 

Para economizar, o governo de Byteland decidiu desligar a iluminação de algumas estradas, desde que todas as junções ainda possam ser conectadas por pelo menos um caminho iluminado.

O desafio é calcular a **quantidade máxima de dinheiro que o governo pode economizar** ao desligar algumas luzes, sem comprometer a segurança dos habitantes. Em outras palavras, precisamos desligar a iluminação de algumas estradas enquanto garantimos que todas as junções continuem conectadas.

### 🔢 Entrada

A entrada contém vários casos de teste:

1. Cada caso de teste começa com dois inteiros `m` e `n`:
   - `m` (1 ≤ m ≤ 200,000): o número de junções (nós).
   - `n` (m-1 ≤ n ≤ 200,000): o número de estradas (arestas).
   
2. Seguem `n` linhas com três inteiros `x`, `y` e `z`, onde:
   - `x` e `y` (0 ≤ x, y < m, x ≠ y): identificam as junções conectadas por uma estrada.
   - `z` (0 ≤ z): indica o comprimento da estrada em metros, que também representa o custo diário para mantê-la iluminada.

3. A entrada termina com `m = 0` e `n = 0`.

### 🖨️ Saída

Para cada caso de teste, o programa deve imprimir um número inteiro que representa a **quantidade máxima de dinheiro que pode ser economizada** diariamente ao desligar a iluminação de algumas estradas, enquanto mantém todas as junções conectadas.

### 📚 Exemplo de Entrada
```
7 11
0 1 7
0 3 5
1 2 8
1 3 9
1 4 7
2 4 5
3 4 15
3 5 6
4 5 8
4 6 9
5 6 11
0 0
```

### 📤 Exemplo de Saída
```
51
```

---

### 🛠️ Solução Utilizada

A solução utiliza o **Algoritmo de Kruskal** para encontrar a **Árvore Geradora Mínima (MST)** do grafo, garantindo que todas as junções permaneçam conectadas ao menor custo possível.

### 🚀 Abordagem

1. **Estrutura de Dados Utilizada**:
   - Utilizamos a estrutura de **Union-Find** (também conhecida como Disjoint Set Union, DSU) para gerenciar os componentes conectados.
   - Implementamos **path compression** e **union by rank** para otimizar o desempenho da estrutura.

2. **Algoritmo de Kruskal**:
   - Ordenamos todas as estradas pelo custo.
   - Utilizamos a estrutura de Union-Find para adicionar as estradas que não formam ciclos, até obter a Árvore Geradora Mínima (MST).
   - A economia é calculada subtraindo o custo total de todas as estradas pelo custo da MST.

### 💻 Código em Python

```python
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
```

### 🧩 Explicação do Código

1. **Union-Find**:
   - Inicializa cada nó como seu próprio pai.
   - Utiliza **path compression** para otimizar as operações de busca.
   - Implementa **union by rank** para manter a árvore balanceada durante as operações de união.

2. **Kruskal**:
   - Ordena todas as estradas por comprimento.
   - Adiciona arestas ao MST enquanto evita ciclos.
   - Calcula o custo total da MST e a economia possível.

### 📈 Complexidade

- O algoritmo de Kruskal, utilizando a estrutura de Union-Find, possui complexidade de **O(E log E)**, onde `E` é o número de estradas. 
- Esta solução é eficiente para os limites do problema (`m, n ≤ 200,000`).

---

### 🎯 Conclusão

Esta solução encontra a quantidade máxima de dinheiro que o governo de Byteland pode economizar enquanto mantém todas as junções conectadas de forma segura. O uso do **Algoritmo de Kruskal** e **Union-Find** é ideal para este tipo de problema de otimização de grafos.
