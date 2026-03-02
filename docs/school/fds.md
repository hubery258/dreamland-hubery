# fds

## chapter 2: Algorithm analysis

- what is algorithm?
    1. 0 or more input(pi)
    2. \>= 1 output
    3. definiteness & **finiteness** & effectiveness

*default: sort in increasing order*

- what to analyze?
    - run times: Machine and compiler(编译器)- **dependent**
    - Time & space complexities: machine and compiler-**independent**

- Assumptions:
    - instructions(指令) are executed sequentially
    - each instruction takes exactly one time unit(所有指令/运算消耗等量时间单元)
    - integer size is fixed and we have infinite memory

- 主要关心: Tavg(N)(平均)和Tworst(最差),N是输入的数据规模（也可以有多个输入规模）

??? example "sum numbers"
    ```c
    float sum (float list[], int n){
        float sum = 0 // count = 1
        int i;
        for (i = 0; i < n; i++){ // count++
            sum += list[i]; // count++
        }

        return sum // count + 1
    }
    ```
    Tsum(n) = 2n+3,在for中i从0~n运算了n+1次，实际执行了0~n-1共n次for内部语句,最后return算一句,共2n+3

### asymptotic(渐进) notation(标记)

我们的重点是比较趋势，所以没有必要去专门求步数。

- T(N) = O(f(N)) if there are positive constants `c` and `n0` such that T(N) ≤ c·f(N) for all N>=no.
    - 即求渐进上界/worst case,T(N)的阶不会高于f(N)
- Ω(omega，小写ω)类似，是T(N) = Ω(f(N)) ... T(N) >= c·f(N) for all N >= n0.
    - 即求渐进下界/best case
- Θ(Theta,是大写): T(N) = Θ(h(N)),当且仅当T(N) = O(h(N)) and T(N) = Ω(h(N))
    - 等价,二者同阶，如N^2 = O(N^2) 又同时 = Ω...
- T(N) = o(p(N)) if T(N) =O(p(N)) and T(N) ≠ Θ(p(N))
    - 严格渐进上界，T(N)的阶必须低于p(N)
- 要求:始终寻找尽可能最贴近的值

#### Rules

1. If T1(N) = O(f(N)) and T2(N) = O(g(N)),then:
    - (a) T1(N) + T2(N) = max( O(f(N)), O(g(N)) )
        - 顺序结构中常见
    - (b) T1(M) * T2(N) =O( f(N)*g(N) ).
        - for循环里常见
2. If T(N) is a *polynomial*(多项式) of degree k(*k次多项式*) , then T(N) = Θ(N^k).
3.  logk N= O(N) for any constant k. 对大规模N，对数函数比线性函数小得多。