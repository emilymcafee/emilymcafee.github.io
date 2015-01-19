---
layout: post
title:  "1 + 2 = 12"
date:   2015-1-2 18:00:00
---

Reflections on **[Project Euler #35][eulerproblem]**

This problem is relatively straightforward and not terribly exciting, but it's been on my mind for awhile now for a subtle reason: the distinction that it requires between a number as an integer vs. a number as a string.The relationship that the *integers* 197, 971, and 719 have is obvious to a human but not to Ruby. 

Example: to produce the rotated versions of an integer `123` using Ruby, we must...

1. Convert it to a string `"123"`
2. Separate the characters and store them somehow `["1","2","3"]`
3. Rotate them `["3","1","2"]`
4. Re-connect them `"312"`
5. Convert back to an integer `312`
6. And then rotate *again*, to get `231`


I love this problem because it underscores the fact that although computers are terrifyingly powerful, they are no rival the complexity of the human brain. Computers are simple. It's only with the application of the sophisticated thought processes of humans that they become powerful.


It's easy for us to understand that `1 + 2` is `3` and *also* that rotating the digits of `12` would produce `21`. The distinction between those multiple data types is intuitive for us, and most importantly, the two can exist simultaneously in our minds without raising errors. :) 


Anyway... my solution: 


{% highlight ruby %}
require 'prime'

def circular_prime?(num) 
  num_array = num.to_s.split("") 
  rotating_array = [num] 
  (num_array.length-1).times {rotating_array << num_array.rotate!.join.to_i}
	
  prime_array = rotating_array.find_all {|n| Prime.prime?(n)}
  return true if prime_array.length == num_array.length
end

def count_cps(max)
  circle_primes = [] 
  Prime.each(max) {|n| circle_primes << n if circular_prime?(n)}
  circle_primes.length
end
{% endhighlight %}


**Technologies Used:**

-	Ruby



[eulerproblem]:      https://projecteuler.net/problem=35