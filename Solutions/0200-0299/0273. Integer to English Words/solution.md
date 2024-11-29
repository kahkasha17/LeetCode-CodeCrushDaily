# [273. Integer to English Words](https://leetcode.com/problems/integer-to-english-words)

## Description

<!-- description:start -->

<p>Convert a non-negative integer <code>num</code> to its English words representation.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> num = 123
<strong>Output:</strong> &quot;One Hundred Twenty Three&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> num = 12345
<strong>Output:</strong> &quot;Twelve Thousand Three Hundred Forty Five&quot;
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> num = 1234567
<strong>Output:</strong> &quot;One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Python3

```python
class Solution:
    def numberToWords(self, num: int) -> str:
        if num == 0:
            return 'Zero'

        lt20 = [
            '',
            'One',
            'Two',
            'Three',
            'Four',
            'Five',
            'Six',
            'Seven',
            'Eight',
            'Nine',
            'Ten',
            'Eleven',
            'Twelve',
            'Thirteen',
            'Fourteen',
            'Fifteen',
            'Sixteen',
            'Seventeen',
            'Eighteen',
            'Nineteen',
        ]
        tens = [
            '',
            'Ten',
            'Twenty',
            'Thirty',
            'Forty',
            'Fifty',
            'Sixty',
            'Seventy',
            'Eighty',
            'Ninety',
        ]
        thousands = ['Billion', 'Million', 'Thousand', '']

        def transfer(num):
            if num == 0:
                return ''
            if num < 20:
                return lt20[num] + ' '
            if num < 100:
                return tens[num // 10] + ' ' + transfer(num % 10)
            return lt20[num // 100] + ' Hundred ' + transfer(num % 100)

        res = []
        i, j = 1000000000, 0
        while i > 0:
            if num // i != 0:
                res.append(transfer(num // i))
                res.append(thousands[j])
                res.append(' ')
                num %= i
            j += 1
            i //= 1000
        return ''.join(res).strip()
```

#### Java

```java
class Solution {
    private static Map<Integer, String> map;

    static {
        map = new HashMap<>();
        map.put(1, "One");
        map.put(2, "Two");
        map.put(3, "Three");
        map.put(4, "Four");
        map.put(5, "Five");
        map.put(6, "Six");
        map.put(7, "Seven");
        map.put(8, "Eight");
        map.put(9, "Nine");
        map.put(10, "Ten");
        map.put(11, "Eleven");
        map.put(12, "Twelve");
        map.put(13, "Thirteen");
        map.put(14, "Fourteen");
        map.put(15, "Fifteen");
        map.put(16, "Sixteen");
        map.put(17, "Seventeen");
        map.put(18, "Eighteen");
        map.put(19, "Nineteen");
        map.put(20, "Twenty");
        map.put(30, "Thirty");
        map.put(40, "Forty");
        map.put(50, "Fifty");
        map.put(60, "Sixty");
        map.put(70, "Seventy");
        map.put(80, "Eighty");
        map.put(90, "Ninety");
        map.put(100, "Hundred");
        map.put(1000, "Thousand");
        map.put(1000000, "Million");
        map.put(1000000000, "Billion");
    }

    public String numberToWords(int num) {
        if (num == 0) {
            return "Zero";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 1000000000; i >= 1000; i /= 1000) {
            if (num >= i) {
                sb.append(get3Digits(num / i)).append(' ').append(map.get(i));
                num %= i;
            }
        }
        if (num > 0) {
            sb.append(get3Digits(num));
        }
        return sb.substring(1);
    }

    private String get3Digits(int num) {
        StringBuilder sb = new StringBuilder();
        if (num >= 100) {
            sb.append(' ').append(map.get(num / 100)).append(' ').append(map.get(100));
            num %= 100;
        }
        if (num > 0) {
            if (num < 20 || num % 10 == 0) {
                sb.append(' ').append(map.get(num));
            } else {
                sb.append(' ').append(map.get(num / 10 * 10)).append(' ').append(map.get(num % 10));
            }
        }
        return sb.toString();
    }
}
```
#### JavaScript

```js
function numberToWords(num) {
    if (num === 0) return 'Zero';

    // prettier-ignore
    const f = (x) => {
    const dict1 = ['','One','Two','Three','Four','Five','Six','Seven','Eight','Nine','Ten','Eleven','Twelve','Thirteen','Fourteen','Fifteen','Sixteen','Seventeen','Eighteen','Nineteen',]
    const dict2 = ['','','Twenty','Thirty','Forty','Fifty','Sixty','Seventy','Eighty','Ninety',]
    let ans = ''

    if (x <= 19) ans = dict1[x] ?? ''
    else if (x < 100) ans = `${dict2[Math.floor(x / 10)]} ${f(x % 10)}`
    else if (x < 10 ** 3) ans = `${dict1[Math.floor(x / 100)]} Hundred ${f(x % 100)}`
    else if (x < 10 ** 6) ans = `${f(Math.floor(x / 10 ** 3))} Thousand ${f(x % 10 ** 3)}`
    else if (x < 10 ** 9) ans = `${f(Math.floor(x / 10 ** 6))} Million ${f(x % 10 ** 6)}`
    else ans = `${f(Math.floor(x / 10 ** 9))} Billion ${f(x % 10 ** 9)}`

    return ans.trim()
  }

    return f(num);
}
```
### Solution 2

#### Java

```java
class Solution {
    private String[] lt20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight",
        "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen",
        "Seventeen", "Eighteen", "Nineteen"};
    private String[] tens
        = {"", "Ten", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};
    private String[] thousands = {"Billion", "Million", "Thousand", ""};

    public String numberToWords(int num) {
        if (num == 0) {
            return "Zero";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 1000000000, j = 0; i > 0; i /= 1000, ++j) {
            if (num / i == 0) {
                continue;
            }
            sb.append(transfer(num / i)).append(thousands[j]).append(' ');
            num %= i;
        }
        return sb.toString().trim();
    }

    private String transfer(int num) {
        if (num == 0) {
            return "";
        }
        if (num < 20) {
            return lt20[num] + " ";
        }
        if (num < 100) {
            return tens[num / 10] + " " + transfer(num % 10);
        }
        return lt20[num / 100] + " Hundred " + transfer(num % 100);
    }
}
```

<!-- solution:end -->

<!-- problem:end -->