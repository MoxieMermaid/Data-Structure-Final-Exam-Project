# Splay Trees
### By: Xavier Agostino

> Definition: A binary search with one additional property where it splays every item to the root of the tree for every operation.



![Logo](https://www.link.cs.cmu.edu/splay/tree5.jpg)


## Authors

- [@MoxieMermaid](https://github.com/MoxieMermaid)


## Find
* Looks for item you want and when found item is splayed
```java
private Node find(int thing, Node curr){
		if (curr == null){
			return null;
		}
		if(curr.data == thing){
			splay(curr);
			return curr;
		}else{
			if (thing > curr.data){
				curr = curr.right;
			}else{
				curr = curr.left;
			}
			Node found = find(thing,curr);
			return found;
		}
	}

	public int find(int thing){
		return find(thing, root).data;
	}
```
## Insert 
* Does Binary Search Tree insert and then splays inserted item
```java
public boolean insert(int thing){
		if(root == null){
			root = new Node(thing);
			return true;
		}else{
			Node curr = root;
			boolean added = false;
			while(!added){
				if (thing < curr.data){
					//I want to be on the left
					if(curr.left == null){
						curr.left = new Node(thing);
						curr.left.parent = curr;
						splay(curr.left);
						added = true;
					}else{
						curr = curr.left;
					}
				}else if(thing > curr.data){
					//I want to be on the right
					if(curr.right == null){
						curr.right = new Node(thing);
						curr.right.parent = curr;
						splay(curr.right);
						added = true;
					}else{
						curr = curr.right;
					}
				}else{
					return false;
				}
			}
			return true;
		}
	}
```
## Splay
* Takes in node and the move that node to the root of the tree
```java
private void splay(Node num){
		while(num.parent != null){
			if(num.parent.parent == null){
				if(num == num.parent.left){
					//Right Rotate
					zig(num.parent);
				}else{
					//Left Rotate
					zag(num.parent);
				}
			}else if(num == num.parent.left && num.parent == num.parent.parent.left){
				//Right-Right Rotations
				zig(num.parent.parent);
				zig(num.parent);
			}else if(num == num.parent.right && num.parent == num.parent.parent.right){
				//Left-Left Rotations
				zag(num.parent.parent);
				zag(num.parent);
			}else if(num == num.parent.right && num.parent ==  num.parent.parent.left){
				//Left-Right Rotation
				zag(num.parent);
				zig(num.parent);
			}else{
				//Right-Left Rotation
				zig(num.parent);
				zag(num.parent);
			}
		}
		
	}
```

## Delete
* takes in a value, finds value, deletes the value, and then tree is split into two trees and finds maximum of left subtree and then splays that maximum value and then connects right pointer to right subtree
```java
public void delete(int key){

		if (root == null){
			return;
		}

		Node rightTree = null; 
		Node leftTree = null;
		Node me = find(key, root);
		System.out.println("We just splayed the: " + key + " " + preOrder());

		if (me == null) {
			System.out.println("Key isn't present in the tree");
			return;
		}
		// split operation
		if (me.right != null) {
			rightTree = me.right;
			rightTree.parent = null;
		} else {
			rightTree = null;
		}
		leftTree = me.left;
		me = null;

		// join operation
		if (leftTree != null){ // remove me
			leftTree.parent = null;
		}
		root = join(leftTree, rightTree);
	}
```

## Join
* Combines left and right subtrees after deletion of node
```java
private Node join(Node leftTree, Node rightTree){
		if (leftTree == null) {
			return rightTree;
		}

		if (rightTree == null) {
			return leftTree;
		}
		Node newRoot = maximum(leftTree);
		splay(newRoot);
		newRoot.right = rightTree;
		rightTree.parent = newRoot;
		return newRoot;
	}
```

## Zig-Rotation
*  every node moves one position to the right from its current position.
```java
private void zig(Node item){
		Node top = item.left;
		item.left = top.right;
		if (top.right != null) {
			top.right.parent = item;
		}
		top.parent = item.parent;
		if (item.parent == null) {
			this.root = top;
		} else if (item == item.parent.right) {
			item.parent.right = top;
		} else {
			item.parent.left = top;
		}
		top.right = item;
		item.parent = top;

	}
```

## Zag-Rotation
* every node moves one position to the left from its current position.
```java
private void zag(Node item){
		Node top = item.right;
		item.right = top.left;
		if (top.left != null) {
			top.left.parent = item;
		}
		top.parent = item.parent;
		if (item.parent == null) {
			this.root = top;
		} else if (item == item.parent.left) {
			item.parent.left = top;
		} else {
			item.parent.right = top;
		}
		top.left = item;
		item.parent = top;

	}
```

## Maximum
* Looks for the node with the maximum value and returns it
```java
public Node maximum(Node node){
		Node curr = node;
		while (curr.right != null){
			curr = curr.right;
		}
		return curr;
	}
```

## Pre-Order
* Node -> Left-> Right
```java
public String preOrder(){
		return "Pre-Order: " + preOrder(root);

	}

	private String preOrder(Node curr){
		if (curr == null){
			return "";
		}else{
			int n = curr.data;
			String left = preOrder(curr.left);
			String right = preOrder(curr.right);
			return n + ", " + left + right;
		}
	}
```

## In-Order
* Left -> Node -> Right
```java
public String inOrder(){
		return "In Order: " + inOrder(root);
	}

	private String inOrder(Node curr){
		//Left, Node, Right
		if(curr == null){
			return "";
		}else{
			return inOrder(curr.left) + curr.data + ", " + inOrder(curr.right);
		}

	}
```

## Post-Order
* Node -> Left-> Right
```java
public String postOrder(){
		return "Post Order: " + postOrder(root);
	}

	private String postOrder(Node curr){
		if(curr == null){
			return "";
		}else if(curr.right == null && curr.left == null){
			return curr.data + ", ";
		}else{
			int n = curr.data ;
			String left = postOrder(curr.left);
			String right = postOrder(curr.right);

			return left + right + n + ", ";
		}
	}
```

## 💻 Programming Language
Java...


## How to use

- For every operation input the required parameter asked for.(Node, Integer, etc...)

