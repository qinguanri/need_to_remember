## 排序算法

 1. 快速排序
 2. 归并排序
 3. 堆排序
 4. 基数排序
 5. 几个排序算法的比较
 
## 查找算法
 5. 二分查找

二分查找算法要注意两点，1防止死循环，2 防止溢出。标准代码如下，注意1、4、5处:
```c++
public int bsearch(int[] data, int x, int y, int v) {
    int m;
    while(x<=y){ //1
        m = x + (y-x)/2; //2
        if(data[m] == v) return m; //3
        else if(data[m] > v) y = m - 1; //4
        else x = 1 + m; //5
    }
    return -1; //6
}
```
 
## 缓存算法
 6. LRU

## 数据结构
####单链表
####二叉树
####二叉搜索树
####栈
#### 哈希表
####红黑树
红黑树是由以下5条规则定义的二叉查找树：
 1. 节点非红即黑。
 2. 根节点是黑色。
 3. 所有NULL结点称为叶子节点，且认为颜色为黑。
 4. 所有红节点的子节点都为黑色。
 5. 从任一节点到其叶子节点的所有路径上都包含相同数目的黑节点。

## 动态规划
## 贪心算法
## 回溯
## NP
## 最短路算法
 
