# The Merge Sort Algorithm

The next algorithm in the divide-and-conquer design paradigm chapter the [book](http://beust.com/algorithms.pdf) examines is the merge sort algorithm. Sorting problems are suitable for using the divide-and-conquer design paradigm: given an array of numbers ``a[i...n]``, return a sorted version of ``a``. These are the steps taken by the merge sort algorithm to sort an array of numbers:

- Split the array into two halves
- Recursively sort each half
- Merge the two sorted arrays

The correctness of the merge sort algorithm is self-evident as long as a proper merge subroutine is implemented. But how can we merge two subarrays ``x`` and ``y`` into a single sorted array ``z``? The method the author proposes is by looking at the very first index of each subarray, ``x[1]`` and ``y[1]``, and whichever one is the smallest will be the very first element of the sorted array ``z``. The rest of ``z`` can then be built recursively.

To analyze the running time of merge sort algorithm, we can take a look at the relation of ``a/b^d`` presented by the master method (where ``a = # recursive calls``, ``b = branching factor``, ``d = exponent of the combined step running time``), ``a`` would be equal to ``2``, ``b = 2``, and ``d = 1``, which will lead to ``a/b^d = 1``, and that corresponds to a running time of ``O(nlogn)``. In other words, the rate of work shrinkage per subproblem is equal to the rate of subproblem proliferation (same amount of work at each level of the recursion tree).

#### Pseudocode:

```
function mergesort(a[1 . . . n])
  Input: An array of numbers a[1 . . . n]
  Output: A sorted version of this array

  if n > 1:
    return merge(mergesort(a[1 . . . bn/2c]), mergesort(a[bn/2c + 1 . . . n]))
  else:
    return a

function merge(x[1 . . . k], y[1 . . . l])
  if k = 0: return y[1 . . . l]
  if l = 0: return x[1 . . . k]
  if x[1] ≤ y[1]:
    return x[1] ◦ merge(x[2 . . . k], y[1 . . . l])
  else:
    return y[1] ◦ merge(x[1 . . . k], y[2 . . . l])
```

#### Implementation:

``` ruby
class MergeSort
  # Sorts an array using the merge sort algorithm
  def sort(arr)
    length = arr.length
    if length > 1
      mid = (length/2).floor
      return merge(sort(arr[0..(mid - 1)]), sort(arr[mid..length]))
    else
      return arr
    end
  end

  private

  # Merges two array in sorted order
  def merge(left, right)
    if left.empty?
      return right
    elsif right.empty?
      return left
    elsif left.first < right.first
      return [left.first].concat(merge(left.drop(1), right))
    else
      return [right.first].concat(merge(left, right.drop(1)))
    end
  end
end
```

#### Tests:

``` ruby
require 'minitest/autorun'
require './merge_sort'

describe MergeSort do
  before do
    @merge_sort = MergeSort.new
  end

  describe "#sort" do
    it "should sort an array of 1 element" do
      assert_equal(@merge_sort.sort([1]), [1])
    end

    it "should sort an array of multiple elements" do
      assert_equal(@merge_sort.sort([5, 2, 7, 8, 9, 3]),  [2, 3, 5, 7, 8, 9])
    end

    it "should sort an array of multiple elements with equal slots" do
      assert_equal(@merge_sort.sort([5, 5, 1, 3, 3, 7]),  [1, 3, 3, 5, 5, 7])
    end

    it "should sort an array of multiple elements with negative numbers" do
      assert_equal(@merge_sort.sort([-1, -4, 2, 0, -2, 7]),  [-4, -2, -1, 0, 2, 7])
    end

    it "should sort an array of odd length" do
      assert_equal(@merge_sort.sort([8, 3, 6, 2, 6, 8, 0]),  [0, 2, 3, 6, 6, 8, 8])
    end
  end
end
```

### Conclusion

Just as the author explained, sorting problems immediately lend towards a divide-and-conquer approach. Next, I will be revisiting medians, matrix multiplication, and the fast Fourier transform.
