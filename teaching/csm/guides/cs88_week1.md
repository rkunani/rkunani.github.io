# Week 1: Control, Loops, Functions

This guide is meant to build the fundamentals of and point out common misconceptions in control, loops, and functions. I hope you find this useful!

## Expressions
Before we can get to control, loops, and functions, I want to take a moment to cover expressions because expressions are the one of the most fundamental building blocks of code. An expression is anything that the computer can _evaluate_ to a value. There's a reason why I emphasized _evaluate_: when your computer runs code, all it's doing is evaluating a bunch of expressions. The result of these evaluations is what you see as output. 

Ok, but what does an expression actually look like? Well, `2` is an expression, which just evaluates to the integer 2. `2` is an example of what we call a **primitive expression**. Primitive expressions are the most basic type of expression because they only take 1 step to evaluate. In other words, when the computer sees `2`, it knows that it's just the integer 2.

Other examples of primitive expressions are _strings_ and _variables_. For example, `"hi"` is a string which the computer knows is just the string "hi". Likewise, if you assign a variable, the computer knows immediately what that variable is when you ask the computer to evaluate the variable:
```
>>> a = 0
>>> a
0
```

Expressions can be more complicated, though. `2 + 3` is such an example. In order to evaluate `2 + 3`, the computer needs to evaluate `2`, then `+`, then `3`. After it determines that `2` is 2, `+` is the add operator, and `3` is 3, then it can determine what happens when you add 2 and 3. The output is, of course, 5.

The last thing I want to say about expressions is that they can be nested and involve variables too! This is best illustrated by an example:
```
>>> a = 1
>>> b = 2
>>> (a + b) * (a - b)
```
What do you think will be the output? Let's walk through it. First, the computer sees `(a + b)`. This will evaluate to 3. Then the computer sees `*`, which is the multiply operator. Next, the computer sees `(a - b)`, which will evaluate to -1. Finally, the computer combines it all and computes `3 * -1`, which is just -3.

The most important thing to remember about expressions is that they are just statements that get evaluated by the computer to give some output. They might look scary sometimes, but if you just look at each individual piece of an expression at a time, you can figure out what it evaluates to.

## Control
By now you've been introduced to the idea of **boolean expressions** (e.g. `x > 1`), so I'm not going to go over what those mean in detail. At a high level, boolean expressions are expressions that evaluate to either `True` or `False`. 

> Remember that, in Python, all numbers are `True` except 0, which is `False`. Additionally, all strings are `True` except the empty string `''`, which is `False`.

What's important about boolean expressions is they allow you to write if-statements. You should already be familiar with if-statements, but there are some common misconceptions about how if-statements work that we went over in section. I'll recap them here.

As a reminder, an **if-statement** is comprised of 3 parts:
1. **if** (e.g. `if x < 1:`)
2. **elif** (e.g. `elif x < 5:`)
3. **else** (e.g. `else:`)

**Common Mistake:** Many students get confused when they see an `if` without an `elif` or an `else`. It is important to note that the `elif` and the `else` are _optional_; they are not necessary for an `if` statement to be valid.

**Common Mistake:** Another common mistake students make when it comes to if-statements is they execute the code in multiple parts of an if-statement. This mistake is best shown with an example:
```
x = 0
if x < 10:
    print('ayy')
elif x < 5:
    print('lmao')
else:
    print('dank')
```
Since `x < 10` and `x < 5` are both `True`, many students would say the output is:
```
ayy
lmao
```
_Don't fall into this trap_. You can only execute the code inside **one** of the `if`, `elif`, and `else` blocks. In the case of the above example, the correct output is:
```
ayy
```
This is because, after you evaluate `x < 10` to `True` (because 0 is less than 10), you enter the `if` block and execute all the code inside it. That's why ayy gets printed to the screen. _Since you executed the code in the `if` block, you don't execute the code in the `elif` or `else` blocks; just skip them_.

**Common Mistake**: Many students tend to misapply the rule above about only executing only one part of an if-statement. If there are multiple if-statements, treat them separately. I'll slightly modify the previous example to illustrate what I mean:
```
x = 0
if x < 10:
    print('ayy')
elif x < 5:
    print('lmao')
if x <= 0:
    print('dank')
```
What's the output? Let's apply the rules we know. Starting at the top of the code, we define a variable `x` to be 0. Then we come across an if-statement, so we start our process for executing if-statements. First, we evaluate `x < 10` to `True`. Thus, we will execute the code in the `if` block, which prints ayy to the screen. _Since we executed the code inside the `if` block, we are done with this if-statement._ This means we skip the `elif` and move onto the next part of the code, where we encounter another (_separate_) if-statement. Therefore, we restart our procedure for executing if-statements. First, we evaluate `x <= 0` to `True` and thus execute all the code in the `if` block, which prints dank to the screen. Since there are no `elif` or `else` blocks, we are done! The final output is:
```
ayy
dank
```

The purpose of if-statements is to shift **control** of your program. In less computer science-y terms, if-statements allow you to have more control of exactly what code your computer will run. In the above example, there is no way that both ayy and lmao can get printed to the screen (can you see why?); it depends on what value `x` takes on. In this way, you can control what gets printed by setting `x` to different values. I know in section we talked a little about what control is conceptually, but I didn't do the best job of explaining it. Hopefully this gives you a better idea of what control is.

## Functions
You may have heard about functions in math before, for example $f(x) = x^2$. But what do functions have to do with programming? As it turns out, functions --- like expressions --- are also crucial building blocks for code. But before we even talk about functions, let's motivate why functions are even desirable in the first place.

### Motivation for Functions
Let's say you are writing code that finds the slope between 2 points ($x_1$, $y_1$) and ($x_2$, $y_2$). Using the formula:

$$ \text{slope} = \frac{y_2 - y_1}{x_2 - x_1} $$

you might write some code like:
```
x1 = 2
y1 = 3
x2 = 5
y2 = 1
slope = (y2 - y1) / (x2 - x1)
print(slope)
```
This code is perfectly correct. But what if you wanted the slope between a different 2 points? You would have to change the values of `x1`, `y1`, `x2`, and `y2`, and then run your code again to get the new slope. But then you lose your old slope! This brings up 2 issues with the current code for calculating slope:

1. The code isn't reusable. If you want to get the slope between 2 new points, you need to rewrite some parts of the code.
2. The code doesn't let you save previous slopes.

This is where functions come into play. Instead of writing code that gives _specific_ values to `x1`, `y1`, `x2`, and `y2`, we'll write code that lets `x1`, `y1`, `x2`, and `y2` take on any value. In other words, we'll write code that tells the computer what the formula for slope is, so we can just give the computer our desired points ($x_1$, $y_1$) and ($x_2$, $y_2$) and it will calculate the slope for us. This is analogous to having a friend who is an expert at calculating slopes, and every time you need the slope between 2 points you just ask that friend what the slope between those 2 points is. Let's see what our new and improved code looks like:
```
def slope(x1, y1, x2, y2):
	s = (y2 - y1) / (x2 - x1)
	print(s)
```
Let's deconstruct what's going on here, and I'll draw an analogy to the math function $f(x) = x^2$ to make the concept easier to see. 

When you define a function in math, you have to give the function 2 things: a _name_ and an _input_. In the example of $f(x) = x^2$, the name of the function is $f$ and the input is $x$. Now let's look at our example code. The first line that says `def slope(x1, y1, x2, y2):` is just telling the computer "Hey, I'm defining a function with the name `slope` and input `x1`, `y1`, `x2`, and `y2`."

In math, after you specify a name and input for a function, you have to say what the function actually does to the input to get to the output. In the case of $f(x) = x^2$, the function squares the input to give the output, which we write in math as $x^2$. Now, let's look at our example code. The second and third lines that say `s = (y2 - y1) / (x2 - x1)` and `print(s)` tells the computer "Hey, remember that function named `slope` that I just told you about? Here's what I want `slope` to do with its inputs: divide `(y2 - y1)` by `(x2 -x1)`, and print out the resulting value as the output." 

Using `slope`, we can find the slope between any 2 points! Here's how it looks:
```
>>> slope(2, 3, 5, 9)
0.5
>>> slope(0, 0, 1, 1)
1
>>> slope(3, 4, 18, 9)
3
```
Notice that in our new usage of `slope`, we resolved both issues we found with our old code. We were able to reuse `slope` with different numbers to get different slopes without having to rewrite the code, and we were able to save previous slope calculations. Hopefully this slope example showed you why functions are useful in programming: they help us avoid having to rewrite code by making our code reusable.

### Details of Functions
Now that you have some motivation behind what a function does, let's take a look at some of the details of defining and using a function.

#### Defining a Function
We already said above that `def slope(x1, y1, x2, y2):` tells the computer to define a function called `slope` with the inputs `x1`, `y1`, `x2`, and `y2`. In programming, we call these inputs **parameters** of the `slope` function.

However, in the second and third lines telling the computer what to do with the parameters to get the output, we told the computer to divide `(y2 - y1)` by `(x2 -x1)` and `print` the result. While this shows the correct output to the screen, it actually doesn't do what exactly what we want. Let me show you what I mean:
```
>>> slope(3, 4, 18, 9)
3
>>> s = slope(3, 4, 18, 9)
3 # note that 3 is printed, not returned
>>> s
>>>
```
What? If `slope` is supposed to calculate the slope between the given points, then why doesn't `s` evaluate to 3?

_In Python, when we tell the computer what the output of a function should be, we have to use a `return` statement (e.g. `return 1`). The output of the function will then be whatever the expression (see how it all connects back to expressions?) inside the `return` statement evaluates to._ So if a function had a `return` statement that looked like `return 3 + 5`, the function would return 8.

Going back to the `slope` example, notice that `slope` doesn't have a `return` statement. But if the computer requires a `return` statement for it to know what the output of a function is, how did the computer know what the output of `slope` should be? _In Python, if a function doesn't have a `return` statement, the function returns `None`, a value that represents "nothing"._ Therefore, `slope` actually returned `None`, which is why evaluating `s` showed no output.

> Note: If an expression evaluates to `None`, no output will show on the screen if you ask the computer to evaluate that expression.

How can we fix `slope` to work properly? Instead of printing the result of dividing `(y2 - y1)` by `(x2 -x1)`, we should `return` the result:
```
def slope(x1, y1, x2, y2):
	s = (y2 - y1) / (x2 - x1)
	return s
```

Now `slope` will work as intended:
```
>>> slope(3, 4, 18, 9)
3
>>> s = slope(3, 4, 18, 9)
>>> s
3
```
> Semantic note on `print` vs. `return` with strings: `print("hi")` will show `hi` to the screen, while `return "hi"` will show `"hi"` to the screen.

#### Using Functions
Once you have a function, how do you use it? You should already be familiar with this concept, but there are nuances to it that are favorites for exam questions so I want to touch on those. We already saw with the `slope` example how to use a function: `slope(3, 4, 18, 9)`. To **call a function**, you write the function's name and, inside of parentheses, specify the values for the parameters of the function. Specifying the parameters is also known in computer science as *passing in **arguments** to the function*.

**Common mistake:** Many students confuse parameters and arguments. Parameters are the _variables_ a function takes as input. Arguments are the specific values that parameters take on during a specific call to the function. For example, `x1`, `y1`, `x2`, and `y2` are the parameters of `slope`, whereas `3`, `4`, `18`, and `9` are the arguments of `slope` when we call `slope(3, 4, 18, 9)`. 

When you call a function, you're writing an expression (it all ties back to expressions again) that the computer then evaluate to some value. These expressions are called **call expressions**, and the computer has a specific order of steps that it takes to evaluate call expressions.

To evaluate a call expression:
1. Evaluate the operator (function)
2. Evaluate the operand(s) (arguments)
3. Apply the operator to the operands

Let's look at an example to see these steps in action.
```
def square(x):
	return x * x

square(3 + 5)
```
You can probably already tell what the output of this code will be, but let's step through it like a computer would. First, we define a function `square`. Then we see a call expression that calls `square` with the argument `3 + 5`. Therefore, we must evaluate the call expression.

Step 1: Evaluate the operator (function). In this case the computer sees that the operator is the `square` function. Since we just defined the `square` function above, the computer knows what `square` is.

Step 2: Evaluate the operand(s) (arguments). The computer sees `3 + 5` and evaluates it to 8.

Step 3: Apply the operator to the operand(s). In this case, we are applying the `square` function on the input 8. Since this returns 64, we say that `square(8)` evaluates to 64 and 64 gets outputted to the screen.

In order to use a function, you have to write a call expression for that function on some arguments. In order to figure out what that call expression evaluates to, follow the 3 steps above.


#### Print vs. Return in Functions
**Common Mistake:** Students often think that when a function prints something, whatever it prints is what that function gets evaluated to when it is called. _Remember, functions get evaluated to whatever the `return` statement evaluates to, and nothing else._ 

If you are in the middle of evaluating a function call and you come across a `print` call expression, just treat it like any other call expression! Evaluate the operator to the `print` function, evaluate the operands, and then print the operands (which just shows the operands to the screen). After you're done evaluating the `print` call expression, continue evaluating your original function call.

But if all functions are supposed to evaluate to something when they are called, what does the `print` function evaluate to? Remember what I said previously about functions that don't have a `return` statement. They just return `None`! Thus, all call expressions  with `print` as the operator evaluate to `None`.

**Common Mistake:** Students often think that when a function prints something, they should stop execution of the code in that function. _In reality, only a `return` statement stops the execution of the code in a function._

## Loops
We saw that functions help us avoid having to rewrite code. Loops are another technique used to help us avoid rewriting code, but they're quite different from functions.

### Motivation for Loops
When you drink water, you continue to drink water until you aren't thirsty anymore. A human brain can execute a command to drink water very easily: "drink water until you aren't thirsty anymore." But a computer isn't as smart as a human brain. It can only do take one step at a time, and only does _exactly_ the steps you tell it to do. So if you had to write code that told a robot to drink water, your code would probably look something like:
```
if you are thirsty:
	take 1 sip of water
else:
	stop drinking water

if you are still thirsty:
	take 1 sip of water
else:
	stop drinking water

if you are still thirsty:
	take 1 sip of water
else:
	stop drinking water
.
.
.
# Continues until you are not thirsty anymore
```
In theory, the above code could go on forever. So how could you possibly write code that would tell a robot how to drink water? That's where loops come in.

Loops allow you to repeat a certain chunk of code until some condition is met. In our robot example, we want to write code that will tell the robot to repeatedly take 1 sip of water until it isn't thirsty anymore. This what that would look like in code:
```
while you are thirsty:
	take 1 sip of water
```
Let's deconstruct this code to see what's happening. `while` tells the computer that we are writing a loop. `take 1 sip of water` is the code that we want to computer to repeatedly execute. `you are thirsty` tells the computer to continue to repeatedly execute the code until `you are thirsty` is `False`.

### Details of Loops
A general loop takes the form:
```
while condition:
	# some lines of code
```
As we saw above, this loop would repeatedly execute the code inside the loop until `condition` is `False`. But what does it mean for `condition` to be `False`. Well, it means that `condition` has to be something that can evaluate to `False`. What does that sound like? A boolean expression! Notice it all links back to expressions again.

Let's see how we can use this knowledge to write real code to tell a robot to drink water. Let's define a variable `thirst_level` to represent how thirsty the robot is and pretend we have a function `take_sip` that just tells the robot to take a sip of water. Then the code might look something like:
```
thirst_level = 10
while thirst_level > 0:
	take_sip()
	thirst_level = thirst_level - 1
```
**Common mistake:** Many students forget to write code inside their loop that allows the program to make progress toward making the condition of the loop `False`. If you don't do this, your condition would never be `False` and your code will run forever!

# Conclusion

Hopefully this guide helped you clear up some confusion you had about any of these concepts. Also, note that this guide isn't meant to be comprehensive. If you want a more comprehensive analysis of the concepts, read your [assigned reading](http://composingprograms.com/) for the course. As always, if you have any questions about anything I said here (or anything else), please reach out to me and I'm more than happy to answer them!
