## Project 0 C++ Primer

##### Testing

```shell
cmake -DCMAKE_BUILD_TYPE=Debug ..
make -j$(nproc)

make starter_trie_test
./test/starter_trie_test
```

##### Formatting

```shell
make format
make check-lint
make check-clang-tidy-p0

```

##### Memory Leaks

Configure CMake in debug mode and run tests as you normally would.

Or you can run [[C++ Debug#Valgrind|Valgrind]]

```shell
valgrind \
    --error-exitcode=1 \
    --leak-check=full \
    ./test/starter_trie_test
```

##### Logging

Instead of using `printf`, use `LOG_*` macros for logging and debugging.

```c++
LOG_INFO("# Pages: %d", num_pages);
LOG_DEBUG("Fetching page %d", page_id);
```

You will need to add `#include "common/logger.h"` to any file in which you want to make use of the logging infrastructure.



## Project 1 Buffer Pool

Need to be thread-safe, assume that multiple threads are accessing the internal data, so you need to protect critical sections with latches.

##### RAII lock

```c++
std::scoped_lock<std::mutex> lock(latch_);
```

### Extendible Hash Table

Need to incrementally grow in size as needed but no need for shrink.

##### Files

1. `src/include/container/hash/extendible_hash_table.h`
2. `src/container/hash/extendible_hash_table.cpp`
3. `test/container/hash/extendible_hash_table_test.cpp`

##### API

**Insert(K, V)** 

Insert/overwrite. If full, do following before retrying:

1. If local depth == global depth, double the size of the directory.
2. Increment the local depth of the bucket.
3. Split the bucket and redistribute.

* **Find(K, V)** find V given V
* **Remove(K)**
* **GetGlobalDepth()**
* **GetLocalDepth(dir_index)**
* **GetNumBuckets()**

### LRU-K Replacement Policy

##### Files

1. `src/include/buffer/lru_k_replacer.h`
2. `src/buffer/lru_k_replacer.cpp`
3. `test/buffer/lru_k_replacer_test.cpp`

Evicts a frame whose backward k-distance is maximum of all frames in the replacer. k-distance is difference in time between current timestamp and timestamp of kth previous access.

When multiple frames have +inf backward k-distance, the replacer evicts the frame with the earliest timestamp.

##### API

* **Evict(frame_id_t*)**: evict the frame with largest backward k-distance
* **RecordAccess(frame_id_t)** add access history
* **Remove(frame_id_t)** clear all access history
* **SetEvictable(frame_id_t, bool set_evictable)** evictable when pin count is 0
* **Size()** number of evictable frames

### Buffer Pool Manager

`BufferPoolManagerInstance` fetches pages from `DiskManager`.

##### Files

1. `src/include/buffer/buffer_pool_manager_instance.h`
2. `src/buffer/buffer_pool_manager_instance.cpp`
3. `test/buffer/buffer_pool_manager_instance_test.cpp`

##### API

1. **FetchPgImp(page_id)**
2. **UnpingPgImp(page_id, is_dirty)
3. **FlushPgImp(page_id)**
4. **NewPgImp(page_id)**
5. **DeletePgImp(page_id)**
6. **FlushAllPagesImpl()**




