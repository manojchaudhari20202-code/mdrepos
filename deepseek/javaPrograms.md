
## Array / String

---

### 1. Merge Strings Alternately
**Problem**: Merge two strings by alternating characters, starting with `word1`. Append remaining characters from the longer string.

**Solution**:
```java
public String mergeAlternately(String word1, String word2) {
    StringBuilder merged = new StringBuilder();
    int i = 0, j = 0;
    while (i < word1.length() || j < word2.length()) {
        if (i < word1.length()) merged.append(word1.charAt(i++));
        if (j < word2.length()) merged.append(word2.charAt(j++));
    }
    return merged.toString();
}
```

---

### 2. Greatest Common Divisor of Strings
**Problem**: Find the largest string `x` that divides both `str1` and `str2` (i.e., `str1 = x + x + ...` and same for `str2`).

**Solution**:
```java
public String gcdOfStrings(String str1, String str2) {
    if (!(str1 + str2).equals(str2 + str1)) return "";
    int gcdLen = gcd(str1.length(), str2.length());
    return str1.substring(0, gcdLen);
}

private int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```

---

### 3. Kids With the Greatest Number of Candies
**Problem**: After giving `extraCandies` to each kid, determine if that kid can have the greatest number of candies.

**Solution**:
```java
public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
    int max = 0;
    for (int c : candies) max = Math.max(max, c);
    List<Boolean> result = new ArrayList<>();
    for (int c : candies) result.add(c + extraCandies >= max);
    return result;
}
```

---

### 4. Can Place Flowers
**Problem**: Can we plant `n` new flowers in a flowerbed without adjacent flowers? (1 = planted, 0 = empty).

**Solution**:
```java
public boolean canPlaceFlowers(int[] flowerbed, int n) {
    int count = 0;
    for (int i = 0; i < flowerbed.length && count < n; i++) {
        if (flowerbed[i] == 0) {
            boolean leftEmpty = (i == 0) || (flowerbed[i - 1] == 0);
            boolean rightEmpty = (i == flowerbed.length - 1) || (flowerbed[i + 1] == 0);
            if (leftEmpty && rightEmpty) {
                flowerbed[i] = 1; // plant
                count++;
                i++; // skip next plot
            }
        }
    }
    return count >= n;
}
```

---

### 5. Reverse Vowels of a String
**Problem**: Reverse only the vowels in the given string.

**Solution**:
```java
public String reverseVowels(String s) {
    char[] chars = s.toCharArray();
    int left = 0, right = chars.length - 1;
    String vowels = "aeiouAEIOU";
    while (left < right) {
        while (left < right && vowels.indexOf(chars[left]) == -1) left++;
        while (left < right && vowels.indexOf(chars[right]) == -1) right--;
        if (left < right) {
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            left++;
            right--;
        }
    }
    return new String(chars);
}
```

---

### 6. Reverse Words in a String
**Problem**: Reverse the order of words in a string (words are separated by spaces, may have leading/trailing/multiple spaces).

**Solution**:
```java
public String reverseWords(String s) {
    String[] words = s.trim().split("\\s+");
    StringBuilder reversed = new StringBuilder();
    for (int i = words.length - 1; i >= 0; i--) {
        reversed.append(words[i]);
        if (i > 0) reversed.append(" ");
    }
    return reversed.toString();
}
```

---

### 7. Product of Array Except Self
**Problem**: Return an array `output` where `output[i]` is the product of all elements except `nums[i]`, without division and in O(n).

**Solution**:
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    
    // Left products
    result[0] = 1;
    for (int i = 1; i < n; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    
    // Right products and combine
    int right = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= right;
        right *= nums[i];
    }
    return result;
}
```

---

### 8. Increasing Triplet Subsequence
**Problem**: Return true if there exists `i < j < k` such that `nums[i] < nums[j] < nums[k]`.

**Solution**:
```java
public boolean increasingTriplet(int[] nums) {
    int first = Integer.MAX_VALUE;
    int second = Integer.MAX_VALUE;
    for (int num : nums) {
        if (num <= first) {
            first = num;          // smallest so far
        } else if (num <= second) {
            second = num;         // second smallest
        } else {
            return true;          // found a number greater than both
        }
    }
    return false;
}
```

---

### 9. String Compression
**Problem**: Compress a character array in-place using counts of consecutive characters. Return the new length.

**Solution**:
```java
public int compress(char[] chars) {
    int write = 0; // position to write compressed result
    int anchor = 0; // start of current group
    for (int read = 0; read < chars.length; read++) {
        // If at end of group or end of array
        if (read + 1 == chars.length || chars[read] != chars[read + 1]) {
            chars[write++] = chars[anchor]; // write the character
            if (read > anchor) { // more than one occurrence
                String count = Integer.toString(read - anchor + 1);
                for (char c : count.toCharArray()) {
                    chars[write++] = c;
                }
            }
            anchor = read + 1; // move anchor to next group
        }
    }
    return write;
}
```
## Two Pointers / Sliding Window

---
### 1. Move Zeroes
**Problem**: Move all zeros to the end while maintaining the relative order of non-zero elements. Do it in-place.

**Solution** (Two pointers):
```java
public void moveZeroes(int[] nums) {
    int lastNonZeroFoundAt = 0;
    // Move all non-zero elements to the front
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[lastNonZeroFoundAt++] = nums[i];
        }
    }
    // Fill the remaining positions with zeros
    for (int i = lastNonZeroFoundAt; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

---

### 2. Is Subsequence
**Problem**: Given two strings `s` and `t`, check if `s` is a subsequence of `t`.

**Solution** (Two pointers):
```java
public boolean isSubsequence(String s, String t) {
    int i = 0, j = 0;
    while (i < s.length() && j < t.length()) {
        if (s.charAt(i) == t.charAt(j)) i++;
        j++;
    }
    return i == s.length();
}
```

---

### 3. Container With Most Water
**Problem**: Find two lines that together with the x-axis form a container that holds the most water.

**Solution** (Two pointers from ends):
```java
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1;
    int max = 0;
    while (left < right) {
        int area = Math.min(height[left], height[right]) * (right - left);
        max = Math.max(max, area);
        if (height[left] < height[right]) left++;
        else right--;
    }
    return max;
}
```

---

### 4. Max Number of K-Sum Pairs
**Problem**: Find the maximum number of operations to remove pairs that sum to `k` from an array.

**Solution** (Sort + two pointers):
```java
public int maxOperations(int[] nums, int k) {
    Arrays.sort(nums);
    int left = 0, right = nums.length - 1;
    int count = 0;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == k) {
            count++;
            left++;
            right--;
        } else if (sum < k) left++;
        else right--;
    }
    return count;
}
```

---

### 5. Maximum Average Subarray I
**Problem**: Find a contiguous subarray of length `k` with the maximum average value.

**Solution** (Sliding window):
```java
public double findMaxAverage(int[] nums, int k) {
    int sum = 0;
    for (int i = 0; i < k; i++) sum += nums[i];
    int maxSum = sum;
    for (int i = k; i < nums.length; i++) {
        sum += nums[i] - nums[i - k];
        maxSum = Math.max(maxSum, sum);
    }
    return (double) maxSum / k;
}
```

---

### 6. Maximum Number of Vowels in a Substring of Given Length
**Problem**: Given a string, find the maximum number of vowels in any substring of length `k`.

**Solution** (Sliding window with vowel set):
```java
public int maxVowels(String s, int k) {
    String vowels = "aeiou";
    int count = 0;
    // Initial window
    for (int i = 0; i < k; i++) {
        if (vowels.indexOf(s.charAt(i)) != -1) count++;
    }
    int max = count;
    for (int i = k; i < s.length(); i++) {
        if (vowels.indexOf(s.charAt(i)) != -1) count++;
        if (vowels.indexOf(s.charAt(i - k)) != -1) count--;
        max = Math.max(max, count);
    }
    return max;
}
```

---

### 7. Max Consecutive Ones III
**Problem**: Given a binary array, find the longest subarray with at most `k` zeros that can be flipped to ones.

**Solution** (Sliding window):
```java
public int longestOnes(int[] nums, int k) {
    int left = 0, right;
    for (right = 0; right < nums.length; right++) {
        if (nums[right] == 0) k--;
        if (k < 0) {
            if (nums[left] == 0) k++;
            left++;
        }
    }
    return right - left;
}
```

---

### 8. Longest Subarray of 1's After Deleting One Element
**Problem**: Find the longest subarray containing only 1's after deleting exactly one element (any element).

**Solution** (Sliding window with at most one zero):
```java
public int longestSubarray(int[] nums) {
    int left = 0, zeroCount = 0, maxLen = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 0) zeroCount++;
        while (zeroCount > 1) {
            if (nums[left] == 0) zeroCount--;
            left++;
        }
        maxLen = Math.max(maxLen, right - left); // window length, but we delete one zero, so actual 1's = length-1? Wait careful.
        // Actually we need the length of subarray of 1's after deleting one element.
        // The window can contain at most one zero, and we delete that zero.
        // So the number of 1's = window length - 1 (if window contains a zero) or window length - 1? If no zero, we must delete one 1, so it's length-1 as well.
        // Therefore answer = max window length - 1.
    }
    return maxLen; // we'll compute after loop as maxLen-1, but we need to be precise.
}
// Corrected version:
public int longestSubarray(int[] nums) {
    int left = 0, zeroCount = 0, maxLen = 0;
    for (int right = 0; right < nums.length; right++) {
        if (nums[right] == 0) zeroCount++;
        while (zeroCount > 1) {
            if (nums[left] == 0) zeroCount--;
            left++;
        }
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen - 1; // delete one element
}
```

## Prefix Sum / Hash Set & Map

Java Find the Highest Altitude
Java Find Pivot Index
Java Find the Difference of Two Arrays
Java Unique Number of Occurrences
Java Determine if Two Strings Are Close
Java Equal Row and Column Pairs

## Stack / Queue

Java Removing Stars From a String
Java Asteroid Collision
Java Decode String
Java Number of Recent Calls
Java Dota2 Senate

## Linked List

Java Delete the Middle Node of a Linked List
Java Odd Even Linked List
Java Reverse Linked List
Java Maximum Twin Sum of a Linked List

## Binary Tree

Java Maximum Depth of Binary Tree
Java Leaf-Similar Trees
Java Count Good Nodes in Binary Tree
Java Path Sum III
Java Longest ZigZag Path in a Binary Tree
Java Lowest Common Ancestor of a Binary Tree
Java Binary Tree Right Side View
Java Maximum Level Sum of a Binary Tree
Java Search in a Binary Search Tree
Java Delete Node in a BST

## Graphs & Search (BFS/DFS)

Java Keys and Rooms
Java Number of Provinces
Java Reorder Routes to Make All Paths Lead to the City Zero
Java Evaluate Division
Java Nearest Exit from Entrance in Maze
Java Rotting Oranges

## Heaps, Bit Manipulation, & DP

Java Kth Largest Element in an Array
Java Smallest Number in Infinite Set
Java Maximum Subsequence Score
Java Total Cost to Hire K Workers
Java Counting Bits
Java Single Number
Java N-th Tribonacci Number
Java Min Cost Climbing Stairs
Java House Robber
Java Longest Common Subsequence




