# Iterators and Exceptions

This guide is meant to go over the key ideas of iterators and interfaces and point out common mistakes students make with these concepts.

## Interfaces Review
Remember that an interface is a _template_ for a class. We say that a class _implements_ an interface if the class provides an implementation for all the methods the interface requires.

**Common Mistake:** Many students (including myself) make the mistake of trying to instantiate an instance of an interface. Remember, you can only instantiate an instance of a _class_ and nothing else. Since an interface is not a class, you can't make an instance of an interface. Here's an example of what I mean:
```
Stack<Integer> s = new Stack();
```
The above code is incorrect, because `Stack` is an interface. A correct version of this instantiation would look like:
```
Stack<Integer> s = new ArrayStack();
```
This code will work because `ArrayStack` is a class that implements the `Stack` interface.

## Iterators
You should recall from 61A that an iterator is an object that returns the next element in some sequence. If this doesn't ring a bell for you, I would highly encourage you to stop here and either look back at [61B lecture slides](https://docs.google.com/presentation/d/1LIz15B3VmMMhllz1t5HNo9gkOPTGgmAynHNEt6Rzng0/edit#slide=id.g11a6f75162_0_250) or look at [61A material](http://composingprograms.com/pages/42-implicit-sequences.html) to get a conceptual understanding of what an iterator is. An iterator is a class that implements the `Iterator` interface, which looks like:
```
public interface Iterator<T> {
	public boolean hasNext();
	public T next();
}
```
In 61B, you are expected to know how to design your own iterator to have a certain behavior. Let's take a look at a simple example to see what designing an iterator looks like.

Our task is to design an iterator over the all the elements of an array. That is, we want to design an iterator such that every time we call `next()`, we get the next element of the array.

```
public class ArrayIterator implements Iterator<T> {
	private int currIndex;
	private T[] myArray;
	
	public ArrayIterator(T[] arr) {
		this.currIndex = 0;
		this.myArray = arr;
	}
	
	public boolean hasNext() {
		return currIndex < myArray.length;
	}
	
	public T next() {
		if hasNext() {
			T next = myArray[currIndex];
			currIndex += 1;
			return next;
		}
		return null;
	}
}
```
**Common Strategy:** Notice that I used `hasNext()` in the `next()` method. Many students implement `next()` without using `hasNext()`. It's not required to do this, but it makes the implementation easier. Logically, if you want to return the next item in a sequence, you should first check if the sequence even has another item to return.

**Common Strategy:** Notice I also used private fields `currIndex` and `myArr`. These variables help me compute the next item when I need to, and every iterator you design will have similar internal variables. _Don't forget to make these variables private!_

## Exceptions
You should have seen exceptions in 61A, so I expect that you remember conceptually what an exception is. To reiterate, an exception is a program's way of saying something is wrong. In Java, exceptions are handled using try-catch blocks.

I would normally give some common mistakes and walk through an example, but [these slides](https://docs.google.com/presentation/d/1j418bduZS2Ltm6dVVg-b3WpGbOGrRUm6DKRkZCByuaQ/edit#slide=id.p) do that extremely well and pretty much anything I would say is covered in the slides. These slides helped me learn exceptions when I was in the course, so I highly recommend you give them a read.

# Conclusions
I hope you found this guide useful. As always, if you have any questions please don't hesitate to ask!
