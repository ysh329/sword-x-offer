# sword-x-offer

sword-x-offer contains 66 common && classic interview programming questions. Each question has multiple solutions:

| num |              name               |    type   |   difficult    | method(s) |   OJ link   |
|:---:|:-------------------------------:|:---------:|:--------------:|:---------:|:-----------:|
| 01  | find-value-in-matrix            |           |                |           |             |
| 02  | replace-space                   |           |                |           |             |
| 03  | print-linked-list-from-tail     |           |                |           |             |
| 04  | reconstruct-binary-tree         |           |                |           |             |
| 05  | stack-operation-based-two-queues|           |                |           |             |
| 06  | min-num-in-reverse-array        |           |                |           |             |
| 07  | fibonacci-sequence              |           |                |           |             |
| 08  | jump-floor                      |           |                |           |             |
| 09  | jump-floor-II                   |           |                |           |             |
| 10  | rectangle-cover                 |           |                |           |             |
| 11  | number-of-one-in-binary         |           |                |           |             |
| 12  | integer-power-of-number         |           |                |           |             |
| 13  | reorder-array-as-odd-number-is-in-the-front |           |                |           |             |
| 14  | countdown-k-node-in-linked-list |           |                |           |             |
| 15  | reverse-linked-list             |           |                |           |             |
| 16  | meerge-two-sorted-linked-list   |           |                |           |             |
| 17  | judge-the-substructure-of-a-binary-tree |           |                |           |             |
| 18  | mirror-of-binary-tree           |           |                |           |             |
| 19  | print-matrix-clockwise          |           |                |           |             |
| 20  | stack-contain-min-func          |           |                |           |             |
| 21  | push-and-pop-sequence-of-stack  |           |                |           |             |
| 22  | print-binary-tree-from-top-to-bottom |           |                |           |             |
| 23  | postorder-traversal-of-binary-search-tree |           |                |           |             |
| 24  | target-length-path-of-binary-tree |           |                |           |             |
| 25  | replication-of-complex-linked-list|           |                |           |             |
| 26  | convert-binary-search-tree-to-bi-directional-linked-list |           |                |           |             |
| 27  | string-permutation   |           |                |           |             |
| 28  | the-number-occurs-more-than-half |           |                |           |             |
| 29  | k-minimum-number     |           |                |           |             |
| 30  | the-largest-sum-of-consecutive-subarrays |           |                |           |             |
| 31  | number-of-1-between-1-and-n |           |                |           |             |
| 32  | arrange-the-array-into-the-smallest-number |           |                |           |             |
| 33  | ugly-number          |           |                |           |             |
| 34  | first-character-that-appears-only-once |           |                |           |             |
| 35  | inverse-pairs-in-array |           |                |           |             |
| 36  | find-first-common-node-in-two-linked-lists |           |                |           |             |
| 37  | number-of-occurrences-in-the-sorted-array |           |                |           |             |
| 38  | depth-of-binary-tree |           |                |           |             |
| 39  | judge-balanced-binary-tree |           |                |           |             |
| 40  | find-num-appear-once-in-array |           |                |           |             |
| 41  | sum-s-of-continuous-positive-sequence |           |                |           |             |
| 42  | two-sum              |           |                |           |             |
| 43  | left-rotate-string   |           |                |           |             |
| 44  | reverse-words-in-sentence |           |                |           |             |
| 45  | is-poker-continuous  |           |                |           |             |
| 46  | the-last-number-in-the-circle |           |                |           |             |
| 47  | sum-from-1-to-n      |           |                |           |             |
| 48  | sum-of-two-numbers   |           |                |           |             |
| 49  | str-to-int           |           |                |           |             |
| 50  | duplicate-number-in-array |           |                |           |             |
| 51  | construct-multiply-triangle |           |                |           |             |
| 52  | regular-expression-match |           |                |           |             |
| 53  | numeric-string           |           |                |           |             |
| 54  | first-appearing-once-in-str-stream |           |                |           |             |
| 55  | entry-of-loop-linked-list |           |                |           |             |
| 56  | delete-duplication-in-linked-list |           |                |           |             |
| 57  | next-node-of-binary-tree |           |                |           |             |
| 58  | is-symmetrical-binary-tree |           |                |           |             |
| 59  | print-binary-tree-layer-by-layer |           |                |           |             |
| 60  | print-binary-tree-layer-by-layer-II |           |                |           |             |
| 61  | serialize-binary-tree |           |                |           |             |
| 62  | kth-node-of-binary-search-tree |           |                |           |             |
| 63  | get-median-of-num-stream |           |                |           |             |
| 64  | max-in-sliding-window |           |                |           |             |
| 65  | path-in-matrix        |           |                |           |             |
| 66  | range-of-robot        |           |                |           |             |


## Judge Script

```shell
#!bin/bash
while true;
do
./data
./std
./test
if diff std.out test.out;then
echo AC
else
echo WA
exit 0
fi
done
```
