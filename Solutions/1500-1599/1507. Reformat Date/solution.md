# [1507. Reformat Date](https://leetcode.com/problems/reformat-date)

## Description

<!-- description:start -->

<p>Given a <code>date</code> string in the form&nbsp;<code>Day Month Year</code>, where:</p>

<ul>
	<li><code>Day</code>&nbsp;is in the set <code>{&quot;1st&quot;, &quot;2nd&quot;, &quot;3rd&quot;, &quot;4th&quot;, ..., &quot;30th&quot;, &quot;31st&quot;}</code>.</li>
	<li><code>Month</code>&nbsp;is in the set <code>{&quot;Jan&quot;, &quot;Feb&quot;, &quot;Mar&quot;, &quot;Apr&quot;, &quot;May&quot;, &quot;Jun&quot;, &quot;Jul&quot;, &quot;Aug&quot;, &quot;Sep&quot;, &quot;Oct&quot;, &quot;Nov&quot;, &quot;Dec&quot;}</code>.</li>
	<li><code>Year</code>&nbsp;is in the range <code>[1900, 2100]</code>.</li>
</ul>

<p>Convert the date string to the format <code>YYYY-MM-DD</code>, where:</p>

<ul>
	<li><code>YYYY</code> denotes the 4 digit year.</li>
	<li><code>MM</code> denotes the 2 digit month.</li>
	<li><code>DD</code> denotes the 2 digit day.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> date = &quot;20th Oct 2052&quot;
<strong>Output:</strong> &quot;2052-10-20&quot;
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> date = &quot;6th Jun 1933&quot;
<strong>Output:</strong> &quot;1933-06-06&quot;
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> date = &quot;26th May 1960&quot;
<strong>Output:</strong> &quot;1960-05-26&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The given dates are guaranteed to be valid, so no error handling is necessary.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Java

```java
class Solution {
    public String reformatDate(String date) {
        var s = date.split(" ");
        String months = " JanFebMarAprMayJunJulAugSepOctNovDec";
        int day = Integer.parseInt(s[0].substring(0, s[0].length() - 2));
        int month = months.indexOf(s[1]) / 3 + 1;
        return String.format("%s-%02d-%02d", s[2], month, day);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reformatDate(string date) {
        string months = " JanFebMarAprMayJunJulAugSepOctNovDec";
        stringstream ss(date);
        string year, month, t;
        int day;
        ss >> day >> t >> month >> year;
        month = to_string(months.find(month) / 3 + 1);
        return year + "-" + (month.size() == 1 ? "0" + month : month) + "-" + (day > 9 ? "" : "0") + to_string(day);
    }
};
```

<!-- solution:end -->

<!-- problem:end -->