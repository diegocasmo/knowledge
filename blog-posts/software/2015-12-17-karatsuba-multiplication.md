# The Karatsuba Multiplication Algorithm

### Back to basics
Over the past few months I have developed a special interest for Artificial Intelligence. Because of this, I have decided I will start off by reviewing (while learning some new things as well) the fundamentals of computer science before diving into Artificial Intelligence itself. To do this, I will be reading the book [Algorithms by S. Dasgupta, C. H. Papadimitriou, and U. V. Vazirani](http://beust.com/algorithms.pdf). If you would like to read it, keep in mind it does have a [few mistakes](http://cseweb.ucsd.edu/~dasgupta/book/errata.pdf), thus I recommend correcting them before you start it.

### Divide-and-conquer
The second chapter starts off with the divide-and-conquer design paradigm. The main idea of the divide-and-conquer design paradigm is:

- Break a problem into subproblems
- Recursively solve these subproblems
- Combine their answers

To better illustrate this, the author explains the Karatsuba multiplication algorithm. The whole idea behind this algorithm, is that the product of two complex numbers can be done with just three real-number multiplications (instead of four). In the divide-and-conquer design paradigm, reducing the number of subproblems (that is, reducing the branching factor of the recursion tree) produces a big impact in the overall running time of the algorithm. This happens because the reduction of subproblems occurs at every level of the recursion tree, producing a compounding effect which leads to a better performance.

### The Karatsuba Multiplication Algorithm

#### [Pseudocode:](https://en.wikipedia.org/wiki/Karatsuba_algorithm)

```
procedure karatsuba(num1, num2)
  if (num1 < 10) or (num2 < 10)
    return num1*num2
  /* calculates the size of the numbers */
  m = max(size_base10(num1), size_base10(num2))
  m2 = m/2
  /* split the digit sequences about the middle */
  high1, low1 = split_at(num1, m2)
  high2, low2 = split_at(num2, m2)
  /* 3 calls made to numbers approximately half the size */
  z0 = karatsuba(low1,low2)
  z1 = karatsuba((low1+high1),(low2+high2))
  z2 = karatsuba(high1,high2)
  return (z2*10^(2*m2))+((z1-z2-z0)*10^(m2))+(z0)
```

#### Implementation:

``` ruby
class Karatsuba
  # Multiply two numbers using the Karatsuba
  # multiplication algorithm
  def multiply(num_1, num_2)
    if num_1 < 10 || num_2 < 10
      return num_1 * num_2
    end
    m_2 = [num_1.to_s.length, num_2.to_s.length].max/2
    # Split the digit sequences about the middle
    high_1, low_1 = num_1.divmod(10**m_2)
    high_2, low_2 = num_2.divmod(10**m_2)
    # Recursively calculate the product using three
    # multiplications of smaller numbers
    z_0 = multiply(low_1, low_2)
    z_1 = multiply((low_1 + high_1), (low_2 + high_2))
    z_2 = multiply(high_1, high_2)
    return (z_2 * 10**(2 * m_2)) + ((z_1 - z_2 - z_0) * 10**(m_2)) + (z_0)
  end
end
```

#### Tests:

``` ruby
require 'minitest/autorun'
require './karatsuba'

describe Karatsuba do
  before do
    @karatsuba = Karatsuba.new
  end

  describe "#multiply" do
    it "should multiply small numbers of equal size" do
      assert_equal(@karatsuba.multiply(5, 5), 25)
    end

    it "should multiply small numbers of different size" do
      assert_equal(@karatsuba.multiply(3, 12), 36)
    end

    it "should multiply small numbers with zeros inbetween" do
      assert_equal(@karatsuba.multiply(10, 100), 1000)
    end

    it "should multiply large numbers with zeros inbetween" do
      assert_equal(@karatsuba.multiply(100020001, 10030501), 1003250720050501)
    end

    it "should multiply large numbers of equal size" do
      assert_equal(@karatsuba.multiply(345345345, 146348395), 50540736961471275)
    end

    it "should multiply large numbers of different size" do
      assert_equal(@karatsuba.multiply(7655432, 7853), 60118107496)
    end
  end
end
```

### Conclusion

The Karatsuba multiplication algorithm was relatively straightforward and fun to implement. I look forward to keep reviewing/learning more while I enjoy reading this book.
