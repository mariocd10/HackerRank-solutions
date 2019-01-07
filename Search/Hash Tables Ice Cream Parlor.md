# Hash Tables: Ice Cream Parlor
[Hacker Rank Problem](https://www.hackerrank.com/challenges/ctci-ice-cream-parlor/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search)

## Explanation / Thought Process
Two people want to buy ice cream. They have total money = m = 5.

The ice cream flavors provided with their cost are in an array = cost = [2,1,3,5,6]

The goal is to spend all the money available. When referencing which ice cream flavors to choose, show the 1-based index number associated with the cost array.

They would purchase ID's 1 and 3 for a cost of 2 + 3 = 5. 

Print the smaller ID first.

We know in arithmetic that when we do `a + b = c` . `c` is going to be greater than `a` or `b`. So to make our search for the two values efficient, we can sort the array *cost* and then perform binary search to find the location of where to start looking. 

Best case `c` is in the array and we can start searching for `a` and `b` to the left of index `c`.
Worse case `c` isn't in the array so we need to alter binary search to return the left index when not found. This will give us the starting point of where to search we'll call it `indexBoundary`

Now that we have the point to end our search. Since we're still dealing with an array, and we're going to do many search operations it'll be useful to use another data structure.

Iterate through the sorted cost array from the beginning until `indexBoundary`. 

At i'th element b =  m - i. Now we check if b is in the subarray. if yes then we save i as a and look for b as we iterate through the array. Once found then we'll display the output as indexOf(a)+1, indexOf(b)+1. IF b is not in the subarray then we go to the next i'th element and repeat the search.

## Solution

```java
    // Complete the whatFlavors function below.
        static void whatFlavors(int[] cost, int money) {
            int a=0, b=0;
            int indexBoundary;
            int[] sortedCost = cost.clone();
            Arrays.sort(sortedCost);
            indexBoundary = binarySearchRecursive(sortedCost, money, 0, cost.length-1);
            Hashtable<Integer, Integer> subList = new Hashtable<Integer,Integer>();
            for(int i = 0; i < indexBoundary; i++){
                a = sortedCost[i];
                b = money - a;
                if( subList.containsKey(b) ){
                    break;
                } else if( subList.containsKey(a)  ){
                    int value = subList.get(a);
                    subList.put(a, value+1);
                } else {
                    subList.put(a, 1);
                }
            }
    
            int indexA = 0;
            int indexB = 0;
            for(int i =0; i<cost.length; i++){
                if( cost[i] == a){
                    indexA = i;
                } else if( cost[i] == b && a > 0){
                    indexB = i;
                }
            }
    
            if(indexA > indexB){
                System.out.println((indexB+1)+" "+(indexA+1)); 
            } else {
                System.out.println((indexA+1)+" "+(indexB+1)); 
            }    
        }
    
        private static int binarySearchRecursive(int[] array, int x, int left, int right){
            if( left > right ){
                return right;
            }
            int mid = (left + right)/2;
            if( array[mid] == x ){
                return mid;
            } else if( x < array[mid]){
                return binarySearchRecursive(array, x, left, mid-1);
            } else if( x > array[mid] ){
                return binarySearchRecursive(array, x, mid+1, right);
            }
            return x;
        }
```