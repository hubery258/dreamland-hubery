# chapter5

## Priority Queues（优先队列）

### 定义  
- 优先队列是一种特殊的队列：删除操作总是删除 **优先级最高/最低** 的元素。  
- 常见实现：最小优先队列（DeleteMin），最大优先队列（DeleteMax）。  

### ADT 模型  
- **对象**：有限有序列表  
- **操作**：  
  - `Initialize(MaxElements)`：初始化  
  - `Insert(X, H)`：插入元素  
  - `DeleteMin(H)`：删除最小元素  
  - `FindMin(H)`：查找最小元素  

### 简单实现方式  

1. **数组（无序）**  
   - 插入：O(1)  
   - 删除最小：O(n)  

2. **链表（无序）**  
   - 插入：O(1)  
   - 删除最小：O(n)  

3. **有序数组/链表**  
   - 插入：O(n)  
   - 删除最小：O(1)  

> 缺点：插入或删除效率总有一方很差。  


### 用Binary Heap（二叉堆）实现

#### 1. 结构性质（Structure Property）  

- 堆是一棵 **完全二叉树**。  
- 用**数组表示二叉树**时：  
  - 根节点下标 = 1  
  - 左孩子 = 2i  
  - 右孩子 = 2i+1  
  - 父节点 = i/2  

#### 2. 堆序性质（Heap Order Property）  

- **最小堆（Min Heap）**：每个节点 ≤ 子节点  
- **最大堆（Max Heap）**：每个节点 ≥ 子节点  

#### 实现代码  

```c

#define MinData -999999   // 哨兵值

// 结构体
typedef struct HeapStruct {
    int Capacity;       // 最大容量
    int Size;           // 当前元素个数
    int* Elements;      // 存储堆元素的数组
} *PriorityQueue;

// 初始化
PriorityQueue Initialize(int MaxElements) {
    PriorityQueue H;
    H = malloc(sizeof(struct HeapStruct));
    H->Elements = malloc((MaxElements + 1) * sizeof(int));
    H->Capacity = MaxElements;
    H->Size = 0;
    H->Elements[0] = MinData; // 哨兵
    return H;
}

/* 插入（Insert）  
- 新元素插到最后一个位置  
- 上滤（Percolate Up）：不断与父节点比较并上移  
- 时间复杂度：O(log n)  */

void Insert(int X, PriorityQueue H) {
    int i;
    if (H->Size == H->Capacity) {
        printf("Heap is full!\n");
        return;
    }
    // 上滤：不断和父节点比较
    for (i = ++H->Size; H->Elements[i/2] > X; i /= 2) {
        H->Elements[i] = H->Elements[i/2];
    }
    H->Elements[i] = X;
}



/* 删除最小值（DeleteMin）  
- 删除根节点（最小值）  
- 用最后一个元素填补根  
- 下滤（Percolate Down）：不断与较小的孩子交换  
- 时间复杂度：O(log n)*/  

int DeleteMin(PriorityQueue H) {
    int i, Child;
    int MinElement, LastElement;

    if (H->Size == 0) {
        printf("Heap is empty!\n");
        return H->Elements[0];
    }

    MinElement = H->Elements[1];              // 根节点最小值
    LastElement = H->Elements[H->Size--];     // 最后一个元素

    // 下滤：不断和较小的孩子比较
    for (i = 1; i*2 <= H->Size; i = Child) {
        Child = i*2;
        if (Child != H->Size && H->Elements[Child+1] < H->Elements[Child])
            Child++; // 如果右边的孩子更大就用右边的去比
        if (LastElement > H->Elements[Child]) 
            H->Elements[i] = H->Elements[Child]; //如果最后的元素比孩子大，那么就让孩子上浮，自己下滤
        else break;
    }
    H->Elements[i] = LastElement;
    return MinElement;
}

```

## Segment Tree（线段树）

### 动机  

- 给定一个大数组 A，需要频繁计算区间 [L, R] 的和。  
- 朴素方法：逐个加 → O(n)，效率太低。  
- 线段树：把数组分成区间，用树结构存储区间信息 → 查询和更新都能做到 O(log n)。  

### 结构  

- 完全二叉树，每个节点表示一个区间。  
- 叶子节点：单个元素。  
- 内部节点：左右子区间的和。  
- 存储方式：用**数组存树**，`tree[node]` 表示区间和。  

<hr>

### 实现

```c
/* 构建（Build）  
时间复杂度：O(n)，只需构建一次。 */ 
void Build(int node, int start, int end) {
    if (start == end) { // 叶子节点
        tree[node] = A[start];
        return;
    }
    int mid = (start + end) / 2;
    Build(2*node, start, mid); //左子树
    Build(2*node+1, mid+1, end); //右子树
    tree[node] = tree[2*node] + tree[2*node+1];
}

/* 查询（Query）时间复杂度：O(log n)。*/  
int Query(int node, int start, int end, int L, int R) {
    if (R < start || end < L) return 0; // 无重叠
    if (L <= start && end <= R) return tree[node]; // 完全覆盖
    int mid = (start + end) / 2;
    int left_sum = Query(2*node, start, mid, L, R);
    int right_sum = Query(2*node+1, mid+1, end, L, R);
    return left_sum + right_sum;
}
/* 更新（Update）  时间复杂度：O(log n)。 */

void Update(int node, int start, int end, int idx, int val) {
    if (start == end) { // 找到叶子
        tree[node] = val;
        return;
    }
    int mid = (start + end) / 2;
    if (idx <= mid) Update(2*node, start, mid, idx, val);
    else Update(2*node+1, mid+1, end, idx, val);
    tree[node] = tree[2*node] + tree[2*node+1]; // 回溯更新
}
```
