```python

# coding: utf-8
"""
给定一个可能具有重复数字的列表，返回其所有可能的子集

注意
子集中的每个元素都是非降序的

两个子集间的顺序是无关紧要的

解集中不能包含重复子集

样例
如果S = [1,2,2]，一个可能的答案为：

[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
"""

def subsetsWithDup(self, S):
    
    # edge cases
    
    if S == None:
        return []
    if S == []:
        return [[]]
    
    S = sorted(S)
    subsets = []
    
    def remove_element(lst):
        # remove smallest element(s) from sorted list
        # e.g. [1,1,2,2] remove 1 => [2,2]
        l = lst[:]
        element_rm = l[0]
        while l[0] == element_rm:
            l.pop(0)
            if l == []:
                break
        return l
    
    def get_subsets(elements_left, current_subset):
        """
        elements_left: elements left to choose from
        current_subset: elements already choosen
        """
        if elements_left == []:
            subsets.append(current_subset)  # reach leaf node
            return
            
        # pick smallest
        current_subset_left = current_subset[:]
        current_subset_left.append(elements_left[0])
        get_subsets(elements_left[1:], current_subset_left)
        
        # don't pick
        get_subsets(remove_element(elements_left), current_subset)
        
    get_subsets(S, [])
    return subsets
    
    
if __name__ == '__main__':
    print(subsetsWithDup(None, [1,2,2]))
    print(subsetsWithDup(None, [1,1,2,3,3,3]))
    
```

这题如果用 set 之类的东西来去重就没意义了, 所以就想别的办法去重  
找子集的方法肯定是递归，关键是如何在递归中不产生重复的子集。
递归模式如下，叶子节点是最重要添加的子集：
![](http://images.cnitblog.com/blog/517264/201311/30224521-1bedbfb595cb438ba6d40c0b972edb71.jpg)
    
为了去重，我维护了一个剩余元素列表，即代码中的`elements_left`（以下简称 el）。
每次分支，左边是选择当前el中最小的元素，右边是不选。什么情况会有重复？比如左边分支选了1，右边不选，
但是右边分支下一次选1，这样就有可能重复。所以关键是让右边分支无法选择左边已经选了的元素。

`remove_elements`函数的作用就在于此。它从el中把最小元素（也就是左边分支选择的元素）直接删掉（可能有多个），剩下的el作为
右边分支的el传下去。而左边分支的新el就只删掉选择的那个元素就好了。

递归结束条件是el为空，即成为叶子节点，然后把当前分支的选择的元素`current_subset`加入最终结果即可。
    
    
    
    
    
    
    
    
    
    
    
    
    