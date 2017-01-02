# Largest Number From Numbers Array

### Question:

Given an array of positive numbers, arrange them in a way that yields the largest value.

For example, if the given numbers are [54, 546, 548, 60], the arrangement 6054854654 gives the largest value. And if the given numbers are [1, 34, 3, 98, 9, 76, 45, 4], then the arrangement 998764543431 gives the largest value.

---

### Think Out Loud:

Let us start with a simple case, assuming the numbers are only single digits.
Then the answer is straightforward, sort the array in descending order and then combine all numbers
into a string. Assume the array is [6, 9, 7], sort the array to [9, 7, 6] and then combine it
to "976".

Moving further a little bit, let's assume numbers are more than one digits but have same number of digits.
If the array is [12, 32, 15], can we use the same way, sort the array in descending order and then combine them?
The answer is yes. To make the final number as large as possible, it is natural
to put the largest number to the leftmost position. So sort the array to [32, 15, 12] and combine it to "321512".

Now, what if numbers have different digits, like [5, 54], can we sort it?
Quick answer is no. If we sort it to [54, 5] and combine it to "545", there is an apparently larger number, 554.
Does that mean we cannot use the above algorithm? The answer is we can use the
same algorithm but with another sorting criteria instead of the default sorting
algorithm. Then, how are we going to sort [5, 54]? Remember, our goal is to combine numbers
to form a largest number, so the easiest way is to combine 5 and 54 to either
"554" or "545" and compare them, if "554" > "545", then 5 should be before 54 and vice versa.
So the comparator function is taking two arguments, a and b, and determining their order
by combining them to "ab" and "ba" and comparing them.

So, the algorithm is to use the sort function provided by library and feed it
with a customized comparator.

---

### Show Me Your Code:

##### CPP Code

```cpp
static bool my_comparator (int number1,int number2) {
    string number1_str = to_string(number1);
    string number2_str = to_string(number2);
    return (number1_str + number2_str) > (number2_str + number1_str);
}
string combine_number(vector<int>& nums) {
    sort(nums.begin(), nums.end(), my_comparator);
    string result = "";
    for(auto num : nums) {
        result += to_string(num);
    }
    return result;
}
```


##### Ruby Code

```ruby
def combine_number(numbers)
  numbers_in_str = numbers.map(&:to_s) # convert number to string
  sorted_numbers = numbers_in_str.sort do |number1, number2|
    (number2 + number1) <=> (number1 + number2)
  end
  sorted_numbers.join
end
```

##### Javascript Code

```javascript
var combineNumber = function(nums) {
    const numsInStr = nums.map(n => n.toString());
    const sortedNums = numsInStr.sort((n1, n2) => {
        var n1N2 = n1 + n2;
        var n2N1 = n2 + n1;
        if(n1N2 === n2N1) {
            return 0;
        }
        return n1N2 > n2N1 ? -1 : 1;
    });
    return sortedNums.join("");
}
```
