# 9th-week-plan-and-code
------------------------------------------------------
Week 9: Advanced Trees, Heaps, Tries
------------------------------------------------------
Day 57: BST insert/search/delete (LC #701, #700)
Day 58: Lowest common ancestor (LC #236)
Day 59: Heap basics, top K elements (LC #347, #973)
Day 60: Median from data stream (LC #295)
Day 61: Trie basics, implement insert/search
Day 62: Word dictionary problem (LC #208)

"""
Day 57: Binary Search Tree – Insert, Search, Delete (LeetCode #701, #700)
Author: [Your Name]
Date: [Today's Date]

Problems Covered:
1️⃣ LC #701 – Insert into a BST
2️⃣ LC #700 – Search in a BST
3️⃣ Delete a node in a BST (standard problem)
"""

# ----------------------------------------------------
# 🧩 BST Node Definition
# ----------------------------------------------------
class TreeNode:
    def __init__(self, val=0):
        self.val = val
        self.left = None
        self.right = None


# ----------------------------------------------------
# 1️⃣ LC #700 – Search in a BST
# ----------------------------------------------------
"""
Problem:
Given the root of a BST and a value, return the node where the value exists.
If not found, return None.

Approach:
- BST property: 
   Left < Root < Right
- Compare the target with current node value:
   → If smaller, go left
   → If larger, go right
   → If equal, return node

Time Complexity: O(h)
Space Complexity: O(1) (iterative) or O(h) (recursive)
"""

def searchBST(root, val):
    if not root:
        return None
    if root.val == val:
        return root
    elif val < root.val:
        return searchBST(root.left, val)
    else:
        return searchBST(root.right, val)


# ----------------------------------------------------
# 2️⃣ LC #701 – Insert into a BST
# ----------------------------------------------------
"""
Problem:
Insert a value into the correct position in a BST and return the root.

Approach:
- Traverse recursively until we find a null position.
- Insert the new node there.

Time Complexity: O(h)
Space Complexity: O(h)
"""

def insertIntoBST(root, val):
    if not root:
        return TreeNode(val)
    if val < root.val:
        root.left = insertIntoBST(root.left, val)
    else:
        root.right = insertIntoBST(root.right, val)
    return root


# ----------------------------------------------------
# 3️⃣ Delete Node in a BST
# ----------------------------------------------------
"""
Problem:
Delete a given key from BST and return the root.

Cases:
1️⃣ Node not found → return None
2️⃣ Node has 0 children → remove it
3️⃣ Node has 1 child → replace with the child
4️⃣ Node has 2 children → replace with inorder successor (smallest in right subtree)

Time Complexity: O(h)
Space Complexity: O(h)
"""

def deleteNode(root, key):
    if not root:
        return None
    
    if key < root.val:
        root.left = deleteNode(root.left, key)
    elif key > root.val:
        root.right = deleteNode(root.right, key)
    else:
        # Found the node to delete
        if not root.left:
            return root.right
        elif not root.right:
            return root.left

        # Find inorder successor (smallest in right subtree)
        temp = root.right
        while temp.left:
            temp = temp.left
        root.val = temp.val
        root.right = deleteNode(root.right, temp.val)
    return root


# ----------------------------------------------------
# 🌳 Helper: Inorder Traversal (for debugging)
# ----------------------------------------------------
def inorder(root):
    return inorder(root.left) + [root.val] + inorder(root.right) if root else []


# ----------------------------------------------------
# Example Usage
# ----------------------------------------------------
if __name__ == "__main__":
    # Create initial BST
    root = TreeNode(4)
    root.left = TreeNode(2)
    root.right = TreeNode(7)
    root.left.left = TreeNode(1)
    root.left.right = TreeNode(3)

    print("Initial BST (Inorder):", inorder(root))

    # Insert
    root = insertIntoBST(root, 5)
    print("After Insert 5:", inorder(root))

    # Search
    node = searchBST(root, 2)
    print("Search 2 ->", "Found" if node else "Not Found")

    # Delete
    root = deleteNode(root, 3)
    print("After Deleting 3:", inorder(root))


# ----------------------------------------------------
# 🔑 Key Notes:
# ----------------------------------------------------
# ✅ BST property simplifies searching & insertion.
# ✅ Deletion uses inorder successor (right subtree’s smallest node).
# ✅ Fundamental for:
#     - Balanced trees (AVL, Red-Black)
#     - Binary search-based data structures
# ✅ Mastering BST helps in understanding recursion with conditions.
# ----------------------------------------------------


"""
🚀 Week 9: Advanced Trees, Heaps, and Tries
Days 58–63 | Author: [Your Name]
Date: [Today's Date]

Topics Covered:
✅ Day 58: Lowest Common Ancestor (LC #236)
✅ Day 59: Heap basics + Top K elements (LC #347, #973)
✅ Day 60: Median from Data Stream (LC #295)
✅ Day 61: Trie basics (Insert/Search)
✅ Day 62: Word Dictionary (LC #208)
✅ Day 63: Practice BST/Heap/Trie Problems
"""

# ----------------------------------------------------
# 🌳 Day 58 – Lowest Common Ancestor of a Binary Tree (LC #236)
# ----------------------------------------------------
"""
Find the lowest common ancestor (LCA) of two nodes in a binary tree.
"""

class TreeNode:
    def __init__(self, val=0):
        self.val = val
        self.left = None
        self.right = None

def lowestCommonAncestor(root, p, q):
    """
    Approach:
    - Base case: if root is None or equals p or q → return root
    - Recur left and right.
    - If both left and right return valid nodes → root is LCA
    - Else return non-null one.
    """
    if not root or root == p or root == q:
        return root
    left = lowestCommonAncestor(root.left, p, q)
    right = lowestCommonAncestor(root.right, p, q)
    if left and right:
        return root
    return left or right


# ----------------------------------------------------
# 💎 Day 59 – Heap Basics + Top K Problems
# ----------------------------------------------------
import heapq

# LC #347 – Top K Frequent Elements
def topKFrequent(nums, k):
    """
    Count frequencies and use min-heap to track top K frequent elements.
    """
    from collections import Counter
    freq = Counter(nums)
    heap = []
    for num, count in freq.items():
        heapq.heappush(heap, (count, num))
        if len(heap) > k:
            heapq.heappop(heap)
    return [num for count, num in heap]


# LC #973 – K Closest Points to Origin
def kClosest(points, k):
    """
    Use max-heap to store points by distance from origin.
    """
    heap = []
    for (x, y) in points:
        dist = -(x*x + y*y)
        heapq.heappush(heap, (dist, x, y))
        if len(heap) > k:
            heapq.heappop(heap)
    return [(x, y) for (dist, x, y) in heap]


# ----------------------------------------------------
# ⚖️ Day 60 – Median from Data Stream (LC #295)
# ----------------------------------------------------
"""
Maintain two heaps:
- maxHeap for left half
- minHeap for right half
Keep them balanced so their sizes differ by at most 1.
"""
class MedianFinder:
    def __init__(self):
        self.small = []  # maxHeap (store negatives)
        self.large = []  # minHeap

    def addNum(self, num):
        heapq.heappush(self.small, -num)
        if self.small and self.large and (-self.small[0]) > self.large[0]:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)

    def findMedian(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        if len(self.large) > len(self.small):
            return self.large[0]
        return (-self.small[0] + self.large[0]) / 2


# ----------------------------------------------------
# 🔠 Day 61 – Trie Basics (Insert and Search)
# ----------------------------------------------------
"""
A Trie (Prefix Tree) is used for efficient prefix and word search.
"""
class TrieNode:
    def __init__(self):
        self.children = {}
        self.endOfWord = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                cur.children[ch] = TrieNode()
            cur = cur.children[ch]
        cur.endOfWord = True

    def search(self, word):
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                return False
            cur = cur.children[ch]
        return cur.endOfWord

    def startsWith(self, prefix):
        cur = self.root
        for ch in prefix:
            if ch not in cur.children:
                return False
            cur = cur.children[ch]
        return True


# ----------------------------------------------------
# 📚 Day 62 – Word Dictionary (LC #208)
# ----------------------------------------------------
"""
Implements a Trie that supports '.' wildcard character in search.
"""
class WordDictionary:
    def __init__(self):
        self.root = TrieNode()

    def addWord(self, word):
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                cur.children[ch] = TrieNode()
            cur = cur.children[ch]
        cur.endOfWord = True

    def search(self, word):
        def dfs(j, node):
            cur = node
            for i in range(j, len(word)):
                ch = word[i]
                if ch == '.':
                    for child in cur.children.values():
                        if dfs(i + 1, child):
                            return True
                    return False
                else:
                    if ch not in cur.children:
                        return False
                    cur = cur.children[ch]
            return cur.endOfWord
        return dfs(0, self.root)


# ----------------------------------------------------
# ⚙️ Day 63 – Practice Problems (BST + Heap + Trie)
# ----------------------------------------------------
"""
Review tasks:
- Insert/search in BST
- Top K problems
- Median stream
- Word search using Trie
"""
if __name__ == "__main__":
    print("🔹 Day 58 – LCA:")
    root = TreeNode(3)
    root.left, root.right = TreeNode(5), TreeNode(1)
    root.left.left, root.left.right = TreeNode(6), TreeNode(2)
    print("LCA of 5 and 1:", lowestCommonAncestor(root, root.left, root.right).val)

    print("\n🔹 Day 59 – Top K Frequent Elements:")
    print(topKFrequent([1,1,1,2,2,3], 2))  # [1,2]

    print("\n🔹 Day 59 – K Closest Points:")
    print(kClosest([[1,3],[-2,2],[5,8]], 2))  # [[-2,2],[1,3]]

    print("\n🔹 Day 60 – Median Finder:")
    mf = MedianFinder()
    for num in [1, 2, 3]:
        mf.addNum(num)
    print("Median:", mf.findMedian())

    print("\n🔹 Day 61 – Trie Operations:")
    trie = Trie()
    trie.insert("apple")
    print("Search 'apple':", trie.search("apple"))
    print("StartsWith 'app':", trie.startsWith("app"))

    print("\n🔹 Day 62 – Word Dictionary:")
    wd = WordDictionary()
    wd.addWord("bad")
    wd.addWord("dad")
    print("Search 'b..':", wd.search("b.."))


# ----------------------------------------------------
# 🧠 Key Takeaways:
# ----------------------------------------------------
# ✅ BST → Efficient search using ordered structure
# ✅ Heap → Prioritization and streaming data (Top K / Median)
# ✅ Trie → Efficient word and prefix searches
# 🚀 These data structures are core to real-world systems:
#     - Search engines (Trie)
#     - Recommendation systems (Heap)
#     - Binary indexed storage (BST)
# ----------------------------------------------------
