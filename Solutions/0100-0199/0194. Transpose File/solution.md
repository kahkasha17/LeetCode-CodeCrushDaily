# [194. Transpose File](https://leetcode.com/problems/transpose-file)

## Description

<!-- description:start -->

<p>Given a text file <code>file.txt</code>, transpose its content.</p>

<p>You may assume that each row has the same number of columns, and each field is separated by the <code>&#39; &#39;</code> character.</p>

<p><strong class="example">Example:</strong></p>

<p>If <code>file.txt</code> has the following content:</p>

<pre>
name age
alice 21
ryan 30
</pre>

<p>Output the following:</p>

<pre>
name alice ryan
age 21 30
</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: awk

#### Shell

```bash
# Read from the file file.txt and print its transposed content to stdout.
awk '
{
  for (i=1; i<=NF; i++) {
    if(NR == 1) {
      res[i] = re$i
    } else {
      res[i] = res[i]" "$i
    }
  }
}END {
  for (i=1;i<=NF;i++) {
    print res[i]
  }
}
' file.txt
```
<!-- solution:end -->

<!-- problem:end -->