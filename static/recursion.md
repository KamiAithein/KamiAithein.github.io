# Recursion

### A preface
Recursion is easy. However difficult you may think it is, you're wrong: If you can write a function to do something once, then you can do infinitely many times (although usually we try to avoid that.)

We're going to start very simple just to reassure ourselves that recursion *is* simple, and then we'll make our ways up to more topics. 

### The basics of basics
#### Alien Invasion
How do you count? I mean *seriously*, how do you count? If I said to count down from 5, you'd probably say out loud: *5, 4, 3, 2, 1, blastoff!* Or you're a bummer and would say nothing. Either way we went from 5 to 1, but *how* did we do that? If an alien came to Earth and had you at blaster point and asked you to explain yourself, what would be the simplest way to explain your thought process?

The simplest way you might come up with is to say "well we start with 5, and then we take one off 5..." and very belaboured you'd say "and then we have 4... And then we take one off 4... and then we have 3..." At this point you'd probably feel like it's stupid to keep explaining your thought process-- counting down from 5 to 4 is basically the same as 4 to 3, and **it'll be the same all the way down.**

The alien might interrupt you -- "WAIT WAIT LET ME TRY!!" The alien apparently likes figuring things out on their own. From your explanation they might write the following program in *flogtoflorp*:
```py
def count_down(n: int):
	print(n)
	count_down(n-1)
```
Upon running `count_down(5)` their terminal would spit out
```
5
4
3
2
1
0
-1
-2
-3
...
Error: out of memory
```
Angrily they'd jam their blaster into your face. "WHY IT CRASH HOOMAN? HAVE YOU LIED TO ME??" The answer is no, you didn't lie to them. But you forgot a crucial step: When does it stop making sense to keep counting? Well obviously it stops at 1, so you tell the alien that. **You tell them that it doesn't make sense to keep counting after 1.** Almost ignoring you the alien goes back to typing.
```py
def count_down(n: int):
	if n < 1:
		return
	
	print(n)
	count_down(n-1)
```
Running `count_down(5)` the alien would smile
```
5
4
3
2
1
```

Mastering the ways of the hooman, the alien would bid you adieu and take off, leaving humanity unharmed for now. You would be congratulated by the POTUS, Ghandi, even Jesus might show up to give you a high five for doing such a good job.

#### What did the alien teach us?
Well, when we tried to explain counting we saw that we really only needed to explain how to count from 5 to 4, and from 4 to 3. At that point we could just continue that process until some stopping point. And that's the gist of recursion. Once you know how to do something once, you can just do it again, and again, and again, until it doesn't make sense to do it anymore. So let's try to apply what we learned from the alien.

#### Counting sums
If we wanted to get the sum of every positive number up to some $n$, how would you do it? Don't worry there won't be any aliens this time. If you were taught the closed-form solution then it's just $\frac{n(n+1)}{2}$, but however technically right that is it's a boring way to find the answer. Maybe think for a minute.

If we started at 5, then the sum of all the numbers up to 5 is just 5 + the sum of all the numbers up to 4 actually. And the sum of all the numbers up to 4 is just 4 + the sum of all the numbers up to 3. This looks a lot like what we were describing to the alien earlier, so maybe we can modify the code they wrote for us! 
```py
def sum_from(n: int):
	if n < 1:
		return
	
	n + sum_from(n-1)
```
Instead of printing n, we're adding n to sum_from(n-1). But this doesn't do anything with that number we create, we should probably return it.
```py
def sum_from(n: int):
	if n < 1:
		return
	
	return n + sum_from(n-1)
```
Annnnd this almost works but there's a bug. Let's play try to spot the bug!

Did you get it?

That's right, we don't return anything in our base case. Instead of just doing nothing in our base case, we need to return a value. So what's the sum from 0 to 1? Zero! And what's the sum from -1 to 1? Also Zero! And what's the sum from -2 to 1? ... Negative one! But a negative input doesn't really make sense. It really stops making sense at 1, so anything less than 1 should return something that doesn't affect our answer, so let's just return 0.
```py
def sum_from(n: int):
	if n < 1:
		return 0
	
	return n + sum_from(n-1)
```
And now this works! So now we can count, and we can sum! Amazing job. As long as we **remember to have a base case for when calculations stop making sense**, and our recursion works for one case, **the recursion will work all the way down**. In all of our recursions we had a big problem we wanted to solve-- Counting from from 5, summing all the numbers from 5-- which could be broken down into smaller steps: Counting down from 5 is just counting 5, and then counting down from 4; summing all the numbers from 5 was just adding 5 to the sum of all the numbers from 4. **Recursion at its core solves a large problem by solving all of that problem's subproblems.**

### Less than basic basics
The fibonacci sequence is 1, 1, 2, 3, 5, 8, ... Hopefully you can see some pattern here, I'm going to explain it anyways so. The $i$ th fibonacci number in the fibonacci sequence is just the $i-1$ th + the $i-2$ th. More often this will be written as:
$$Fib(n)=\begin{cases} 1 & \text{if } n < 2 \\ Fib(n-1) + Fib(n-2) & \text{otherwise} \end{cases}$$

This way of writing out the fibonacci sequence actually tells us exactly how to write a program calculating the $i$ th fib number: The base case is when $n<2$ and the value to return then is 1, otherwise we return the $n-1$ th + the $n-2$ th value!

```py
def fib(n: int) -> int:
	if n < 2:
		return 1

	return fib(n-1) + fib(n-2) 
```

That was pretty simple once we had that fancy mathy function up there. In fact it's often easier to think of recursion in terms of fancy math and then to write our program. We could write our sum program in fancy math too:
$$sum\_from(n)=\begin{cases}0 & \text{if } n < 1 \\ n + sum\_from(n-1) & \text{otherwise}\end{cases}$$

In general, although I personally hate math, mathy symbols and mathy concepts can be pretty helpful. And it's often way easier to come up with an abstract idea of what a recursion does than to just *do* recursion. For example, how might you find the sum of all numbers less than a given $n$ and greater than a given $k$, which are not a multiple of 8, and in this sum instead of adding the value 2 we'll add 42, and for all odd numbers we'll add the opposite of the odd number? For example `sum_from_to(-3, 5) = 3 + -2 + 1 + 0 + -1 + 2 + -3 + 4 + -5 = -1` 

You should be able to do it with all the knowledge you've gained so far. I highly suggest you solve this yourself. Here's a math way to put it though, following what we did for `sum_from`:
$$sum\_from\_to(k, n)=\begin{cases}0 & \text{if }n < k \\0 + sum\_from\_to(k, n-1) & n\mod 8 = 0\\ -n + sum\_from\_to(k, n-1) & \text{if n odd} \\ n + sum\_from\_to(k, n-1) & \text{if n even}\end{cases}$$ 

The not divisible by 8 part may have been tricky as we don't want to just return 0, because that would stop our recursion. We want to just not add our n, to discard it, and to continue the recursion as normal. 
```py
def sum_from_to(k: int, n: int) -> int:
	if n < k:
		return 0
	if n % 8 == 0:
		return sum_from_to(k, n-1)
	
	return n + sum_from_to(k, n-1)
	
```

That was a pretty difficult program! Or maybe it was easy. Or maybe you couldn't solve it. I can't really tell. Anyways it would be nice if there were some *useful* things we knew how to use recursion for. If only.

### Useful things we can use recursion for
#### Tower of Hanoi
The Tower of Hanoi is a game were you have 3 pins, and on the first pin you have k disks of increasing size from the bottom up, meaning the smallest is at the top. The goal of this game is to move all the disks from the left to the pin to the right-most pin. You cannot stack a larger disk on top of a smaller disk. You might be able to brute force this on your own, I high encourage you to look up an online version and to try your own methods. But there is a recursive solution. Try to think about how this relates to our previous summation problems.

Let's consider k=5. To move 5 disks we need to first move the 4 disks below, and then move the 5th top disk. To move the 4th disk we need to move the 3 disks below that, and then move the 4th top disk... This sounds familiar. Let's get right into a mathy way of writing it!

$$hanoi(k) = \begin{cases}\text{nothing} & k < 1 \\ hanoi(k-1) \text{ then handle }k\text{th disk} & \text{otherwise}\end{cases}$$

Okay so getting right into the code, I'm pumped to get writing!
```py
def hanoi(k):
	if k < 1:
		return
	
	hanoi(k-1)
	handle(k) # im tired
```

Okay maybe we were over eager...