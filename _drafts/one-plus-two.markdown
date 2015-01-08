---
layout: post
title:  "1 + 2 = 12"
date:   2017-1-2 18:00:00
---

Reflections on **[Project Euler #35][eulerproblem]**

I love this problem because it underscores the fact that although computers are terrifyingly powerful, they are no rival the complexity of the human brain. Computers are dumb simple. It's only with the application of *our* sophisticated thought processes that they become powerful.

{% highlight ruby %}
require 'prime'

def circular_prime?(num) 
  num_array = num.to_s.split("") 
  rotating_array = [num] 
  (num_array.length-1).times {rotating_array << num_array.rotate!.join.to_i}
	
  prime_array = rotating_array.find_all {|n| Prime.prime?(n) == true}
  return true if prime_array.length == num_array.length
end

def count_cps(max)
  circle_primes = [] 
  Prime.each(max) {|n| circle_primes << n if circular_prime?(n) == true}
  circle_primes.length
end
{% endhighlight %}


**Learned:**

-	

**Technologies Used:**

-	Ruby



[eulerproblem]:      https://projecteuler.net/problem=35