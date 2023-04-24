# Problem Solving - Divide and Conquer

In this exercise, you’ll utilize problem solving patterns to solve the following code challenges:

countZeroes
---

Given an array of 1s and 0s which has all 1s first followed by all 0s, write a function called countZeroes, which returns the number of zeroes in the array.

Constraints: Time Complexity: O(log N)

Examples:
<pre>countZeroes([1,1,1,1,0,0]) // 2
countZeroes([1,0,0,0,0]) // 4
countZeroes([0,0,0]) // 3
countZeroes([1,1,1,1]) // 0</pre>

Solution:
<pre>
function countZeroes(arr) {
  let firstZeroIdx = binarySearchForFirstZero(arr);
  
  if(firstZeroIdx === -1){ 
    return 0; 
  }

  return arr.length - firstZeroIdx;
}

function binarySearchForFirstZero(arr, low = 0, high = arr.length - 1) {
  if (high >= low) {
    let mid = low + Math.floor((high - low) / 2);
    if ((mid === 0 || arr[mid - 1] === 1) && arr[mid] === 0) {
      return mid;
    } else if (arr[mid] === 1) {
      return binarySearchForFirstZero(arr, mid + 1, high)
    }
    return binarySearchForFirstZero(arr, low, mid - 1)
  }
  return -1;
}
</pre>

sortedFrequency
---

Given a sorted array and a number, write a function called sortedFrequency that counts the occurrences of the number in the array

Constraints: Time Complexity: O(log N)

Examples:
<pre>sortedFrequency([1,1,2,2,2,2,3],2) // 4
sortedFrequency([1,1,2,2,2,2,3],3) // 1
sortedFrequency([1,1,2,2,2,2,3],1) // 2
sortedFrequency([1,1,2,2,2,2,3],4) // -1</pre>

Solution:
<pre>
function sortedFrequency(arr, num) {
  let firstIdx = findFirst(arr, num);
  if (firstIdx == -1) return firstIdx;
  let lastIdx = findLast(arr, num);
  return lastIdx - firstIdx + 1;
}

function findFirst(arr, num, low = 0, high = arr.length - 1) {
  if (high >= low) {
    let mid = Math.floor((low + high) / 2)
    if ((mid === 0 || num > arr[mid - 1]) && arr[mid] === num) {
      return mid;
    } else if (num > arr[mid]) {
      return findFirst(arr, num, mid + 1, high)
    } else {
      return findFirst(arr, num, low, mid - 1)
    }
  }
  return -1
}

function findLast(arr, num, low = 0, high = arr.length - 1) {
  if (high >= low) {
    let mid = Math.floor((low + high) / 2)
    if ((mid === arr.length - 1 || num < arr[mid + 1]) && arr[mid] === num) {
      return mid;
    } else if (num < arr[mid]) {
      return findLast(arr, num, low, mid - 1)
    } else {
      return findLast(arr, num, mid + 1, high)
    }
  }
  return -1
}
</pre>

findRotatedIndex
---

Write a function called findRotatedIndex which accepts a rotated array of sorted numbers and an integer. The function should return the index of num in the array. If the value is not found, return -1.

Constraints: Time Complexity: O(log N)

Examples:
<pre>findRotatedIndex([3,4,1,2],4) // 1
findRotatedIndex([6, 7, 8, 9, 1, 2, 3, 4], 8) // 2
findRotatedIndex([6, 7, 8, 9, 1, 2, 3, 4], 3) // 6
findRotatedIndex([37,44,66,102,10,22],14) // -1
findRotatedIndex([6, 7, 8, 9, 1, 2, 3, 4], 12) // -1</pre>

Solution:
<pre>
function findRotatedIndex(array, num) {
  let pivot = findPivot(array)
  if (pivot > 0 && num >= array[0] && num <= array[pivot - 1]) {
    return binarySearch(array, num, 0, pivot - 1);
  } else {
    return binarySearch(array, num, pivot, array.length - 1);
  }
}

function binarySearch(array, num, start, end) {
  if (array.length === 0) return -1;
  if (num < array[start] || num > array[end]) return -1;

  while (start <= end) {
    let mid = Math.floor((start + end) / 2);
    if (array[mid] === num) {
      return mid;
    } else if (num < array[mid]) {
      end = mid - 1;
    } else {
      start = mid + 1;
    }
  }
  return -1;
}

function findPivot(arr) {
  if (arr.length === 1 || arr[0] < arr[arr.length - 1]){
    return 0;
   }
  let start = 0;
  let end = arr.length - 1;
  while (start <= end) {
    let mid = Math.floor((start + end) / 2);
    if (arr[mid] > arr[mid + 1]){
      return mid + 1;
    }
    else if (arr[start] <= arr[mid]) {
      start = mid + 1;
    } else {
      end = mid - 1;
    }
  }
}
</pre>

findRotationCount
---

Write a function called findRotationCount which accepts an array of distinct numbers sorted in increasing order. The array has been rotated counter-clockwise n number of times. Given such an array, find the value of n.

Constraints: Time Complexity: O(log N)

Examples:
<pre>findRotationCount([15, 18, 2, 3, 6, 12]) // 2
findRotationCount([7, 9, 11, 12, 5]) // 4
findRotationCount([7, 9, 11, 12, 15]) // 0</pre>

Solution:
<pre>
function findRotationCount(arr, low = 0, high = arr.length - 1) {
  if (high < low){
    return 0;
  }
  if (high === low){
    return low;
  }
  
  let mid = Math.floor((low + high) / 2);

  if (mid < high && arr[mid + 1] < arr[mid]){
    return mid + 1;
  }
  
  if (mid > low && arr[mid] < arr[mid - 1]) {
    return mid;
  }

  if (arr[high] > arr[mid]) {
    return findRotationCount(arr, low, mid - 1);
  }

  return findRotationCount(arr, mid + 1, high);
}
</pre>

findFloor
---

Write a function called findFloor which accepts a sorted array and a value x, and returns the floor of x in the array. The floor of x in an array is the largest element in the array which is smaller than or equal to x. If the floor does not exist, return -1.

Constraints: Time Complexity: O(log N)

Examples:
<pre>findFloor([1,2,8,10,10,12,19], 9) // 8
findFloor([1,2,8,10,10,12,19], 20) // 19
findFloor([1,2,8,10,10,12,19], 0) // -1</pre>

Solution:
<pre>
function findFloor(arr, num, low = 0, high = arr.length - 1) {
  if (low > high){ 
    return -1;
   }
   
  if (num >= arr[high]){
    return arr[high];
  }

  let mid = Math.floor((low + high) / 2)

  if (arr[mid] === num){
    return arr[mid];
   }

  if (mid > 0 && arr[mid - 1] <= num && num < arr[mid]) {
    return arr[mid - 1];
  }

  if (num < arr[mid]) {
    return findFloor(arr, num, low, mid - 1);
  }

  return findFloor(arr, num, mid + 1, high)
}
</pre>
