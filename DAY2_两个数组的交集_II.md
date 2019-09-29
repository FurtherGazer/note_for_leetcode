# 两个数组的交集

给定两个数组，编写一个函数来计算它们的交集。

例如：

```
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]
```

```
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]
```

要求：
输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
我们可以不考虑输出结果的顺序。


## 方案

这题的关键在于，找的是交集元素 & 交集元素的出现次数（示例容易产生误导，实际不是求最大子字符串）。这题用字典的方式来处理就好

比如示例1，为：

nums1_dict = { 1:2, 2:2}
nums2_dict = { 2:2}

交集，就是 相同元素的 values 取小，就是 {2:2}，还原一下 [2,2]


```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        if nums1 == 0 or nums2 == 0:
            return []
        
        nums1_dict = {i:nums1.count(i) for i in nums1}
        nums2_dict = {i:nums2.count(i) for i in nums2}

        if len(nums1_dict) < len(nums2_dict):
            intersection = [i for i in nums1_dict if i in nums2_dict] # 求出交集
            intersection_dict = { i: min(nums1_dict.get(i,0),nums2_dict.get(i,0)) for i in nums1_dict }
        else:
            intersection = [i for i in nums2_dict if i in nums1_dict] # 求出交集
            intersection_dict = { i: min(nums1_dict.get(i,0),nums2_dict.get(i,0)) for i in nums2_dict }
        
        # 展开
        _r = []
        for i in intersection_dict:
            _r = _r + [i for j in range(intersection_dict[i])]

        return _r

```

上述方法可以通过提交，但是效率比较差，而且占用的空间也多，对上述方案进行优化。

可以优化的点在于，nums1_dict = {i:nums1.count(i) for i in nums1} 这种方式，进行了大量无效的 count 运算。
而且只对其中一个做 map 就可以了，下一个只需和上一个做笔记即可。




```python
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        if nums1 == [] or nums2 == []:
            return []
        
        _r_dict = {} # 构建相同元素字典
        _r = []
        
        for i in nums1:
            _r_dict[i] = _r_dict.get(i,0) + 1 # 
        for j in nums2:
            if _r_dict.get(j,0) != 0: # 如果 nums2 中的数，在 _r_dict 中已经有了。
                _r.append(j)
                _r_dict[j] = _r_dict[j] - 1
        
        return _r
```

上述方式可以击败 90.59% 的用户。