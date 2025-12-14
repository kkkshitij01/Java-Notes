Given an array of strings `strs`, group the anagrams together. You can return the answer in **any order**.

**Example 1:**

**Input:** strs = `["eat","tea","tan","ate","nat","bat"]`

**Output:**` [["bat"],["nat","tan"],["ate","eat","tea"]]`

**Explanation:**

- There is no string in strs that can be rearranged to form `"bat"`.
- The strings `"nat"` and `"tan"` are anagrams as they can be rearranged to form each other.
- The strings `"ate"`, `"eat"`, and `"tea"` are anagrams as they can be rearranged to form each other.

**Example 2:**

**Input:** strs =` [""]`

**Output:**` [[""]]`

**Example 3:**

**Input:** strs =` ["a"]`

**Output:**` [["a"]]`

**Constraints:**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

---

# Approach: 

### **• Step 1 — Create a HashMap**

- Key → a _sorted version_ of the word
    
- Value → list of all original words that match this sorted key
    

Example:  
`"eat" → "aet"`  
`"tea" → "aet"`  
`"tan" → "ant"`

So:

`"aet" → ["eat", "tea", "ate"] "ant" → ["tan", "nat"]`

---

# **• Step 2 — For every string:**

### **1. Convert string to char array**

`char ch[] = str.toCharArray();`

### **2. Sort the characters**

`Arrays.sort(ch);`

### **3. Convert sorted chars back to a string**

`String a = new String(ch);`

This sorted string becomes the **key**.

---

# **• Step 3 — Put into HashMap**

### **If key does NOT exist → create a new list**

`if (!map.containsKey(a)) {     map.put(a, new ArrayList<>()); }`

### **Add the original string to that list**

`map.get(a).add(str);`

---

# **• Step 4 — Return all grouped values**

`return new ArrayList<>(map.values());`

This gives a list of all anagram groups.

---


```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {

        List<List<String>> ans = new ArrayList<>();
        Map<String , List<String> > map = new HashMap<>();

        for(String str : strs){

            // Convert string to char array so we can sort it
            char ch[] = str.toCharArray();
            Arrays.sort(ch);

            // Sorted version becomes the key
            String a = new String(ch);

            // If this key does not exist, create a new list
            if(!map.containsKey(a)){
                map.put(a, new ArrayList<>());
            }

            // Add the original word into the anagram group
            map.get(a).add(str);
        }

        // ---- Using iterator to add each group's values into ans ----
        Iterator<String> it = map.keySet().iterator();
        while(it.hasNext()){
            String key = it.next();
            ans.add(map.get(key));   // add the list of anagrams for this key
        }

        return ans;
    }
}



```