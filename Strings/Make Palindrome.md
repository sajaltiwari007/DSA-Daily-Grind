## Question Link-: https://codeforces.com/problemset/problem/600/C

### Approach and Intuition:

1. **Frequency Count**:

   * Count how many times each character appears using a `freq[26]` array.

2. **Odd Frequency Adjustment**:

   * For a string to be a palindrome:

     * At most **one character** can have an odd frequency (only in case of odd-length strings).
   * While more than one character has an odd count, adjust:

     * Increment a smaller odd-frequency character (left side).
     * Decrement a larger odd-frequency character (right side).
   * This minimizes character changes and ensures lexicographically smaller result.

3. **Build the Palindrome**:

   * From updated frequency:

     * Build half of the palindrome from left to right.
     * If there's a middle character (odd frequency), place it in the center.
     * Mirror the first half to get the second half (by reversing).

4. **Construct the Final String**:

   * Concatenate the first half + middle (if any) + reversed half.

---

### Time and Space Complexity:

* **Time Complexity**: O(n + 26) = O(n)

  * n for reading and counting characters.
  * 26 (constant) for processing the alphabet.
* **Space Complexity**: O(26 + n) = O(n)

  * 26 for frequency array, n for building the palindrome.
---
### Java Solution





        
      

    import java.util.*;

    public class LexicographicallySmallestPalindrome {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();
        int[] freq = new int[26];for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
        }
        int odd = 0;
        for (int i : freq) {
            if (i % 2 != 0) odd++;
        }

   
        int left = 0;
        int right = 25;
        while (odd > 1) {
            while (left <= 25 && freq[left] % 2 == 0) left++;
            while (right >= 0 && freq[right] % 2 == 0) right--;
            freq[left]++;
            freq[right]--;
            odd -= 2;
        }
        StringBuilder sb = new StringBuilder();
        String mid = "";
        for (int i = 0; i < 26; i++) {
            if (freq[i] % 2 != 0) {
                mid = String.valueOf((char) (i + 'a'));
                freq[i]--;
            }
            for (int j = 0; j < freq[i] / 2; j++) {
                sb.append((char) (i + 'a'));
            }
        }

        // Step 5: Build final palindrome
        String answer = sb.toString() + mid + sb.reverse().toString();
        System.out.println(answer);
    }}
