### Leetcode 2401 Longest Nice SubArray, Medium

##### [Link to the problem](https://leetcode.com/problems/longest-nice-subarray/)

##### Method
Use an integer cache to save all bits that are using in current range.

Whenever we are visiting a new number, remove from left until there is no conflict.


##### Code
```java
public class Main {
    public int longestNiceSubarray(int[] nums) {
        int cache = 0, left = 0, right = 0, length = nums.length, max = 0;
        while (right < length) {
            // current number conflicts with numbers in range
            while ((cache & nums[right]) != 0)
                cache ^= nums[left++]; // Remove from left until no conflict

            // Record current bits
            cache |= nums[right];
            // Update result
            max = Math.max(max, right - left + 1);

            right++;
        }
        return max;
    }
}
```