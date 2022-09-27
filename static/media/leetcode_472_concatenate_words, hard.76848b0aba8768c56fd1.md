### Leetcode 472 Concatenate Words, Hard

##### [Link to the problem](https://leetcode.com/problems/concatenated-words)

##### Method

DFS

First, sort all words by their length

Save words in order.

When save string s, we use DFS to search if the prefix of it is a visited word, then do it to the rest of part. Until reached the end of string (It's dividable). Otherwise, it's not.

##### Optimize

Use trie to save all the words, which reduce the complexity of searching words.

##### Code

```java
static class Trie{
        Trie[] children;

        public Trie(){
            // 26 lower case characters, the last one is to indicate it's a complete word
            this.children = new Trie[27];
        }

        /*
            Helper function to add a word into Trie
            The function will return whether the word is dividable by existed words
         */
        public void addWord(String word, int index){
            // End of the word
            if(index == word.length()){
                // Mark it's a complete word
                this.children[26] = new Trie();
                return;
            }

            // Recursive
            char ch = word.charAt(index);
            if(this.children[ch - 'a'] == null)
                    this.children[ch - 'a'] = new Trie();

            this.children[ch - 'a'].addWord(word, index + 1);
        }
    }


    Trie GlobalTrie = new Trie();
    List<String> result = new ArrayList<>();

    /*
        DFS
        Helper function to find if a word can be divided to multiple words
     */
    public boolean divideWord(Trie trie, String word, int startIndex, int curIndex){
        /*
            Finished divide
            The word can be empty
            The word must be complete and  can't be itself
         */
        if(curIndex == word.length()) return startIndex != 0 && (trie == GlobalTrie || trie.children[26] != null);

        if(trie.children[26] != null){
            // It's a word, search if the rest part is dividable
            if(divideWord(GlobalTrie, word, curIndex, curIndex)) return true;
        }

        char ch = word.charAt(curIndex);
        // Doesn't exist
        if(trie.children[ch - 'a'] == null) return false;
        // Continue searching
        return divideWord(trie.children[ch - 'a'], word, startIndex, curIndex + 1);
    }

    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        /*
            Sort words by length
         */
        Arrays.sort(words, (a, b) -> a.length() - b.length());

        /*
            Save words in to a Trie one by one.
            When saving a word, we use a list to save all words that can be prefixes of it.
            Then we check whether the postfix exists in the list
         */
        for(String word: words){
            GlobalTrie.addWord(word, 0);

            if(divideWord(GlobalTrie, word, 0, 0)) result.add(word);
        }

        return result;
    }
```


