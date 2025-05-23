# Interview important DSA questions

## Array

### 1.Second Largest

Given an array of positive integers arr[], return the second largest element from the array. If the second largest element doesn't exist then return -1.

Note: The second largest element should not be equal to the largest element.

```cpp

class Solution {
  public:
    int getSecondLargest(vector<int> &arr) {
        int largest=-1,second_largest=-1;
        for(auto num:arr){
            if(num>largest){
                second_largest=largest;
                largest=num;
            }
            else if(num<largest && num>second_largest)
            second_largest=num;
        }
        return second_largest;
    }
};

```
