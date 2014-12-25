---
layout: post
title:  "Fibonacci Recursion"
date:   2014-12-10 13:01:17
---

**[Project Euler #2][eulerproblem]**, without iteration (that is to say -- using recursion)

**Background:**

*"To understand recursion, you must understand recursion."*

Iteration goes in order through a certain sequence, where the execution of a method is independent from what it is manipulating. In recursion, however, executing a method requires using that very same method. Solving a problem in this way is like digging a deep, multi-pronged hole in the ground. A method calls itself over and over until it reaches a base value, then it comes back up by way of returning values to itself until it gets back to the original calculation.

Start at ground level -- given a problem:
{% highlight ruby %}
fib(5) = fib(4) + fib(3)
{% endhighlight %}
In order to calculate `fib(5)`, you need `fib(4)` and `fib(3)`. So you branch off into those:
{% highlight ruby %}
fib(4) = fib(3) + fib(2)
fib(3) = fib(2) + fib(1)
{% endhighlight %}
The equations expand over and over until you get to a base value. In the case of the Fibonacci sequence, the base values are 0 and 1.
{% highlight ruby %}
fib(0) = 1
fib(1) = 1
{% endhighlight %}
If you didn't have a base value, the "stack" of equations would expand down indefinitely (down into `fib(-1)`, `fib(-2)` etc...); eventually becoming too deep for the computer to handle.

Once a base value is reached, the program can begin to come back from the depths of the stack. Once each prong reaches `fib(0)` or `fib(1)`, the value of 1 is (finally!) returned to the equation the next level up. That equation, in turn, can now return a value to the equation above it, and so on, and so forth. 

**Solving the problem:**

I started by trying to construct the Fibonacci sequence backwards --starting with the max value. This is the traditional way to construct the Fibonacci sequence recursively [code], and I wrestled with this for several days because I assumed it was the best. My biggest roadblock was how to capture each value of the sequence along the way? It could return the end product, but trying to capture multiple values that the program returned to itself at different points in execution seemed impossible.

Luckily a classmate of mine showed me the light and suggested I construct the sequence from 0 up to the max, still using recursion. It took about 10 minutes to come up with a solution after that:


Setting up initial sequence `[0,1,...]`, quick test for even/odd at each addition to the sequence, incrementing the "evens" sum along the way until max is reached ... *viola!*

{% highlight ruby %}

def even_fibs(max, a = [], evens = 0)
  case a[-1]
  when nil 
    a << 0
  when 0
    a << 1
  else
    if a[-1] % 2 == 0
      evens += a[-1]
    end
    a << a[-1] + a[-2]
  end

  if a[-1] > max
    return evens
  else
    even_fibs(max,a,evens)
  end
end

{% endhighlight %}

**Learned:**

-	Recursion, how it works.
-	Why recursion? For issues of hierarchy & ancestry (visualize a family tree), or when it is not known how far is needed to get to a base value. Also for "divide and conquer" branching iteration. Use only when it easily works to deconstruct into a base value.
-	Asking for help works. Different people think differently.

**Technologies Used:**

-	Ruby

[eulerproblem]:      https://projecteuler.net/problem=2

