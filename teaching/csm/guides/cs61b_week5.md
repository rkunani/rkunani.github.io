# Trees and Hashing
This guide is meant to go over the key concepts and common mistakes about trees (binary, BST, balanced) and hashing.

## Trees
### Relationship between Balance and Binary Trees
**Common Mistake:** Many students mistake binary trees for BSTs. This is incorrect. As we covered in section, a binary tree is _any_ tree in which each node has at most 2 children. A BST is a binary tree in which each node's left child has a value smaller than the parent node's value, and the right child's value is greater than the parent node's value.

There was some confusion on how to answer Question 1.1 on this week's worksheet. Some of you asked if you could convert a tree into its BST form to answer the question. I wasn't sure during section if this was a correct approach, but after looking at the solutions I've determined that you can't answer the question by seeing whether the binary tree has the same height as a BST with the same elements. The question wasn't worded well, but essentially the question is asking whether the given binary tree has the same height as the optimal binary tree with the same elements. We know that an optimal binary tree must be balanced, so one way to answer the question is to check whether each binary tree is balanced. Another similar way to answer the question is to try to move leaf nodes into open spaces at higher levels of the tree and seeing if you could reduce the height.

**Key Takeaway:** An optimal tree with a given set of elements must be balanced.

### Balanced Trees
#### 2-3 Trees
A 2-3 tree is a type of tree in which each node can hold up to 2 values and have up to 3 children. I like to think of a 2-3 tree as a special kind of BST, because it has the same property of a BST regarding values less than the parent going on the left and values greater than the parent going on the right.

Let's say we wanted to insert $X$ into a 2-3 tree. These are the steps:
1. Find the node in the 2-3 tree where $X$ would go if the tree was a BST. Let's call that node $N$. Put $X$ in the proper place in $N$ according to its value compared to the other values already in $N$.
2. If step 1 causes $N$ to have 3 values, push the middle value into the parent node (we'll call it $P$), and split 2 remaining values of $N$ into 2 separate nodes which will be children of $P$.
3. Repeat step 2 on $P$ if it has more than 2 values.

**Key Takeaway:** 2-3 trees have some other properties that might be useful to know. This [Wikipedia page](https://en.wikipedia.org/wiki/2%E2%80%933_tree) does a good job of covering them.

#### Left-Leaning Red Black (LLRB) Trees
A left-leaning red black tree is a BST with 2 kinds of edges (connections between nodes). LLRBs are the way 2-3 trees are implemented. An LLRB represents a node in a 2-3 tree that has 2 values as 2 nodes with a red edge between them. All other edges are black.

To convert a 2-3 tree to an LLRB:
1. Find a node $N$ that has 2 values. Split the 2 values of $N$ into 2 nodes $X_1$ and $X_2$.
2. Push the lower of $X_1$ and $X_2$ to one level lower in the tree, and draw a red edge between $X_1$ and $X_2$.
3. The node that got pushed to a lower level takes node $N$'s left and middle children, and the node that stayed at the same level takes node $N$'s right child.
4. Repeat for all nodes that have 2 values.

**Key Takeaway**: LLRBs are only one way to represent 2-3 trees. We could have just as easily used an RLRB (right-leaning red black tree). Think about how converting a 2-3 tree into an RLRB would be different than converting to an LLRB.

## Hashing
Hashing is a way of storing a collection of items in a way that allows for quick retrieval. The basic idea is, instead of storing all the items in a list, store them all in an array of fixed length $M$. To do this, assign each item a number $X$ between $0$ and $M-1$, and put the item in the list of items stored at index $X$ of the array. The assignment of the number $X$ to the item is called _hashing_ the item.

### HashMap
`HashMap<K, V>` is the main way the `Map<K, V>` ADT is implemented. The basic idea of `HashMap` is that you hash the key, then store the value (with the key as a label) in the proper index of the underlying array.

#### Runtime Analysis of HashMaps
The entire purpose of hashing is to make retrieval of an item a fast operation, let's see how well the `HashMap` lives up to the challenge.

If you have $N$ elements stored in a `HashMap` with $M$ buckets, you'll have $M$ lists, each with $\frac{N}{M}$ items (assuming you have a good `hashCode()`, which we will talk about later). So, in order to retrieve an item, you'd have to search a list of length $\frac{N}{M}$, which is definitely an improvement from storing all the items in a list, where retrieval would require searching a length $N$ list. However, since $M$ is a constant number, $\frac{N}{M}$ is still $\Theta(N)$, so it's not improving the asymptotic runtime of retrieval.

What if $M$ was not a constant number? Then $\frac{N}{M}$ has a chance of being something less than $\Theta(N)$ and we'd be chillin in terms of retrieval time. Specifically, if $M$ is $\Theta(N)$, then $\frac{N}{M}$ is $\Theta(1)$, meaning retrieval of an item would only take constant time since we would only have to search a list of constant size!
 
#### Hashcodes
The worksheet solutions do a great job of explaining what a valid hashcode is, so I highly suggest you read those. What I want to talk about is the dangers of using `HashMap` to store mutable data. These dangers arise because of a problem with `hashCode`.

Consider the following class:
```
public class A {
	public int x;
	public int y;
	public A (int x, int y) {
		this.x = x;
		this.y = y;
	}
	public int hashCode {
		return this.x + this.y;
	}
}
```

Since the `x` and `y` fields are public, they can be changed and thus `A` is mutable. Let's say we have a specific instance of `A`: `a = A(1, 2);` that we put it into a `HashMap`. We hash `a` to get the hashCode $3$, and then put `a` into the `HashMap` at index $3$. Later on, we want to check if `a` is in the `HashMap`, but `a.x` is now $3$ and `a.y` is now $7$. To check if `a` is in the `HashMap`, we hash `a` to get the hashCode $10$, then check at index $10$ of our `HashMap` if `a` is there. Since `a` was originally put into the `HashMap` at index $3$, we won't find `a` at index $10$. Thus, we would incorrectly conclude that `a` is not in the `HashMap`.

What went wrong? With mutable types, you no longer have the guarantee that calling `hashCode()` on an object multiple times will return the same value. This is the root cause of the problem above.

**Common Mistake:** Many students think that the above danger implies that hashCodes are invalid for mutable objects. This is untrue for a subtle reason. The deterministic condition for hashCode only applies to objects that are truly the same object. Since a mutable object changes, when we call `hashCode()` on a mutable object multiple times, we are calling `hashCode()` on objects that aren't really the same object. 

# Conclusion
Now that you're getting introduced to so many data structures, it can be easy to get lost in trying to remember all of them. While it is possible to memorize all the data structures, their properties, their associated insertion/deletion/etc. procedures, and their runtimes, it will serve you a lot better to understand at a conceptual level what each data structure is accomplishing, and the rest of the information will follow naturally. 
