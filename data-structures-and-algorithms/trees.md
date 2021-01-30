## Trees

- Instead of predecessor-successor there are predecessor and two successors
- Tree gives an hierarchical order for elements
- Binary tree is good for search structure
- Generic tree is good for presenting hierarchical data
- Tree is made of nodes
- Path is a line of nodes
- Height of the node is the longest path from node to a leaf node
- Height of the tree is the height of the root
- Depth is the length of the path from node to root
- Leaf node is a node without children
- Branch node is a node with children
- Orders of a tree:
  - Inorder
    1. Left branch
    2. Root
    3. Right branch
  - Preorder
    1. Root
    2. Left branch
    3. Right branch
  - Postorder
    1. Left branch
    2. Right branch
    3. Root

### Binary tree

- Amount of children is limited to two
- Benefits:
  - Straightforward to implement
  - Proceeding into one of the two children based on one comparison (time complexity O(height of the tree))
- Operations:
  - Create an empty binary tree
  - Create an unattached root node with an element x
  - getRoot(): returns the root node
  - setRoot(): sets the root node
  - getParent(): return the parent of a node n
  - getLeftChild()
  - getRightChild()
  - getElement()
  - setLeftChild()
  - setRightChild()
  - killNode(): removes and releases node n from tree T as well as all children and successors

```java
  /**
  * Returns the first node of inordered binary tree.
  *
  * @param T Tree.
  * @param <E> Type of the elements
  * @return The first node in inorder or null if the tree is empty
  */
  @Override
  public <E> BTreeNode<E> inorderFirst(BTree<E> T) {

      // check if tree is empty
      if(T.isEmpty()) return null;

      // get root and check if it is the first
      BTreeNode<E> root = T.getRoot();
      if(root.getLeftChild() == null) return root;

      // get child and loop until there is no left child
      BTreeNode<E> child = root.getLeftChild();

      while(child.getLeftChild() != null) {
          child = child.getLeftChild();
      }

      return child;
  }

  /**
    * Returns the successor node in inordered binary tree
    *
    * @param n Node of the binary tree
    * @return Successor node or null if there is no successor
    */
  @Override
  public <E> BTreeNode<E> inorderSuccessor(BTreeNode<E> n) {

      // if no right child and no parent, no successor
      if(n.getRightChild() == null && n.getParent() == null) {
          return null;
      }

      // if no right child, traverse upwards until previous is left child of parent
      if(n.getRightChild() == null) {
          BTreeNode<E> current = n;
          BTreeNode<E> previous;

          while(current.getParent() != null) {
              previous = current;
              current = current.getParent();
              if(current.getLeftChild() == previous) {
                  return current;
              }
          }

          // if conditions not met, return null (applies only to the last node of the tree)
          return null;
      }

      // get the most left child of the right child
      BTreeNode<E> child = n.getRightChild();

      while(child.getLeftChild() != null) {
          child = child.getLeftChild();
      }

      return child;
  }
}

```

### Search trees

- Searching, adding and removing are O(height of the tree)

### AVL-tree

- The balance (difference of heights) between left and right branches cannot be bigger than one
- Balance is broken if it is -- or ++
- Left --, right ++
- Balancing an AVL-tree

  - If the balance is lost, it's fixed only on the specific node
  - Perform a rotation on the unbalanced node
  - Two types of rotations (and to both directions)
    - If balance of node is -- and balance of left child is -, rotation to the right
    - If nalance of node is -- and balance of left child is +, double-rotation to right (left-right-rotation)
    - If balance of node is ++ and balance of right child is +, rotation to the right
    - If balance of node is ++ and balance of right child is -, double-rotation to left (right-left-rotation)
  - Rotation stops the balance error, one rotation is enough
  - Other than nodes, which participate the rotation, are left untouched
  - All rotations are done in constant time (4-8 updates of references)
    - No rebuilding the tree
  - Removal is O(logn) because rotation might have to be performed on multiple levels

### Generic tree

- E.g. organisation chart, process flow, game situtations
- Can be implemented with a linked list
  - The children of an element are a linked list, which are containing the information of the first, the last and the next as well as their children
