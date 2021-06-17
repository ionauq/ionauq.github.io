---
title: "你必须知道的拍重策略"
date: 2021-06-08T18:59:58+08:00
tags: ["拍重", "Bloom filter"]
categories: ["拍重"]
draft: true
---

## 精准去重


HashMap


BitSet



### BitMap

### Roaring Bitmap

We partition the range of 32-bit indexes ([0, n)) into chunks of $2^{16}$ integers sharing the same 16 most significant digits. We use specialized containers to store their 16 least significant bits


将 32-bit的 Integer的高16位bits用来计算 `Container`, 低16位bits存放在 `Container`内。

那么`Container`个数就为$2^{16}$ = 65536。

计算Container位置

```java
  protected static char highbits(int x) {
    return (char) (x >>> 16);
  }
```

Container

* ArrayContainer

* BitmapContainer

* RunContainer

![](/images/exclude_duplicate/roaring_memory.png)


https://blogs-1252825791.cos.ap-chengdu.myqcloud.com/high-performance-olap-public.pdf?1=1


https://blog.bcmeng.com/post/doris-bitmap.html


https://blog.bcmeng.com/post/kylin-distinct-count-global-dict.html


## 概率性去重

### Bloom Filter


谷歌使用了哈佛这篇论文的结论，利用两个32位的hash函数即可映射到n个bit上。

首先将object通过funnel转换为基本类型，计算出64位hash，并且将高位低位分别作为hash。这样一来只调用了一次hash函数，大大节约了时间开销。


Hint: 联想一下算法导论散列表那一节，对于开放地址法，算法导论提出了线性探查、二次探查、双重散列等几种探查方法，以双重散列为冲突最小，这里的思路其实就是双重散列！联想一下上面提到的因为hash冲突而导致误判，那么减少冲突其实是自然而然的。

[公式]

那么我们发现，本质上，这个布隆过滤器其实就是向散列表中插入了numHashFunctions个相同的对象，而散列表恰好采用了开放地址法+双重散列罢了。

https://zhuanlan.zhihu.com/p/198906949


https://www.eecs.harvard.edu/~michaelm/postscripts/rsa2008.pdf

#### Counting Bloom Filter

#### Cuckoo Filter

### HyperLogLog






## 参考文章


[Better bitmap performance with Roaring bitmaps](https://arxiv.org/pdf/1402.6407v4.pdf)

https://www.jasondavies.com/bloomfilter/

https://llimllib.github.io/bloomfilter-tutorial/


https://www.pdai.tech/md/algorithm/alg-domain-bigdata-bloom-filter.html


https://tianzhipeng-git.github.io/2020/09/07/bitmap-index.html#%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5


https://bbs.huaweicloud.com/blogs/122320


https://cn.kyligence.io/blog/count-distinct-bitmap/


https://www.infoq.cn/article/j3b2eoz_v7wzm27odwxx
