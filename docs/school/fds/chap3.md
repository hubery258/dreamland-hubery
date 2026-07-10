# chapter 3: List, Stacks and Queues

??? note "本章零碎单词积累"
    - **Polynomials**: 多项式
        - **Coefficient**: 系数
        - **Exponent**: 指数

## List(列表)

- 两种代表: 
    - 数组(sequential list)，查找快但插入或删除操作麻烦
    - 链表(linked list)
        - 为了操作第一个节点，通常会创造**虚拟头节点**(dummy head node),不存数据，指针指向第一个node
        - 双链表多一个指向上一个node的指针，方便查找上下位
        - cursor space: (ppt-list-13,14),无需`malloc`与`free`，用数组模拟链表
??? example "cursor space实例"
    ```
    #include <stdio.h>
    #define MAX_SIZE 100 // 定义游标空间的最大容量

    // 定义游标空间的节点结构（数组元素的类型）
    typedef struct {
        int Coefficient; // 系数
        int Exponent;    // 指数
        int cursor;      // 代替指针，存下一个节点的数组下标
    } CursorNode;

    // 定义游标空间（全局数组）
    CursorNode CursorSpace[MAX_SIZE];
    int AVAIL; // 指向空闲节点的起始下标

    // 初始化游标空间（关键：把所有节点连成一个空闲链表）
    void InitCursorSpace() {
        for (int i = 0; i < MAX_SIZE - 1; i++) {
            CursorSpace[i].cursor = i + 1; // 每个节点的cursor指向下一个下标
        }
        CursorSpace[MAX_SIZE - 1].cursor = -1; // 最后一个节点cursor为-1（NULL）
        AVAIL = 0; // 初始时，空闲链表从下标0开始
    }

    // 模拟malloc：从空闲空间分配一个节点（返回节点下标）
    int CursorMalloc() {
        if (AVAIL == -1) return -1; // 空间已满
        int new_node = AVAIL;       // 取当前空闲节点的下标
        AVAIL = CursorSpace[AVAIL].cursor; // AVAIL指向下一个空闲节点
        return new_node;
    }

    // 模拟free：释放节点回空闲空间
    void CursorFree(int node) {
        if (node < 0 || node >= MAX_SIZE) return;
        CursorSpace[node].cursor = AVAIL; // 释放的节点指向当前AVAIL
        AVAIL = node; // AVAIL指向这个释放的节点
    }

    // 测试：创建3x² + 5x的多项式链表
    int main() {
        InitCursorSpace(); // 初始化游标空间
    
        // 分配第一个节点（3x²）
        int node1 = CursorMalloc();
        CursorSpace[node1].Coefficient = 3;
        CursorSpace[node1].Exponent = 2;
    
        // 分配第二个节点（5x¹）
        int node2 = CursorMalloc();
        CursorSpace[node2].Coefficient = 5;
        CursorSpace[node2].Exponent = 1;
    
        // 链接节点（代替指针的Next）
        CursorSpace[node1].cursor = node2;
        CursorSpace[node2].cursor = -1; // 链表尾
    
        // 打印验证
        printf("第一个项：%dx^%d\n", CursorSpace[node1].Coefficient, CursorSpace[node1].Exponent);
        printf("第二个项：%dx^%d\n", CursorSpace[node2].Coefficient, CursorSpace[node2].Exponent);
    
        // 释放节点
        CursorFree(node1);
        CursorFree(node2);
        return 0;
    }
    ```

## Stack(栈)

### 定义与基本操作<br>
- **FILO**(first in, last out)的list
- 基本操作: push,pop,create,makeempty,top

### 实现

```c
// 结构定义

typedef struct Node{
    int element;
    struct Node* next; // Node 还没被定义要写出来struct
} Node;
typedef Node* stack;

// 创建空栈,S是头节点，表示栈顶，初始指向node
stack CreateStack() {
    stack S = (stack)malloc(sizeof(Node));
    if (S == NULL) {
        // error
    }
    S->next = NULL;
    return S;
}

// push 元素
void stack_push(int x, stack S){
    stack newstack = (stack)malloc(sizeof(Node));
    if (newstack == NULL){
        // error    
    }

    newstack->element = x;
    newstack->next = S->next;
    S->next = newstack;
} // 具体的操作流程图见代码下方

// pop元素
void stack_pop(stack S){
    stack first = S->next;
    if (S->next == NULL){
        //error,表示stack里没有元素，无法正常弹出
    }
    S->next = first->next;
    free(first);
}

// 取top
int stack_top(stack S){
    if (S->next == NULL){
        // error,无元素
    }
    return S->next->element;
}

void stack_empty(stack S){
    while(S->next != NULL){
        stack_pop(S);
    }
}
```

??? example
    现在我们先创建一个栈:
    ```
    S -> NULL
    ```

    如果我们进行了push，那么:
    ```
    S -> [1] -> NULL
    ```

    再次push:
    ```
    S -> [2] -> [1] -> NULL
    ```

    `[2]`就在`[1]`的上面，pop就会先把`[2]`pop出来

<hr>

## Queues(队列)

### 定义与基本操作  
- **FIFO（First In, First Out）** 
- 插入在队尾（rear），删除在队头（front）  
- 基本操作：`enqueue`、`dequeue`、`create`、`makeempty`、`front`  


### 实现（数组循环队列 Circular Queue）

队列的链表实现太简单（头删尾插），重点是**数组循环队列**

```c
// 结构定义
typedef struct QueueRecord {
    int Capacity;   // 最大容量
    int Front;      // 指向队头前一个位置
    int Rear;       // 指向队尾
    int* Array;     // 存储元素的数组
} QueueRecord;
typedef struct QueueRecord* queue;

// 创建空队列

queue CreateQueue(int MaxSize) {
    queue Q = (queue)malloc(sizeof(QueueRecord));
    Q->Array = (int*)malloc(sizeof(int) * MaxSize); // 数组定义
    Q->Capacity = MaxSize; // 实际可用MaxSize - 1个位置
    Q->Front = 0; // Front指向队开头的前一个位置(浪费一个空间以区分队空/队满)
    Q->Rear = 0;
    return Q;
}  

// enqueue（入队）
void enqueue(int x, queue Q) {
    if ((Q->Rear + 1) % Q->Capacity == Q->Front) {
        // error: 队满
        return;
    }

    Q->Rear = (Q->Rear + 1) % Q->Capacity;
    Q->Array[Q->Rear] = x;
}

// dequeue（出队）
int dequeue(queue Q) {
    if (Q->Front == Q->Rear) {
        // error: 队空
        return -1;
    }

    Q->Front = (Q->Front + 1) % Q->Capacity;
    return Q->Array[Q->Front];
}

// front（取队头元素）
int queue_front(queue Q) {
    if (Q->Front == Q->Rear) {
        // error: 队空
        return -1;
    }
    return Q->Array[(Q->Front + 1) % Q->Capacity];
}

// makeempty（清空队列）
void queue_empty(queue Q) {
    Q->Front = Q->Rear = 0;
}
```

??? example 

    假设容量为 6（实际可用 5 个元素）：

    初始空队列：
    ```
    Front = 0
    Rear  = 0
    [ ] [ ] [ ] [ ] [ ] [ ]
    ```

    执行 enqueue(1)：
    ```
    Rear = 1
    [ ] [1] [ ] [ ] [ ] [ ]
    ```

    执行 enqueue(2)：
    ```
    Rear = 2
    [ ] [1] [2] [ ] [ ] [ ]
    ```

    执行 enqueue(3)：
    ```
    Rear = 3
    [ ] [1] [2] [3] [ ] [ ]
    ```

    执行 dequeue()：
    ```
    Front = 1
    返回 1
    队列内容：2,3
    ```

    执行 enqueue(4)、enqueue(5)：
    ```
    Front = 1
    Rear  = 5
    [ ] [F] [2] [3] [4] [5]
    ```

    队列从队头到队尾：2, 3, 4, 5，enqueue(6) 是合法的，因为下标 0 还空着，enqueue(7) 会触发“队满”判断，因为队列已经满了。这就是创造一个空着用front表示的原因

<hr>