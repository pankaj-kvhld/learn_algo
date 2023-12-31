# learn_algo

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

This will serve as a quick revision and repo of all algorithms which I
intend to master.

## Exponent

Write a program to implement $n^k$.

### Iterative approach

First is a simple iterative approach.

``` python
def pow(n,k):
    res=1
    for _ in range(k):
        res*=n
    return res
```

``` python
assert pow(2,5)==32
```

### Recursive approach

The key is to realize that $n^k$ can be expressed as sequence of
squaring operations. For ex:

We divide the exponent ($k$) by $2$, if we get an even number we divide
by $2$ again, if we get an odd number we multiply by the number and
continue the process with $k-1$.

``` python
def pow(n,k):
    if k==1: return n
    return pow(n,k//2) * pow(n, k//2) if k%2==0 else n * pow(n, k-1)
```

``` python
assert pow(2,5)==2**5
assert pow(3,5)==3**5
```

### Recursive implemented using a stack

Any recursive function can be represented as an iterative one with a
stack data structure. Which is what is happening in the recursive one.

``` python
def pow(n,k):
    op_stack=[]
    res=n
    # Create the stack
    while k>1:
        if k%2==0:
            op_stack.append('square')
            k=k//2
        else:
            op_stack.append('multiply')
            k-=1
    # Unravel the stack
    while op_stack:
        op=op_stack.pop()
        if op=='square':
            res=res*res
        else:
            res*=n
    return res
```

``` python
assert pow(2,10)==2**10
```

## CanSum

### Vanilla version with recursion

``` python
def can_sum(target,nums):
    if target==0: return True
    if target<0: return False
    for num in nums:
        if can_sum(target-num, nums):
            return True
    return False
```

Given a target and list of numbers write a function which determines
whether the target can be obtained by summing numbers from the list.

``` python
assert can_sum(7, (2,3))
assert not can_sum(13, [2,4,6])
```

The trick is to return immediately as soon as target becomes 0. This
return passes all the way to the top call and function returns `True`.
However if after looping over all the numbers in the list, no solution
is found, we return `False`.

The time complexity of `can_sum` is $O(n^m)$ where `n` is the target and
`m` is the length of `nums`. The following example highlights this.

``` python
# can_sum(250, [7,14])
```

### Recursion with memoization

``` python
def can_sum(target,nums,memo={}):
    if target in memo: return memo[target]
    if target==0: return True
    if target<0: return False
    for num in nums: 
        if can_sum(target-num,nums,memo):
            memo[target]=True
            return True
    memo[target]=False
    return False
```

Memoization makes the call to `can_sum(250, [7,14])` which previously
took way too long super fast.

``` python
can_sum(250, [7,14])
```

    False

## How sum

Given a target and list of numbers return a subset which sums to the
target. All numbers are positive and a give number from the list can be
used multiple times.

If the numbers cannot be summed to target, return None.

``` python
def how_sum_memo(target,nums,memo={}):
    if target in memo: return memo[target]
    if target==0: return []
    if target<0: return None
    for num in nums:
        res=how_sum_memo(target-num, nums, memo)
        if isinstance(res,list):
            res.append(num)
            memo[target]=res
            return res
    memo[target]=None
    return None
```

``` python
how_sum_memo(200, [7,14])
```

    CPU times: user 39 µs, sys: 4 µs, total: 43 µs
    Wall time: 47.9 µs

## All sums

Given a target and a list of numbers, return a list of all subsets which
sum to this target. Multiple usage of a number is permitted.

``` python
def all_sums(target,nums):
    if target==0: return [[]]
    if target<0: return []
    sol=[]
    for num in nums:
        res=all_sums(target-num, nums)
        if len(res)>=1:
            for l in res:
                l.append(num)
            sol+=res
    return sol
```

``` python
all_sums(5, [2,3])
```

    [[3, 2], [2, 3]]

## Best sums

Given a target and a list of numbers, return the subset that sums to
target and has the least number of elements in it.

``` python
def best_sum(target,nums):
    if target==0: return []
    if target<0: return None
    sol=None
    for num in nums:
        res=best_sum(target-num,nums)
        if isinstance(res,list):
            res.append(num)
            if (sol is None) or (len(res) < len(sol)):
                sol=res
    return sol
```

``` python
best_sum(7,[1,3,7])
```

    [7]

## Can construct

Write a function `canConstruct(target, wordBank)` that accepts target
string and an array of strings. The function should return a boolean
indicating whether or not the `target` can be constructed by
concatenating elements of the wordBank array. You may reuse elements of
wordBank as many times as needed.

``` python
def can_construct(target, words):
    if target=='': return True
    for word in words:
        if target.startswith(word):
            return can_construct(target[len(word):], words)
    return False
```

``` python
can_construct('abcd', ['ab', 'bc', 'cd'])
```

    True

``` python
can_construct("", ["f"])
```

    True

``` python
can_construct('ffffffffffffffffffffffd', ['f','fff','fffff', 'd'])
```

    True

## Count construct

Write a function `countConstruct(target, wordBank)` that accepts a
target string and an array of strings. The function should return the
number of ways that the target can be constructed by concatenating
elements of the wordBank array.

You may reuse elements of wordBank as many times as needed.

``` python
def count_construct(target, words):
    if target=='': return 1
    res=0
    for word in words:
        if target.startswith(word):
            res += count_construct(target[len(word):], words)
    return res
```

``` python
count_construct('abcdef', ['ab', 'abc', 'cd', 'def', 'abcd'])
```

    1

``` python
count_construct('purple', ['purp', 'p', 'ur', 'le', 'purpl'])
```

    2

## All construct

Given a target string and list of substrings, return all the set of
substrings which can be concatenated to create the target.

Multiple usage of substrings is permitted.

``` python
def all_construct(target, subs):
    if target=='': return [[]]
    sol=[]
    for sub in subs:
        if target.startswith(sub):
            res=all_construct(target[len(sub):], subs)
            if len(res)>=1:
                for l in res:
                    l.append(sub)
                sol+=res
    return sol
```

``` python
all_construct('abc',['a','bc', 'ab', 'c'])
```

    [['bc', 'a'], ['c', 'ab']]
