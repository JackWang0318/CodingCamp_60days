## re**Trie**val

- 前缀树

```python
class Node:
    __status__ = 'end', 'son'
    def __init__(self):
        self.son = {} # 用一个dict来存储其儿子节点
        self.end = False

class Trie:
    '''
        前缀树
    '''
    def __init__(self):
        self.root = Node()

    def insert(self, word: str) -> None:
        cur = self.root
        for char in word:
            if char not in cur.son:
                cur.son[char] = Node()
            cur = cur.son[char]
        cur.end = True

    def find(self, word):
        '''
            查找函数 
            return  0: 为找到
                    1: 找到但是结尾不是叶子节点 服务startsWith()
                    2: 找到且结尾是叶子结点  服务search()
        '''
        cur = self.root
        for char in word:
            if char not in cur.son:
                return 0
            cur = cur.son[char]
        return 2 if cur.end else 1

    def search(self, word: str) -> bool:
        return self.find(word) == 2

    def startsWith(self, prefix: str) -> bool:
        return self.find(prefix) != 0

# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```
