# Pattern 4 : Merge Intervals

## Merge Intervals (medium)
https://leetcode.com/problems/merge-intervals/

> Given a list of intervals, merge all the overlapping intervals to produce a list that has only mutually exclusive intervals.

Our goal is to merge the intervals whenever they overlap. 
The diagram above clearly shows a merging approach. Our algorithm will look like this:

1. Sort the intervals on the start time to ensure `a.start <= b.start`
2. If `a` overlaps `b` (i.e. `b.start <= a.end`), we need to merge them into a new interval `c` such that:
````
    c.start = a.start
    c.end = max(a.end, b.end)
````
We will keep repeating the above two steps to merge `c` with the next interval if it overlaps with `c`.

````
class Interval {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  get_interval() {
    return "[" + this.start + ", " + this.end + "]";
  }
}

function merge (intervals) {
  if(intervals.length < 2) {
    return intervals
  }
  
  //sort the intervals on the start time
  intervals.sort((a,b) => a.start - b.start
                )
  const mergedIntervals = []
  
  let start = intervals[0].start
  let end = intervals[0].end
  
  for(let i = 1; i < intervals.length; i++) {
    const interval = intervals[i]
    if(interval.start <= end) {
      //overlapping intervals, adjust the end
      end = Math.max(interval.end, end)    
    } else {
      //non-overlapping intercal, add the precious interval and reset
      mergedIntervals.push(new Interval(start, end))
      start = interval.start
      end = interval.end
    }
  }
  //add the last interval
  mergedIntervals.push(new Interval(start, end))
  return mergedIntervals;
};

merged_intervals = merge([new Interval(1, 4), new Interval(2, 5), new Interval(7, 9)]);
result = "";
for(i=0; i < merged_intervals.length; i++) {
  result += merged_intervals[i].get_interval() + " ";
}
console.log(`Merged intervals: ${result}`)
//Output: [[1,5], [7,9]]
//Explanation: Since the first two intervals [1,4] and [2,5] overlap, we merged them into one [1,5].


merged_intervals = merge([new Interval(6, 7), new Interval(2, 4), new Interval(5, 9)]);
result = "";
for(i=0; i < merged_intervals.length; i++) {
  result += merged_intervals[i].get_interval() + " ";
}
console.log(`Merged intervals: ${result}`)
//Output: [[2,4], [5,9]]
//Explanation: Since the intervals [6,7] and [5,9] overlap, we merged them into one [5,9].


merged_intervals = merge([new Interval(1, 4), new Interval(2, 6), new Interval(3, 5)]);
result = "";
for(i=0; i < merged_intervals.length; i++) {
  result += merged_intervals[i].get_interval() + " ";
}
console.log(`Merged intervals: ${result}`)
//Output: [[1,6]]
//Explanation: Since all the given intervals overlap, we merged them into one.
````
OR 
````
function merge(intervals) {
    if(intervals.length < 2) return intervals
  //sort
  intervals.sort((a, b) => a[0] - b[0])
  for(let i = 1; i < intervals.length; i++) {
    let current = intervals[i]
    let previous = intervals[i-1]
    if(current[0] <= previous[1]) {
      intervals[i] =[previous[0], Math.max(previous[1], current[1])]
      intervals.splice(i-1, 1)
      i--
       }
  }
  return intervals
};
````

- The time complexity of the above algorithm is `O(N * logN)`, where `N` is the total number of intervals. We are iterating the intervals only once which will take `O(N)`, in the beginning though, since we need to sort the intervals, our algorithm will take `O(N * logN)`.
- The space complexity of the above algorithm will be `O(N)` as we need to return a list containing all the merged intervals. We will also need `O(N)` space for sorting

>  Given a set of intervals, find out if any two intervals overlap.

 We can follow the same approach as discussed above to find if any two intervals overlap.
 
 ## Insert Interval (medium)
 https://leetcode.com/problems/insert-interval/
 
 > Given a list of non-overlapping intervals sorted by their start time, <b>insert a given interval at the correct position</b> and merge all necessary intervals to produce a list that has only mutually exclusive intervals.

If the given list was not sorted, we could have simply appended the new interval to it and used the `merge()` function from <b>Merge Intervals</b>. But since the given list is sorted, we should try to come up with a solution better than `O(N * logN)`

When inserting a new interval in a sorted list, we need to first find the correct index where the new interval can be placed. In other words, we need to skip all the intervals which end before the start of the new interval. So we can iterate through the given sorted listed of intervals and skip all the intervals with the following condition:
`intervals[i].end < newInterval.start`
Once we have found the correct place, we can follow an approach similar to <b>Merge Intervals</b> to insert and/or merge the new interval. Let’s call the new interval `a` and the first interval with the above condition `b`. There are five possibilities:

The diagram above clearly shows the merging approach. To handle all four merging scenarios, we need to do something like this:
````
    c.start = min(a.start, b.start)
    c.end = max(a.end, b.end)
````
Our overall algorithm will look like this:

1. Skip all intervals which end before the start of the new interval, i.e., skip all `intervals` with the following condition:
````
    intervals[i].end < newInterval.start
````
2. Let’s call the last interval `b` that does not satisfy the above condition. If `b` overlaps with the new interval (a) (i.e. `b.start <= a.end`), we need to merge them into a new interval `c`:
````
    c.start = min(a.start, b.start)
    c.end = max(a.end, b.end)
````
3. We will repeat the above two steps to merge `c` with the next overlapping interval.

````
class Interval {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }

  print_interval() {
    process.stdout.write(`[${this.start}, ${this.end}]`);
  }
}

function insert (intervals, newInterval) {
  let merged = [];
  let i = 0
  
  //skip and add to output all intervals that come before the newInterval
  while(i < intervals.length && intervals[i].end < newInterval.start) {
    merged.push(intervals[i])
    i++
  }
  
  // merge all intervals that overlap with newInterval
  while(i < intervals.length && intervals[i].start <= newInterval.end) {
    newInterval.start = Math.min(intervals[i].start, newInterval.start)
    newInterval.end = Math.max(intervals[i].end, newInterval.end)
    i++
  }
  
  //insert the newInterval
  merged.push(newInterval)
  
  //add all the remaining intervals to the output
  while(i < intervals.length) {
    merged.push(intervals[i])
    i++
  }
  return merged;
};

//Input: Intervals=[[1,3], [5,7], [8,12]], New Interval=[4,6]
// Output: [[1,3], [4,7], [8,12]]
// Explanation: After insertion, since [4,6] overlaps with [5,7], we merged them into one [4,7].
process.stdout.write('Intervals after inserting the new interval: ');
let result = insert([
  new Interval(1, 3),
  new Interval(5, 7),
  new Interval(8, 12),
], new Interval(4, 6));
for (i = 0; i < result.length; i++) {
  result[i].print_interval();
}
console.log();

// Input: Intervals=[[1,3], [5,7], [8,12]], New Interval=[4,10]
// Output: [[1,3], [4,12]]
// Explanation: After insertion, since [4,10] overlaps with [5,7] & [8,12], we merged them into [4,12].
process.stdout.write('Intervals after inserting the new interval: ');
result = insert([
  new Interval(1, 3),
  new Interval(5, 7),
  new Interval(8, 12),
], new Interval(4, 10));
for (i = 0; i < result.length; i++) {
  result[i].print_interval();
}
console.log();

// Input: Intervals=[[2,3],[5,7]], New Interval=[1,4]
// Output: [[1,4], [5,7]]
// Explanation: After insertion, since [1,4] overlaps with [2,3], we merged them into one [1,4].
process.stdout.write('Intervals after inserting the new interval: ');
result = insert([new Interval(2, 3),
  new Interval(5, 7),
], new Interval(1, 4));
for (i = 0; i < result.length; i++) {
  result[i].print_interval();
}
console.log();
````

OR 

````
function insert (intervals, newInterval) {
    let merged = []
    let i = 0
    
    //skip and add to output all intervals that come before the newInterval
    while(i < intervals.length && intervals[i][1] < newInterval[0]) {
        merged.push(intervals[i])
        i++
    }
    
    //merge all intervals that overlap with newInterval
    while(i < intervals.length && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(intervals[i][0], newInterval[0])
        newInterval[1] = Math.max(intervals[i][1], newInterval[1])
        i++
    }
    //insert the newInterval
    merged.push(newInterval)
    
    //add all the remaining intervals to the output
    while(i < intervals.length) {
        merged.push(intervals[i])
        i++
    }
    return merged
};
````

- As we are iterating through all the intervals only once, the time complexity of the above algorithm is `O(N)`, where `N` is the total number of intervals.
- The space complexity of the above algorithm will be `O(N)` as we need to return a list containing all the merged intervals.

## Intervals Intersection (medium)
https://leetcode.com/problems/interval-list-intersections/

## Conflicting Appointments (medium)
https://leetcode.com/problems/meeting-rooms/

## 🌟 Minimum Meeting Rooms (hard) 
https://leetcode.com/problems/meeting-rooms-ii/
## 🌟 Maximum CPU Load (hard)
## 🌟 Employee Free Time (hard)
https://leetcode.com/problems/employee-free-time/
