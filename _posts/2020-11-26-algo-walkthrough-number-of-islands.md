---
ID: 9
post_title: Algo walkthrough, number of islands
author: sprajagopal
post_excerpt: ""
layout: post
permalink: >
  https://notes-3dfd54.ingress-baronn.easywp.com/algo-walkthrough-number-of-islands/
published: true
post_date: 2020-11-26 06:20:17
---
## [The problem](https://leetcode.com/problems/number-of-islands/) {#the-problem}

> Given an m x n 2d grid map of '1's (land) and '0's (water), return the number of islands.
> An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.


## Exploration {#exploration}

The first instinct is to lower the dimension of the problem and see what that looks like. A 1D world with stretches of land and water:

|   |   |   |   |   |   |   |   |   |   |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 0 | 0 | 0 | 1 | 1 | 1 | 0 | 0 | 1 |

In this case, there are 3 islands. We can count this in a single pass of the array. Every time we encounter land, we increment our `total_islands` variable. After this we skip all the land blocks until we hit water (or end of array). After the single pass `total_islands` variable has the correct count.


### Counting = naming {#counting-naming}

When we count islands, we are actually naming the blocks of the island with a single name.

| blocks | 1 | 0 | 0 | 0 | 1 | 1 | 1 | 0 | 0 | 1 |
|--------|---|---|---|---|---|---|---|---|---|---|
| name   | 1 | 0 | 0 | 0 | 2 | 2 | 2 | 0 | 0 | 3 |

The total number of islands is the same as the largest name for any of the islands (assuming we start naming from `1`)


### Connected blocks get same name {#connected-blocks-get-same-name}

We are skipping subsequent land blocks after the first because they are all named the same. Here the inherent assumption is that land blocks can also be connected in the horizontal direction. This is one assumption that breaks in the 2D case. We'll explore this further.


## Back to 2D {#back-to-2d}

Let's try to blinding apply the same strategy to the 2D case to see what breaks.
This is our example:

| 1 | 1 | 1 | 1 | 0 |
|---|---|---|---|---|
| 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 | 0 |
| 0 | 0 | 0 | 0 | 0 |

We start with the first row and start naming the 1D islands in the first row. The name is prepending with `i` to avoid confusion with numbers denoting land and water.

| _i1_ | _i1_ | _i1_ | _i1_ | 0 |
|------|------|------|------|---|
| 1    | 1    | 0    | 1    | 0 |
| 1    | 1    | 0    | 0    | 0 |
| 0    | 0    | 0    | 0    | 0 |

After the first row, let's do the same with second (assuming the second row is a 1D stretch of land, ignore the first row):

| _i1_ | _i1_ | _i1_ | _i1_ | 0 |
|------|------|------|------|---|
| _i1_ | _i1_ | 0    | _i2_ | 0 |
| 1    | 1    | 0    | 0    | 0 |
| 0    | 0    | 0    | 0    | 0 |

The issue is that islands in 2D can connect both horizontally and vertically. So _i2_ above is actually part of _i1_. We missed it because we were incrementing based on discontinuities in the horizontal direction.


### Checking neighbours explicitly {#checking-neighbours-explicitly}

What if we checked the neighbours explicitly? In the 1D case just the act of iterating was equivalent to checking if our previous horizontal block is land. But in the 2D case we have to explicitly check the vertical direction too. Say, for every block, we check the four surrounding blocks. If any of them have a name, we inherit that name for this block. We'll do this for the full 2D array of this example.

| _i1_ | _i1_ | _i1_ | _i1_ | 0 |
|------|------|------|------|---|
| _i1_ | _i1_ | 0    | _i1_ | 0 |
| _i1_ | _i1_ | 0    | 0    | 0 |
| 0    | 0    | 0    | 0    | 0 |

This works. We have the correct name for the island(s). Let's test against more examples to be rigorous:


#### Another example {#another-example}

| 1 | 1 | 0 | 0 | 0 |
|---|---|---|---|---|
| 1 | 1 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 |
| 0 | 0 | 0 | 1 | 1 |

Let's name the islands based on the 4 neighbours again. Note that the first block has 2 water blocks around it and 2 unnamed land blocks. We name it `i1`. The second block has neighbour behind it with the name `i1`. We proceed this way until we hit the third row.

| _i1_ | _i1_ | 0 | 0 | 0 |
|------|------|---|---|---|
| _i1_ | _i1_ | 0 | 0 | 0 |
| 0    | 0    | 1 | 0 | 0 |
| 0    | 0    | 0 | 1 | 1 |

In the third row we encounter the third block which has all water neighbours. Until now we have seen `1` island. So we increment that number and name this island `i2`.

| _i1_ | _i1_ | 0    | 0 | 0 |
|------|------|------|---|---|
| _i1_ | _i1_ | 0    | 0 | 0 |
| 0    | 0    | _i2_ | 0 | 0 |
| 0    | 0    | 0    | 1 | 1 |

In the fourth row, we have the fourth block with 3 water neighbours and 1 unnamed land block. We need a new name. We have seen 2 islands until now, so we name this one `i3`.

| _i1_ | _i1_ | 0    | 0    | 0    |
|------|------|------|------|------|
| _i1_ | _i1_ | 0    | 0    | 0    |
| 0    | 0    | _i2_ | 0    | 0    |
| 0    | 0    | 0    | _i3_ | _i3_ |

This seems to work quite well. Let's try writing it up to see what implementation issues come up.


### Implementation {#implementation}

Let's generate random examples first. Random test cases can help in figuring our edge cases early on. This way we avoid any biases we have about what an input is.

```python
import random
length = 5
breadth = 4
array = [[random.choice([0,1]) for i in range(0, length)] for j in range(0, breadth)]
[print(row) for row in array]
```

```text
[0, 0, 1, 1, 1]
[0, 1, 1, 1, 0]
[1, 1, 1, 0, 1]
[1, 0, 0, 1, 1]
```

We plan to iterate through the 2D array with index values for both dimensions. Say we call the horizontal index `col` and vertical index `row`. We need a function to check for neighbours of a given `(row, col)` pair. Note that `row` can be `[0, breadth-1]` both inclusive and `col` can be `[0, height-1]` both inclusive.

```python
def neighbours(r, c):
    rdown = r + 1 if r + 1 < breadth else None
    rup = r - 1 if r - 1 >= 0 else None
    cleft = c - 1 if c - 1 >= 0 else None
    cright = c + 1  if c + 1 < length else None

    blocks = []
    if rup is not None:
        blocks.append(array[rup][c])
    else:
        blocks.append(None)

    if cright is not None:
        blocks.append(array[r][cright])
    else:
        blocks.append(None)

    if rdown is not None:
        blocks.append(array[rdown][c])
    else:
        blocks.append(None)

    if cleft is not None:
        blocks.append(array[r][cleft])
    else:
        blocks.append(None)

    return blocks
```

The function returns the value of the neighbouring blocks in the clock wise direction starting from top of the block. Let's check if the edge cases (the four corners) are correct:

```python
print(neighbours(0, 0))
print(neighbours(0, length-1))
print(neighbours(breadth-1, 0))
print(neighbours(breadth-1, length-1))
```

```text
[None, 0, 0, None]
[None, None, 0, '1']
['2', 0, None, None]
['4', None, None, '4']
```

This works. We can now iterate through the given 2D array and process it. During processing of a block, we can simply rename the block's value with a string such as '1'. That way we can also quickly check whether any of `neighbours(r,c)` are characters.

```python
num_islands = 0
for r in range(0, breadth):
    for c in range(0, length):
        if array[r][c] == 0:
            continue
        named_neighbours = list(filter(lambda e: type(e) == str, neighbours(r,c)))
        if len(named_neighbours) == 0:
            array[r][c] = str(num_islands + 1)
            num_islands += 1
        else:
            array[r][c] = named_neighbours[0]

[print(row) for row in array]
```

```text
[0, 0, '1', '1', '1']
[0, '2', '1', '1', 0]
['2', '2', '1', 0, '4']
['2', 0, 0, '4', '4']
```

That's a weird outcome. What's happening here? We assumed that the neighbours will either be unnamed or will have one name.
But what if two neighbours have two different names? Would this ever happen? Let's specifically try to design and example where this could happen. Let's make sure we count two islands in the first row:

|   |   |   |   |   |
|---|---|---|---|---|
| 1 | 0 | 0 | 0 | 1 |

We'll have named them the first island `i1` and the second one `i2`. Because they have no named blocks around them and they are not touching each other.
But what if the second row reveals that they are in fact connected?

| i1 | 0  | 0  | 0  | i2 |
|----|----|----|----|----|
| 11 | i1 | i1 | i1 | ?? |

When we reach the 2nd row, 5th block, we now have conflicting names to work with. Worse still, the (1,5) block has a wrong name! You can imagine how that block could be "in touch" with other blocks that are wrongly named too.
It would seem a single pass is not possible. But let's see how we can rectify this. Say we have two neighbours named `a, b`. Let's choose to name the current with the minimum of the two: `min(a,b)`.

| i1 | 0  | 0  | 0  | i2 |
|----|----|----|----|----|
| 11 | i1 | i1 | i1 | i1 |

Note that 1st row, 5th block is still wrong. What if we perform a second pass?

| i1 | 0  | 0  | 0  | i1 |
|----|----|----|----|----|
| 11 | i1 | i1 | i1 | i1 |

Looks like we can do multiple passes with this new approach to "settle" the names.
Let's rewrite our code to see how this works:

```python
def array_pass():
    num_islands = 0
    for r in range(0, breadth):
        for c in range(0, length):
            if array[r][c] == 0:
                continue
            named_neighbours = list(filter(lambda e: type(e) == str, neighbours(r,c)))
            if len(named_neighbours) == 0:
                array[r][c] = str(num_islands + 1)
                num_islands += 1
            else:
                array[r][c] = str(min([int(e) for e in named_neighbours]))

array_pass()
array_pass()
[print(row) for row in array]
```

```text
[0, 0, '1', '1', '1']
[0, '1', '1', '1', 0]
['1', '1', '1', 0, '4']
['1', 0, 0, '4', '4']
```

Two passes seems to work, but will they work for any example? Let's try to think of an example where `2` passes may not work.

> The idea is to see whether we only need constant passes. If the number of passes depends on the grid size or the number of islands, then we may need a better solution.

Let's try two passes on this:

| 1 | 0 | 0 | 0 | 1 |
|---|---|---|---|---|
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

Pass 1:

| i1 | 0  | 0  | 0  | i2 |
|----|----|----|----|----|
| i1 | 0  | 0  | 0  | i2 |
| i1 | 0  | 0  | 0  | i2 |
| i1 | i1 | i1 | i1 | i1 |

Pass 2:

| i1 | 0  | 0  | 0  | i2 |
|----|----|----|----|----|
| i1 | 0  | 0  | 0  | i2 |
| i1 | 0  | 0  | 0  | i1 |
| i1 | i1 | i1 | i1 | i1 |

Looks like we may need at least 2 more passes. We can always find more examples which arbitrarily more passes.


## A new approach {#a-new-approach}

This means we have to name all the blocks of an island at the same time. Notice that in the 1D case this is true: all the blocks of an island are named at the same time.


### Naming neighbours too {#naming-neighbours-too}

For each new island we encounter, let's start by naming the block, then their neighbours. Once all the neighbours are named, let's choose a new block from the neighbours to continue this process.

> This is simply a breadth first traversal. We keep going until there are no more unnamed blocks in an island.

Let's try this on:

| 1 | 0 | 0 | 0 | 1 |
|---|---|---|---|---|
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

We encounter an unnamed land block (the current block is denoted by *) and name it `i1`.

| i1* | 0 | 0 | 0 | 1 |
|------|---|---|---|---|
| 1    | 0 | 0 | 0 | 1 |
| 1    | 0 | 0 | 0 | 1 |
| 1    | 1 | 1 | 1 | 1 |

After this, we name its neighbour also `i1`:

| i1* | 0 | 0 | 0 | 1 |
|------|---|---|---|---|
| i1   | 0 | 0 | 0 | 1 |
| 1    | 0 | 0 | 0 | 1 |
| 1    | 1 | 1 | 1 | 1 |

No more neighbours left. Let's choose one neighbour from the available (we have only one) and make it the current block:

| i1   | 0 | 0 | 0 | 1 |
|------|---|---|---|---|
| i1* | 0 | 0 | 0 | 1 |
| 1    | 0 | 0 | 0 | 1 |
| 1    | 1 | 1 | 1 | 1 |

Continuing this way leads to:

| i1 | 0  | 0  | 0  | i1 |
|----|----|----|----|----|
| i1 | 0  | 0  | 0  | i1 |
| i1 | 0  | 0  | 0  | i1 |
| i1 | i1 | i1 | i1 | i1 |

In this case, we did not have multiple neighbours at any point. Let's try an example where there are multiple neighbours:

| 1 | 0 | 0 | 0 | 1 |
|---|---|---|---|---|
| 1 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

We start with the first land block, name the block and the neighbours `i1`. Let's also mark the available neighbours with `-`

| i1* | 0 | 0 | 0 | 1 |
|------|---|---|---|---|
| i1-  | 1 | 1 | 0 | 1 |
| 1    | 0 | 0 | 0 | 1 |
| 1    | 1 | 1 | 1 | 1 |

Mark the current as done `#`. Now we move to one of the blocks marked with `-` (we have just one now). We make it the current block:

| i1#  | 0 | 0 | 0 | 1 |
|------|---|---|---|---|
| i1* | 1 | 1 | 0 | 1 |
| 1    | 0 | 0 | 0 | 1 |
| 1    | 1 | 1 | 1 | 1 |

We name it `i1` and all its neighbours too. We also mark the unvisited neighbours with `-` (this means we ignore the block above)

| i1#  | 0   | 0 | 0 | 1 |
|------|-----|---|---|---|
| i1* | i1- | 1 | 0 | 1 |
| i1-  | 0   | 0 | 0 | 1 |
| 1    | 1   | 1 | 1 | 1 |

We have two unvisited neighbours now. We can choose either as current:

| i1# | 0    | 0  | 0 | 1 |
|-----|------|----|---|---|
| i1# | i1* | i1 | 0 | 1 |
| i1- | 0    | 0  | 0 | 1 |
| 1   | 1    | 1  | 1 | 1 |

| i1# | 0   | 0    | 0 | 1 |
|-----|-----|------|---|---|
| i1# | i1# | i1* | 0 | 1 |
| i1- | 0   | 0    | 0 | 1 |
| 1   | 1   | 1    | 1 | 1 |

At this point we have no more neighbours. Let's mark the current block as done.

| i1# | 0   | 0   | 0 | 1 |
|-----|-----|-----|---|---|
| i1# | i1# | i1# | 0 | 1 |
| i1- | 0   | 0   | 0 | 1 |
| 1   | 1   | 1   | 1 | 1 |

We have pending blocks with `-`. We'll make one of them current:

| i1#  | 0   | 0   | 0 | 1 |
|------|-----|-----|---|---|
| i1#  | i1# | i1# | 0 | 1 |
| i1* | 0   | 0   | 0 | 1 |
| 1    | 1   | 1   | 1 | 1 |

| i1# | 0   | 0   | 0 | 1 |
|-----|-----|-----|---|---|
| i1# | i1# | i1# | 0 | 1 |
| i1# | 0   | 0   | 0 | 1 |
| i1- | 1   | 1   | 1 | 1 |

As we proceed this way, we will have covered all blocks of this island:

| i1# | 0   | 0   | 0   | i1# |
|-----|-----|-----|-----|-----|
| i1# | i1# | i1# | 0   | i1# |
| i1# | 0   | 0   | 0   | i1# |
| i1# | i1# | i1# | i1# | i1# |


### BFT {#bft}

What we covered in the previous section is a standard technique of traversing a graph in a breadth fast manner, called [Breadth First Search/Traversal](https://en.wikipedia.org/wiki/Breadth-first%5Fsearch). With the intuition now clear, I'd recommend reading the formal process in the [linked wiki page](https://en.wikipedia.org/wiki/Breadth-first%5Fsearch).


### A new representation {#a-new-representation}

We are now clear about the problem. It's a BFT applied on a land mass to identify the number of islands. Notice that applying BFT on a disconnected graph does not cover all the vertices. We may need repeated applications of BFT to cover all the vertices. This is good for us. We can count the number of applications and this is the number of islands in the landmass.
In graph theory related questions, the implementation can get unwieldy if the right representation is not used.
Now that we know the given 2D matrix is actually just a disconnected graph, we should convert our problem input to a more usable format.


#### Single index {#single-index}

Instead of using two numbers to refer to one block, let's reduce it to a single index.
From this representation:

|       | 1 | 2 | 3 | 4 | 5 |
|-------|---|---|---|---|---|
| **1** | 1 | 0 | 0 | 0 | 1 |
| **2** | 1 | 0 | 0 | 0 | 1 |
| **3** | 1 | 0 | 0 | 0 | 1 |
| **4** | 1 | 1 | 1 | 1 | 1 |

let's flatten it to get:

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|---|---|---|---|---|---|---|---|---|----|----|----|----|----|----|----|----|----|----|----|
| 1 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | 1  | 1  | 0  | 0  | 0  | 1  | 1  | 1  | 1  | 1  | 1  |


##### (r,c) to one number {#r-c--to-one-number}

Notice that `(r,c)` in our previous representation is now `r * length + c`


##### Checking neighbours {#checking-neighbours}

Given a number `n`, we can simply divide by length to get the row number. The reminder of this operation is the column.
But there's a better way to find for neighbours. Say there are a total of `num` blocks (water and land).
For `n`:

-   `n+1` ( `n` not divisible by length), is the right side neighbour
-   `n-1 > 0` ( `n-1` not divisible by length), is the left side neighbour
-   `n-length > 0` is the neighbour above
-   `n + length < num`, is the neighbour below


#### Ignore the water (0s) {#ignore-the-water--0s}

We have no use for the water blocks any more. Given a land block, we have a way to calculate the adjacent blocks. If we have a set of all land blocks only, then we can check if the adjacent block is in the set.
Once we cover a block, we simply remove it. So we can no use to keep the water blocks anymore because we are changing the representation to a graph.


#### Two sets {#two-sets}

For our approach, we will be using two sets. One is simply the graph representation of the given 2D matrix. We'll call this `vertices`. We also maintain a queue `waiting` to add the adjacent vertices of current node.

-   Our task is to pop a block from this queue and see if the block's neighbours are already visited. If they are already visited, they will not be present in the `vertices`. We remove the current block from `vertices`.
-   We go on until we have nothing left in `waiting`. This concludes one island.
-   If there are still elements left in `vertices` they can be used to form more islands. So we pop one from `vertices` and add it to `waiting` and restart the process.


##### Potential issue {#potential-issue}

One issue is with the `waiting` queue. It's possible that duplicates are added to the queue. To solve this, we have to check if the block we pop from `waiting` is in `vertices`. If it's not that, it's already been processed and we can skip to the next pop in `waiting`.

> This should give us a hint. What if we use a set for `waiting` instead of queue? But a set is unordered and this is not a BFS anymore. In this particular implementation it doesn't matter but in general BFS utilizes a queue to make sure that the trajectory of the search is deterministic.


### Implementation {#implementation}

Let's now try an implementation of this:

```python
import random
import queue

length = 5
breadth = 5
array = [[random.choice(["0","1"]) for i in range(0, length)] for j in range(0, breadth)]
[print(row) for row in array]

def main(array):
    vertices = set()
    waiting = queue.Queue()
    for i in range(0, breadth):
        for j in range(0, length):
            if array[i][j] != '0':
                # a land block
                vertices.add( i * length + (j+1) )
    print(vertices)

    num_islands = 0
    start_vertex = next(iter(vertices))
    block = start_vertex
    while block != None:
        prev = block - 1
        nxt = block + 1
        up = block - length
        down = block + length
        # print(prev, nxt, up, down)
        if prev in vertices and prev % length != 0:
            waiting.put(prev)
        if nxt in vertices and block % length != 0:
            waiting.put(nxt)
        if up in vertices and up > 0:
            waiting.put(up)
        if down in vertices and down <= length * breadth:
            waiting.put(down)
        # mark as visited
        vertices.remove(block)
        try:
            block = waiting.get_nowait()
            while block not in vertices:
                block = waiting.get_nowait()
        except Exception:
            num_islands += 1
            try:
                block = next(iter(vertices))
            except:
                print("All blocks visited. A total of %d islands found" % (num_islands))
                return num_islands

main(array)
```

```text
['0', '1', '0', '1', '1']
['0', '1', '0', '1', '0']
['0', '1', '1', '1', '0']
['1', '1', '1', '0', '0']
['1', '0', '0', '0', '0']
{2, 4, 5, 7, 9, 12, 13, 14, 16, 17, 18, 21}
All blocks visited. A total of 1 islands found
```


## Testing {#testing}

Let's now generate more test cases to check against leetcode.

```python
import random

tests = []
def generate(length, breadth):
    array = [[random.choice(["0","1"]) for i in range(0, length)] for j in range(0, breadth)]
    tests.append(array)

[generate(300, 300) for i in range(0, 1)]
with open("leetcode_test_num_islands.txt", "w") as file:
    for array in tests:
        file.write(str(array) + "n")
```


### Fail 1 - edge case of all water {#fail-1-edge-case-of-all-water}

Looks like we missed the most obvious edge case. All water!
Let's add a `return 0` if zero vertices are created.

```python
    if len(vertices) == 0:
        print("0 islands.")
        return 0
```


### Result {#result}

All tests passed.

> Runtime: 220 ms, faster than 5.14% of Python3 online submissions for Number of Islands.
> Memory Usage: 18.5 MB, less than 16.87% of Python3 online submissions for Number of Islands.

Looks like this could be one of the poorer solutions for this problem.


## Improvements {#improvements}

The runtime is exceedingly high in our implementation as per the results.
Possible issues:

-   The `mod` function for reminder is definitely slow. Is there a way to avoid that altogether?
    -   we could explicitly surround the given array with water. We would still iterate on the inner land only but we will not need the modulo check
-   Could be because of the initial set creation loop. Is there an easier way to avoid `set` creation all together?
    -   We could retain the grid and use our earlier `neighbours` function to find the adjacent elements
    -   Instead of adding and removing elements from the `set` we could use our older approach of simply mutating the array
-   BFS definitely seems the right approach here. BFS/DFS doesn't change much in terms of time. So traversal of the whole island is crucial and unavoidable


### Removing the mod {#removing-the-mod}

Let's modify the incoming array with a surrounding 1 cell thickness of water. This ensures we won't need to use the mod function during the adjacency check. In fact we don't have to actually surround the incoming array with water. We'll just re-map `(r,c)` to the vertex number such that we assume an extra 2 length and 2 breadth.

```python
import random
import queue

length = 5
breadth = 5
array = [[random.choice(["0","1"]) for i in range(0, length)] for j in range(0, breadth)]
[print(row) for row in array]

def main(grid):
    breadth = len(grid)
    length = len(grid[0])
    vertices = set()
    waiting = queue.Queue()
#    grid.append([0] * (length + 2))
#    grid = [[0] * (length + 2)] + grid
    length += 2
    breadth += 2
    for i in range(1, breadth-1):
  #      grid[i].append("0")
   #     grid[i] = ["0"] + grid[i]
        for j in range(1, length-1):
            if grid[i-1][j-1] != '0':
                # a land block
                vertices.add( i * length + (j+1) )
    if len(vertices) == 0:
        return 0
    num_islands = 0
    start_vertex = next(iter(vertices))
    block = start_vertex
    while block != None:
        prev = block - 1
        nxt = block + 1
        up = block - length
        down = block + length
        # print(prev, nxt, up, down)
        if prev in vertices:
            waiting.put(prev)
        if nxt in vertices:
            waiting.put(nxt)
        if up in vertices and up > 0:
            waiting.put(up)
        if down in vertices and down <= length * breadth:
            waiting.put(down)
        # mark as visited
        vertices.remove(block)
        try:
            block = waiting.get_nowait()
            while block not in vertices:
                block = waiting.get_nowait()
        except Exception:
            num_islands += 1
            try:
                block = next(iter(vertices))
            except:
                break

    return num_islands

print(main(array))
```

```text
['1', '0', '0', '0', '1']
['1', '1', '0', '0', '1']
['0', '0', '1', '1', '0']
['1', '0', '0', '1', '0']
['1', '1', '0', '0', '0']
4
```


#### Results {#results}

> Runtime: 212 ms, faster than 5.14% of Python3 online submissions for Number of Islands.
> Memory Usage: 18.5 MB, less than 16.70% of Python3 online submissions for Number of Islands.

That's exactly the same as before. So no luck here.