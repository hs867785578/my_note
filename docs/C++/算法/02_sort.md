# Sort

## Assumption
To simplify matters, we will assume that the algorithms we describe will all be interchangeable:

1. All array positions contain data to be sorted;
2. The `N` is the number of elements passed to our sorting routines;
3. the `>` and `<` operators exists, which can be used to place a consistant ordering on the input.

Sorting under these conditions is known as comparision-based sorting.

## Sort with O(n^2) Complexity

### Bubble Sort

![bubble](images/sort/bubble_sort.gif)

Bubblesort is the simplest sorting algorithm that works by repeatedly swapping the adjacent elements if they are in wrong order. The algorithm is a comparison sort, is named for the way smaller or larger elements "bubble" to the top of the list. Although the algorithm is simple, it is too slow and impractical for most problems even when compared to Insertionsort. Bubblesort can be practical if the input is in mostly sorted order with some out-of-order elements nearly in position.

```C++
template <typename T>
void bubbleSort(vector<T>& arr) {
  int m = arr.size();
  for (int i = 0; i < m; ++i) {
    for (int j = i + 1; j < m; ++j) {
      if (arr[j] < arr[i]) swap(arr[j], arr[i]);
    }
  } 
}
```

### Selection Sort

![select](images/sort/select_sort.gif)

The selection sort divides the input list into two parts: a sorted sublist of items which is built up from left to right at the front (left) of the list and a sublist of the remaining unsorted items that occupy the rest of the list.

```C++
void SelectionSort(vector<int>& arr) {
  int m = arr.size();
  for (int i = 0; i < m; ++i) {
    int min_idx = i;
    for (int j = i + 1; j < m; ++j) if (arr[j] < arr[min_idx]) min_idx = j;
    swap(arr[i], arr[min_idx]);
  }
}
```

### Insertion Sort

![insert](images/sort/insert_sort.gif)

One of the simplest sorting algorithms is the insertion sort.
Insertion sort consists of N - 1 passes. For pass p = 1 through N - 1, insertion sort ensures that the elements in position 0 through p are in sorted order. Insertion sort makes use of the fact that elements in position 0 through p - 1 are already known to be in sorted order.

```C++
void InsertSort(vector<int>& arr) {
  int m = arr.size();
  for (int i = 1; i < m; ++i) {
    int j = i;
    while (j && arr[j] < arr[j - 1]) swap(arr[j - 1], arr[j--]);
  }
}
```

Becuase of the nested loops, each of which can take N iterations, insertion sort is $O(N^2)$. Furthermore, this bound is tight, because input in reverse order can achieve this bound.

## Sort with O(nlogn) Complexity

### Shell Sort

![shell](images/sort/shell_sort.gif)

Shellsort, named after its inventor, Donald Shell, was one of the first algorithms to break the quadratic time barrier, althoungh it was not until several years after its initial discovery that a subquadratic time bound was proven.
It works by comparing elements that are distant; the distance between comparisons decreases as the algorithm runs until the last phase, in which adjacent elements are compared. For this reason, Shellsort is sometimes referred to as diminishing increment sort.
Shellsort uses a sequence, h1, h2,…, ht, called increment sequence. Any increment sequence will do as long as h1 = 1, but some choices are better than others.

A popular(but poor) choice for increment sequence is to use the sequence suggested by Shell:

$$
h_t = [N / 2]$ and $h_k = [h_{k + 1} / 2]
$$

```C++
void ShellSort(vector<int>& arr) {
  int m = arr.size();
  for (int gap = m / 2; j; gap /= 2) {
    for (int i = gap; i < m; i += gap) {
      int j = i;
      while (j >= gap && nums[j] < nums[j - gap]) {
        swap(nums[j], nums[j - gap]);
        j -= gap;
      }
    }
  }
}
```

The performance of Shellsort is quite acceptable in practice, even for N in the tens of thousands. The simplicity of the code makes it the algorithm of choice for sorting up to moderately large input.

### Heap Sort

Priority queues can be used to sort in $O(NlogN)$ time. The algorithm based on this idea is known as heapsort and gives the best Big-Oh running time we have seen so far. The basic strategy is to build a binary heap of N elements. This stage takes $O(N)$ time. We then preform N deleteMin operations. The elements leave the heap smallest first, in sorted order. By recording these elements in a second array and then copying the array back, we sort N elements. Since each deleteMin takes $O(logN)$ time, the total running time is $O(NlogN)$.

The main problem with this algorithm is that it uses an extra array. Thus, the memory requirement is doubled. This could be a problem in some instances. Notice that the extra time spent copying the second array back to the first is only $O(N)$, so that this is not likely to affect the running time significantly. The problem is space.

A clever way to avoid using a second array makes use of the fact that after each deleteMin, the heap shrinks by 1. Thus the cell that was last in the heap can be used to store the element that was just deleted.

Using this strategy, after the last deleteMin the array will contain the elements in decreasing sorted order. If we want the elements in the more typical increasing sorted order, we can change the ordering property so that the parent has a larger elements than the child. Thus, we have a max-heap.

```C++
void percolateDown(vector<int> &arr, int hole, int heap_size) {
  int m = arr.size();
  auto left_child = [](int x) { return x * 2 + 1; };
  int tmp = arr[hole];
  for (int child = getLeftChild(hole); child < heap_size; child = getLeftChild(hole)) {
    if (child != heap_size - 1 && arr[child + 1] > arr[child]) ++child;
    if (tmp >= arr[child]) break;
    arr[hole] = arr[child];
    hole = child;
  }
  arr[hole] = tmp;
}

void HeapSort(vector<int> &arr) {
  int m = arr.size();
  // build heap
  for (int i = m / 2; i >= 0; --i) { percolateDown(arr, i, cnt); }
  // delete max
  for (int i = m - 1; i >= 0; --i) {
    swap(arr[0], arr[i]);
    percolateDown(arr, 0, i);
  }
}
```

### Merge Sort

![merge](images/sort/merge_sort.gif)

Mergesort runs in $O(NlogN)$ worse-case running time, and the number of comparisons used is nearly optimal. It is a fine example of a recursive algorithm.

The fundamental operation in this algorithm is merging two sorted lists. Because the lists are sorted, this can be done in one pass through the input, if the output is put in a third list. The basic merging algorithm takes two input array A and B, an output array C, and three counters, Actr, Bctr, and Cctr, which are initially set to the beginning of their respective arrays. The smaller of A[Actr] and B[Bctr] is copied to the next entry in C, and the appropriate counters are advanced. When either input list is exhausted, the remainder of the other list is copied to C.

```C++
void MergeSort(vector<int>& arr, vector<int>& tmp, int left, int right) {
  if (left >= right) return;

  int mid = left + (right - left) / 2;
  MergeSort(arr, tmp, left, mid);
  MergeSort(arr, tmp, mid + 1, right);

  int l = left, r = mid + 1, i = left;
  while (l <= mid && r <= right) arr[i++] = arr[l] < arr[r] ? tmp[l++] : tmp[r++];
  while (l <= mid) arr[i++] = tmp[l++];
  while (r <= right) arr[i++] = tmp[r++];
  while (left <= right) arr[left++] = tmp[left];
}
```

Mergesort is a classic example of the techniques used to analyze recursive routines: We have to write a recurrence relation for the running time. We will assume that N is a power of 2 so that we always split into even halves. For N = 1, the time to mergesort is constant, which we will denote by 1. Otherwise, the time to mergesort N numbers is equal to the time to do two recursive mergesort of size N/2, plus the time to merge, which is linear:

$$T(1) = 1$$

$$T(N) = 2T(N/2) + N$$

Although mergesort’s running time is $O(NlogN)$, it has the significant problem that merging two sorted lists uses linear extra memory. The additional work involved in copying to the temporary array and back, throughtout the algorithm, slows the sort considerably. This copying can be avoided by judiciously switching the roles of a and tmpArray at alternate levels of the recursion.
The running time of mergesort, when compared with other $O(NlogN)$ alternatives, depends heavily on the relative costs of comparing elements and moving elements in the array(and the temporary array). These costs are language dependent.

### Quick Sort

![quick](images/sort/quick_sort.gif)

As its name implies for C++, quicksort has historically been the fastest known generic sorting algorithm in practice. Its average running time is $O(NlogN)$. It is very fast, mainly due to a very tight and highly optimized inner loop. It has $O(N^2)$ worst-case performance, but this can be made exponentially unlikely with a little effort. By combining quicksort with heapsort, we can achieve quicksort’s fast running time on almost all inputs, with heapsort’s $O(NlogN)$ worst-case running time.
The quicksort algorithm is simple to understand and prove correct, although for many years it had the reputation of being an algorithm that could in theory be highly optimized but in practice was impossible to code correctly. Like mergesort, quicksort is a divide-and-conquer recursive algorithm.

The classic quicksort algorithm to sort an array S consists of the following four easy steps:

1. Pick any element v in S. This called the `pivot`;
2. Partition S-{v}(the remaining elements in S) into two disjoint groups: $S_1 = {x \in S - {v} | x \le v}$, and $S_x = {x \in S - {v} | x \ge v}$;
3. Return {quicksort($S_1$) and quicksort($S_2$)}.

```C++
void QuickSort(vector<int>& arr, int left, int right) {
  if (left >= right) return;
  int l = left - 1, r = right + 1, p = arr[left + (right - left) / 2];
  while (l < r) {
    while (arr[++l] < p) {};
    while (arr[--r] > p) {};
    if (l < r) swap(arr[l], arr[r]);
  }
  QuickSort(arr, left, r);
  QuickSort(arr, r + 1, right);
}
```

## Sort with O(N) Complexity

$O(N)$ sorting algorithms were exsiting for a long time, but they can only be used in special scenes.

### Counting Sort

`Counting sort` uses an extra array `cnt`, the ith element of `cnt` is the couting number of the ith element in origin array, and at last we set the element of origin array to its proper position according to `cnt`.

The process is:

1. Couting how many times each number in origin array occurs;
2. Calculating the pre-sum of each number;
3. Get the index of each number according to its times from pre-sum list.

![couting](images/sort/counting_sort.svg)

```C++
void CountingSort(vector<int>& a) {
  int low = *min_element(a.begin(), a.end());
  int high = *max_element(a.begin(), a.end());
  
  vector cnt(high - low + 1, 0);
  for (auto n : a) cnt[n - low]++;

  // start to end
  // int sum = 0;
  // for (auto& n : cnt) tie(sum, n) = pair{sum + n, sum};

  // auto tmp = a;
  // for (auto n : tmp) a[cnt[n - low]++] = n;

  // end to start
  for (int i = 1; i < cnt.size(); ++i) cnt[i] += cnt[i - 1];

  auto tmp = a;
  for (int i = cnt.size() - 1; i >= 0; --i) a[--cnt[tmp[i]- low]] = tmp[i];
}
```

### Radix Sort

`Radix sort` divides an element into k digit, and sort elements by its kth digit; after that, it does the same thing with $(k-1), (k-2), /cdots, 0$th digit.

The main process is:

1. find the maximum element of the array;
2. get the radix of each element;
3. loop for `max_digit_length` times, and sort elements by their radix.

![radix](images/sort/radix_sort.png)

```C++
void RadixSort(vector<int>& a) {
  // Get Max digital length
  int mv = *max_element(a.begin(), a.end());
  int ml = 0;
  while (mv) {
    ++ml;
    mv /= 10;
  }

  int dev = 1;
  auto tmp = a;
  vector cnt(19, 0);
  for (int i = 0; i < ml; ++i) {
    // digit count
    for (auto n : a) {
      int radix = n / dev % 10 + 9;
      cnt[radix]++;
    }

    // counting sort
    for (int j = 0; j < 19; ++j) cnt[j] += cnt[j - 1];

    // restore current digit result
    for (int j = a.size() - 1; j >= 0; --j) {
      int radix = a[j] / dev % 10 + 9;
      tmp[--cnt[radix]] = a[j];
    }

    a = tmp;
    cnt = vector(19, 0);
    dev *= 10;
  }
}
```

### Bucket Sort

![bucket](images/sort/bucket_sort.png)

The main process of `bucket sort` is:

1. divide the array into `bucket_size` interval of range `bucket_len`, each interval is call a `bucket`;
2. traversal the array, put all elements to buckets;
3. sort every bucket;
4. merge the numbers in all buckets in order.

```C++
void BucketSort(vector<int>& a, int bucket_len) {
  int low = *min_element(a.begin(), a.end());
  int high = *max_element(a.begin(), a.end());
  int bucket_cnt = (high - low) / bucket_len + 1;

  vector buckets(bucket_cnt, vector<int>());
  for (auto n : a) buckets[(n - a) / bucket_len] = n;
  for (auto& b : buckets) sort(b.begin(), b.end());

  int i = 0;
  for (auto& b : buckets) for (auto n : b) a[i++] = n;
}
```

- [Sort An Array](https://leetcode-cn.com/problems/sort-an-array/)
