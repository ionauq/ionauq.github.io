---
title: "爱生气的书店老板"
date: 2021-02-24T18:10:06+08:00
categories: ["leetcode"]
---

今天，书店老板有一家店打算试营业 customers.length 分钟。每分钟都有一些顾客（customers[i]）会进入书店，所有这些顾客都会在那一分钟结束后离开。

在某些时候，书店老板会生气。 如果书店老板在第 i 分钟生气，那么 grumpy[i] = 1，否则 grumpy[i] = 0。 当书店老板生气时，那一分钟的顾客就会不满意，不生气则他们是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 X 分钟不生气，但却只能使用一次。

请你返回这一天营业下来，最多有多少客户能够感到满意的数量。

---

###### 示例

```
输入：customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], X = 3
输出：16
解释：
书店老板在最后 3 分钟保持冷静。
感到满意的最大客户数量 = 1 + 1 + 1 + 1 + 7 + 5 = 16.

```

###### 提示

*    1 <= X <= customers.length == grumpy.length <= 20000
*    0 <= customers[i] <= 1000
*    0 <= grumpy[i] <= 1


## 实现

### Rust

```rust
pub struct Solution {}

impl Solution {

    pub fn max_satisfied(customers: Vec<i32>, grumpy: Vec<i32>, x: i32) -> i32 {
        let x = x as usize;
        let mut base: i32 = 0;

        for (i, b) in grumpy.iter().enumerate() {
            if b.eq(&0) {
                base += customers.get(i).unwrap()
            }
        }

        let mut max_increase = 0;
        let mut increase = 0;
        for (i, v) in customers.iter().enumerate() {
            if i < x {
                increase += v * grumpy.get(i).unwrap();
                if i == x - 1 {
                    max_increase = increase;
                }
            } else {
                increase = increase + v * grumpy.get(i).unwrap() - customers.get(i - x).unwrap() * grumpy.get(i - x).unwrap();
                max_increase = max(max_increase, increase);
            }
        }

        base + max_increase
    }
}


#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test() {
        assert_eq!(
            16,
            Solution::max_satisfied(
                vec![1, 0, 1, 2, 1, 1, 7, 5],
                vec![0, 1, 0, 1, 0, 1, 0, 1],
                3,
            )
        );
    }
}
```

### Java

```java
public class Solution {
    
    public int maxSatisfied(int[] customers, int[] grumpy, int x) {
        int base = 0;
        for (int i = 0; i < customers.length; i++) {
            if (grumpy[i] == 0) {
                base += customers[i];
            }
        }

        int increase = 0;
        for (int i = 0; i < x; i++) {
            increase += customers[i] * grumpy[i];
        }

        int maxIncrease = increase;
        for (int i = x; i < customers.length; i++) {
            increase = increase + customers[i] * grumpy[i] - customers[i - x] * grumpy[i - x];
            maxIncrease = Math.max(maxIncrease, increase);
        }

        return base + maxIncrease;
    }
}
```
