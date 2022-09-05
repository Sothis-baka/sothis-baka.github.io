### Leetcode 2400 Number of Ways to Reach a Position After Exactly k Steps, Medium

##### [Link to the problem](https://leetcode.com/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps/)

##### Method

If we start at index a, and end at index b.

We need to move to right for total diff = (b - a) steps (might be negative).

Since we need to move exactly k steps, we need to move countR = (k + diff) / 2 to right and countL = (k - diff) / 2 to left

Check if they add up to k. (odd value can't be divided by 2)

Check if either of them is negative. (unreachable)

Then it will be find all orders to move countR times to right and move countL times to left. A DP problem. Use dfs with cache here.

##### Code

```java
public class Main {
    int modVal = (int) Math.pow(10, 9) + 7;

    public int numberOfWays(int startPos, int endPos, int k) {
        /*
            If we start at index a, and end at index b.
            We need to move to right for total diff = (b - a) steps (might be negative).
            Since we need to move exactly k steps, we need to move countR = (k + diff) / 2 to right and countL = (k - diff) / 2 to left
            Check if they add up to k. (odd value can't be divided by 2)
            Check if either of them is negative. (unreachable)
            Then it will be find all orders to move countR times to right and move countL times to left
         */
        int diff = startPos - endPos;
        int countR = (k + diff) / 2;
        int countL = (k - diff) / 2;

        // Not reachable
        if(countL < 0 || countR < 0) return 0;
        // Odd value
        if(countL + countR != k) return 0;

        // DP
        int[][] cache = new int[countL + 1][countR + 1];
        return helper(countL, countR, cache);
    }

    private int helper(int countL, int countR, int[][] cache){
        // Finished
        if(countL == 0 && countR == 0) return 1;
        // Visited
        if(cache[countL][countR] != 0) return cache[countL][countR];

        long count = 0;
        // DFS
        if(countL > 0) count += helper(countL - 1, countR, cache);
        if(countR > 0) count += helper(countL, countR - 1, cache);
        // Mod
        count %= modVal;
        cache[countL][countR] = (int) count;
        return cache[countL][countR];
    }
}
```
