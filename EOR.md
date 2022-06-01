# 异或运算及相关面试题

### 异或运算公式

- 公式1：相同的值异或后等于0
- 公式2：异或运算满足交换律和结合律
- 公式3：任何值异或0都等于它本身

### 题目1：交换变量

题目：有两个数值变量 A 和 B，交换两个变量的值。

要求：额外空间复杂度为0。

##### 分析

1. A = A(原始A) ^ B(原始B) => 原始A ^ 原始B

2. B = A(原始A) ^ B(原始B) ^ B(原始B) => 原始A ^ 原始B ^ 原始B => 原始A ^ 0 => 原始A
3. A = A(原始A) ^ B(原始B) ^ B(原始A) => 原始A ^ 原始B ^ 原始A => 原始B ^ 0 => 原始B
4. 交换成功

##### 代码

```csharp
public void Swap (int[] arr, int x, int y) {
    if (x == y) {
        return;
    }
    arr[x] = arr[x] ^ arr[y];
    arr[y] = arr[x] ^ arr[y];
    arr[x] = arr[x] ^ arr[y];
}
```

### 题目2：找出出现奇数次的数

题目：一个数组中，有一种数出现了奇数次，其他数都出现了偶数次，找到这个出现了奇数次的数。

##### 分析

1. 根据公式1：所有偶数次出现的值异或后都为0，将会只剩下奇数次的值。

##### 代码

```csharp
public int GetOddNumber (int[] arr) {
    int oddNum = 0;
    foreach (int val in arr) {
        oddNum ^= val;
    }
    return oddNum;
}
```

### 题目3：找出二进制中最右侧的1

题目：存在一个不为0的数，找到这个数2进制中最右侧的1。

##### 分析

1. 公式，非0值的最右侧1为：A & -A

##### 代码

```csharp
public int GetRightOne (int value) {
    int right = value & -value;
 	return right;
}
```

### 题目4：找到出现奇数次的两个数

题目：一个数组中有两种数X和Y出现了奇数次，其他数都出现了偶数次，找出这个两个奇数次的值。

要求：额外空间复杂度O(1)，时间复杂度O(N)。

##### 分析

	1. 将所有值进行异或后，得到 X ^ Y。
	1. X 和 Y不为同一个值，所以 X ^ Y 的结果必定不等于0，那么X 和 Y必定存在2进制中某个位置的值不同。
	1. 通过题目3找到最右侧1的位置，即X和Y不同的位置。
	1. 通过这个1的位置，将数组中的值划分为两种：该位置为 1 的和为 0 的，而 X 和 Y 分别对应这两种情况。

##### 代码

```csharp
public int[] GetOddNumbers (int[] arr) {
    int[] odds = new int[2];
 
    int eor = 0;
    foreach (int val in arr) {
        eor ^= val;
    }
    int rightOne = value & -value;
    foreach (int val in arr) {
        if (val & rightOne > 0) {
            odds[0] ^ = val;
        }
    }
    odds[1] ^= eor;
    return odds;
}
```

### 题目5：找到出现了K次的数

**题目：**一个数组中有一个数出现了K次，其他数都出现了M次，M > 1，K < M，找到出现了K次的数。

**要求：**额外空间复杂度O(1)，时间复杂度O(N)。

##### 分析

1. 建立一个辅助数组。
2. 统计所有值二级制1出现的频次。
3. 遍历辅助数组，如果值模上M不等于零，那么K次的数一定在改位上出现过。

##### 代码

```csharp
public int[] GetOnlyKTimes (int[] arr, int k, int m) {
    int t = new int[32];
    foreach (int val in arr) {
        for (int i = 0; i < 32; i++) {
        	t[i] += (val >> i) & 1;
        }
    }
    int kValue = 0;
    for (int i = 0; i < 32; i++) {
        if (t[i] % m != 0) {
            kValue |= 1 << i;
        }
    }
    return kValue;
}
```

