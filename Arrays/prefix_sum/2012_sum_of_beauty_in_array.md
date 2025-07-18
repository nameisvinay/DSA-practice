````md
==================================================================
🧩 Problem: Sum of Beauty in the Array
🔗 Link   : https://leetcode.com/problems/sum-of-beauty-in-the-array/
📚 Topic  : Arrays
📈 Level  : Medium
==================================================================

📄 Description:
--------------------------------------------------
You are given a 0-indexed integer array nums. For each index i (1 <= i <= nums.length - 2) the beauty of nums[i] equals:
    2, if nums[j] < nums[i] < nums[k], for all 0 <= j < i and for all i < k <= nums.length - 1.
    1, if nums[i - 1] < nums[i] < nums[i + 1], and the previous condition is not satisfied.
    0, if none of the previous conditions holds.
Return the sum of beauty of all nums[i] where 1 <= i <= nums.length - 2.

  Example 1:
  
  Input: nums = [1,2,3]
  Output: 2
  Explanation: For each index i in the range 1 <= i <= 1:
  - The beauty of nums[1] equals 2.
  Example 2:
  
  Input: nums = [2,4,6,4]
  Output: 1
  Explanation: For each index i in the range 1 <= i <= 2:
  - The beauty of nums[1] equals 1.
  - The beauty of nums[2] equals 0.

`````
---------------------------------------------------

🧠 My inital idea:

    i took of three nested loops: with conditions given in problem statement.
                 1 <= i <= nums.length - 2  ---> [i<=(len(nums)-2) means including len(nums)-2] =  i -> (1,len(nums)-1)
                 0 <= j < i -->  [< i means upto i not including] =  j in range of (0,i)
                 i < k < len(nums)-1 --> k in range of (i+1,len(nums)-1)


  initially i was mistake in checking statement clearly and though of found any smaller than i break and greater than i also break
  and we now got both so +2 

  code i wrote with these logic. 
  
        beauty = 0
        for i in range(1,len(nums)-1): # --> inclusive
            found_smaller = False
            for j in range(0,i):
                if nums[j] < nums[i]:
                    found_smaller = True
                    break
            
            found_greater = False
            for k in range(i+1,len(nums)):
                if nums[i] < nums[k]:
                    found_greater = True
                    break
        
            if found_smaller and found_greater:
                beauty += 2
            
            elif nums[i-1] < nums[i] < nums[i+1]:
                beauty += 1
            
        return beauty

      ❌ Mistake:
          This logic only checks:
              -  If there is any one number smaller before i
              -  And any one number greater after i
              
          Which violates the problem's actual requirement: 
              -  All numbers before must be less than nums[i], and all numbers after must be greater.


    correct bruteforce approach after modification : O(N^2)
    
            beauty = 0
            for i in range(1, len(nums)-1):
                if max(nums[0:i]) < nums[i] < min(nums[i+1:]):
                    beauty += 2
                elif nums[i-1] < nums[i] < nums[i+1]:
                    beauty += 1
            return beauty

        ✅ This now checks the full ranges before and after.  But Time limit exceeds for larger inputs.


  Better approach Prefix_sum. 

        - Take prefix maximum from 0 to len(nums)
        - Take prefix minimum from len(nums) to 0

          check   max(nums[0..i-1]) < nums[i] < min(nums[i+1..n-1])

      lets take input : [1, 3, 2, 4, 6]

                              ↓
                  1   3   2   4   6
                  1   3   3   4   6   ---> prefix maximum    
                  1   2   2   4   6   ---> prefix minimum

    traverse through array and when reach index 3 -> 4 in array where all previous are minimum and after 4 are maximum.


    For left maximum:
        max_left[0] = nums[0]
        for i -> (1,len(nums))
            max_left[i] = max(max_left[i-1] , nums[i])

    For right minimum:
        min_right[n-1] = nums[n-1]
        for i -> (len(nums)-2,-1,-1)
            min_right[i] = min(min_right[i+1] , nums[i])


Final working code: (prefix and suffix):

        def sumOfBeauties(self, nums: List[int]) -> int:
        
                beauty = 0
                n = len(nums)
        
                max_left = [0] * n
                min_right = [0] * n
        
                max_left[0] = nums[0]
                for i in range(1,len(nums)):
                    max_left[i] = max(max_left[i-1] , nums[i])
                
                min_right[n-1] = nums[n-1]
                for i in range(n-2,-1,-1):
                    min_right[i] = min(min_right[i+1] , nums[i])
        
                for i in range(1,len(nums)-1):
                    if max_left[i-1] < nums[i] < min_right[i+1]:
                        beauty += 2
                    elif nums[i-1] < nums[i] < nums[i+1]:
                        beauty += 1
        
                return beauty

        

remember: choose before i and after i for nums[i]  -->  max_left[i-1] < nums[i] < min_right[i+1]
        
                
                  
                        
        

    











