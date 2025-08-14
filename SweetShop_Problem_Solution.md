# Sweet Shop Maximum Energy Problem (C# Solution)

## Problem Explanation

You are at a sweet shop to buy sweets to increase your energy.  
- There are **N types of sweets**, each with a maximum of 1 kg remaining.
- Each sweet has a **cost per kg** and an **energy per kg**.
- You have a total of **C money**.
- For each sweet, you may buy any fraction up to 1 kg, as long as you don’t exceed your money.
- If you buy `k` kg of a sweet with energy `e`, you gain `e*k` energy.
- **Goal:** Maximize your total energy by purchasing the sweets optimally, i.e., pick fractions of sweets with the best energy/cost ratio until you run out of money.

**Output:**  
Express the result as a fraction `x/y` in lowest terms, then print `(x * y^-1) % (10^9 + 7)`, where `y^-1` is the modular inverse.

---

## Approach

This is a classic **fractional knapsack** problem:
1. Sort sweets by **energy per cost** in descending order.
2. Buy as much as possible of the best ratio sweet, then the next, until money runs out.
3. Keep track of the total energy as a fraction for accuracy.
4. Output the result modulo `10^9+7` as `(numerator * denominator^-1) % MOD`.

---

## Sample C# Code

```csharp
using System;
using System.Linq;
using System.Numerics;

class SweetShop
{
    const long MOD = 1000000007;

    // Modular inverse using Fermat's little theorem (since MOD is prime)
    static long ModInverse(long a, long mod)
    {
        return BigInteger.ModPow(a, mod - 2, mod);
    }

    // Greatest Common Divisor
    static long GCD(long a, long b)
    {
        while (b != 0)
        {
            long t = b;
            b = a % b;
            a = t;
        }
        return a;
    }

    static void Main()
    {
        // Input
        int n = int.Parse(Console.ReadLine());
        long c = long.Parse(Console.ReadLine());
        var sweets = new (long cost, long energy)[n];

        for (int i = 0; i < n; i++)
        {
            var parts = Console.ReadLine().Split().Select(long.Parse).ToArray();
            sweets[i] = (parts[0], parts[1]);
        }

        // Sort by energy per cost, descending
        var sorted = sweets.OrderByDescending(s => (double)s.energy / s.cost).ToArray();

        long num = 0, den = 1; // keep total energy as a fraction

        foreach (var (cost, energy) in sorted)
        {
            if (c == 0) break;
            long buy = Math.Min(cost, c);
            double k = (double)buy / cost;
            long en_num = energy * buy;
            long en_den = cost;

            // Add en_num/en_den to num/den
            num = num * en_den + en_num * den;
            den = den * en_den;

            // Reduce fraction
            long g = GCD(num, den);
            num /= g;
            den /= g;

            c -= buy;
        }

        // Output (x * y^-1) % MOD
        num %= MOD;
        den %= MOD;
        if (num < 0) num += MOD;
        if (den < 0) den += MOD;
        long inv = ModInverse(den, MOD);
        Console.WriteLine((num * inv) % MOD);
    }
}
```

---

## Sample Input and Output

**Input:**
```
2
10
2 4
3 6
```

**Output:**
```
10
```

---
