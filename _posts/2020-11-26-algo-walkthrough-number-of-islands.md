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
<h2><a href="https://leetcode.com/problems/number-of-islands/">The problem</a> {#the-problem}</h2>
<blockquote>
<p>Given an m x n 2d grid map of '1's (land) and '0's (water), return the number of islands.
An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.</p>
</blockquote>
<h2>Exploration {#exploration}</h2>
<p>The first instinct is to lower the dimension of the problem and see what that looks like. A 1D world with stretches of land and water:</p>
<table>
<thead>
<tr>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>In this case, there are 3 islands. We can count this in a single pass of the array. Every time we encounter land, we increment our <code>total_islands</code> variable. After this we skip all the land blocks until we hit water (or end of array). After the single pass <code>total_islands</code> variable has the correct count.</p>
<h3>Counting = naming {#counting-naming}</h3>
<p>When we count islands, we are actually naming the blocks of the island with a single name.</p>
<table>
<thead>
<tr>
<th>blocks</th>
<th>1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
<th>1</th>
<th>1</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>name</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>2</td>
<td>2</td>
<td>2</td>
<td>0</td>
<td>0</td>
<td>3</td>
</tr>
</tbody>
</table>
<p>The total number of islands is the same as the largest name for any of the islands (assuming we start naming from <code>1</code>)</p>
<h3>Connected blocks get same name {#connected-blocks-get-same-name}</h3>
<p>We are skipping subsequent land blocks after the first because they are all named the same. Here the inherent assumption is that land blocks can also be connected in the horizontal direction. This is one assumption that breaks in the 2D case. We'll explore this further.</p>
<h2>Back to 2D {#back-to-2d}</h2>
<p>Let's try to blinding apply the same strategy to the 2D case to see what breaks.
This is our example:</p>
<table>
<thead>
<tr>
<th>1</th>
<th>1</th>
<th>1</th>
<th>1</th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>1</td>
<td>0</td>
<td>1</td>
<td>0</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
</tbody>
</table>
<p>We start with the first row and start naming the 1D islands in the first row. The name is prepending with <code>i</code> to avoid confusion with numbers denoting land and water.</p>
<table>
<thead>
<tr>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>1</td>
<td>0</td>
<td>1</td>
<td>0</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
</tbody>
</table>
<p>After the first row, let's do the same with second (assuming the second row is a 1D stretch of land, ignore the first row):</p>
<table>
<thead>
<tr>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td><em>i1</em></td>
<td><em>i1</em></td>
<td>0</td>
<td><em>i2</em></td>
<td>0</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
</tbody>
</table>
<p>The issue is that islands in 2D can connect both horizontally and vertically. So <em>i2</em> above is actually part of <em>i1</em>. We missed it because we were incrementing based on discontinuities in the horizontal direction.</p>
<h3>Checking neighbours explicitly {#checking-neighbours-explicitly}</h3>
<p>What if we checked the neighbours explicitly? In the 1D case just the act of iterating was equivalent to checking if our previous horizontal block is land. But in the 2D case we have to explicitly check the vertical direction too. Say, for every block, we check the four surrounding blocks. If any of them have a name, we inherit that name for this block. We'll do this for the full 2D array of this example.</p>
<table>
<thead>
<tr>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td><em>i1</em></td>
<td><em>i1</em></td>
<td>0</td>
<td><em>i1</em></td>
<td>0</td>
</tr>
<tr>
<td><em>i1</em></td>
<td><em>i1</em></td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
</tbody>
</table>
<p>This works. We have the correct name for the island(s). Let's test against more examples to be rigorous:</p>
<h4>Another example {#another-example}</h4>
<table>
<thead>
<tr>
<th>1</th>
<th>1</th>
<th>0</th>
<th>0</th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>1</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>Let's name the islands based on the 4 neighbours again. Note that the first block has 2 water blocks around it and 2 unnamed land blocks. We name it <code>i1</code>. The second block has neighbour behind it with the name <code>i1</code>. We proceed this way until we hit the third row.</p>
<table>
<thead>
<tr>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th>0</th>
<th>0</th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td><em>i1</em></td>
<td><em>i1</em></td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>1</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>In the third row we encounter the third block which has all water neighbours. Until now we have seen <code>1</code> island. So we increment that number and name this island <code>i2</code>.</p>
<table>
<thead>
<tr>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th>0</th>
<th>0</th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td><em>i1</em></td>
<td><em>i1</em></td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td><em>i2</em></td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>In the fourth row, we have the fourth block with 3 water neighbours and 1 unnamed land block. We need a new name. We have seen 2 islands until now, so we name this one <code>i3</code>.</p>
<table>
<thead>
<tr>
<th><em>i1</em></th>
<th><em>i1</em></th>
<th>0</th>
<th>0</th>
<th>0</th>
</tr>
</thead>
<tbody>
<tr>
<td><em>i1</em></td>
<td><em>i1</em></td>
<td>0</td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td><em>i2</em></td>
<td>0</td>
<td>0</td>
</tr>
<tr>
<td>0</td>
<td>0</td>
<td>0</td>
<td><em>i3</em></td>
<td><em>i3</em></td>
</tr>
</tbody>
</table>
<p>This seems to work quite well. Let's try writing it up to see what implementation issues come up.</p>
<h3>Implementation {#implementation}</h3>
<p>Let's generate random examples first. Random test cases can help in figuring our edge cases early on. This way we avoid any biases we have about what an input is.</p>
<pre><code class="language-python">import random
length = 5
breadth = 4
array = [[random.choice([0,1]) for i in range(0, length)] for j in range(0, breadth)]
[print(row) for row in array]</code></pre>
<pre><code class="language-text">[0, 0, 1, 1, 1]
[0, 1, 1, 1, 0]
[1, 1, 1, 0, 1]
[1, 0, 0, 1, 1]</code></pre>
<p>We plan to iterate through the 2D array with index values for both dimensions. Say we call the horizontal index <code>col</code> and vertical index <code>row</code>. We need a function to check for neighbours of a given <code>(row, col)</code> pair. Note that <code>row</code> can be <code>[0, breadth-1]</code> both inclusive and <code>col</code> can be <code>[0, height-1]</code> both inclusive.</p>
<pre><code class="language-python">def neighbours(r, c):
    rdown = r + 1 if r + 1 &lt; breadth else None
    rup = r - 1 if r - 1 &gt;= 0 else None
    cleft = c - 1 if c - 1 &gt;= 0 else None
    cright = c + 1  if c + 1 &lt; length else None

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

    return blocks</code></pre>
<p>The function returns the value of the neighbouring blocks in the clock wise direction starting from top of the block. Let's check if the edge cases (the four corners) are correct:</p>
<pre><code class="language-python">print(neighbours(0, 0))
print(neighbours(0, length-1))
print(neighbours(breadth-1, 0))
print(neighbours(breadth-1, length-1))</code></pre>
<pre><code class="language-text">[None, 0, 0, None]
[None, None, 0, &#039;1&#039;]
[&#039;2&#039;, 0, None, None]
[&#039;4&#039;, None, None, &#039;4&#039;]</code></pre>
<p>This works. We can now iterate through the given 2D array and process it. During processing of a block, we can simply rename the block's value with a string such as '1'. That way we can also quickly check whether any of <code>neighbours(r,c)</code> are characters.</p>
<pre><code class="language-python">num_islands = 0
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

[print(row) for row in array]</code></pre>
<pre><code class="language-text">[0, 0, &#039;1&#039;, &#039;1&#039;, &#039;1&#039;]
[0, &#039;2&#039;, &#039;1&#039;, &#039;1&#039;, 0]
[&#039;2&#039;, &#039;2&#039;, &#039;1&#039;, 0, &#039;4&#039;]
[&#039;2&#039;, 0, 0, &#039;4&#039;, &#039;4&#039;]</code></pre>
<p>That's a weird outcome. What's happening here? We assumed that the neighbours will either be unnamed or will have one name.
But what if two neighbours have two different names? Would this ever happen? Let's specifically try to design and example where this could happen. Let's make sure we count two islands in the first row:</p>
<table>
<thead>
<tr>
<th></th>
<th></th>
<th></th>
<th></th>
<th></th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>We'll have named them the first island <code>i1</code> and the second one <code>i2</code>. Because they have no named blocks around them and they are not touching each other.
But what if the second row reveals that they are in fact connected?</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i2</th>
</tr>
</thead>
<tbody>
<tr>
<td>11</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>??</td>
</tr>
</tbody>
</table>
<p>When we reach the 2nd row, 5th block, we now have conflicting names to work with. Worse still, the (1,5) block has a wrong name! You can imagine how that block could be &quot;in touch&quot; with other blocks that are wrongly named too.
It would seem a single pass is not possible. But let's see how we can rectify this. Say we have two neighbours named <code>a, b</code>. Let's choose to name the current with the minimum of the two: <code>min(a,b)</code>.</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i2</th>
</tr>
</thead>
<tbody>
<tr>
<td>11</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
</tr>
</tbody>
</table>
<p>Note that 1st row, 5th block is still wrong. What if we perform a second pass?</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i1</th>
</tr>
</thead>
<tbody>
<tr>
<td>11</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
</tr>
</tbody>
</table>
<p>Looks like we can do multiple passes with this new approach to &quot;settle&quot; the names.
Let's rewrite our code to see how this works:</p>
<pre><code class="language-python">def array_pass():
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
[print(row) for row in array]</code></pre>
<pre><code class="language-text">[0, 0, &#039;1&#039;, &#039;1&#039;, &#039;1&#039;]
[0, &#039;1&#039;, &#039;1&#039;, &#039;1&#039;, 0]
[&#039;1&#039;, &#039;1&#039;, &#039;1&#039;, 0, &#039;4&#039;]
[&#039;1&#039;, 0, 0, &#039;4&#039;, &#039;4&#039;]</code></pre>
<p>Two passes seems to work, but will they work for any example? Let's try to think of an example where <code>2</code> passes may not work.</p>
<blockquote>
<p>The idea is to see whether we only need constant passes. If the number of passes depends on the grid size or the number of islands, then we may need a better solution.</p>
</blockquote>
<p>Let's try two passes on this:</p>
<table>
<thead>
<tr>
<th>1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>Pass 1:</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i2</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i2</td>
</tr>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i2</td>
</tr>
<tr>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
</tr>
</tbody>
</table>
<p>Pass 2:</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i2</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i2</td>
</tr>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i1</td>
</tr>
<tr>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
</tr>
</tbody>
</table>
<p>Looks like we may need at least 2 more passes. We can always find more examples which arbitrarily more passes.</p>
<h2>A new approach {#a-new-approach}</h2>
<p>This means we have to name all the blocks of an island at the same time. Notice that in the 1D case this is true: all the blocks of an island are named at the same time.</p>
<h3>Naming neighbours too {#naming-neighbours-too}</h3>
<p>For each new island we encounter, let's start by naming the block, then their neighbours. Once all the neighbours are named, let's choose a new block from the neighbours to continue this process.</p>
<blockquote>
<p>This is simply a breadth first traversal. We keep going until there are no more unnamed blocks in an island.</p>
</blockquote>
<p>Let's try this on:</p>
<table>
<thead>
<tr>
<th>1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>We encounter an unnamed land block (the current block is denoted by *) and name it <code>i1</code>.</p>
<table>
<thead>
<tr>
<th>i1*</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>After this, we name its neighbour also <code>i1</code>:</p>
<table>
<thead>
<tr>
<th>i1*</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>No more neighbours left. Let's choose one neighbour from the available (we have only one) and make it the current block:</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1*</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>Continuing this way leads to:</p>
<table>
<thead>
<tr>
<th>i1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i1</td>
</tr>
<tr>
<td>i1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i1</td>
</tr>
<tr>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
<td>i1</td>
</tr>
</tbody>
</table>
<p>In this case, we did not have multiple neighbours at any point. Let's try an example where there are multiple neighbours:</p>
<table>
<thead>
<tr>
<th>1</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>We start with the first land block, name the block and the neighbours <code>i1</code>. Let's also mark the available neighbours with <code>-</code></p>
<table>
<thead>
<tr>
<th>i1*</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1-</td>
<td>1</td>
<td>1</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>Mark the current as done <code>#</code>. Now we move to one of the blocks marked with <code>-</code> (we have just one now). We make it the current block:</p>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1*</td>
<td>1</td>
<td>1</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>We name it <code>i1</code> and all its neighbours too. We also mark the unvisited neighbours with <code>-</code> (this means we ignore the block above)</p>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1*</td>
<td>i1-</td>
<td>1</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1-</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>We have two unvisited neighbours now. We can choose either as current:</p>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1#</td>
<td>i1*</td>
<td>i1</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1-</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1#</td>
<td>i1#</td>
<td>i1*</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1-</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>At this point we have no more neighbours. Let's mark the current block as done.</p>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1-</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>We have pending blocks with <code>-</code>. We'll make one of them current:</p>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1*</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>1</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1#</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td>i1-</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>As we proceed this way, we will have covered all blocks of this island:</p>
<table>
<thead>
<tr>
<th>i1#</th>
<th>0</th>
<th>0</th>
<th>0</th>
<th>i1#</th>
</tr>
</thead>
<tbody>
<tr>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
<td>0</td>
<td>i1#</td>
</tr>
<tr>
<td>i1#</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>i1#</td>
</tr>
<tr>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
<td>i1#</td>
</tr>
</tbody>
</table>
<h3>BFT {#bft}</h3>
<p>What we covered in the previous section is a standard technique of traversing a graph in a breadth fast manner, called <a href="https://en.wikipedia.org/wiki/Breadth-first%5Fsearch">Breadth First Search/Traversal</a>. With the intuition now clear, I'd recommend reading the formal process in the <a href="https://en.wikipedia.org/wiki/Breadth-first%5Fsearch">linked wiki page</a>.</p>
<h3>A new representation {#a-new-representation}</h3>
<p>We are now clear about the problem. It's a BFT applied on a land mass to identify the number of islands. Notice that applying BFT on a disconnected graph does not cover all the vertices. We may need repeated applications of BFT to cover all the vertices. This is good for us. We can count the number of applications and this is the number of islands in the landmass.
In graph theory related questions, the implementation can get unwieldy if the right representation is not used.
Now that we know the given 2D matrix is actually just a disconnected graph, we should convert our problem input to a more usable format.</p>
<h4>Single index {#single-index}</h4>
<p>Instead of using two numbers to refer to one block, let's reduce it to a single index.
From this representation:</p>
<table>
<thead>
<tr>
<th></th>
<th>1</th>
<th>2</th>
<th>3</th>
<th>4</th>
<th>5</th>
</tr>
</thead>
<tbody>
<tr>
<td><strong>1</strong></td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td><strong>2</strong></td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td><strong>3</strong></td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
</tr>
<tr>
<td><strong>4</strong></td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<p>let's flatten it to get:</p>
<table>
<thead>
<tr>
<th>1</th>
<th>2</th>
<th>3</th>
<th>4</th>
<th>5</th>
<th>6</th>
<th>7</th>
<th>8</th>
<th>9</th>
<th>10</th>
<th>11</th>
<th>12</th>
<th>13</th>
<th>14</th>
<th>15</th>
<th>16</th>
<th>17</th>
<th>18</th>
<th>19</th>
<th>20</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
<td>0</td>
<td>0</td>
<td>0</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
<td>1</td>
</tr>
</tbody>
</table>
<h5>(r,c) to one number {#r-c--to-one-number}</h5>
<p>Notice that <code>(r,c)</code> in our previous representation is now <code>r * length + c</code></p>
<h5>Checking neighbours {#checking-neighbours}</h5>
<p>Given a number <code>n</code>, we can simply divide by length to get the row number. The reminder of this operation is the column.
But there's a better way to find for neighbours. Say there are a total of <code>num</code> blocks (water and land).
For <code>n</code>:</p>
<ul>
<li><code>n+1</code> ( <code>n</code> not divisible by length), is the right side neighbour</li>
<li><code>n-1 &gt; 0</code> ( <code>n-1</code> not divisible by length), is the left side neighbour</li>
<li><code>n-length &gt; 0</code> is the neighbour above</li>
<li><code>n + length &lt; num</code>, is the neighbour below</li>
</ul>
<h4>Ignore the water (0s) {#ignore-the-water--0s}</h4>
<p>We have no use for the water blocks any more. Given a land block, we have a way to calculate the adjacent blocks. If we have a set of all land blocks only, then we can check if the adjacent block is in the set.
Once we cover a block, we simply remove it. So we can no use to keep the water blocks anymore because we are changing the representation to a graph.</p>
<h4>Two sets {#two-sets}</h4>
<p>For our approach, we will be using two sets. One is simply the graph representation of the given 2D matrix. We'll call this <code>vertices</code>. We also maintain a queue <code>waiting</code> to add the adjacent vertices of current node.</p>
<ul>
<li>Our task is to pop a block from this queue and see if the block's neighbours are already visited. If they are already visited, they will not be present in the <code>vertices</code>. We remove the current block from <code>vertices</code>.</li>
<li>We go on until we have nothing left in <code>waiting</code>. This concludes one island.</li>
<li>If there are still elements left in <code>vertices</code> they can be used to form more islands. So we pop one from <code>vertices</code> and add it to <code>waiting</code> and restart the process.</li>
</ul>
<h5>Potential issue {#potential-issue}</h5>
<p>One issue is with the <code>waiting</code> queue. It's possible that duplicates are added to the queue. To solve this, we have to check if the block we pop from <code>waiting</code> is in <code>vertices</code>. If it's not that, it's already been processed and we can skip to the next pop in <code>waiting</code>.</p>
<blockquote>
<p>This should give us a hint. What if we use a set for <code>waiting</code> instead of queue? But a set is unordered and this is not a BFS anymore. In this particular implementation it doesn't matter but in general BFS utilizes a queue to make sure that the trajectory of the search is deterministic.</p>
</blockquote>
<h3>Implementation {#implementation}</h3>
<p>Let's now try an implementation of this:</p>
<pre><code class="language-python">import random
import queue

length = 5
breadth = 5
array = [[random.choice([&quot;0&quot;,&quot;1&quot;]) for i in range(0, length)] for j in range(0, breadth)]
[print(row) for row in array]

def main(array):
    vertices = set()
    waiting = queue.Queue()
    for i in range(0, breadth):
        for j in range(0, length):
            if array[i][j] != &#039;0&#039;:
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
        if up in vertices and up &gt; 0:
            waiting.put(up)
        if down in vertices and down &lt;= length * breadth:
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
                print(&quot;All blocks visited. A total of %d islands found&quot; % (num_islands))
                return num_islands

main(array)</code></pre>
<pre><code class="language-text">[&#039;0&#039;, &#039;1&#039;, &#039;0&#039;, &#039;1&#039;, &#039;1&#039;]
[&#039;0&#039;, &#039;1&#039;, &#039;0&#039;, &#039;1&#039;, &#039;0&#039;]
[&#039;0&#039;, &#039;1&#039;, &#039;1&#039;, &#039;1&#039;, &#039;0&#039;]
[&#039;1&#039;, &#039;1&#039;, &#039;1&#039;, &#039;0&#039;, &#039;0&#039;]
[&#039;1&#039;, &#039;0&#039;, &#039;0&#039;, &#039;0&#039;, &#039;0&#039;]
{2, 4, 5, 7, 9, 12, 13, 14, 16, 17, 18, 21}
All blocks visited. A total of 1 islands found</code></pre>
<h2>Testing {#testing}</h2>
<p>Let's now generate more test cases to check against leetcode.</p>
<pre><code class="language-python">import random

tests = []
def generate(length, breadth):
    array = [[random.choice([&quot;0&quot;,&quot;1&quot;]) for i in range(0, length)] for j in range(0, breadth)]
    tests.append(array)

[generate(300, 300) for i in range(0, 1)]
with open(&quot;leetcode_test_num_islands.txt&quot;, &quot;w&quot;) as file:
    for array in tests:
        file.write(str(array) + &quot;\n&quot;)</code></pre>
<h3>Fail 1 - edge case of all water {#fail-1-edge-case-of-all-water}</h3>
<p>Looks like we missed the most obvious edge case. All water!
Let's add a <code>return 0</code> if zero vertices are created.</p>
<pre><code class="language-python">    if len(vertices) == 0:
        print(&quot;0 islands.&quot;)
        return 0</code></pre>
<h3>Result {#result}</h3>
<p>All tests passed.</p>
<blockquote>
<p>Runtime: 220 ms, faster than 5.14% of Python3 online submissions for Number of Islands.
Memory Usage: 18.5 MB, less than 16.87% of Python3 online submissions for Number of Islands.</p>
</blockquote>
<p>Looks like this could be one of the poorer solutions for this problem.</p>
<h2>Improvements {#improvements}</h2>
<p>The runtime is exceedingly high in our implementation as per the results.
Possible issues:</p>
<ul>
<li>The <code>mod</code> function for reminder is definitely slow. Is there a way to avoid that altogether?
<ul>
<li>we could explicitly surround the given array with water. We would still iterate on the inner land only but we will not need the modulo check</li>
</ul></li>
<li>Could be because of the initial set creation loop. Is there an easier way to avoid <code>set</code> creation all together?
<ul>
<li>We could retain the grid and use our earlier <code>neighbours</code> function to find the adjacent elements</li>
<li>Instead of adding and removing elements from the <code>set</code> we could use our older approach of simply mutating the array</li>
</ul></li>
<li>BFS definitely seems the right approach here. BFS/DFS doesn't change much in terms of time. So traversal of the whole island is crucial and unavoidable</li>
</ul>
<h3>Removing the mod {#removing-the-mod}</h3>
<p>Let's modify the incoming array with a surrounding 1 cell thickness of water. This ensures we won't need to use the mod function during the adjacency check. In fact we don't have to actually surround the incoming array with water. We'll just re-map <code>(r,c)</code> to the vertex number such that we assume an extra 2 length and 2 breadth.</p>
<pre><code class="language-python">import random
import queue

length = 5
breadth = 5
array = [[random.choice([&quot;0&quot;,&quot;1&quot;]) for i in range(0, length)] for j in range(0, breadth)]
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
  #      grid[i].append(&quot;0&quot;)
   #     grid[i] = [&quot;0&quot;] + grid[i]
        for j in range(1, length-1):
            if grid[i-1][j-1] != &#039;0&#039;:
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
        if up in vertices and up &gt; 0:
            waiting.put(up)
        if down in vertices and down &lt;= length * breadth:
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

print(main(array))</code></pre>
<pre><code class="language-text">[&#039;1&#039;, &#039;0&#039;, &#039;0&#039;, &#039;0&#039;, &#039;1&#039;]
[&#039;1&#039;, &#039;1&#039;, &#039;0&#039;, &#039;0&#039;, &#039;1&#039;]
[&#039;0&#039;, &#039;0&#039;, &#039;1&#039;, &#039;1&#039;, &#039;0&#039;]
[&#039;1&#039;, &#039;0&#039;, &#039;0&#039;, &#039;1&#039;, &#039;0&#039;]
[&#039;1&#039;, &#039;1&#039;, &#039;0&#039;, &#039;0&#039;, &#039;0&#039;]
4</code></pre>
<h4>Results {#results}</h4>
<blockquote>
<p>Runtime: 212 ms, faster than 5.14% of Python3 online submissions for Number of Islands.
Memory Usage: 18.5 MB, less than 16.70% of Python3 online submissions for Number of Islands.</p>
</blockquote>
<p>That's exactly the same as before. So no luck here.</p>