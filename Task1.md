# Task1

使用语言：Java

## 217-存在重复元素

### 第一次

直接开始尝试，想读取数组长度，通过循环对比数组内的各项；通过改变flag值并对其实时检测，只要flag值改变就返回true，否则返回flase

语言虽然是Java但是思路更接近C的顺序类而不是传统java的new对象

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        int L=nums.length;
        int flag=0;
        for(int i=0;i<=L-2;i++){
            int j=i+1;
            do{
                if(nums[i]==nums[j]){
                    flag=1;
                }
                j++;
            }while(flag==0&&j<L);
        }
       if(flag==1){
           return true;
       }else{
           return false;
       }
    }
}
```

```javascript
执行用时：
1039 ms
内存消耗：
54.9 MB
```

没用完全循环嵌套，做了一个类似双标记点的东西，节省了一些运行时间；但是没有用到哈希表或者哈希map

### 第二次

引入HashSet；因为其元素不可重复，在将元素添加时，判断是否成功添加即可得知是否有重复元素

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> judge=new HashSet<Integer>(); 
        for(int i:nums){
            if(!judge.add(i)){
                return true;
            }
        }
        return false;
    }
}
```

```html
执行用时：
5 ms
内存消耗：
49.6 MB
```

耗时明显提升



### 疑问

自己觉得逻辑没有什么问题，但是输出不符合预期，需要加flag之类的判断

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet judge=new HashSet();
        for(int i=0;i<nums.length-1;i++){
            if(!judge.add(nums[i])){
                return true;
            }
        }
        return false;
    }
}
```

#### 猜测一：

没有对哈希表内的值进行格式定义

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> judge=new HashSet<Integer>(); 
        for(int i=0;i<nums.length-1;i++){
            if(!judge.add(nums[i])){
                return true;
            }
        }
        return false;
    }
}
```

编译结果还是不对

#### 猜测二

在比照官方题解后发现在for循环的使用上有区别，但是不知道为什么

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        HashSet<Integer> judge=new HashSet<Integer>(); 
        for(int i:nums){
            if(!judge.add(i)){
                return true;
            }
        }
        return false;
    }
}
```

## 349-两个数组的交集

### 尝试一

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> result=new HashSet<Integer>();
        HashSet<Integer> test=new HashSet<Integer>();

        for(int i:nums1){
            test.add(nums1[i]);
        }
        for(int i:nums2){
            if(!test.add(nums2[i])){
                result.add(nums2[i]);
            }
        }
        int[] a = result.stream().mapToInt(Integer::intValue).toArray();
        return a;
    }
   
}
```

第二个for循环中编译报错OutOfBoundException

哈希表的本质不应该是链表类似的东西吗，为啥会出现越界，而且是只有添加的情况

#### 结果

语法问题foreach的i与普通for循环的i不一样，增强for的i是元素本身

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> result=new HashSet<Integer>();
        HashSet<Integer> test=new HashSet<Integer>();

        for(int i:nums1){
            test.add(i);
        }
        for(int i:nums2){
            if(test.contains(i)){
                result.add(i);
            }
        }
        int[] a = result.stream().mapToInt(Integer::intValue).toArray();
        return a;
    }
   
}
```

同时，在提出问题时得到回答，可以尝试使用contains方法；可以直接判断是否有元素存在功能更直接对应

