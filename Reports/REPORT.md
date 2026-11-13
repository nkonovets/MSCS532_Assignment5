# Assignment 5: Quicksort Algorithm: Implementation, Analysis, and Randomization

## Implementation

For this assignment, I wrote a new in-place version of quicksort instead of reusing my list-based version from Assignment 3. Both versions are in quicksort_in_place.py. The partition() function takes the last number as the pivot, loops through the rest, and swaps every number smaller than or equal to the pivot to the left side. At the end, the pivot is swapped into its final place, and quicksort_deterministic() calls itself on the left and right parts. The randomized version uses the same partition(), but partition_random() first swaps a random number to the end, so only the pivot choice changes.

## Performance Analysis

In the best-case scenario for quicksort, the pivot is always taken in the middle, so the split of the data makes both of its parts even in number of elements on each side. Therefore, every level of recursion goes through n numbers, and there are about log n levels, which gives O(n log n).

In the average case, the splits are not perfect, and the number of elements in each partition varies almost every time. Though even the 90/10 split gives a depth of about log n.

The worst-case scenario for quicksort occurs when the pivot is either the first or the last element in the list of sorted or reverse-sorted numbers. When this happens, one part of the data contains most of the numbers, and the other is empty; therefore, recursion goes n levels deep. The work adds up as n + (n − 1) + … + 1, which gives O(n²). The deterministic version of the algorithm I have implemented hits its worst case on every sorted or reverse-sorted list of numbers because it always takes the last number in the array as the pivot, and therefore this number is either the largest or the smallest.

Since sorting happens in-place, the extra memory is O(1). When I first measured memory with tracemalloc for this assignment, for reasons unknown to me, it made it impossible for my computer to finish sorting even medium-sized reverse-sorted or sorted lists. Therefore, I only measured time for this assignment.

## Randomization

In the quicksort version with a random pivot that I have implemented, the likelihood of reaching the worst-case scenario is much lower than in the deterministic version, though it is not impossible.  Since the pivot is random each time, it is much more likely that quicksort will have a pretty even distribution of numbers in each partition. However, because there is still a chance that a pivot is taken at the beginning or end of the list, the first partition run might still be slow. However, even though it is still possible for the pivot to be taken as the first or last number in the list at every recursive call, which would make it almost as small as the deterministic version, the chances of this happening are incredibly low.

## Empirical Analysis

I tested both random-pivot and deterministic quicksort on 3 sets of data: random, sorted, and reverse-sorted lists with 1,000, 10,000, and 50,000 numbers, one run per case. See Table 1 below for full results.

**Table 1**

| Input | n | Deterministic Time (ms) | Randomized Time (ms) |
|---|---:|---:|---:|
| Random | 1,000 | 0.64 | 0.64 |
|  | 10,000 | 7.11 | 8.04 |
|  | 50,000 | 39.11 | 50.73 |
| Sorted | 1,000 | 19.78 | 0.59 |
|  | 10,000 | 2,019.37 | 7.99 |
|  | 50,000 | 59,039.15 | 52.83 |
| Reverse-sorted | 1,000 | 17.32 | 0.62 |
|  | 10,000 | 1,568.13 | 7.90 |
|  | 50,000 | 45,338.28 | 56.07 |

On a random data set, both algorithms performed well, and the deterministic version was slightly faster than the random version when sorting 50,000 numbers, though I believe that might be because it skips generating random numbers and avoids extra swaps.

The big difference is easy to see in sorted and reverse-sorted lists: the deterministic version took almost a minute to sort 50,000 numbers, while the random-pivot version took only 52.83 milliseconds. When comparing sorting times across sets of different sizes, as the input went from 1000 to 10,000, the deterministic quicksort algorithm took 100 times longer to sort, which is exactly what O(n²) predicts. On the other hand, the randomized version remained between 50 and 56 ms for 50,000 numbers on every input, maintaining its O(n log n) behavior.
