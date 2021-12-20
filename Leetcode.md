# Leetcode

* [yuanguangxin/LeetCode: LeetCode刷题记录与面试整理 (github.com)](https://github.com/yuanguangxin/LeetCode)
* 

## 1. q1 two sum

https://leetcode.com/problems/two-sum/

**Description:**

intput = [2, 7, 11, 15]   target = 9

output = [0, 1](index)

tips: assume each input would have exactly one solution

**Algorithm:**

* build a hash map, set the element and its index as a key-value pair.
* traversing elements in array one by one. 
* search a pair of elements who were summed up to 9. the traverse has choose a value then to check which element is equals to 9 plus a[i]. 
* return the indices of these two elements.

**code:**

````java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] { map.get(complement), i };// the real return value
            }
            map.put(nums[i], i);// put pair into map
        }
        // In case there is no solution, we'll just return null
        return null;
    }
}
````

**Time Complexity:** O(n)

**Space Complexity:** O(n)

## 2. 387. First Unique Character in a String

**Description:**

find the first non-repeating character in a string  return its index. if not exist, returns -1

```
Input: s = "leetcode" //'aabb'
Output: 0//-1
```

**Algorithm:**

* using hash table, the pair is <character, times>.

* build hash table. get each character from string using a loop and its loop times is string's length.

* record how many times that each character appears. searching key 'c' and add 1 after each time it appears. 

  for one pair, judge if it exists in the hash table. if it doesn't exist, put the c-times to hash table while it returns 0, but adding 1 to 0 as times. if it exists, put the c-times to hash table, but it returns the value of the key, adding 1 to value as times. 

* comparing the value of each key, print the first index of key whose value is 1!

**code:**

````java
class Solution {
    public int firstUniqChar(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        int n = s.length();
        for(int i = 0; i < n; i++ ){
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c,0) + 1 );          
        }
        
        for(int i = 0; i < n; i++ ){
            if (map.get(s.charAt(i)) == 1)
                return i;                
        }     
       return -1; 
    } 
}
````

**Time Complexity:** *O(n)*

**Space Complexity:** *O(1)*

**Tips:**

1. how to get character from a string?

   ````
   charAt():
   String myStr = "Hello";
   char result = myStr.charAt(0);
   ````

2. how to know c does not equals to any of the rest elements?

   * the kv pair is <character, times it appears> rather than  <character, index> .
   * the hashtable is empty, if the key is not exist, return 0 and add 1 to it. if the key exist returns it's value which is the times if appeared previously, then adds 1  to it. 
   * the value equals to 1 meaning it is unique. 

3. Map<Character, Integer> map = new HashMap<Character, Integer>() vs  new HashMap<>?

   there is no influence if deletes Character, Integer in <>?

## 3.q2 Add two numbers

**Description:**

q1

two non-empty linked lists stored in reverse order, return the sum as a linked list.

````
Input: l1 = [2,4,3], l2 = [7,6,4]
Output: [7,0,1,1]
Explanation: 342 + 467 = 1107.
````

**Algorithm:**

![q2](C:\Users\xuhan\Desktop\java\notes\pictures\q2.png)



**Code:**

````java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
    }
}
````



**Time Complexity:**  *O(max(m, n))*

**Space Complexity:** *O(max(m,n))*

**Tips**

1. Linked list + addition simulation. casue the two numbers may in different length even sum would has more digits. e.g 123 + 456789

2. Node = Node.next, must be used to push the node back! otherwise it can't loop.

3. how to imput l1, l2? class linknode means?

   

## 4. q19. Remove Nth Node From End of List

**Description:**

**Algorithm:**

**Code:**

**Time Complexity:** 

**Space Complexity:**

**Tips**



## 5.

**Description:**

**Algorithm:**

**Code:**

**Time Complexity:** 

**Space Complexity:**

**Tips**



