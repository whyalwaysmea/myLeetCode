# 计数排序  
待排序的数组元素在一定的范围内，假设n个输入元素中的每一个都是介于0到k之间的整数，即输入的数据是具有上界的整数。     

计数排序的基本思想就是对每一个输入元素x，确定出小于x的元素个数。有了这一信息，就可以把x直接放到它的最终输出数组中的位置上。例如，如果有17个元素小于x，则x就输入第18个输出位置。

1. 找出待排序的数组中最大和最小的元素  
2. 统计数组中每个值为t的元素出现的次数，存入数组count的第t项   
3. 对所有的计数累加（从count中的第一个元素开始，每一项和前一项相加） 
4. 反向填充目标数组：将每个元素t放在新数组的第count(t)项，每放一个元素就将count(t)减去1

示例：arr[1,64,34,99,64,1,34,60] 
1. 统计数组中每个值为t的元素出现的次数，存入数组count的第t项： a[1] = 2, a[34] = 2, a[60] = 1, a[64] = 2, a[99] = 1
2. 对所有的计数累加：a[1] = 2, a[34] = 4, a[60] = 5, a[64] = 7, a[99] = 8  
3. 反向填充：原数组arr最后一个元素是60，a[60]是5，所以result[5-1] = 60，最后a[60]--。 然后arr的倒数第二个元素是34，a[34]是4，所以result[4-1] = 34，最后a[34]--

# 算法实现  
```java
public static int[] countSort(int[] a) {
    int result[] = new int[a.length];
    int max = a[0], min = a[0]; 
    // 1. 找出待排序的数组中最大和最小的元素  
    for (int i : a) {
        if (i > max) {
            max = i;
        }
        if (i < min) {
            min = i;
        }
    } 
    // 这里k的大小是要排序的数组中，元素大小的极值差+1
    int k = max - min + 1;
    int count[] = new int[k];
    // 2.统计数组中值为t元素出现的次数，
    for (int i = 0; i < a.length; ++i) {
        // 优化过的地方，减小了数组count的大小
        count[a[i] - min] ++;
    }
    // 3.对所有的计数累加，
    for (int i = 1; i < count.length; ++i) {
        count[i] = count[i] + count[i - 1];
    }
    // 4. 反向填充
    for (int i = a.length - 1; i >= 0; --i) {
        count[a[i] - min]--;
        result[count[a[i] - min]] = a[i];// 按存取的方式取出count的元素
    }
    return result;
}
```


# 时间复杂度  
它的复杂度为Ο(n+k)（其中k是整数的范围），当O(k)>O(n*log(n))的时候其效率反而不如基于比较的排序  

稳定的排序算法。    