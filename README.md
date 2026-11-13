# MSCS532 Assignment 5: Quicksort Algorithm

In-place deterministic and randomized Quicksort, with an analysis of best, average and worst case time, space use, the effect of randomization, and an empirical comparison on random, sorted and reverse-sorted inputs.
The full report is in [Reports/Assignment_5_Report.docx](Reports/Assignment_5_Report.docx).

## Files

- `quicksort_in_place.py` – `quicksort_deterministic()` (last number as the pivot) and `quicksort_random()` (random pivot swapped to the end), both using the same `partition()`
- `main.py` – runs a sort on a data file and saves the time to `results.csv`
- `recorder.py`, `file_parser.py` – timing and file reading helpers
- `data/` – input files (random, sorted and reverse-sorted lists with 1,000 / 10,000 / 50,000 numbers were used for the tests)
- `results.csv` – benchmark results
- `Reports/` – the written report

`recorder.py`, `file_parser.py` and the `data/` files are adapted from my previous assignments.

## How to run

Requires Python 3.

Sorting (choose `r` for randomized or `d` for deterministic, then enter a file such as `data/sorted_large.txt`):

```bash
python3 main.py
```

To run all 18 tests at once (the deterministic runs on the large sorted and reverse-sorted files take about a minute each):

```bash
for alg in r d; do
  for kind in random sorted reverse; do
    for size in small mid large; do
      printf "$alg\ndata/${kind}_${size}.txt\n" | python3 main.py > /dev/null
    done
  done
done
```

## Summary of findings

- On random input, both versions took about the same time (39 ms deterministic and 51 ms randomized at 50,000 numbers). The deterministic version was slightly faster because it skips generating a random number.
- On sorted and reverse-sorted input, deterministic Quicksort hit its O(n²) worst case: 59 s and 45 s at 50,000 numbers, and about 100 times slower when the input grew from 1,000 to 10,000 numbers.
- Randomized Quicksort stayed between 50 and 56 ms at 50,000 numbers on every input, so randomizing the pivot removes the dependence on input order.
- Memory was not measured, because `tracemalloc` made the worst-case runs too slow to finish.
