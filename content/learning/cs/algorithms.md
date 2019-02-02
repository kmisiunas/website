---
title: "Learning algorithms"
date: 2018-10-02
draft: false
typora-root-url: ../../../static
toc: true
---

# Backtracking [1]

Backtracking is a technique used to build up to a solution to a  problem incrementally. These "partial solutions" can be phrased in terms  of a sequence of decisions. Once it is determined that a "partial  solution" is not viable (i.e. no subsequent decision can convert it into  a solution) then the backtracking algorithm retraces its step to the  last viable "partial solution" and tries again.

Visualizing the decisions as a tree, backtracking has many of the  same properties of depth-first search. The difference is that  depth-first search will guarantee that it visits every node until it  finds a solution, whereas backtracking doesn't visit branches that are  not viable.

Because backtracking breaks problems into smaller subproblems, it is  often combined with dynamic programming, or a divide-and-conquer  approach.

### Concepts

- **State:**  A "partial solution" to the problem.
- **Rejection criterion:** A function that rejects a  partial solution as "unrecoverable". i. e. no sequence of subsequent  decisions can turn this state into a solution. Without rejection  criteria, backtracking is the same as depth-first search.
- **Viable:** A state that may still lead to a solution.  This reflects what is known "at this stage". All states that fail the  rejection criteria are not viable. If we have a state that is initially  viable, and find that all paths to leaves eventually terminate at a  state that fail the rejection criteria, the state will become  non-viable.
- **Heuristic:** Backtracking will (eventually) look at  all the different viable nodes by recursively applying all the possible  decisions. A heuristic is a quick way of ordering which decisions are  likely to be the best ones at each stage, so that they get evaluated  first. The goal is to find a solution early (if one exists).
- **Pruning:** The concept of determining that there are no nodes / states along a particular branch, so it is not worth visiting those nodes.

### Generalised Algorithm

In the algorithms below, it is assumed that each decision is  irreversible, so there is only one path to each state. If modeling a  game like chess, you would have to be more careful, as it is possible to  arrive at the same state multiple different ways. If it is possible to  reach each state multiple ways, then we would also need to keep track of  which states we had already visited.

A recursive implementation of a backtracking algorithm takes the general form

```python
def doBacktrack( current ):
    if current is a solution:
        return current
    for each decision d from current:
        new_state <- state obtained from current by making decision d
        if new_state is viable:
            sol <- doBacktrack(new_state)
            if sol is not None:
                return sol
    # indicate there is no solution
    return None    
```

Iterative solution

```python
def doBacktrackIterative(start):
    stack = [start] 

    while len(stack) > 0:
        current = stack.pop()
        
        if current is a solution:
            yield current

        for decision in possible_decisions(current):
            new_state <- state obtained from current by making decision d
            if is_viable(new_state):
                stack.push(new_state)
```

### Example: n queens

**Placing n queens on an n x n chess board so no queen can be taken**

 We know that we need one queen in every row, and one queen in every column.

- *Initial state:* An empty board
- *The decision:* The decision at stage *i* is which column to put the queen on row *i* into.
- *Rejection criterion:* Reject all configurations that do not  allow any queen on any of the lower rows. (Adding more queens to the  board will not prevent this row from being blocked.)

Here are the states that the backtracking algorithm explores for a 4x4 board. Note that naively there are 4! = 24 *complete configurations*  with 4 queens on the board, if we place the queens row-by-row, and  avoid reusing a column that is already used. With backtracking, many  configurations are eliminated early. This algorithm shows *all* solutions, instead of stopping at the first one found.




![Example of backtracking](/images/2018/backtracking_example.png)

```python
# solution

def _is_solution(current, n) -> "bool":
    if len(current) != n: return False
    return _is_viable(current, n)

def _is_viable(current, n) -> "bool":
    "for each placed queen check if it hits another"
    # by design we do not need to check columns 
    # rows
    if len(current) != len(set(current)):
        return False
    # diagonals 
    for i, pos in enumerate(current):
        for j in range(len(current)):
            if i != j:
                if current[j] == pos + (j-i): return False
                if current[j] == pos - (j-i): return False
    return True
    
def _possible_decisions(current, n) -> "list":
    return list(range(1,n+1))

def _find_queen_positions(n):
    "generator"
    stack = [[]]
    while len(stack)>0:
        current = stack.pop()
        if _is_solution(current, n):
            yield current  
        for d in _possible_decisions(current, n):
            new_state = current.copy()
            new_state.append(d)
            if _is_viable(new_state, n):
                stack.append(new_state)


def nQueens(n):
    if n == 0: return [[]]
    if n == 1: return [[1]]
    return sorted( list( _find_queen_positions(n) ) )
```

### Example: sum subsets

Given a sorted array of integers `arr` and an integer `num`, find all possible unique subsets of `arr` that add up to `num`. Both the array of subsets and the subsets themselves should be sorted in [lexicographical order](keyword://lexicographical-order-for-arrays).

```python
def sumSubsets(arr, num):
    result = set()
    
    def addSumSubsets(arr_i, target, subset):
        if target == 0:
            result.add(subset)
        elif arr_i >= len(arr) or target < 0:
            return
        else:
            n = arr[arr_i]
            addSumSubsets(arr_i+1, target-n, subset+(n,))
            addSumSubsets(arr_i+1, target, subset)
    
    addSumSubsets(0, num, ())
    return sorted(list(result))
```

# Dynamic Programming [2]

Break down problem into reoccurring smaller problems. Then save results for the reoccurring problem, so it would not be required to solve the same problem more than once. Two common approaches include: 

- **Bottom-up strategy (iteration):** In this strategy,  you start solving from the smallest subproblem up towards the original  problem. If you’re doing this, you’ll solve all of the subproblems,  thereby solving the larger problem. The cache in bottom-up solutions is usually an array (either one or multi-dimensional).
- **Top-down strategy (recursion):** Start solving the identified subproblems. If a subproblem hasn’t been solved yet, solve it  and cache the answer; if it has been solved, return the cached answer.  This is called *memoization*. The cache in top-down solutions is usually a hash table, binary search tree, or other dynamic data structure.

## Generalised algorithm 

1. Find a recursive or iterative solution to the problem first (although usually these will have bad run times).
2. This helps you identify the subproblems, which are “smaller” instances of the original problem.
3. Determine whether the subproblems are overlapping. If they’re  overlapping, a recursive algorithm will be solving the same subproblem  over and over. (If they’re not overlapping and the recursive algorithm  generates new subproblems each time it runs, DP isn’t a good solution;  try a divide-and-conquer solution like merge sort or quick sort  instead.)
4. Store/cache the results for every subproblem.
5. Once the subproblems have solutions, the original problem is easy to solve!



## Example: filling blocks

You have a block with the dimensions `4 × n`. Find the number of different ways you can fill this block with smaller blocks that have the dimensions `1 × 2`. You are allowed to rotate the smaller blocks. 

For `n = 1`, the output should be `fillingBlocks(n) = 1`. For `n = 4`, the output should be `fillingBlocks(n) = 36`.



The reoccurring subproblem is along the `n` axis. For each row there are number of combinations ($2^4=16$ in total). We can compute all the possible transitions using the following code

```python
def state_to_int(state):
    out = 0
    for i, x in enumerate(state[::-1]):
        out += x * 2**i
    return out 

def from_int_to_state(x):
    out = tuple()
    for i in range(4):
        out = out + (x % 2 ,)
        x = x // 2
    return out 

def find_options(state) -> "list of states":
    out = []
    stack = [tuple()]
    while len(stack) > 0:
        current = stack.pop()
        if len(current) == 4: 
            out.append(current)
        else:
            i = len(current)
            if state[i]:
                stack.append(current + (0,))
            else:
                stack.append(current + (1,))
                if len(current) < 3 and state[i+1] == 0:
                    stack.append(current + (0,0,))
    return out     

def find_vec_transitions() -> "dict[int: list[int]]":
    out = {}
    for i in range(16):
        res = find_options( from_int_to_state(i) )
        out[i] = list(map(state_to_int, res))
    return out
```

Where the output gives us the transition from layer `n` to `n+1`:

```python
find_vec_transitions()
{0: [0, 3, 9, 12, 15], 
 1: [1, 4, 7], 
 2: [8, 11], 
 3: [0, 3], 
 4: [1, 13], 
 5: [5], 
 6: [9], 
 7: [1], 
 8: [2, 8, 14], 
 9: [0, 6], 
 10: [10], 
 11: [2], 
 12: [0, 12], 
 13: [4], 
 14: [8], 
 15: [0]  }
```

Observe that a lot of the states are unreachable. There are only 6 states that can be reached with a start at `0` state:  `{0, 3, 6, 9, 12, 15}`. Not need to count the states. 




# References 

1. https://app.codesignal.com/interview-practice/topics/backtracking/tutorial
2. https://app.codesignal.com/interview-practice/topics/dynamic-programming-basic/tutorial