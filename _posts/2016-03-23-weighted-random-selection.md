---
layout: post
title: 权重随机选择算法
tags: 算法 Java 开发
---

权重随机算法在抽奖，资源调度等系统中应用还是比较广泛的，一个简单的按照权重来随机的实现，权重为几个随机对象(分类)的命中的比例，权重设置越高命中越容易。

设A/B/C权重分别为10/20/30（此处为方便描述，实际顺序对计算无影响）

```shell
1. 计算权重总和S=60
2. 生成随机数R=RANDOM(S) 0-59
3. 遍历所有元素累加权重值得到C，当C >= R时的元素即为选中元素
```

根据三个元素的权重累加可得到三个区间

```shell
0-10        A
11-30       A + B
31-59       A + B + C
```

随机数平均分布在0-59之间，区间（即权重值）越大，则随机数落在该区间的概率越高

```java
interface Weighter {
    public double getWeight();
}
```

```java
<E extends Weighter> E random(Iterable<? extends E> weighters) {
    double weightSum = 0;
    for (E weighter : weighters) {
        weightSum += weighter.getWeight();
    }
    double r = Math.random() * weightSum;
    double c = 0;
    for (E weighter : weighters) {
        c += weighter.getWeight();
        if (c >= r) {
            return weighter;
        }
    }
    throw new RuntimeException();
}

```
