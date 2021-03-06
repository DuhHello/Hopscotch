## Hopscotch Hashing

This is an implementation of Hopscotch Hashing described in Herlihy et al. (2008).


### Description
Hopscotch hashing combines the advantages of Cuckoo Hashing, Chained Hashing, and Linear probing. The main idea is as follows: All hashed key's has an original bucket index. When key's collide, they are put as close to this bucket as possible (within a user-specified range), and thus the algorithm utilizes spacial locality. All buckets contain a bitmap, which indicates which keys belongs to an index, which allows for faster retrieval and removal of items.  

Here's how to add an item (as described in the original paper):

To add an item x where h(x) = i  
- Start at i, use linear probing to find an empty entry at index j.
- If the empty entry's index j is within H - 1 of i, place x there and return.
- Otherwise, j is too far from i and j, but within H - 1 of j, and whose entry lies below j. Displacing y to j creates a new empty sot closer to i. Repeat. If no such item exists, or if bucket i already contains H items, resize and rehash the table.


### Implementation:  
This particular implementation is based on a bitmap (as opposed to linked list).  
The hash map is split into segments, segments are wrapped.  
For update functions (i.e. add/remove) locking happens on segment granularity. Thus the number of segments determines the concurrency level.  
Get operations uses timestamps (which are updated when displacement happens) and through this, the system is linearizable.


### Assumptions:  
- Little endian architecture
- Power of two nbuckets and nsegments


### Todo:  
- Benchmark
- Implement resize()


### Status:
- When going outside segment boundaries, the segments are not locked, and thus the behavior is not correct at the moment.


### Contact:
mail: cs@tooein.net