### LeetCode 32 Longest Valid Parentheses, hard

##### [Link to the problem](https://leetcode.com/problems/longest-valid-parentheses/)

##### Method

* First version
  
  Below is a sample string comprising with '(' and ')'
  
  Start from the left most character.
  
  It's a left parenthesis, start checking. 
  
  **(**())((())))())
  
  Read until right parenthesis is more than left parenthesis. During this progress, when left and right counts equal, save the length.
  
  -(())-((())))())
  
  -(())((()))-)())
  
  (())((()))**)**())
  
  Check next.
  
  it's a right parenthesis, go next.
  
  (())((())))**(**))
  
  Same procedure.
  
  (())((())))-()-)
  
  (())((())))()**)**
  
  It should work with O(n) since every character is only visited once.
  
  **What's the Problem**
  
  If the left parenthesis is more than right parenthesis, it needs to repeat the check for that part.
  
  An extreme situation is
  
  ((((((((((((((((((((((((((((
  
  Every character here will trigger the check once, and the program will read through the whole string.
  
  With the maximum limit of input length, it may run more than 10 seconds.

* Final version
  
  Still do the same check as above. But after the iteration, do the check in reversed order.
  
  In whatever situation, it will iterate the string twice. Still O(n) complexity

##### Code

* Final version
  
  ```java
  public static int longestValidParentheses(String s) {
      int length = s.length(), max = 0, mark = 0;
  
      // left, right are count of left/right parentheses
      int index = 0, left = 0, right = 0;
      while (index < length){
          if(s.charAt(index) == '('){
              left++;
          }else{
              right++;
          }
  
          // Invalid, start from 0
          if(right > left){
              left = 0; right = 0;
              mark = index;
          }

          // Update result
          if(left == right){
              max = Math.max(max, left + right);
          }
  
          index++;
      }
  
      index = length - 1;
      left = 0;
      right = 0;
  
      while (index > mark){
          if(s.charAt(index) == '('){
              left++;
          }else{
              right++;
          }
  
          if(right < left){
              left = 0; right = 0;
          }
  
          if(left == right){
              max = Math.max(max, left + right);
          }
  
          index--;
      }
  
      return max;
  }
  ```

* First version
  
  ```java
  public static int longestValidParentheses_oldVersion(String s) {
      int length = s.length(), max = 0;
  
      int left = 0;
      while(left < length){
          // If it's (, start check, read until ) is more than (.
          if (s.charAt(left) == '(') {
              int right = left + 1, count = 1, len = 0;
  
              while (right < length) {
                  if (s.charAt(right) == '(') {
                      count++;
                  } else {
                      count--;
                  }
  
                  if(count < 0){
                      // Finish checking, set pointer to left after right
                      left = right;
                      break;
                  } else{
                      if (count == 0) {
                          len = right - left + 1;
                      }
  
                      right++;
                  }
              }
  
              max = Math.max(max, len);
          }
  
          left++;
      }
  
      return max;
  }
  ```

* Main method to check the time diff
  
  ```java
  public static void main(String[] args){
      StringBuffer temp = new StringBuffer();
      temp.append("(".repeat(100000));
      temp.append(")".repeat(50000));
  
      long start = System.currentTimeMillis();
      System.out.println("Result:" + longestValidParentheses(temp.toString()));
      long end = System.currentTimeMillis();
  
      start = System.currentTimeMillis();
      System.out.println("Result:" + longestValidParentheses_oldVersion(temp.toString()));
      end = System.currentTimeMillis();
  
      System.out.println(end - start);
  }
  ```
