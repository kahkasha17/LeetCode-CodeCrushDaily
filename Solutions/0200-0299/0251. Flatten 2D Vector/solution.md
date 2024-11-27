# [251. Flatten 2D Vector 🔒](https://leetcode.com/problems/flatten-2d-vector)

## Description

<!-- description:start -->

<p>Design an iterator to flatten a 2D vector. It should support the <code>next</code> and <code>hasNext</code> operations.</p>

<p>Implement the <code>Vector2D</code> class:</p>

<ul>
	<li><code>Vector2D(int[][] vec)</code> initializes the object with the 2D vector <code>vec</code>.</li>
	<li><code>next()</code> returns the next element from the 2D vector and moves the pointer one step forward. You may assume that all the calls to <code>next</code> are valid.</li>
	<li><code>hasNext()</code> returns <code>true</code> if there are still some elements in the vector, and <code>false</code> otherwise.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input</strong>
[&quot;Vector2D&quot;, &quot;next&quot;, &quot;next&quot;, &quot;next&quot;, &quot;hasNext&quot;, &quot;hasNext&quot;, &quot;next&quot;, &quot;hasNext&quot;]
[[[[1, 2], [3], [4]]], [], [], [], [], [], [], []]
<strong>Output</strong>
[null, 1, 2, 3, true, true, 4, false]

<strong>Explanation</strong>
Vector2D vector2D = new Vector2D([[1, 2], [3], [4]]);
vector2D.next();    // return 1
vector2D.next();    // return 2
vector2D.next();    // return 3
vector2D.hasNext(); // return True
vector2D.hasNext(); // return True
vector2D.next();    // return 4
vector2D.hasNext(); // return False
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= vec.length &lt;= 200</code></li>
	<li><code>0 &lt;= vec[i].length &lt;= 500</code></li>
	<li><code>-500 &lt;= vec[i][j] &lt;= 500</code></li>
	<li>At most <code>10<sup>5</sup></code> calls will be made to <code>next</code> and <code>hasNext</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> As an added challenge, try to code it using only <a href="http://www.cplusplus.com/reference/iterator/iterator/" target="_blank">iterators in C++</a> or <a href="http://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html" target="_blank">iterators in Java</a>.</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Python3

```python
class Vector2D:
    def __init__(self, vec: List[List[int]]):
        self.i = 0
        self.j = 0
        self.vec = vec

    def next(self) -> int:
        self.forward()
        ans = self.vec[self.i][self.j]
        self.j += 1
        return ans

    def hasNext(self) -> bool:
        self.forward()
        return self.i < len(self.vec)

    def forward(self):
        while self.i < len(self.vec) and self.j >= len(self.vec[self.i]):
            self.i += 1
            self.j = 0


# Your Vector2D object will be instantiated and called as such:
# obj = Vector2D(vec)
# param_1 = obj.next()
# param_2 = obj.hasNext()
```

#### Java

```java
class Vector2D {
    private int i;
    private int j;
    private int[][] vec;

    public Vector2D(int[][] vec) {
        this.vec = vec;
    }

    public int next() {
        forward();
        return vec[i][j++];
    }

    public boolean hasNext() {
        forward();
        return i < vec.length;
    }

    private void forward() {
        while (i < vec.length && j >= vec[i].length) {
            ++i;
            j = 0;
        }
    }
}

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D obj = new Vector2D(vec);
 * int param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```

#### C++

```cpp
class Vector2D {
public:
    Vector2D(vector<vector<int>>& vec) {
        this->vec = move(vec);
    }

    int next() {
        forward();
        return vec[i][j++];
    }

    bool hasNext() {
        forward();
        return i < vec.size();
    }

private:
    int i = 0;
    int j = 0;
    vector<vector<int>> vec;

    void forward() {
        while (i < vec.size() && j >= vec[i].size()) {
            ++i;
            j = 0;
        }
    }
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D* obj = new Vector2D(vec);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```
<!-- solution:end -->

<!-- problem:end -->