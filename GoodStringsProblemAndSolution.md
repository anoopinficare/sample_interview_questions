# Problem Statement

You are given a positive integer `n`, and an array `arr` of `n` strings of lowercase Latin letters. You are asked to count the number of good strings in `arr`.

A string is said to be **good** if it has a **special substring** in it.

A string `t` is considered to be **special** if there's **no letter** with number of occurrences strictly greater than `length(t) / 2`.

Print the number of good strings in `arr`.

## Input Format

- The first line contains an integer, `n`, denoting the number of elements in `arr`.
- Each of the next `n` lines contains a single string of lowercase Latin letters.

## Output Format

- Print a single integer — the number of good strings in `arr`.

## Example

**Input:**
```
3
abac
aaaa
abc
```

**Output:**
```
2
```

---

# C# Solution

```csharp
using System;

class Program
{
    // Checks if a string is special
    static bool IsSpecial(string s)
    {
        int[] freq = new int[26];
        int n = s.Length;
        for (int i = 0; i < n; i++)
        {
            freq[s[i] - 'a']++;
        }
        int half = n / 2;
        for (int i = 0; i < 26; i++)
        {
            if (freq[i] > half)
                return false;
        }
        return true;
    }

    // Checks if a string has any special substring
    static bool HasSpecialSubstring(string s)
    {
        int n = s.Length;
        for (int i = 0; i < n; i++)
        {
            for (int j = i + 1; j <= n; j++)
            {
                string sub = s.Substring(i, j - i);
                if (IsSpecial(sub))
                    return true;
            }
        }
        return false;
    }

    static int CountGoodStrings(string[] arr)
    {
        int goodCount = 0;
        for (int i = 0; i < arr.Length; i++)
        {
            if (HasSpecialSubstring(arr[i]))
                goodCount++;
        }
        return goodCount;
    }

    static void Main()
    {
        int n = int.Parse(Console.ReadLine());
        string[] arr = new string[n];
        for (int i = 0; i < n; i++)
        {
            arr[i] = Console.ReadLine().Trim();
        }
        int result = CountGoodStrings(arr);
        Console.WriteLine("Number of good strings in arr: " + result);
    }
}
```