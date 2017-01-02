# Longest Substring Without Repeating Characters

### Question:

Given a string, find the length of the longest substring without repeating characters.

Examples:

Given `"abcabcbb"`, the answer is `"abc"`, which the length is `3`.

Given `"bbbbb"`, the answer is `"b"`, with the length of `1`.

Given `"pwwkew"`, the answer is `"wke"`, with the length of `3`. Note that the answer must be a substring, `"pwke"` is a subsequence and not a substring.

---

### Think Out Loud:

We want to find the longest substring without repeating characters. The first thing comes to my mind is that we need a hash table to store every character in a substring so that when a new character comes in, we can easily know whether this character is already in the substring or not. I call it as `valueIdxHash`.  Then,  a substring has a `startIdx` and `endIdx`. So we need a variable to keep track of the starting index of a substring and I call it as `startIdx`.  Let's assume we are at index `i` and we already have a substring `(startIdx, i - 1)`. Now, we want to check whether this substring can keep growing or not. 

If the `valueIdxHash` **contains** `str[i]`, it means it is a repeated character. But we still need to check whether this repeated character is in the substring `(startIdx, i - 1)`. So we need to retrieve the index of `str[i]` that is appeared last time and then compare this index with `startIdx`. 

- If `startIdx` is larger, it means the last appeared `str[i]` is **outside** of the substring. Thus the subtring can keep growing. 
- If `startIdx` is smaller, it means the last appeared `str[i]` is **within** of the substring. Thus, the substring cannot grow any more. `startIdx` will be updated as `valueIdxHash[str[i]] + 1` and the new substring `(valueIdxHash[str[i]] + 1, i)` has potential to keep growing.

If the `valueIdxHash` **does not contain** `str[i]`,  the substring can keep growing.



### Show Me Your Code:

##### javascript

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    const valueIdxHash = {};
    var longest = 0;
    var startIdx = 0;
    for(var i = 0; i < s.length; i++) {
        if(s[i] in valueIdxHash) { //constain s[i]
            var lastAppearanceIdx = valueIdxHash[s[i]];
            
            if (lastAppearanceIdx < startIdx) {
                //does nothing
            } else {
                startIdx = lastAppearanceIdx + 1;
            }
            
            valueIdxHash[s[i]] = i;
        } else { //does not contain s[i]
            valueIdxHash[s[i]] = i;            
        }
        
        longest = Math.max(longest, i - startIdx + 1);
    }
    return longest;
};

//simplified version
var lengthOfLongestSubstring = function(s) {
    const valueIdxHash = {};
    var longest = 0;
    var startIdx = 0;
    for(var i = 0; i < s.length; i++) {
        if(s[i] in valueIdxHash) {
            startIdx = Math.max(startIdx, valueIdxHash[s[i]] + 1);
        }
        valueIdxHash[s[i]] = i;
        longest = Math.max(longest, i - startIdx + 1);
    }
    return longest;
};
```

