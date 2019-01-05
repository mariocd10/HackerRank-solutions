# Fraudulent Activity Notifications
> Sorting

## Problem

A bank has a policy to warn clients of possible fraudulent account activity. If the amount spent by the client on a particular day is greater than **2xClient's Median** spending for a trailing number of days, they send the client a notification.

Given number of trailing `days d` and client's total daily expenditures for a period of `n days`, find and print the number of times the client will receive a notification over all n days.

Example:

```
d = 3
expenditures = [10,20,30,40,50]
// First three days I just collect spending data. At day 4 I have trailing expenditures
day = 40
trailing = [10,20,30]
median = 20
// 40 >= 2x20 is true, there will be a notice
// The next day, the trailing expenditures are
day = 50
trailing = [20,30,40]
median = 30
// 50 >= 2x30 false, no notice
```

## Solution

```
// Complete the activityNotifications function below.
    static int activityNotifications(int[] expenditure, int d) {
        /* 
		* Implementing a partial counting sort to help get the median quicker
		* /
		
		// Creating the array of range 0-201 for all possible integers
        int[] output = new int[201];
		/*
		* This is where it differs from the traditional counting sort
		* I only increment the freq values of the first d integers
		* This is necessary because I are only worried about the trailing numbers from current expenditure
		* of size d.
		*/
        for(int i=0; i<d; i++){
            output[expenditure[i]]++;
        }
		
		/*
		* Also, I grab a concept of counting sort in getMedian
		* Now that we have the output array populated with the freq of the trailing element we can start iterating through the array starting at d.
		* The key here is that once we check if it needs a notice or not at i, 
		* We need to update our output array to contain the latest trailing numbers so next iteration it'll do a proper check
		*/
        int notice = 0;
        for(int i = d; i<expenditure.length; i++){     
            double median = getMedian(output, d);
            if(expenditure[i] >= (2*median)){
                notice++;
            }
            output[expenditure[i]]++;
            output[expenditure[i-d]]--;
        }
        return notice;
    }

	/**
	* We know the size of our trailing numbers is d
	* and our array has a frequency count of the trailing numbers.
	* Given this info and knowing the array is already sorted by index, we can calculate the median value by 
	* iterating through the array, aggregating a sum by the value and compare if that sum is more than half of d.
	* Because the sum is essentially where that value would be in an array if it were sorted.
	* We skip having to create another array to sort it.
	**/
    private static double getMedian(int[] temp, int exp){
        int count = 0;
        double median = 0;
        if(exp%2 == 0){
            Integer  m1 = null;
            Integer  m2 = null;
            for(int i=0; i< temp.length; i++){
                count += temp[i];
                if(m1 == null && count >= exp/2){
                    m1 = i;
                }
                if(m2 == null && count >= exp/2+1){
                    m2 = i;
                    break;
                }
            }
            // get both median numbers
            median = (m1+m2)/2.0;
        }
        else{   
            for(int i=0; i< temp.length; i++){
                count += temp[i];
                if(count > exp/2){
                    median = i;
                    break;
                }
            }
        }
        return median;
    }
```