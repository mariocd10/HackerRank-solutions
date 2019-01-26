[Hacker Rank Problem](https://www.hackerrank.com/challenges/greedy-florist/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)

# Explanation / Thought Process
---

group of friends = k = 3
want to buy n flowers = n = 4
cost of each flower = c = [1,2,3,4]

The price of a flower increases by the following formula:
(current flower num + num of prev purchases) x (cost of flower) = price to pay for flower

In other words, if one person buys all four flowers
(1+0)x4 = 4
(1+1)x3 = 6
(1+2)x2 = 6
(1+3)x1 = 4
Total = 20

## Approach
So to pay the minimum cost for a flower **per person**, ****you would want the increasing purchasing count to be multiplied by the cheapest flower *AKA* buy the most expensive flower first. 

Now that we know the formula for minimum cost **per person**, we need to come up with the formula  on how much should **a person** buy.

~~Base Case: If num of flower = num of ppl then one flower per person.~~

~~Divisible Case: if num of flower % num ppl = 0. (e.g 9 flowers, 3 ppl.) Then the flowers can be evenly divided amongst the group.~~

~~Indivisble Case: if num of flower % num ppl ≠ 0 (e.g 10 flowers, 3ppl) Then the remaining number will need to be divided evenly amongst the group.~~

The above was meant to figure out how much each person to take depending on the size of the array of flowers. But that calculation isn't needed if we take a different approach. After looking at the sample input/output I noticed the pattern.

Sample Input:

    5 3
    1 3 5 7 9

Sample Output:

    29

Sample Output Explanation:

    The friends buy flowers for 9, 7 and 3, 5 and 1 for a cost of 9+7+3*(1+1)+5+1*(1+1) = 29 .

So basically, we start iterating from the end to the front. Each flower is given to a person (e.g p0...pN-1). So when we run out of people to give a flower, we start from p0 and continue the pattern. This ensures a correct distribution AND ensures the first flower given to a person is the most expensive.  

The only thing missing now though is the previous purchase amount to make our calculation of final price. To do this we create a separate list to keep track for each person and just retrieve/increment it as we iterate through the list.

# Solution
---
``` java
    // Complete the getMinimumCost function below.
        static int getMinimumCost(int k, int[] c) {
            int minimumCost = 0;
            int index = 0;
            int[] personCount = new int[k];
            int[] totalPerPerson = new int[k];
            Arrays.sort(c);
            for(int i=c.length-1; i>=0; i--){
                if(index > k-1){
                    index = 0;
                }
                int prevPurchCount = personCount[index];
                totalPerPerson[index] += (1+prevPurchCount)*c[i];
                personCount[index] +=1;
                index++;
            }
            for(int i=0; i<totalPerPerson.length; i++){
                minimumCost += totalPerPerson[i];
            }
            return minimumCost;
        }
```