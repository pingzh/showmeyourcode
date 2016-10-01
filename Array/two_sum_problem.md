# Two Sum Problem

### Question:

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

Each input would have **exactly** one solution.

```
Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

---

### Method:

**Method 1**: You can easily solve this problem by having two for loops. Get the first value from
outer loop and the second value from the inner value and check the sum.
But the time complexity of this method is O(n^2), which is too much for this
problem.

**Method 2**: Generally speaking, to reduce the time complexity, you can add more space.
One candidate is hash table. So let's have a hash table to the number as key and its index in the array as the value.
And then, loop through the array, find the difference of target and current
value. If the difference is in the hash table, then we find the answer. Wait!
If the array is [3, 1, 5], the target is 6. Our algorithm will fail because the
difference of 6 and 3 is 3, and 3 is in the hash table. What are we missing? We
miss something in the condition, if the difference is in the hash table and
**the index of the difference in the hash table is not the index of current value**, then we can claim we find the
answer. Now, the time complexity is O(n) and space complexity is also O(n);

**Method 3**: What if you are not allowed to use hash table? Let's go back to
Method 1. The gist of Method 1 is to use two pointers, the first
pointer points to a value in the array, and the second pointer goes through the
array from the beginning of the array to find the difference of target and the
value pointed by the first pointer. The main issue is lots of numbers are
scanned several times. Can we cut it down? Let's say if we know the sum
of the value pointed by the first pointer and the value pointed by the second
pointer is greater than the target, it will be great that the second pointer
does not go forward. The answer is sort the array! Thus, the algorithm is to
sort the array first, and have a left pointer pointing to the first value of the
array and a right pointer pointing the last value of the array. If (the value
pointed by the left pointer + the value pointed by the right pointer) is
greater than the target, you will know you should move the right pointer to the
left because anything in the right side of the right pointer is greater than
the value pointed by the right pointer, which makes the sum bigger, and
vice versa. The time complexity is O(nlog(n));


---

### Show Me Your Code:

##### CPP Code (Method 2)

```cpp
bool _is_in_map(unordered_map<int, int>& hash, int value) {
    return hash.find(value) != hash.end();
}
vector<int> two_sum(vector<int> &numbers, int target) {
    vector<int> result;
    unordered_map<int, int> value_index_hash;

    for(int i = 0; i < numbers.size(); i++) {
        value_index_hash[numbers[i]] = i;
    }

    for(int i = 0; i < numbers.size(); i++) {
        int difference = target - numbers[i];

        if(_is_in_map(value_index_hash, difference) && i != value_index_hash[difference]) {
            result.push_back(value_index_hash[difference]);
            result.push_back(i);
            break;
        }
    }
    return result;
}

```

##### Javascript Code (Method 2)

```javascript
var twoSum = function(nums, target) {
    var valueIndexHash = nums.reduce((previsouValue, currentValue, index) => {
        previsouValue[currentValue] = index;
        return previsouValue;
    }, {});

    var result = [];
    nums.some((currentValue, index) => {
        var difference = target - currentValue;
        if(difference in valueIndexHash && valueIndexHash[difference] != index) {
            result = [index, valueIndexHash[difference]];
            return true;
        }
    });
    return result;
};
```

##### CPP Code (Method 3)

```cpp
vector<int> two_sum(vector<int> &numbers, int target) {
    vector<int> result;
    vector<int> numbers_dup = vector<int>(numbers);

    sort(numbers_dup.begin(), numbers_dup.end());

    int left = 0, right = numbers_dup.size() - 1;
    while(left <= right) {
        int sum = numbers_dup[left] + numbers_dup[right];

        if(sum == target) {
            //find the idex of numbers_dup[left] and numbers_dup[right]
            for(int i = 0; i < numbers.size(); i++) {
                if(numbers[i] == numbers_dup[left] || numbers[i] == numbers_dup[right]) {
                    result.push_back(i);
                }
                if(result.size() == 2) {
                    return result;
                }
            }
        }
        else if(sum > target) {
            right--;
        }
        else {
            left++;
        }
    }
}
```
