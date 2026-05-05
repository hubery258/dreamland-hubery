# 离散数学

!!! info
    由于本身对这门课的意义感受不大，不会详细体系地记录笔记，只记录一些必要的必考的知识。

## chapter1

1. 命题 (Proposition)
    - **命题 proposition**：一个陈述句，要么真 (True)，要么假 (False)，不能同时为真和假。  
    - **命题变量 propositional variable**：常用小写字母 (p, q, r, s)。  
    - **真值 truth value**：T (真)，F (假)。  

2. 逻辑运算 (Logical Operators)
    - **否定 negation (¬p)**：真变假，假变真。  
    - **合取 conjunction (p ∧ q)**：只有当 p 和 q 都为真时为真。  
    - **析取 disjunction (p ∨ q)**：至少一个为真时为真（inclusive OR）。  
    - **异或 exclusive OR (p ⊕ q)**：仅当 p 和 q 中恰好一个为真时为真。  
    - **条件 conditional (p → q)**：只有 p 为真且 q 为假时为假。  
    - **双条件 biconditional (p ↔ q)**：p 与 q 真值相同则为真。  

3. 条件语句的变形 (Conditional Variants)
    - **逆命题 converse**：\(q → p\)。  
    - **否命题 inverse**：\(¬ p → ¬ q\)。  
    - **逆否命题 contrapositive**：\(¬q → ¬p\)。  
    - 条件语句与其逆否命题等价。  

4. 运算优先级 (Precedence of Operators)
    - 括号最高，其次是 **¬**，再是 **∧, ∨**，最后是 **→, ↔**。 

1. 复合命题分类 (Classification of Compound Propositions)
    - **永真式 tautology**：始终为真。  
    - **永假式 contradiction**：始终为假。  
    - **偶然式 contingency**：既不是永真也不是永假。  

2. 命题等价 (Logical Equivalence)
    - **逻辑等价 logically equivalent**：若 \(p <-> q\) 是永真式，则称 \(p\) 与 \(q\) 等价。  
    - **记号 notation**：\(p ≡ q\) 或 \(p <-> q\)。  
    - **证明方法**：  
        1. **真值表 truth table**：逐行比较。  
        2. **已知等价式 equivalences**：利用已证明的等价式逐步替换。  

3. 常见等价律 (Common Equivalences)
    - 恒等律 Identity laws：\(p ∧ T ≡ p ; p ∨ F ≡ p\)  
    - 支配律 Domination laws：\(p ∨ T ≡ T ; p ∧ F ≡ F\)  
    - 幂等律 Idempotent laws：\(p ∨ p ≡ p ; p ∧ p ≡ p\)  
    - 双重否定 Double negation：¬(¬p) ≡ p  
    - 交换律 Commutative laws：\(p ∨ q ≡ q ∨ p ; p ∧ q ≡ q ∧ p\)  
    - 结合律 Associative laws：\((p ∨ q) ∨ r ≡ p ∨ (q ∨ r)\)  
    - **分配律 Distributive laws**：\(p ∨ (q ∧ r) ≡ (p ∨ q) ∧ (p ∨ r)\)  
    - **德摩根律 De Morgan’s laws**：¬(p ∧ q) ≡ ¬p ∨ ¬q ；¬(p ∨ q) ≡ ¬p ∧ ¬q  
    - **吸收律 Absorption laws**：\(p ∨ (p ∧ q) ≡ p ; p ∧ (p ∨ q) ≡ p\)  
    - 否定律 Negation laws：\(p ∨ ¬p ≡ T,\; p ∧ ¬p ≡ F\)  
    - **蕴涵律 Implication law**：\(p → q ≡ ¬p ∨ q\)  
    - 双条件律 Equivalence law：\(p ↔ q ≡ (p → q) ∧ (q → p)\)  


4. 可满足性 (Satisfiability)
    - **可满足 satisfiable**：存在某种真值赋值使复合命题为真。  
    - **不可满足 unsatisfiable**：无论如何赋值都为假。  
    - **关系**：命题不可满足 ⇔ 其否定是永真式。  

1. 谓词 (Predicate)
    - **谓词 predicate**：含有变量的陈述句，变量取值后才能确定真值。  
    - **命题函数 propositional function**：如 P(x), Q(x,y)。  
    - 示例：  
        - P(x): x > 3  
        - Q(x,y): x < y 

1. 量词(Quantifier):
    - 全称量词 (Universal Quantifier)
        - **全称量词 universal quantifier**：`∀xP(x)`，表示“对所有 x，P(x) 成立”。  
        - **反例 counterexample**： `∀xP(x)` 为假的元素。  
        - 常见表达：for all, for every, for each, given any。  
    - 存在量词 (Existential Quantifier)
        - **存在量词 existential quantifier**：`∃xP(x)`，表示“存在某个 x 使得 P(x) 成立”。  
        - 常见表达：there exists, for some, at least one。  
        - **唯一量词 uniqueness quantifier**：∃!xP(x)，表示“存在唯一的 x 使得 P(x) 成立”。  

4. 量词的逻辑等价 (Logical Equivalences with Quantifiers)
    - **德摩根律 De Morgan’s laws for quantifiers**：  
        - ¬∃xP(x) ≡ ∀x¬P(x)  
        - ¬∀xP(x) ≡ ∃x¬P(x)  
    - **结合逻辑运算 equivalences**：  
        - ∀x(A(x) ∧ B(x)) ≡ (∀xA(x)) ∧ (∀xB(x))  
        - ∃x(A(x) ∨ B(x)) ≡ (∃xA(x)) ∨ (∃xB(x))  


5. 嵌套量词 (Nested Quantifiers)
  - **嵌套量词 nested quantifiers**：一个量词在另一个量词的作用域内。    

<hr>

1. 有效论证 (Valid Arguments)
  - **论证 argument**：一系列命题，最后一个是结论，其余是前提。  
  - **有效 valid**：如果所有前提为真，则结论必为真。  
  - **论证形式 argument form**：用命题变量表示的复合命题序列。  
  - **判定方法**：若 **P<sub>1</sub> ∧ P<sub>2</sub> ∧ … ∧ P<sub>n</sub> → q** 是永真式，则论证有效。  

2. 命题逻辑的推理规则 (Rules of Inference for Propositional Logic)
  - **肯定前件 Modus Ponens**：  
    - 前提：p, p → q 
    - 结论：q  
  - **否定后件 Modus Tollens**：  
    - 前提：¬q, p → q 
    - 结论：¬p  
  - **假言三段论 Hypothetical Syllogism**：  
    - 前提：p → q, q → r  
    - 结论：p → r  
  - **析取三段论 Disjunctive Syllogism**：  
    - 前提：p ∨ q, ¬p  
    - 结论：q
  - **加法 Addition**：  
    - 前提：p  
    - 结论：p ∨ q  
  - **化简 Simplification**：  
    - 前提：p ∧ q  
    - 结论：p  
  - **合取 Conjunction**：  
    - 前提：p, q  
    - 结论：p ∧ q  
  - **归结 Resolution**：  
    - 前提：p ∨ q, ¬p ∨ r  
    - 结论：q ∨ r  

3. 量词推理规则 (Rules of Inference for Quantified Statements)
  - **全称实例化 Universal Instantiation (UI)**：∀xP(x) ⊢ P(c)。  
  - **全称推广 Universal Generalization (UG)**：若对任意 c 成立，则 ⊢ ∀xP(x)。  
  - **存在实例化 Existential Instantiation (EI)**：∃xP(x) ⊢ P(c)（某个 c）。  
  - **存在推广 Existential Generalization (EG)**：若 `P(c)` 成立，则 ⊢ ∃xP(x)。  

5. 综合推理 (Combining Inference Rules)
  - **全称肯定 Universal Modus Ponens**: ∀x(P(x) → Q(x)), P(a) ⊢ Q(a)。  
  - **全称否定 Universal Modus Tollens**：∀x(P(x) → Q(x)), ¬Q(a) ⊢ ¬P(a)。  

1. 基本术语 (Terminology)
  - **证明 proof**：建立数学命题真实性的有效论证。  
  - **定理 theorem**：可以被证明为真的陈述。  
  - **公理 axiom / postulate**：假定为真的陈述。  
  - **引理 lemma**：辅助证明其他结果的次要定理。  
  - **推论 corollary**：由已证明定理直接推出的结论。  
  - **猜想 conjecture**：尚未证明但被认为可能为真的陈述。  

2. 证明类型 (Types of Proofs)
  - **直接证明 direct proof**：假设前提成立，推导出结论。  
  - **逆否证明 proof by contraposition**：证明 ¬Q → ¬P。  
  - **真空证明 vacuous proof**：若前提 P 为假，则 P → Q 恒为真。  
  - **平凡证明 trivial proof**：若结论 Q 已知为真，则 P → Q 恒为真。  
  - **反证法 proof by contradiction**：假设命题为假，推出矛盾。  
  - **分类讨论 proof by cases**：将前提分解为若干情况，逐一证明。  
  - **穷举证明 exhaustive proof**：通过有限枚举所有可能情况。  
  - **存在性证明 existence proof**：  
    - 构造性 constructive：给出具体例子。  
    - 非构造性 nonconstructive：通过矛盾证明存在性。  
  - **唯一性证明 uniqueness proof**：证明存在并且只有一个满足条件的元素。  

3. 证明策略 (Proof Strategies)
  - **正向推理 forward reasoning**：从前提出发逐步推导结论。  
  - **逆向推理 backward reasoning**：从结论出发，寻找能推出结论的前提。  
  - **等价证明 proofs of equivalence**：证明双向蕴涵或多个命题的循环蕴涵。  

4. 其他方法 (Additional Methods)
  - 数学归纳法 mathematical induction  
  - 结构归纳法 structural induction  
  - 康托对角线法 Cantor diagonalization  
  - 组合证明 combinatorial proofs  

<hr>

## chapter2

1. 集合基础 (Sets)
  - **集合 set**：无序的对象集合。  
  - **元素 element / member**：集合中的对象。  
  - **表示方法**：  
    - 列举法 roster method：如 {1,3,5,7,9}。  
    - 省略号 ellipses：如 {1,2,3,…,99\}。  
    - 描述法 set-builder notation：{x | P(x)\}。  
  - **维恩图 Venn diagram**：用矩形表示全集 U，用圆表示子集。  
  - **空集 empty set**：\(\varnothing\)，是所有集合的子集。  

2. 集合关系 (Relations Between Sets)
  - **子集 subset**：\(A ⊆ B\)。  
  - **真子集 proper subset**：\(A ⊂ B\)。  
  - **相等 equal sets**：若元素完全相同。  
  - **基数 cardinality**：集合中元素个数，记作 |S|。有限集与无限集。  

3. 幂集 (Power Set)
  - **定义 definition**：集合 S 的所有子集构成的集合，记作 P(S)。  
  - **性质**：若 `|S|=n`，则 |P(S)|=2<sup>n</sup>。  

4. 笛卡尔积 (Cartesian Product)
  - **定义 definition**：A × B = {(a,b) | a ∈ A, b ∈ B}。  
  - **性质**：若 |A|=m, |B|=n，则 |A × B| = mn。  
  - **注意**：A × B != B × A。  

5. 真值集 (Truth Sets of Quantifiers)
  - 给定谓词 P 和定义域 D，真值集为 \{x ∈ D | P(x)\}。  

6. 集合运算 (Set Operations)
  - **并 union**：\(A \cup B = \{x | x \in A \vee x \in B\}\)。  
  - **交 intersection**：\(A \cap B = \{x | x \in A \wedge x \in B\}\)。  
  - **差 difference**：\(A - B = \{x | x \in A \wedge x \notin B\}\)。  
  - **补 complement**：\(\overline{A} = \{x | x \notin A, x \in U\}\)。  
  - **对称差 symmetric difference**：\(A \oplus B = (A \cup B) - (A \cap B)\)。  
  - **基数公式**：\(|A \cup B| = |A| + |B| - |A \cap B|\)。  

7. 集合恒等式 (Set Identities)
  - 恒等律 Identity laws：\(A \cup \varnothing = A, A \cap U = A\)。  
  - 支配律 Domination laws：\(A \cup U = U, A \cap \varnothing = \varnothing\)。  
  - 幂等律 Idempotent laws：\(A \cup A = A, A \cap A = A\)。  
  - 交换律 Commutative laws：\(A \cup B = B \cup A\)。  
  - 结合律 Associative laws：\((A \cup B) \cup C = A \cup (B \cup C)\)。  
  - 分配律 Distributive laws：\(A \cup (B \cap C) = (A \cup B) \cap (A \cup C)\)。  
  - 德摩根律 De Morgan’s laws：\(\overline{A \cup B} = \overline{A} \cap \overline{B}\)。  
  - 吸收律 Absorption laws：\(A \cup (A \cap B) = A\)。  
  - 补集律 Complement laws：\(A \cup \overline{A} = U, A \cap \overline{A} = \varnothing\)。  

8. 计算机表示 (Computer Representation)
  - 用位串 bit string 表示集合，1 表示元素在集合中，0 表示不在。  
  - 并 → 位或 OR，交 → 位与 AND，差 → 位运算。  

1. 函数 (Functions)
  - **定义 definition**：函数 \(f: A \to B\)，将集合 A 中每个元素映射到集合 B 中唯一的元素。  
    - A：定义域 domain  
    - B：陪域 codomain  
    - f(A)：值域 range  
  - **图像 graph**：函数的图像是有序对集合 \(\{(a,f(a)) | a \in A\}\)。  
  - **函数性质**：  
    - 单射 injective (一一函数 one-to-one)：不同输入对应不同输出。  
    - 满射 surjective (onto)：陪域中每个元素都有原像。  
    - 双射 bijection：既是单射又是满射。  
    - 单调 monotonic：严格递增或严格递减。  
    - 反函数 inverse function：仅双射函数可逆，记作 \(f^{-1}\)。  
    - 复合函数 composition：\((f \circ g)(a) = f(g(a))\)。  

2. 特殊函数 (Special Functions)
  - **取整函数 floor function**：\(\lfloor x \rfloor\)，不大于 x 的最大整数。  
  - **上取整函数 ceiling function**：\(\lceil x \rceil\)，不小于 x 的最小整数。  
  - **性质 properties**：  
    - \(\lfloor -x \rfloor = -\lceil x \rceil\)，\(\lceil -x \rceil = -\lfloor x \rfloor\)。  
    - \(\lfloor x+m \rfloor = \lfloor x \rfloor + m\)，其中 m 为整数。  

3. 序列 (Sequences)
  - **定义 definition**：序列是从整数集合到某集合 S 的函数。  
  - **几何序列 geometric progression**：\(a, ar, ar^2, …\)。  
  - **算术序列 arithmetic progression**：\(a, a+d, a+2d, …\)。  
  - **常见序列**：平方数 \(n^2\)，立方数 \(n^3\)，阶乘 \(n!\)，幂序列 \(2^n, 3^n\)。  

4. 求和 (Summations)
  - **符号 notation**：\(\sum_{i=m}^n a_i\)。  
  - **常用公式**：  
    - \(\sum_{k=1}^n k = \frac{n(n+1)}{2}\)  
    - \(\sum_{k=1}^n k^2 = \frac{n(n+1)(2n+1)}{6}\)  
    - \(\sum_{k=1}^n k^3 = \left(\frac{n(n+1)}{2}\right)^2\)  
    - 等比数列求和：\(\sum_{k=0}^n ar^k = a\frac{r^{n+1}-1}{r-1}\)  

5. 集合的基数 (Cardinality of Sets)
  - **有限集 finite set**：元素个数有限。  
  - **无限集 infinite set**：元素无限。  
  - **基数相等 equal cardinality**：若存在双射 \(f: A \to B\)，则 \(|A|=|B|\)。  
  - **可数集 countable set**：有限或与正整数集合 \(\mathbb{Z}^+\) 等势。  
  - **不可数集 uncountable set**：如实数区间 (0,1)。  
  - **典型结果**：  
    - 正整数与偶数集合等势。  
    - 有理数集合是可数的。  
    - 实数集合是不可数的（康托对角线法 Cantor diagonalization）。  
    - 幂集的基数大于原集合。  

1. 函数完备性 (Functionally Complete)
  - **定义 definition**：若一组逻辑算子能表达所有复合命题，则称其为函数完备。  
  - 常见完备集合：  
    - {¬, ∧, ∨}  
    - {¬, ∨}  
    - {¬, ∧}  

2. 命题标准形式 (Propositional Normal Forms)
  - **文字 literal**：命题变量 \(p\) 或其否定 ¬p。  
  - **子句 clause**：文字的析取或合取。  
    - 析取子句 disjunctive clause：如 \(p ∨ ¬q\)。  
    - 合取子句 conjunctive clause：如 \(p ∧ ¬q\)。  

3. 合取范式 CNF (Conjunctive Normal Form)
  - **定义 definition**：若公式是若干析取子句的合取，则为 CNF。  
  - 例子：\((p ∨ ¬q) ∧ (¬p ∨ r)\)。  

4. 析取范式 DNF (Disjunctive Normal Form)
  - **定义 definition**：若公式是若干合取子句的析取，则为 DNF。  
  - 例子：\((p ∧ q) ∨ (¬p ∧ r)\)。  

5. 完全析取范式 Full DNF
  - **最小项 minterm**：每个变量都出现一次（正或负）的合取。  
  - **完全析取范式 full DNF**：公式写成所有使其为真的最小项的析取。  
  - 可通过真值表直接得到。  

6. 前束范式 Prenex Normal Form
  - **定义 definition**：谓词逻辑公式若所有量词都移到最前面，则为前束范式。  
  - 转化步骤：  
    1. 消除 → 和 ↔。  
    2. 将否定移到文字层面。  
    3. 必要时重命名变量。  
    4. 将量词前移。  

## chapter 6

1. **Sum Rule（加法原理）**: 如果任务 A 有 n<sub>1</sub> 种做法，任务 B 有 n<sub>2</sub> 种做法，且不能同时发生，则：
**Total = n1 + n2**
  - **Set version：**  若集合 S、T 不相交，则：**|S ∪ T| = |S| + |T|**

2. **Product Rule（乘法原理）**
如果任务 A 有 \(n_1\) 种做法，任务 B 在 A 完成后有 \(n_2\) 种做法，则：

\[
\text{Total} = n_1 \cdot n_2
\]

**Set version：**

\[
|S \times T| = |S| \cdot |T|
\]

---

### **3. Inclusion–Exclusion Principle（容斥原理）**
若 S、T 有交集：

\[
|S \cup T| = |S| + |T| - |S \cap T|
\]

---

### **4. Tree Diagrams（树形图）**
用于可视化所有可能选择路径。

---

# 📑 例题（6.1）

### **Example 1 — Sum Rule**
**Problem (English):**  
How many statement labels are possible if a label must be either a single letter or a single digit?

**中文解析：**  
字母 26 个，数字 10 个，互斥事件 → 加法原理。

**Solution (English):**  
\[
26 + 10 = 36
\]

---

### **Example 2 — Product Rule**
**Problem (English):**  
How many 10-bit strings begin and end with 1?

**中文解析：**  
第 1 位固定为 1，第 10 位固定为 1，中间 8 位自由（0 或 1）。  
每位 2 种 → 乘法原理。

**Solution (English):**  
\[
2^8 = 256
\]

---

### **Example 3 — Inclusion–Exclusion**
**Problem (English):**  
How many integers ≤ 100 are divisible by neither 4 nor 6?

**中文解析：**  
A：被 4 整除  
B：被 6 整除  
求 U − (A ∪ B)

**Solution (English):**  
\[
|A| = 25,\ |B| = 16,\ |A \cap B| = 8
\]
\[
100 - (25 + 16 - 8) = 67
\]


---

# 📘 Chapter 6.2 — The Pigeonhole Principle 抽屉原理

---

## 🟦 1. 基本抽屉原理（Pigeonhole Principle）

### **定义 Definition**
如果把 **k+1 个或更多对象（pigeons）** 放入 **k 个盒子（pigeonholes）**，  
那么至少有一个盒子里包含 **两个或更多对象**。

\[
\text{If } N > k,\ \text{then some box contains at least 2 objects.}
\]

### **等价函数表述**
若 \(f: A \to B\)，且 \(|A| > |B|\)，  
则必存在不同的 \(a_1 \neq a_2\) 使得：

\[
f(a_1) = f(a_2)
\]

→ **说明 f 不可能是单射（injective）**

---

## 🟦 2. 广义抽屉原理（Generalized Pigeonhole Principle）

### **定义 Definition**
如果把 **N 个对象** 放入 **k 个盒子**，  
则至少有一个盒子包含 **至少 \(\lceil N/k \rceil\) 个对象**。

\[
\exists\ \text{box with at least } \left\lceil \frac{N}{k} \right\rceil\ \text{objects}
\]

---

## 🟦 3. 抽屉原理的典型应用类型

### **(1) 生日问题 Birthday problem**
367 人 → 366 天 → 至少两人同生日。

### **(2) 同余类 Congruence classes**
11 个整数 → mod 10 有 10 个余数 → 至少两个同余。

### **(3) 社交网络 Friends problem**
在 n 人的聚会中，至少两人拥有相同数量的朋友。

### **(4) 数论应用**
在 \(n+1\) 个不超过 \(2n\) 的正整数中，必有一个整除另一个。

### **(5) 连续区间求和**
任意 n 个整数 → 存在连续子段和可被 n 整除。

### **(6) Ramsey Theory（拉姆齐理论）**
6 个人 → 必有 3 个互为朋友或 3 个互为敌人。

---

# 📑 例题（精选 3 题）

---

## **Example 1 — Basic Pigeonhole Principle**

**Problem (English):**  
Among any group of 367 people, show that at least two have the same birthday.

**中文解析：**  
一年最多 366 种生日（含闰年）。  
367 个人 → 366 个抽屉 → 至少两个同生日。

**Solution (English):**  
There are 366 possible birthdays.  
With 367 people, by the pigeonhole principle, at least two share the same birthday.

---

## **Example 2 — Generalized Pigeonhole Principle**

**Problem (English):**  
A bowl contains 10 red balls and 10 blue balls.  
How many balls must be selected to guarantee at least **3 balls of the same color**?

**中文解析：**  
两个抽屉：红、蓝。  
要保证至少 3 个同色 → 最坏情况：2 红 + 2 蓝 = 4  
再拿 1 个 → 必定形成 3 个同色。

**Solution (English):**  
Using the generalized pigeonhole principle:  
\[
\left\lceil \frac{5}{2} \right\rceil = 3
\]  
Thus selecting 5 balls guarantees at least 3 of the same color.

---

## **Example 3 — Elegant Number Theory Application**

**Problem (English):**  
Show that among any \(n+1\) integers chosen from \(\{1,2,\dots,2n\}\),  
one integer divides another.

**中文解析：**  
把每个数写成：  
\[
a_i = 2^{k_i} q_i,\quad q_i\ \text{为奇数}
\]  
奇数部分 \(q_i\) 只有 n 种可能（1,3,5,...,2n−1）。  
但我们选了 n+1 个数 → 至少两个有相同的奇数部分。  
较小的那个一定整除较大的。

**Solution (English):**  
Write each number as \(a_i = 2^{k_i} q_i\) with \(q_i\) odd.  
Since there are only n odd numbers ≤ 2n, two numbers share the same \(q\).  
The smaller one divides the larger one.

---

# 📘 6.2 总结一句话版

> **抽屉原理的核心思想：对象比容器多 → 必有容器装多个对象。  
> 广义版：平均数向上取整 → 至少有一个容器达到这个数量。**

它是计数论、数论、图论、拉姆齐理论的基础工具。


好的 Jun，我们继续 **6.3 Permutations & Combinations（排列与组合）** 的知识总结。  
依旧保持你要求的格式：  
- **关键术语中英双语**  
- **知识点结构化梳理**  
- **例题（英文题目 + 中文解析 + 英文解答）**  

---

# 📘 Chapter 6.3 — Permutations & Combinations 排列与组合

---

# 🟦 1. Permutations（排列）

### **定义 Definition**
排列 = **有序**（ordered）选取  
r-permutation = 从 n 个不同元素中选 r 个并排成顺序。

\[
P(n,r) = n(n-1)(n-2)\cdots(n-r+1) = \frac{n!}{(n-r)!}
\]

### **特殊情况**
- \(P(n,n) = n!\)  
- \(P(n,0) = 1\)

### **函数视角（非常重要）**
若 \(A=\{a_1,\dots,a_n\}\)，\(B=\{b_1,\dots,b_r\}\)，  
一个 **从 B 到 A 的单射（injective function）** 就对应一个 r-permutation。

---

# 🟦 2. Combinations（组合）

### **定义 Definition**
组合 = **无序**（unordered）选取  
r-combination = 从 n 个不同元素中选 r 个，不考虑顺序。

\[
C(n,r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}
\]

### **组合与排列关系**
\[
P(n,r) = C(n,r)\cdot r!
\]

因为：先选 r 个（无序），再对这 r 个排成 r! 种顺序。

---

# 🟦 3. 组合的函数视角（bit-string representation）

将集合 A 的 r-combination 对应到函数：

\[
f: A \to \{0,1\}
\]

其中恰好 r 个元素映射到 1。  
→ 这就是组合的另一种计数方式。

---

# 🟦 4. 组合恒等式（Corollary）

### **对称性 Symmetry**
\[
C(n,r) = C(n,n-r)
\]

**解释：**选 r 个元素 = 不选 n−r 个元素。

---

# 📑 例题（精选 3 题）

---

## **Example 1 — Permutation**

**Problem (English):**  
In how many ways can we select 3 students from 5 to stand in line?

**中文解析：**  
选 3 个并排成顺序 → 排列  
\[
P(5,3) = 5\cdot4\cdot3
\]

**Solution (English):**  
\[
P(5,3) = \frac{5!}{2!} = 60
\]

---

## **Example 2 — Combination**

**Problem (English):**  
A soccer club has 8 female and 7 male members.  
How many ways to choose **6 females and 5 males**?

**中文解析：**  
女生选 6，男生选 5 → 组合相乘。

**Solution (English):**  
\[
C(8,6)\cdot C(7,5) = 28\cdot 21 = 588
\]

---

## **Example 3 — Combination with constraints**

**Problem (English):**  
How many ways to choose 11 players with **at most 5 males**?

**中文解析：**  
男生数量可能是 0,1,2,3,4,5  
但必须满足总人数 11  
→ 逐项列出：

- 5 男 → 6 女  
- 4 男 → 7 女  
- 3 男 → 8 女  

**Solution (English):**  
\[
C(8,6)C(7,5) + C(8,7)C(7,4) + C(8,8)C(7,3)
\]

---

# 📘 6.3 总结一句话版

> **排列看顺序，组合不看顺序；排列 = 组合 × r!；  
> 组合的本质是从 n 个元素中选 r 个子集。**

---

如果你准备好了，我继续总结 **6.4 Binomial Coefficients（二项式系数）**。

好的 Jun，我们继续 **6.4 Binomial Coefficients（二项式系数）** 的知识总结。  
依旧保持你要求的结构：  
- **关键术语中英双语**  
- **知识点结构化梳理**  
- **例题（英文题目 + 中文解析 + 英文解答）**  

---

# 📘 Chapter 6.4 — Binomial Coefficients 二项式系数

---

# 🟦 1. Binomial Theorem（二项式定理）

### **定理 Theorem**
对任意变量 \(x, y\) 和非负整数 \(n\)，有：

\[
(x+y)^n = \sum_{j=0}^{n} \binom{n}{j} x^{n-j} y^j
\]

其中：

\[
\binom{n}{j} = \frac{n!}{j!(n-j)!}
\]

### **含义**
- 展开 \((x+y)^n\) 时，每一项的系数就是组合数 \(\binom{n}{j}\)  
- 组合意义：从 n 个位置中选 j 个放 y，其余放 x

---

# 🟦 2. 组合恒等式（Corollaries）

---

## **Corollary 1：组合数求和**

\[
\sum_{k=0}^{n} \binom{n}{k} = 2^n
\]

**解释：**  
\((1+1)^n = 2^n\)  
展开后每项系数之和就是左边。

---

## **Corollary 2：交替求和**

\[
\sum_{k=0}^{n} (-1)^k \binom{n}{k} = 0
\]

**解释：**  
\((1-1)^n = 0\)

---

## **Corollary 3：带权求和**

\[
\sum_{k=0}^{n} 2^k \binom{n}{k} = 3^n
\]

**解释：**  
\((1+2)^n = 3^n\)

---

# 🟦 3. Pascal’s Identity（帕斯卡恒等式）

### **定理 Theorem**
\[
\binom{n+1}{k} = \binom{n}{k} + \binom{n}{k-1}
\]

### **组合意义**
从 \(n+1\) 个元素中选 k 个：

- 不选第一个元素 → 从剩下 n 个选 k 个  
- 选第一个元素 → 从剩下 n 个选 k−1 个  

两者相加。

---

# 🟦 4. Pascal’s Triangle（帕斯卡三角形）

每个数 = 左上 + 右上  
用于快速查组合数。

---

# 🟦 5. Vandermonde’s Identity（范德蒙德恒等式）

### **定理 Theorem**
\[
\binom{m+n}{r} = \sum_{k=0}^{r} \binom{m}{r-k} \binom{n}{k}
\]

### **组合意义**
从两个不相交集合 A（m 个）和 B（n 个）中选 r 个：

- 从 A 选 r−k 个  
- 从 B 选 k 个  
- 对所有 k 求和

---

# 🟦 6. 重要推论：中心二项式系数

\[
\binom{2n}{n} = \sum_{k=0}^{n} \binom{n}{k}^2
\]

这是 Vandermonde 恒等式在 \(m=n, r=n\) 时的特例。

---

# 📑 例题（精选 3 题）

---

## **Example 1 — Coefficient in Binomial Expansion**

**Problem (English):**  
What is the coefficient of \(x^{12} y^{13}\) in \((2x - 3y)^{25}\)?

**中文解析：**  
要得到 \(x^{12} y^{13}\)，必须选 12 次 \(2x\)，13 次 \(-3y\)。  
对应组合数 \(\binom{25}{13}\)。

**Solution (English):**  
\[
\binom{25}{13} (2)^{12} (-3)^{13}
\]

---

## **Example 2 — Sum of Binomial Coefficients**

**Problem (English):**  
Compute \(\sum_{k=0}^{n} \binom{n}{k}\).

**中文解析：**  
直接使用二项式定理：  
\((1+1)^n = 2^n\)

**Solution (English):**  
\[
\sum_{k=0}^{n} \binom{n}{k} = 2^n
\]

---

## **Example 3 — Pascal’s Identity**

**Problem (English):**  
Show that \(\binom{n+1}{k} = \binom{n}{k} + \binom{n}{k-1}\).

**中文解析：**  
组合意义证明：  
从 \(n+1\) 个元素中选 k 个：  
- 不选第一个 → \(\binom{n}{k}\)  
- 选第一个 → \(\binom{n}{k-1}\)

**Solution (English):**  
\[
\binom{n+1}{k} = \binom{n}{k} + \binom{n}{k-1}
\]

---

# 📘 6.4 总结一句话版

> **二项式系数是组合数的核心，所有恒等式都来自“从 n 个元素中选 k 个”的不同计数方式。  
> 二项式定理是展开 \((x+y)^n\) 的基础工具。**

---

如果你准备好了，我可以继续总结 **6.5 Generalized Permutations and Combinations（广义排列组合）** 或 **6.6 Generating Permutations and Combinations**。

好的 Jun，我们来总结 **Chapter 8.1–8.2 Advanced Counting Techniques（递推关系与解法）**。依旧保持你喜欢的结构：  
- **关键术语中英双语**  
- **知识点结构化梳理**  
- **例题（英文题目 + 中文解析 + 英文解答）**  

---

# 📘 Chapter 8.1 Applications of Recurrence Relations 递推关系的应用

---

## 🟦 1. Recurrence Relation（递推关系）
- **定义 Definition**：递推关系是一个公式，用来表示序列的某一项 \(a_n\) 与前面若干项的关系。  
  \[
  a_n = f(a_0, a_1, \dots, a_{n-1})
  \]
- **解 solution**：满足递推关系的序列。  
- **阶 degree**：依赖的前项个数。例如 Fibonacci 是二阶。  

---

## 🟦 2. 常见例子
- **Fibonacci 数列**：\(F_n = F_{n-1} + F_{n-2}, F_0=0, F_1=1\)。  
- **Pascal 恒等式**：\(\binom{n+1}{k} = \binom{n}{k} + \binom{n}{k-1}\)。  
- **汉诺塔 Tower of Hanoi**：\(H_n = 2H_{n-1} + 1\)。  
- **无连续 0 的二进制串**：\(a_n = a_{n-1} + a_{n-2}\)。  

---

## 📑 例题（8.1）

### Example 1 — Tower of Hanoi
**Problem (English):**  
Find a recurrence relation for the minimum moves to solve Tower of Hanoi with n disks.

**中文解析：**  
每次移动 n−1 个盘子到辅助柱，再移动最大盘，再移动 n−1 个盘子。  
所以递推关系：\(H_n = 2H_{n-1} + 1\)。

**Solution (English):**  
\[
H_n = 2H_{n-1} + 1,\quad H_1=1
\]  
Explicit solution: \(H_n = 2^n - 1\).

---

---

# 📘 Chapter 8.2 Solving Linear Recurrence Relations 线性递推关系的解法

---

## 🟦 1. Linear Homogeneous Recurrence Relation（线性齐次递推）
- **形式**：
  \[
  a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k}
  \]
- **特征方程 characteristic equation**：
  \[
  r^k - c_1 r^{k-1} - c_2 r^{k-2} - \dots - c_k = 0
  \]
- **解 solution**：若根为 \(r_1, r_2, \dots, r_k\)，则通解为：
  \[
  a_n = A_1 r_1^n + A_2 r_2^n + \dots + A_k r_k^n
  \]
  系数由初始条件决定。

---

## 🟦 2. 特殊情况
- **重根 repeated root**：若根有重数 m，则解中需乘以多项式因子 \(n^{j}\)。  
- **非齐次 nonhomogeneous**：解 = 齐次解 + 特殊解。  

---

## 📑 例题（8.2）

### Example 2 — Fibonacci Numbers
**Problem (English):**  
Find an explicit formula for Fibonacci sequence \(F_n = F_{n-1} + F_{n-2}, F_0=0, F_1=1\).

**中文解析：**  
特征方程：\(r^2 - r - 1 = 0\)。  
根：\(\frac{1 \pm \sqrt{5}}{2}\)。  
通解：\(F_n = A(\frac{1+\sqrt{5}}{2})^n + B(\frac{1-\sqrt{5}}{2})^n\)。  
用初始条件解出 A, B。

**Solution (English):**  
\[
F_n = \frac{1}{\sqrt{5}}\left[\left(\frac{1+\sqrt{5}}{2}\right)^n - \left(\frac{1-\sqrt{5}}{2}\right)^n\right]
\]  
This is Binet’s Formula.

---

### Example 3 — Simple Recurrence
**Problem (English):**  
Solve \(a_n = 3a_{n-1}, a_0=4\).

**中文解析：**  
特征方程：\(r-3=0\)，根为 3。  
解：\(a_n = C \cdot 3^n\)。  
初始条件：\(a_0=4\) → \(C=4\)。

**Solution (English):**  
\[
a_n = 4 \cdot 3^n
\]

---

# 📘 总结一句话版

> **8.1：递推关系是用前项定义后项的公式，常见于 Fibonacci、汉诺塔等问题。  
> 8.2：解线性递推关系的核心是特征方程，解的形式由根决定，非齐次情况需加上特殊解。**

---

下一步我可以帮你总结 **8.3 Divide-and-Conquer Algorithms and Recurrence Relations（分治算法与递推）**，要不要继续？

对，你确实发过 **6.5 Generalized Permutations and Combinations** 的 PPT，我刚才跳到第八章有点快了。我们现在来总结 **6.5**，保持之前的结构。  

---

# 📘 Chapter 6.5 — Generalized Permutations and Combinations 广义排列与组合

---

## 🟦 1. Permutations with Repetition（允许重复的排列）
- **定义 Definition**：从 n 个元素中选 r 个，允许重复，顺序重要。  
- **公式 Formula**：  
  \[
  P_{rep}(n,r) = n^r
  \]  
- **例子 Example**：长度为 10 的字母串 → \(26^{10}\)。

---

## 🟦 2. Permutations with Indistinguishable Objects（不可区分元素的排列）
- **定义 Definition**：若 n 个元素中有 \(n_1\) 个同类、\(n_2\) 个同类…，总数为 n。  
- **公式 Formula**：  
  \[
  \frac{n!}{n_1! n_2! \dots n_k!}
  \]  
- **例子 Example**：单词 **MISSISSIPPI** 的不同排列数：  
  \[
  \frac{11!}{4!4!2!1!}
  \]

---

## 🟦 3. Circular Permutations（环排列）
- **定义 Definition**：环形排列中，旋转视为相同。  
- **公式 Formula**：  
  \[
  \frac{P(n,r)}{r}
  \]  
- **例子 Example**：7 人围坐 → \(\frac{7!}{7} = 6!\)。

---

## 🟦 4. Combinations with Repetition（允许重复的组合）
- **定义 Definition**：从 n 种元素中选 r 个，允许重复，顺序不重要。  
- **公式 Formula**：  
  \[
  C_{rep}(n,r) = \binom{n+r-1}{r}
  \]  
- **解释 Explanation**：Stars and Bars 方法：r 个星星 + (n−1) 个隔板。

---

## 🟦 5. Distributing Objects into Boxes（分配问题）
- **情况分类**：  
  1. 可区分对象 + 可区分盒子 → 多项式系数。  
  2. 可区分对象 + 不可区分盒子 → Stirling 数。  
  3. 不可区分对象 + 可区分盒子 → Stars and Bars。  
  4. 不可区分对象 + 不可区分盒子 → 枚举或分拆问题。  

---

## 🟦 6. Stirling Numbers of the Second Kind（二类斯特林数）
- **定义 Definition**：把 n 个可区分对象分到 j 个不可区分盒子（每盒非空）的方式数。  
- **记号 Notation**：\(S(n,j)\)。  
- **例子 Example**：把 4 个员工分到 3 个不可区分办公室 → \(S(4,3)=6\)。

---

# 📑 例题（精选 3 题）

---

## **Example 1 — Permutation with Repetition**
**Problem (English):**  
How many bit strings of length 10?  
**中文解析：** 每位可选 0 或 1，共 2 种。长度 10 → \(2^{10}\)。  
**Solution (English):**  
\[
2^{10} = 1024
\]

---

## **Example 2 — Indistinguishable Objects**
**Problem (English):**  
How many distinct strings can be formed from MISSISSIPPI?  
**中文解析：** 总长 11，字母频率：M=1, I=4, S=4, P=2。  
**Solution (English):**  
\[
\frac{11!}{1! \cdot 4! \cdot 4! \cdot 2!} = 34,650
\]

---

## **Example 3 — Combination with Repetition**
**Problem (English):**  
How many solutions to \(x_1+x_2+x_3+x_4=16\) with nonnegative integers?  
**中文解析：** Stars and Bars：4 种变量，16 个星星。  
**Solution (English):**  
\[
C(16+4-1,16) = C(19,16) = C(19,3) = 969
\]

---

# 📘 6.5 总结一句话版

> **广义排列组合扩展了经典公式：允许重复、不可区分元素、环排列、分配问题。核心方法是 Stars and Bars 与 Stirling 数。**

---

下一步我可以帮你总结 **6.6 Generating Permutations and Combinations（生成排列与组合）**，要不要继续？

好的 Jun，我们依次来总结 **8.3 Divide-and-Conquer Algorithms and Recurrence Relations**、**8.4 Generating Functions**、**8.5 Inclusion-Exclusion**，保持你喜欢的结构：  
- **关键术语中英双语**  
- **知识点结构化梳理**  
- **例题（英文题目 + 中文解析 + 英文解答）**  

---

# 📘 8.3 Divide-and-Conquer Algorithms and Recurrence Relations 分治算法与递推关系

---

## 🟦 1. Divide-and-Conquer Paradigm（分治范式）
- **定义 Definition**：将问题拆分为规模更小的子问题，分别解决，再合并结果。  
- **典型例子 Examples**：  
  - Binary Search（二分查找）  
  - Merge Sort（归并排序）

---

## 🟦 2. Divide-and-Conquer Recurrence（分治递推）
- 一般形式：  
  \[
  f(n) = a f\left(\frac{n}{b}\right) + g(n)
  \]  
  - a：子问题个数  
  - b：划分比例  
  - g(n)：合并时的额外操作

---

## 🟦 3. Master Theorem（主定理）
- 若 \(f(n) = a f(n/b) + c n^d\)，则：
  - 若 \(a < b^d\)，则 \(f(n) = O(n^d)\)  
  - 若 \(a = b^d\)，则 \(f(n) = O(n^d \log n)\)  
  - 若 \(a > b^d\)，则 \(f(n) = O(n^{\log_b a})\)

---

## 📑 例题（8.3）

### Example 1 — Binary Search
**Problem (English):**  
Give a big-O estimate for comparisons in binary search.  
**中文解析：**  
递推关系：\(f(n) = f(n/2) + 2\)。  
**Solution (English):**  
By Master Theorem, \(f(n) = O(\log n)\).

---

### Example 2 — Merge Sort
**Problem (English):**  
Give a big-O estimate for comparisons in merge sort.  
**中文解析：**  
递推关系：\(M(n) = 2M(n/2) + n\)。  
**Solution (English):**  
By Master Theorem, \(M(n) = O(n \log n)\).

---

# 📘 8.4 Generating Functions 生成函数

---

## 🟦 1. Generating Function（生成函数）
- **定义 Definition**：序列 \(\{a_k\}\) 的生成函数：  
  \[
  G(x) = \sum_{k=0}^{\infty} a_k x^k
  \]  
- **用途 Uses**：  
  - 解决计数问题  
  - 解递推关系  
  - 证明组合恒等式

---

## 🟦 2. 常见生成函数
- 序列 \(1,1,1,\dots\)：\(\frac{1}{1-x}\)  
- 序列 \(0,1,2,3,\dots\)：\(\frac{x}{(1-x)^2}\)  
- 序列 \(k^2\)：\(\frac{x(1+x)}{(1-x)^3}\)

---

## 🟦 3. Extended Binomial Theorem（扩展二项式定理）
\[
(1+x)^u = \sum_{k=0}^{\infty} \binom{u}{k} x^k
\]
其中 \(\binom{u}{k}\) 可推广到实数 u。

---

## 📑 例题（8.4）

### Example 1 — r-combinations with repetition
**Problem (English):**  
Use generating functions to count r-combinations from n elements with repetition.  
**中文解析：**  
每个元素可选 0 次、1 次… → 生成函数 \((1+x+x^2+\dots)^n = \frac{1}{(1-x)^n}\)。  
**Solution (English):**  
Coefficient of \(x^r\) is \(\binom{n+r-1}{r}\).

---

### Example 2 — Restricted integer solutions
**Problem (English):**  
Find number of solutions to \(x_1+x_2+x_3=17\) with \(2\le x_1\le5, 3\le x_2\le6, 4\le x_3\le7\).  
**中文解析：**  
生成函数：\((x^2+x^3+x^4+x^5)(x^3+x^4+x^5+x^6)(x^4+x^5+x^6+x^7)\)。  
**Solution (English):**  
Coefficient of \(x^{17}\) in expansion gives the answer.

---

# 📘 8.5 Inclusion-Exclusion Principle 容斥原理

---

## 🟦 1. 基本公式
\[
|A \cup B| = |A| + |B| - |A \cap B|
\]

推广到 n 个集合：
\[
|A_1 \cup \dots \cup A_n| = \sum |A_i| - \sum |A_i \cap A_j| + \sum |A_i \cap A_j \cap A_k| - \dots
\]

---

## 🟦 2. 应用场景
- 计算不满足某些条件的元素数目  
- 排列/组合中避免某些模式  
- 数论中排除倍数

---

## 📑 例题（8.5）

### Example 1 — Students in courses
**Problem (English):**  
Out of 1807 freshmen, 453 take CS, 567 take Math, 299 take both. How many take neither?  
**中文解析：**  
\(|U|=1807\)，\(|A|=453\)，\(|B|=567\)，\(|A\cap B|=299\)。  
**Solution (English):**  
\[
1807 - (453+567-299) = 1086
\]

---

### Example 2 — Integers not divisible
**Problem (English):**  
How many integers ≤1000 not divisible by 5,6,8?  
**中文解析：**  
用容斥：总数 1000 − (被 5 整除 + 被 6 整除 + 被 8 整除 − 两两交集 + 三者交集)。  
**Solution (English):**  
Answer = 600.

---

### Example 3 — Permutations avoiding substrings
**Problem (English):**  
How many permutations of 26 letters avoid “fish”, “rat”, “bird”?  
**中文解析：**  
总数 26! 减去包含这些子串的排列数，用容斥。  
**Solution (English):**  
\[
26! - (23!+24!+23! - 21!)
\]

---

# 📘 总结一句话版

- **8.3 分治算法**：递推关系形式 \(f(n)=af(n/b)+g(n)\)，用 Master Theorem 得到复杂度。  
- **8.4 生成函数**：把序列编码成幂级数，解决组合计数与递推。  
- **8.5 容斥原理**：通过加减交集来避免重复计数，是处理“至少/不包含”问题的核心工具。  

---

要不要我接着帮你总结 **8.6 Applications of Inclusion-Exclusion（容斥原理的应用）**？