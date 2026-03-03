# C++ 算法模板：最大公约数（欧几里得算法 / 辗转相除法）

* **时间复杂度：** O(log(min(a, b)))，速度极快。

## 核心数学原理
辗转相除法的核心基于一个定理：**两个正整数 a 和 b（a > b）的最大公约数，等于 b 和 a 对 b 取余数（a % b）的最大公约数。**

我们可以用一个简单的记法来表示：**gcd(a, b) = gcd(b, a % b)**

* **边界条件（递归出口）：** 当余数为 0 时，说明上一步的除数就是我们要找的最大公约数。在代码逻辑里表现为：当 b 等于 0 时，a 就是最大公约数。



## C++ 代码模板

```cpp
#include <iostream>

using namespace std;

/*
 * 求最大公约数 
 * 使用三目运算符的极简写法
 */
int gcd(int a, int b) {
    // 逻辑解析：
    // 1. 如果 b 不等于 0（即布尔值为 true），则继续递归调用 gcd(b, a % b)
    // 2. 如果 b 等于 0（即布尔值为 false），说明找到了最大公约数，直接返回此时的 a
    return b ? gcd(b, a % b) : a;
}

int main() {
    int n;
    cin >> n;
    
    while (n--) {
        int a, b;
        cin >> a >> b;
        cout << gcd(a, b) << endl;
    }
    
    return 0;
}
