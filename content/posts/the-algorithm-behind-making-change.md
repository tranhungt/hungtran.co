---
title: "The Algorithm Behind Making Change"
date: 2014-08-14T19:00:55-05:00
draft: false
---

Have you ever gone to a grocery store or a fast food restaurant and paid for your food in cash? And perhaps they got one of those fancy coin boxes that automatically dispenses the coins when the cashier hands you back the bill portion of your change?

Have you also gone on to wonder how those coin changers were able to caculate what's the most efficient way to give back change, i.e. the least number of coins it can give back for a certain amount?

### Example
If the box had only pennies, dimes, and quarters left, what coins should it give back for a 30 cent change? Would it first start by giving the highest value coin first, and then work it's way down, i.e. a quarter and 5 pennies, or does it iterate all possible combinations and figure out that by giving 3 dimes, it would effectively give only 3 coins instead of 6 for the same amount?


### The Standard Approach
A simple algorithm would simply start giving out change starting with the largest denominator and work its way down.

Using the same constraint as earlier, with only pennies, dimes, and quarters, we can construct the algorithm as follows.

```ruby
def make_change(amount, coins=[25,10,5,1])
  coins = coins.sort.reverse # start with the largest coins
  change = []
  coins.each do |coin|
    while (amount - coin) > 0
      change << coin # put coin in the change list
      amount -= coin
    end
  end
  change
end

make_change(30, [25,10,1]) # [25,1,1,1,1,1]

```
By working from the largest coins first, and working our way down, we would get a solution that is 1 quarter and 5 pennies (no nickels, remember?). This is pretty good. It gives a decent solution, and it's mighty fast. But, as we have noticed, this isn't the optimal solution, since we could have achieved a 30 cent change by giving only three dimes.

### Exhaustive Search
Another approach would be to exhaust all possible change conbinations that would give you the right amount and select the combination that requires the least number of coins. I say combination for the sake of generality and coherence, but the algorithm actually solves for all permutations. Combination refers to the fact that order does not matter, much like a set, e.g. {1,2} is the same as {2,1}, whereas in permutations, order does matter, e.g. [1,2] and [2,1] are two different entities.

```
def make_change(amount, coins=[25,10,5,1])
  coins = coins.sort.reverse
  possible_combos = []
  iterate_combos(amount, coins, possible_combos)
  possible_combos.min_by(&:length)
end

def iterate_combos(amount, coins, current_change=[], possible_combos)
  if amount == 0
    return possible_combos << current_change
  end

  coins.each do |coin|
    if coin <= amount
      new_combo = current_change.dup << coin
      new_amount = amount - coin
      iterate_combos(new_amount, coins, new_combo, possible_combos)
    end
  end
end 

p make_change(30, [25,10,1]) # [10,10,10] => 3 dimes
```
Since we calculated for all possible combinations, we find that the optimal solutions is 3 dimes for a change of 30 cents.
The efficiency of this algorithm... isn't so efficient. For example, image that in Catland there are three different types of coins, but each valuing only 1 cent. If you had to give a 30 cent change amount, this search algorithm would provide you with 3^30 possible permutations of what coins to give back - that's a lot. The time complexity would be O(n^m), where m is the amount, and n is the number of coin denominations. That's for demonstration purposes as the actual complexity is calculated to be less in the real world when you have higher valued coins like quarters and dimes, which will reduce the coefficient only by a scalar value. However, the time complexity is still polynomal time, making the exhaustive search prohibitively expensive.

### Enter Dynamic Programming
You are astute to notice that by calculating all permutations, we were essentially solving for the same combination more than once.
Dynamic programming is a way to break up a big problem into many subproblems. It's very similar to recursions, except that dynamic programming is usually iterative and saves the optimal solutions of the sub problems you have already solved as reference. This is achieved by solving for the smaller or sub problems first and working your way up.

That is, once you already know the optimal solution for a 10 cent change is a dime, you can always refer back to this fact when you you are iterating through possible solutions at 20 or 30 cents that requires the solution to a 10 cent change. 

In this next example, I decided to make a CoinBox class that would hold state of some of the variables and allow for the encapsulation of the methods.

```
class CoinBox
  def initialize(coins = [1, 5, 10, 25])
    @coins = coins.sort.reverse
    @changes = []
  end

  def make_change(amount)
    return @changes[amount] if @changes[amount]
    @changes.length.upto(amount) do |amt|
      @changes[amt] = _get_min_coins(amt)
    end
    @changes[amount]
  end

  def _get_min_coins(amt)
    possible_combos = {}
    @coins.each do |coin|
      if coin <= amt
        rem = amt - coin
        possible_combos[coin] = [coin] + Array(@changes[rem])
      end
    end
    smallest_combo = possible_combos.values.min_by(&:length)
  end
end


box = CoinBox.new([1,10,25])
p box.make_change(30) # [10,10,10] => 3 dimes
```

The `make_change` method essentially iterates from 1 to the change amount, and stores the optimal coin combintation at each location referencing the amount.

The juice of the dynamic programming magic lies in the `_get_min_coins` method where the optimal solution is actually calculated. This method goes through each coin and asks - if I take this coin, what is the optimal solution at the remainder amount? That is, if the amount is 20 cents, if we choose a dime, what is the optimal solution at 10 cents, versus if we choose a penny, what is the optimal solution at 19 cents? Since 10 and 19 cent sub problems have already been solved for by the time we reach this problem, by choosing the dime, we know that the 10 cent sub problem is much better off than the 19 cent sub problem. Therefore, 2 dimes at 20 cents is the optimal solution.

### OMG We Solved An NP Hard Problem!
Not really...

This algorithm effectively eliminates the need to iterate through all possible solutions. By working from small problems first and building onto bigger problems, we are able to solve the entire problem in linear time. The linear time complexity is based on O(m*n), where m is the amount, and n and number of coins.

It wasn't that we solved an NP hard problem, it was just that for this particular problem, we were able to sequentailly work through each subproblem, which is much easier than to exhaust all combinations.