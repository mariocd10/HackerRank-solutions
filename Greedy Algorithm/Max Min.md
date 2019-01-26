[Hacker Rank Problem](https://www.hackerrank.com/challenges/angry-children/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)

# Explanation / Thought Process

---

given an array = arr
given an integer = k

Create an array of length k from elements in arr. Call that subarr.
*Unfairness* = max(subarr) - min(subarr)

Find the minimum Unfairness

**Note**: Integers in **arr** may not be unique

## Approach

So to get the minimum Unfairness we need to find all possible sub arrays, find the max and min and compare it to a global min value. The key here is to not actually calculate *every* possible sub array since that's not efficient. We want to be selective in which sub arrays we create. 

If we sort the array **arr,** we know the max and min from the start of the sorted **arr** will give us the smallest possible difference. 

There is a possibility that **arr** may contain duplicates, and if in the case that the max and min of sub array are duplicates then the Unfairness value would 0. Meaning this value is the global minimum and we can return that value. 

Taking into consideration the duplicates. The duplicates may not be in the start of the array which means we'll need to do a search if there is duplicate value of count k, meaning max and min would be the same. If found then that's the value, if not then it'll be the first k values of the sorted array.

As a result, searching for dups of count k will need to be our first priority. 

# Solution
---
```java
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