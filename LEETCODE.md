# LeetCode Python 3.14 environment notes

Notes on the LeetCode runtime that don't belong on the website.

## Recursion limit

Python's default recursion limit is 1000, which deep DFS on a 10^5-node tree or a linked list will exceed with `RecursionError`. Raise it at module level, or rewrite the recursion iteratively with an explicit stack.

```python
import sys
sys.setrecursionlimit(10**6)

# @cache on deep recursion still recurses; raising the limit applies to it too.
from functools import cache

@cache
def dp(i: int) -> int:
    if i < 2:
        return i
    return dp(i - 1) + dp(i - 2)

# Iterative alternative: explicit stack
stack = [root]
while stack:
    node = stack.pop()
    ...
```

`@cache` persists across test cases if it's defined at module level. Define it inside the method, or call `dp.cache_clear()`.

- [sys.setrecursionlimit](https://docs.python.org/3.14/library/sys.html#sys.setrecursionlimit)
- [functools.cache](https://docs.python.org/3.14/library/functools.html#functools.cache)

## Typing in LeetCode signatures

LeetCode stubs still use `List[int]` and `Optional[TreeNode]` from `typing`, which are pre-imported. Built-in generics and `X | None` work too. In 3.14, annotations are evaluated lazily (PEP 649), so forward references like `-> Node` inside the `Node` class need no quotes.

```python
from typing import List, Optional  # pre-imported on LeetCode

class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]: ...
    def invertTree(self, root: TreeNode | None) -> TreeNode | None: ...
```

- [Generic alias types](https://docs.python.org/3.14/library/stdtypes.html#types-genericalias)
- [Union type](https://docs.python.org/3.14/library/stdtypes.html#types-union)
- [typing.Optional](https://docs.python.org/3.14/library/typing.html#typing.Optional)

## Fast string building and I/O

Strings are immutable, so `s += c` in a loop is O(n^2) in the worst case. Collect the parts in a list and `''.join` them once. LeetCode calls your method directly, so you don't need `input()`. For stdin-style problems, read everything at once with `sys.stdin`.

```python
parts = []
for c in s:
    parts.append(c)
result = ''.join(parts)

import sys
data = sys.stdin.read().split()
```

- [str.join](https://docs.python.org/3.14/library/stdtypes.html#str.join)
- [sys.stdin](https://docs.python.org/3.14/library/sys.html#sys.stdin)
