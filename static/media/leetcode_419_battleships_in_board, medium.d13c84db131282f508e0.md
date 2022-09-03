### LeetCode 419 Battleships in a Board, Medium

(One visit, O(m * n) time, O(1) memory)

##### [Link to the problem](https://leetcode.com/problems/battleships-in-a-board/)

##### Method

Iterator all cells.

When we find a cell with 'X', we should check if its left cell or top shell is 'X', which means we have visited it.

Otherwise, it's a new ship, update count. 

##### Code

```java
public class Main {
    public int countBattleships(char[][] board) {
        int height = board.length, width = board[0].length, count = 0;
        for(int i=0; i<height; i++){
            for(int j=0; j<width; j++){
                if(board[i][j] == 'X' && (j == 0 || board[i][j-1] == '.') && (i==0 || board[i-1][j] == '.')){
                    count++;
                }
            }
        }
        return count;
    }
}
```
