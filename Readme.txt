Every software program has its own behavior, this behavior can change over the course of the program execution. The fixed associative cache design could result in higher miss rate
due to increased conflict misses(and hence bad IPC). A proposed solution to overcome this issue, is “Variable-way cache implementation”(we shall call it V-way). The Vway cache implementation effectively increases the associativity of specific cache sets, at the
cost of other cache sets that are less likely to be used. The tag store locations are increased to accommodate for increased associativity. However, the number of data store locations are still the same(number of sets*associativity). The concept of data blocks being associated
with fixed cache sets does not exist here. The entire data store array appears as one big
chunk and any cache set can use any data store location in the array. The replacement policy used in this methodology, has a global information of the data usage frequency. The
replacement policy uses counters(one counter per data block in the data store array) to keep track of the reuse frequency. A 3-bit saturating counter is used for this purpose. The
counter is used to find a data victim for eviction whenever a replacement is being done.

V-WAY Cache Organization:
Tag Store Array
The number of ways in tag store array is increased(doubled in our case), in each set. Each tag store entry has three parameters associated with it.
Tag - the tag bits from the physical address of the block
FPTR - a forward pointer to the corresponding data block in the data store array Valid bit - indicates validity of tag
Data Store Array
The number of data blocks in the data store array is given by the “number of sets * associativity”. Each data block has three parameters associated with it.
Address - Physical address in the main memory
RPTR - pointer to the corresponding tag in the tag store array
Reuse Counter - a 2 bit saturating counter to hold reuse frequency information
The reuse counters associated with the data store array is present as a reuse table, a pointer used to index the reuse counter.
ptr (reuse counter pointer) - to store the reuse value
Our implementation in gem5 Ruby memory model
Following are the files modified to implement the V-way cache:
src/mem/ruby/system/CacheMemory.cc 
src/mem/ruby/system/CacheMemory.hh 
src/mem/ruby/slicc_interface/AbstractCacheEntry.hh 
src/mem/ruby/slicc_interface/AbstractCacheEntry.cc 
src/mem/protocol/MESI_CMP_directory-L2cache.sm Files added
src/mem/ruby/slicc_interface/reusereplacement.hh 
src/mem/ruby/slicc_interface/TagStruct.hh
