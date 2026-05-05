# chapter8

## Union-Find（并查集）

### 动机  

- 给定等价关系，判断两个元素是否属于同一个集合。  
- 操作：  
  - `Union(x, y)`：合并两个集合  
  - `Find(x)`：找到元素所在集合的代表（根）  

### 基本实现  

- 用数组存父节点：`S[x] = parent`  
- 根节点：`S[root] <= 0`  

```c
SetType Find(int x, DisjSet S) {
    while (S[x] > 0) x = S[x]; // 往上找
    return x;
}

void Union(DisjSet S, int root1, int root2) {
    S[root2] = root1;
}
```

---

### 优化策略  

#### Union by Size  
- 总是把小树挂到大树下面。  
    - 为了实现这点，我们的root要存的负数的绝对值应该为这个集合的元素个数
- 保证树高 ≤ log n。  

#### Union by Height  
- 总是把浅树挂到深树下面。  

---

### Path Compression（路径压缩）  

```c
SetType Find(int x, DisjSet S) {
    if (S[x] <= 0) return x;
    return S[x] = Find(S[x], S); // 压缩路径，最终返回root节点的编号
}
```

### 性能分析  
- Tarjan 定理：Union by Rank + Path Compression → 平均复杂度接近 O(1)。  
- 精确复杂度：O(α(n))，其中 α(n) 是反 Ackermann 函数，增长极慢（对所有实际规模 n ≤ 10^18，α(n) ≤ 4）。 