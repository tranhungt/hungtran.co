---
title: "More Than One Way to Fizzbuzz"
date: 2014-08-10T19:00:42-05:00
draft: false
---

The infamous fizzbuzz question... Everyone's done it, but how many ways can it be done, and how short can it be written? Does it require complex algorithm structures, or just a few if-else? Does it fill up multiple lines, or can you fit it in a single line tweet?

Here, I'd like to walk through one possible progression of thought where, a simple multi-line solution is then evolved to something that's more mature and compact, though not entirely practical.

The FizzBuzz Game
-------------
To understand the classic fizzbuzz, here's quick excerpt from wikipedia describing the game:

<cite>Fizz buzz is a group word game for children to teach them about division. Players take turns to count incrementally, replacing any number divisible by three with the word "fizz", and any number divisible by five with the word "buzz".  Numbers divisible by both become "fizz buzz".</cite>

----------------------------------
In Search of The Shortest Solution
-------------------------

#### The Standard If-Else
Here, we can take the english requirements and turn them into simple if-else statements.
Counting 1 to 100.. that's a for-loop.
If divisible by both 5 and 3, then then we have fizzbuzz, if only divisible by 3, we have fizz, and 5 if divisible only by 5. Simple enough.
```
for(var i = 0; i <= 100; i++){
  var out = ''
  if( i % 3 == 0 && i % 5 == 0){
    out = 'fizzbuzz'
  } else if (i % 3 == 0) {
    out = 'fizz'
  } else if (i % 5 == 0){
    out = 'buzz'
  } else {
    out = i
  }
  console.log(out)
}
```

But how can we make this better? Of course, we can make nested for loops, but how can we make it so that we don't have to evaluate four different cases?

#### String Concatenation
What if we say that, we start with an empty string, and tack on the word "fizz" if the number is divisible by 3, and independently tack on the word "buzz" if the number is divisible 5? If our string is empty after those two cases, we assign the iterating number as the output?
```
for(var i = 0; i <= 100; i++){
  var out = ''
  if (i % 3 == 0) out += 'fizz'
  if (i % 5 == 0) out += 'buzz'
  if (out.length == 0) out = i 
  console.log(out)
}
```

I would argue that having 3 different if statements is more concise than having a four-block if-else trace, as we can sequentially work through each statement and augment our output as it qualifies.

But, what if we want to get rid of this 'if' structure and go with something more concise?

#### One-Liner Nested Ternary
Ternary evaluation allows us a concise way to represent the if-else structure without the cumberson structure itself. Here, we nest one ternary inside another to give the same logic as a nested if-else statement.

```
for(var i = 0; i <= 100; i++) console.log(i % 3 == 0 ? i % 5 == 0 ? 'fizzbuzz' : 'fizz' : i % 5 == 0 ? 'buzz' : i)
```

Could we shorten this ternary structure further?

#### Using String Concatenation with Ternary
By using the above technique of string concatentation, we can split the nested ternary structure into two different evaluations so we can eliminate the need to check for the "fizzbuzz" condition. Also, an empty string is evaluated as false in javascript, so we can use that to check whether neither fizz or buzz occured.

```
for(var i = 0; i <=100; i++) console.log((i % 3 == 0 ? 'fizz' : '') + (i % 5 == 0 ? 'buzz' : '') || i)
```

Could we shorten it further still?

#### Bodiless For-Loop
Understanding that in a javascript for-loop, the `i++` portion is evaluated as a stand-alone script, we can put the body of the for loop-inside the loop construction itself resulting in a for-loop without a body.

```
for(var i = 0; i <=100; console.log((i % 3 == 0 ? 'fizz' : '') + (i % 5 == 0 ? 'buzz' : '') || i), i++);
```

But can we make this even shorter?

#### Javascript 0 Evaluation
Understanding that javascript evaluates `0` as false, we can flip the logic in a way that instead of checking for the truthiness of `i % 5 == 0`, we can evaluate for the falsiness of `i % 5` in order to find  divisilibty.

```
for(var i = 0; i <= 100; console.log((i%3 ? '' : 'fizz') + (i%5 ? '' : 'buzz') || i), i++);
```

But can we make this even shorter?

#### ```i++``` vs ```++i```
Can we increment i++ within our algorithm inside the console.log()?
Yes, but calling i++ does not guarantee that the evaluation of i is consistent within the iteration of that particular instance of i. Why?
Example:
```
var i = 15
i++ % 3 == 0 // i is evaluated at 15
i % 5 != 0 // i is evaluted at 16
```
Understanding ++i.
The difference between i++ and ++i is that i++ will pass the value of i, and then increment, whereas ++i will increment then pass the value.
Example:
```
var i = 1
console.log(5 + i++) // 6
console.log(i) // 2

var i = 1
console.log(5 + ++i) // 7
console.log(i) // 2
```
With this, we can put the ++i directly inside our algorithm. Using ++i will make sure that the evaluation of i at the instant of ++i is the same as the evaluation of i at any other point inside the algorithm after it has been incremented.

```
for(var i = 0; i < 100; console.log((++i%3 ? '' : 'fizz') + (i%5 ? '' : 'buzz') || i));
```

#### Minification
And lastly, remove the var scoping and delete all spaces. :)

```
for(i=0;i<100;console.log((++i%3?'':'fizz')+(i%5?'':'buzz')||i));
```

This comes in at 65 characters long, so you can write 2 of these functions inside a tweet with room to spare...not bad :)

------------------
Final Words
--------
Of course, there are many ways to fizz a buzz. This is only one progression of thought. The beauty of programming is that you can use many different thought processes and tools to achieve the same result. It really depends on what your goal is. Is it readibilty, conciseness, reusability, or simply the least characters possible?

If this puzzle piques your curiosity, send me a messsage and let me know what you think or if you have a different solution.