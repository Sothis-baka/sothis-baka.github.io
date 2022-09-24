### LeetCode 140 Word Break II, Hard

##### [Link to the problem](https://leetcode.com/problems/surrounded-regions/)

##### Method

Basically we are using DFS and DP here.



EX: 

s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]



Using DFS with a helper function, we can find all postfixes from index.

EX:

Start from index 0.

We find [pine, pineapple]



Then, for each string str we find, continue searching from index + str.length

Join the strings with spaces.

EX:

For pine, we start search at index 4, we can find[apple, applepen]



Until we reach the end of the string.



##### Optimize

Using DP:

Save the result of search for each indexes, so that we won't do repeat caculations.



Using Trie:

Use Trie to save all the strings, so that the complexity of searching strings will be lower when the input get larger.



##### Code

```java
static class Trie{
        Trie[] children;

        Trie(){
            // Save corresponding tries for 26 chars, the last one is to confirm it's a complete string
            children = new Trie[27];
        }

        // Helper function to save a string into tTrie
        void add(String s, int index){
            if(index == s.length()) this.children[26] = new Trie();
            else {
                int indexOfCh = s.charAt(index) - 'a';
                if(this.children[indexOfCh] == null) this.children[indexOfCh] = new Trie();
                this.children[indexOfCh].add(s, index + 1);
            }
        }

        // Helper function to find all available substrings str that target[index: end] starts with str
        List<String> matchedStrings(String target, int index){
            List<String> result = new ArrayList<>();

            // It's a complete string
            if(this.children[26] != null) result.add("");

            // Reached end
            if(index == target.length()) return result;

            // Find all future strings
            char ch = target.charAt(index);
            int indexOfCh = ch - 'a';
            if(this.children[indexOfCh] != null){
                List<String> candidates = this.children[indexOfCh].matchedStrings(target, index + 1);
                for(String candidate: candidates) result.add(ch + candidate);
            }

            return result;
        }
    }

    public List<String> wordBreak(String s, List<String> wordDict) {
        /*
            Construct a trie with wordDict to find all available strings from index i
         */
        Trie trie = new Trie();
        for(String word: wordDict) trie.add(word, 0);

        /*
            DFS
            Start from index i, find all strings from the pos.
            Recursively find all possible postfixes after those strings.
            DP
            Save the postfixes to avoid repeat visit
         */
        Map<Integer, List<String>> cache = new HashMap<>();

        return findAllResult(cache, s, 0, trie);
    }

    // Find the string results from index
    private List<String> findAllResult(Map<Integer, List<String>> cache, String target, int index, Trie trie){
        if(cache.containsKey(index)) return cache.get(index);

        List<String> result = new ArrayList<>();
        List<String> candidates = trie.matchedStrings(target, index);

        for(String candidate: candidates){
            // Complete search
            if(index + candidate.length() == target.length()){
                result.add(candidate);
                continue;
            }

            List<String> postfixes = findAllResult(cache, target, index + candidate.length(), trie);

            for(String postfix: postfixes){
                result.add(candidate + " " + postfix);
            }
        }

        cache.put(index, result);
        return result;
    }
```


