---
title: Leetcode 638.大礼包
comments: true
toc: true
date: 2019-08-13 21:50:25
categories: Algorithm
tags: Other Algorithm
---

# 题目

在LeetCode商店中， 有许多在售的物品。

然而，也有一些大礼包，每个大礼包以优惠的价格捆绑销售一组物品。

现给定每个物品的价格，每个大礼包包含物品的清单，以及待购物品清单。请输出确切完成待购清单的最低花费。

每个大礼包的由一个数组中的一组数据描述，最后一个数字代表大礼包的价格，其他数字分别表示内含的其他种类物品的数量。

任意大礼包可无限次购买。

**示例1:**
```java
输入: [2,5], [[3,0,5],[1,2,10]], [3,2]
输出: 14
解释: 
有A和B两种物品，价格分别为¥2和¥5。
大礼包1，你可以以¥5的价格购买3A和0B。
大礼包2， 你可以以¥10的价格购买1A和2B。
你需要购买3个A和2个B， 所以你付了¥10购买了1A和2B（大礼包2），以及¥4购买2A。
```

**示例2:**
```java
输入: [2,3,4], [[1,1,0,4],[2,2,1,9]], [1,2,1]
输出: 11
解释: 
A，B，C的价格分别为¥2，¥3，¥4.
你可以用¥4购买1A和1B，也可以用¥9购买2A，2B和1C。
你需要买1A，2B和1C，所以你付了¥4买了1A和1B（大礼包1），以及¥3购买1B， ¥4购买1C。
你不可以购买超出待购清单的物品，尽管购买大礼包2更加便宜。
```

# 思路 + 代码

DFS + 剪枝

即使暴力法，然后提出一些不满足条件的情况，即购买数量超出 (大礼包数量不对)

```java
class Solution {
    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        return subShoppingOffers(price, special, needs, 0);
    }
    private int subShoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs, int index){
        // index标记，顺序开始计算，防止(1,2) 与 (2,1) 的重复计算
        int minCost = originalCost(price, needs);
        for(int i=index; i<special.size(); i++){
            // 统计当前还需要购买多少物品
            List<Integer> currentNeeds = new ArrayList<>();
            // 当前礼包信息
            List<Integer> offer = special.get(i);
            for(int j=0; j<needs.size(); j++){
                // 剪枝
                if(offer.get(j) > needs.get(j)){
                    currentNeeds = null;
                    break;
                }
                currentNeeds.add(needs.get(j)-offer.get(j));
            }
            // 如果礼包中物品的数量没有超,则加上该礼包金额，并且继续深度优先(DFS)遍历
            if(currentNeeds!=null){
                minCost = Math.min(minCost, offer.get(offer.size()-1)+subShoppingOffers(price, special, currentNeeds, i));
            }
        }
        return minCost;
    }
    private int originalCost(List<Integer> price, List<Integer> needs){
        int sum = 0;
        for(int i=0; i<needs.size(); i++){
            sum += price.get(i) * needs.get(i);
        }
        return sum;
    }
}
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shopping-offers
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
