### Leetcode 15 3Sum, Hard

##### [Link to the problem](https://leetcode.com/problems/3sum)

##### Method

First, we sort the array to make it non-decreasing.

Create 3 pointers i, j, k

Iterate i from index 0 to end of the array.

In every run, j should start from the right side of i, and k should start from the end of array. Using two pointer method to search for combinations sum up to the target.

Example:

arr: [-1,0,1,2,-1,-4], target:0

sort: [-4, -1, -1, 0, 1, 2]

[**-4**, **-1**, -1, 0, 1, **2**], sum: -3, should move j to right (skip same values).

[**-4**, -1, -1, **0**, 1, **2**], sum: -2, should move j to right.

[**-4**, -1, -1, 0, **1**, **2**], sum: -2, j can't move, move i.

[-4, **-1**, **-1**, 0, 1, **2**], sum: 0, save case, move j, k inward.

[-4, **-1**, -1, **0**, **1**, 2], sum: 0, save case, j, k can't move, move i (skip same values).

[-4, -1, -1, **0**, **1**, **2**], sum: 3, finish search.

##### Code

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        int length = nums.length;
        for(int i=0; i<length-2; i++){
            int numi = nums[i];
            // There is no way sum of positive numbers be 0

            if(numi > 0) break;
            // Same value as last loop, skip
            if(i > 0 && nums[i] == nums[i-1]) continue;

            // Two pointer
            int j = i+1, k = length - 1;
            while(j < k){
                int sum = numi + nums[j] + nums[k];
                // Save case if add to target, otherwise move toward target.

                if(sum == 0){
                    List<Integer> list = new ArrayList<>();
                    list.add(numi);
                    list.add(nums[j]);
                    list.add(nums[k]);
                    result.add(list);
                    while(j < k && nums[j] == nums[j+1]) j++;
                    while(j < k && nums[k] == nums[k-1]) k--;

                    j++;
                    k--;
                }else if(sum < 0){
                    j++;
                }else{
                    k--;
                }
            }
        }

        return result;
    }
}
```
