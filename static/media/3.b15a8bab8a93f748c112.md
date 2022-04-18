### LeetCode 403 Frog Jump, Hard

(Ez & Faster than 90% Solution)

##### [Link to the problem](https://leetcode.com/problems/frog-jump/)

##### Method

DP search

Base case:

Start from first index, jump distance is 1.

General case:

Started from i index, jump distance is k.

Then current position is i + k

If i + k is the last stone, return true.

If i + k isn't a stone, return true.

From i + k, jump k - 1 / k / k + 1, if either of which is true, return true.

##### Optimize

Use a HashMap to save visited cases.

The keyset of the hashmap can be use to test if an index is a stone

The valueset of the hashmap prevent revisit cases.

##### Code

```java
public class Main {
    public boolean canCross(int[] stones) {
        Map<Integer, Set<Integer>> cache = new HashMap<>();

        for(int stone: stones){
            cache.put(stone, new HashSet<>());
        }

        return helper(0, 1, stones, cache);
    }

    private boolean helper(int index, int jump, int[] stones, Map<Integer, Set<Integer>> cache){
        int cur = index + jump;
        if(cur == stones[stones.length - 1]){
            // Arrived
            return true;
        }

        // Not a stone or visited
        if(!cache.containsKey(cur) || cache.get(index).contains(jump))
            return false;

        // Save to cache
        cache.get(index).add(jump);
        // Shouldn't jump 0
        return (jump > 1 && helper(cur, jump - 1, stones, cache)) || helper(cur, jump, stones, cache) || helper(cur, jump + 1, stones, cache);
    }
}
```
