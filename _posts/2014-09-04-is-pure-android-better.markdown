# C++位运算和异或运算完整指南

## 异或（XOR）运算详解

### 异或的基本规则
异或符号：`^`，遵循"相同为0，不同为1"的规则：

```
0 ^ 0 = 0
0 ^ 1 = 1  
1 ^ 0 = 1
1 ^ 1 = 0
```

### 异或运算示例
```cpp
// 二进制异或
5 ^ 3 = ?
5: 101
3: 011
   ---
   110 = 6

// C++代码
int a = 5, b = 3;
cout << (a ^ b) << endl;  // 输出: 6
```

### 异或的重要特性

```cpp
// 1. 交换律: a ^ b = b ^ a
5 ^ 3 == 3 ^ 5  // true

// 2. 结合律: (a ^ b) ^ c = a ^ (b ^ c)  
(5 ^ 3) ^ 2 == 5 ^ (3 ^ 2)  // true

// 3. 任何数与0异或等于自身
a ^ 0 = a

// 4. 任何数与自身异或等于0
a ^ a = 0

// 5. 三次异或回到原值
a ^ b ^ b = a  // 常用于交换变量
```

### 异或的经典应用

#### 1. 不用临时变量交换两数
```cpp
void swap(int &a, int &b) {
    a = a ^ b;  // a = a^b, b = b
    b = a ^ b;  // a = a^b, b = (a^b)^b = a
    a = a ^ b;  // a = (a^b)^a = b, b = a
}
```

#### 2. 找出数组中唯一的单独数字
```cpp
// 数组中除了一个数字，其他都出现两次，找出单独的数字
int findSingle(vector<int>& nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;  // 相同的数字异或后变为0
    }
    return result;  // 最后剩下单独的数字
}
```

## 位运算全家桶

### 1. 按位与（AND）- `&`
```cpp
// 规则：都为1才为1
5 & 3 = ?
5: 101
3: 011  
   ---
   001 = 1

// 应用：判断奇偶
bool isOdd(int n) {
    return n & 1;  // 最低位为1则为奇数
}

// 应用：清除特定位
int clearBit(int num, int pos) {
    return num & ~(1 << pos);  // 将第pos位清0
}
```

### 2. 按位或（OR）- `|`
```cpp
// 规则：有1就为1
5 | 3 = ?
5: 101
3: 011
   ---
   111 = 7

// 应用：设置特定位
int setBit(int num, int pos) {
    return num | (1 << pos);  // 将第pos位设为1
}
```

### 3. 按位取反（NOT）- `~`
```cpp
// 规则：0变1，1变0
~5 = ?
5:  00000101
~5: 11111010 (补码表示)

// 应用：创建遮罩
int mask = ~((1 << n) - 1);  // 创建前n位为0的遮罩
```

### 4. 左移（<<）和右移（>>）
```cpp
// 左移：相当于乘以2的n次方
5 << 2 = 20   // 5 * 2^2 = 20
101 -> 10100

// 右移：相当于除以2的n次方
20 >> 2 = 5   // 20 / 2^2 = 5
10100 -> 101

// 快速计算2的幂
int powerOf2(int n) {
    return 1 << n;  // 2^n
}
```

### 位运算技巧集锦

```cpp
// 1. 判断是否为2的幂
bool isPowerOf2(int n) {
    return n > 0 && (n & (n-1)) == 0;
}

// 2. 计算二进制中1的个数
int countBits(int n) {
    int count = 0;
    while (n) {
        count++;
        n &= (n-1);  // 每次消除最右边的1
    }
    return count;
}

// 3. 获取最低位的1
int getLowestBit(int n) {
    return n & (-n);
}

// 4. 翻转特定位
int flipBit(int num, int pos) {
    return num ^ (1 << pos);
}

// 5. 检查第pos位是否为1
bool checkBit(int num, int pos) {
    return (num >> pos) & 1;
}
```

### 实际算法应用

```cpp
// 子集生成（使用位运算枚举所有子集）
void generateSubsets(vector<int>& nums) {
    int n = nums.size();
    for (int mask = 0; mask < (1 << n); mask++) {
        vector<int> subset;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {  // 检查第i位
                subset.push_back(nums[i]);
            }
        }
        // 处理当前子集
    }
}
```

## C++引用与传值的差别

### 函数参数传递方式对比

#### 1. 按值传递（Pass by Value）
```cpp
double trans1(double celsius) {  // 没有 &
    celsius = celsius + 100;     // 修改参数值
    return celsius + 273.15;
}

int main() {
    double temp = 25.0;
    double result = trans1(temp);
    cout << "原始值: " << temp << endl;      // 输出: 25.0 (未改变)
    cout << "返回值: " << result << endl;    // 输出: 398.15
}
```

#### 2. 按引用传递（Pass by Reference）
```cpp
double trans1(double &celsius) {  // 有 &
    celsius = celsius + 100;      // 修改参数值
    return celsius + 273.15;
}

int main() {
    double temp = 25.0;
    double result = trans1(temp);
    cout << "原始值: " << temp << endl;      // 输出: 125.0 (被改变了!)
    cout << "返回值: " << result << endl;    // 输出: 398.15
}
```

### 主要差别总结

| 特性 | 按值传递 | 按引用传递 |
|------|----------|------------|
| **内存开销** | 复制一份副本 | 直接使用原变量 |
| **原变量是否改变** | 不会改变 | 可能被改变 |
| **安全性** | 更安全，不会意外修改 | 需要小心，可能意外修改 |
| **效率** | 大对象时效率低 | 效率高，无复制开销 |

### 实际应用建议

```cpp
// 1. 只读取，不修改 - 使用 const 引用
double convertToKelvin(const double &celsius) {
    return celsius + 273.15;  // 不能修改 celsius
}

// 2. 需要修改原变量 - 使用引用
void addTax(double &price, double taxRate) {
    price *= (1 + taxRate);   // 直接修改原价格
}

// 3. 基本类型且不修改 - 按值传递也可以
double square(double x) {
    return x * x;             // 简单计算，按值传递
}
```

## 常见代码问题分析

### 类型错误示例
```cpp
// ❌ 错误代码
class Solution {
public:
    int traverse(TreeNode* root) {
        int sum=0;
        sum+=root->left;   // 错误：root->left 是指针，不是数值！
        sum+=root->right;  // 错误：root->right 是指针，不是数值！
        return sum;
    }
    bool checkTree(TreeNode* root) {
        return root->val==traverse(root)?true:false;
    }
};
```

### 修正后的代码
```cpp
// ✅ 正确代码
class Solution {
public:
    int traverse(TreeNode* root) {
        int sum = 0;
        if (root->left) sum += root->left->val;   // 访问左子节点的值
        if (root->right) sum += root->right->val; // 访问右子节点的值
        return sum;
    }
    
    bool checkTree(TreeNode* root) {
        return root->val == traverse(root);  // 简化三元运算符
    }
};
```

### 问题分析

1. **类型错误**：混淆了指针和指针指向的值
   - `root->left` 是指向左子节点的**指针**
   - `root->left->val` 是左子节点的**值**

2. **空指针检查缺失**：需要先检查指针是否为空

3. **三元运算符冗余**：比较结果本身就是bool类型

## Word线性格式数学公式

在Word的线性格式中，适应度函数公式表示为：

```
Fitness = -∑(Q_s(t) - Q_i(t) - Q_s(0))^2
```

或者更详细地写成：

```
Fitness = -∑_{i}(Q_s(t) - Q_i(t) - Q_s(0))^2
```

### 在Word中输入的步骤：
1. 直接输入：`Fitness = -∑(Q_s(t) - Q_i(t) - Q_s(0))^2`
2. 选中这段文本
3. 按 `Alt + =` 或者插入 → 公式，Word会自动将其转换为数学公式格式

### 线性格式语法：
- `∑` 表示求和符号
- 下标用 `_` 表示（如 `Q_s` 表示Q的下标s）
- 上标用 `^` 表示（如 `^2` 表示平方）
- 括号直接使用 `()` 即可

---

*位运算在算法竞赛、底层编程和性能优化中都非常重要，掌握这些基本操作会让你的代码更加高效！*

