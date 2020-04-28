# Recursion

This guide is meant to go over the key ideas behind recursion.

## Motivating Recursion
Imagine you are standing in a really long line and you can't see the front, but you want to know what position you are in line. How would you go about doing that?

Well, you know that your position in line is one more than the position of the person in front of you in line. Thus, if the person in front of you knew their position, you could easily find out your position. With this ingenious plan, you ask the person in front of you what their position is. 

But the person in front of you also can't see the front of the line, so they don't know their position either. So they adopt your plan, and ask the person in front of them what their position in line is. The next person also doesn't know their position, so they too have to ask the person in front of them.

This process of asking continues until the first person in line is asked what position they're in. Since they're first, they can confidently say "I'm in position #1." Then the second person in line knows they're in position #2, the third knows they're position #3, and so on. Eventually, the person in front of you will figure out their position in line. Let's say they are in position #364. Since you know your position in line is one more than their position, you can correctly say you are in position #365.

The technique you used to find your position in line is called _recursion_. In computer science, recursion is a problem-solving technique that uses solutions to problems of smaller sizes to get the solution to problems of larger sizes. In the line example, the large problem was finding your position in line, and you used the solution to the smaller problem of the person in front of you finding their position to help you solve your problem.

More formally, recursion is when something is defined in terms of itself. I know that definition seems weird, but you've actually seen recursion before in math when you did sequences. Some sequences are defined recursively, which means that a given value in the sequence depends on previous values in the sequence. The Fibonacci sequence is one such example, and it's a classic example computer scientists use to introduce recursion. The Fibonacci sequence is:
$$ 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ... $$
where the $n$th number in the sequence is the sum of the $(n-1)$th and $(n-2)$th numbers in the sequence. You should have seen this in lab.

## Details of Recursion
Any recursive function has 2 main parts:
1) **Base Case**: the solution to the problem when the input is the simplest it can be
2) **Recursive Call**: how to split the problem with the current input into subproblems with smaller inputs

Let's return to the line example to see where the base case and recursive call come into play. Let's start off with the base case, which is the simplest case of the problem. In the line example, the simplest case of the problem was asking the first person in line what their position was, because they just knew their position by definition of being first in line! Therefore, the base case of the line example was the first person.

Now, let's look at the recursive call, where we figure out how to split the problem into smaller subproblems. Our original problem was finding our position in line, but we figured out that if we know the position of the person in front of us, we could easily get our position by adding one. Therefore, our recursive call was to ask the person in front of us what their position is and then add one to it.

This week's worksheet has really good examples of recursive functions, so I would highly recommend making sure you understand them. Recursion is also a favorite for midterm questions.

# Conclusion
If recursion isn't sticking with you as easily as the other concepts have, don't worry! Recursion is one of the hardest (if not, the hardest) concept you'll learn this semester. But like riding a bike, it's one of those things that when you get it, you will never forget it. As always, if you have any questions please don't hesitate to ask!
