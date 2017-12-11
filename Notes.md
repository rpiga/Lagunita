Algorithms: Design and Analysis 1
===============================================

*Stanford University, https://lagunita.stanford.edu/*

Introduction
------------

### Integer Multiplication

- Multiplication is an example of an algorithm
- Define a computational problem:

```markdown
INPUT: two n-digits numbers x & y
OUTPUT: product x*y
PRIMITIVE OPERATIONS: add or multiply 2 single digit numbers.
```

- To define the Algorithm for the multiplication, we should focus on the number of basic operations performed as function of the number of digits ( $$ n $$ )
- Analysis of the example:

```markdown
  5678
x 1234
```

* First partial product

- Basic operations performed 4 times plus carries

```markdown
4
x 5678
```

  - Repeat for the remaining digits
  - In total, worst case where every digit will consist in in a multiplication plus a carry, a partial product will last $$\leq 2 \times n$$ basic operations.
  - This is repeated $$n$$ times (rows)
  - Total of operations for all partial products then is $$2 \times n^2$$
  - Then we have to sum all the partial products. This could result approximately in another $$2 \times n^2$$ operations.
  - Total should be $$k \times n^2​$$ where $$k \approx 4​$$ (it’s a rough number)

### Karatsuba Multiplication

- With same example as before, let’s check a different approach in order to understand if there’s a more effective way to perform the calculation

```markdown
  5678
x 1234
```

- Consider the digits split in halves

```markdown
A = 56    B = 78
C = 12    D = 34
```

- Then perform the following calculations

```markdown
R1 = A * C = 672
R2 = B * D = 2652
R3 = (A+B) * (C+D) = 6164
```

- Apply a simil Gauss’s multiplication trick

```markdown
R4 = R3 - R1 - R2 = 2840
```

- At this point will perform the following addition
  $$(R1*10^n) + R2 + (R4*10^{\frac{n}{2}}) = TOTAL$$
- This new Algorithm is more efficient because the number of operations performed is less than for traditional multiplication method.

- Perform a further analysis
  - From example above, with $$n$$ is the number of digits
    $$x = 10^{\frac{n}{2}}A + B$$
    $$y = 10^{\frac{n}{2}}C + D$$
  - So, I’ll obtain the base case:
    $$x*y = (10^{\frac{n}{2}}A + B) * (10^{\frac{n}{2}}C + D)$$
      $$= 10^nAC + 10^{\frac{n}{2}}(AD + BC) + BD$$
  - This is a 4 multiplication (AC, AD, BC, BD) plus some additions

- Is there, at this point, a way to further optimize and perform 3 recursive calculations instead of 4?
  - Recall the base case: $$x*y = 10^nAC + 10^{\frac{n}{2}}(AD + BC) + BD$$

```markdown
    1. Compute AC
    2. Compute BD
    3. Compute (A+B) * (C+D) = AC + AD + BC + BD
    4. Apply Gauss’s Multiplication optimization (AC + AD + BC + BD) - AC - BD
    5. Obtain = (AD+BC)
```

- At the end we’ll have 3 recursive multiplications and some additions
  $$x*y = 10^nAC + 10^{\frac{n}{2}}(A+B)*(C+D) + BD$$

### Merge Sort Algorithm
- Why start with Merge Sort?
  - It’s a good introduction to the **Divide & Conquer** algorithm strategy
  - It improves over **Selection**, **Insertion**, **Bubble sorts** ($$n^2$$ algorithms)
  - It provides guiding principles for algorithm analysis
    - worst case & asymptotic analysis
- Sorting problem:

```markdown
INPUT: array of n-items unsorted
OUTPUT: same items, increasing order
ASSUME: numbers are distinct, no duplicates

        |5|4|1|8|7|2|6|3|
            /        \
    |5|4|1|8|       |7|2|6|3|
        |   recursive   |
    |1|4|5|8|       |2|3|6|7|
         \     merge   /
        |1|2|3|4|5|6|7|8|
```

**Pseudocode**

- In order to obtain the result we have to
- Recursively sort 1st half
- Recursively sort 2nd half
- merge the two sorted sublists

```markdown
C = output array [length n]
A = 1st array sorted [length n/2]
B = 2nd array sorted [length n/2]

i = 1 # iterator for array A
j = 1 # iterator for array B

for k = 1 to n  **#1**
  if A(i) < B(j)  **#2a**
    C(k) = A(i)  **#3a**
    i++  **#4a**
  else if B(j) < A(i)  **#2b**
    C(k) = B(j)  **#3b**
    j++  **#4b**
```

- What is the Merge Sort running time?
  - What is the running time for an array of n elements?
- Let’s count the operations
  - For each N we perform 4 operations (#1, #2a/b, …)
  - For an array of M elements, the total number will be $$\leq4m+2\approx6m$$
- We claim that Merge Sort will take $$6n*\log_2 n + 6n$$
  - Where log represent the *number of times we divide n by 2 to reach 1 as result*

**Analysis**

- Proof of chain
  - Assume n is a power of 2
  - Use of “recursion tree”

```markdown
    level 0          root     X
                             / \
    level 1                X     X
                          / \   / \
    level 2              X   X X   X
```

  - Number of levels will be $$\log_2 n + 1$$ (recursive layers plus root)
  - Base case is the simple element of the array
- each sub-level will have $$2^{level}$$ sub-problems with $$n/2^j$$ elements each
- So total numbers of operations at level j
  - $$\leq2^j * 6n$$  [6n as from previous demonstration]
    $$\leq 2^j * 6 * n/2^j$$  [number of subproblems * number of elements in each subproblem]
    $$\leq 6n$$  [number of operations for just one level]
- Total operations for all levels
  - $$\leq 6n * (\log_2 n + 1)$$  [number of operations in one level * number of levels]
    $$\leq 6n*\log_2 n + 6n$$  [as we claim above]


