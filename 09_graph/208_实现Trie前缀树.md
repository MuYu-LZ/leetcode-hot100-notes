# 208. 实现 Trie（前缀树）

## 题目
Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当好的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：
- Trie() 初始化前缀树对象。
- void insert(String word) 向前缀树中插入字符串 word 。
- boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
- boolean startsWith(String prefix) 如果之前已插入的字符串 word 的前缀之一为 prefix，返回 true ；否则，返回 false 。

示例:
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

## 思路
该题使用**树形结构**实现 Trie：
1. 每个节点包含一个哈希表/字典，存储子节点（以字符为键）
2. 每个节点还需一个标志表示是否是完整单词的结尾
3. insert：从根节点逐个插入字符，创建必要的节点
4. search：从根节点逐个查找字符，最后检查是否是单词结尾
5. startsWith：与 search 类似，但不检查单词结尾标志

## Python 代码

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False

class Trie:
    def __init__(self):
        # 1. 初始化根节点
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        # 2. 从根节点开始遍历
        node = self.root
        for char in word:
            # 3. 如果字符不存在，创建新节点
            if char not in node.children:
                node.children[char] = TrieNode()
            # 4. 移动到子节点
            node = node.children[char]
        # 5. 标记单词结尾
        node.is_end = True
    
    def search(self, word: str) -> bool:
        # 6. 查找单词
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        # 7. 检查是否是单词结尾
        return node.is_end
    
    def startsWith(self, prefix: str) -> bool:
        # 8. 检查前缀
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        # 9. 只要前缀存在就返回 True
        return True

```

## 易错点
1. **is_end 标志的必要性**：区分前缀和完整单词。例如 "apple" 和 "app"，app 虽然是 apple 的前缀，但不是独立的单词
2. **search vs startsWith**：search 需要检查 is_end，startsWith 不需要
3. **节点的创建**：只在 insert 时创建节点，search 和 startsWith 只进行查找
4. **时间复杂度**：insert、search、startsWith 都是 O(L)，其中 L 是字符串长度
