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
