### CSP-J | 普及组 代码模板专栏
> 包含CSP-J的常考知识

| 序号 | 主要内容 | 概览 |
|--|--|--|
| A1 |  |
# CSP-J 常用 STL 整理（适配 CSP-J，剔除 S 组冷门，重点写**函数 + 特性 + 坑点**）

> 头文件总览

```
#include <iostream>
#include <algorithm> // sort, min, max, reverse, unique, lower_bound, upper_bound
#include <vector>
#include <queue>
#include <stack>
#include <string>
#include <map>
#include <set>
// #include <deque> CSP-J用得少
using namespace std;
```

> 重要前置：STL 迭代器 `begin()` 首元素、`end()` **尾后位置（不是最后一个元素！）** `a.begin()` 指向第一个元素；`a.end()` 是最后一个元素的下一位。

----------

## 一、vector 动态数组（⭐CSP-J 最常用）

```
vector<int> v;
vector<int> v(n,0); // 开n个，初始0
```

### 成员函数

-   `v.push_back(x)`：末尾追加 x ✅（最常用）
-   `v.pop_back()`：删除末尾元素，**无返回值**
-   `v.size()`：返回元素个数，返回类型 `size_t`（无符号！坑点：`v.size()-1` 空 vector 会爆）
-   `v.empty()`：为空返回 true
-   `v.clear()`：清空，size=0，不释放内存
-   `v.front()`：第一个元素；`v.back()`：最后一个元素
-   `v[i]`：像数组一样下标访问（**不越界检查**）
-   `v.insert(it, x)`：迭代器 it 位置插入 x（慢，尽量少用）
-   `v.erase(it)`：删除 it 位置元素；`erase(l,r)` 删除 [l,r)

### 特性

1.  自动扩容，不用提前算大小，模拟、存数据首选
2.  可以直接排序：`sort(v.begin(),v.end())`
3.  可以作为数组传参、存结构体
4.  支持范围 for：`for(auto x:v)`（CSP-J 现在允许）

### 坑

size_t 无符号，判断 `if(v.size()>0)`，不要写 `if(v.size()-1>=0)`

----------

## 二、string 字符串（⭐T1/T2 高频）

```
string s;
```

### 成员函数

-   `s.size() / s.length()`：字符串长度，两者等价
-   `s.empty()` 是否空
-   `s += "abc"` / `s += 'a'` 拼接（非常好用）
-   `s.substr(pos, len)`：从 pos 开始，取 len 个字符；len 省略就取到末尾 `s.substr(2,3)`
-   `s.erase(pos,len)`：从 pos 删 len 个字符
-   `s.insert(pos,str)`：pos 位置插入字符串
-   `s.find(t)`：查找子串 t，找到返回下标；找不到返回 `string::npos`
    
    ```
    if(s.find("ab") != string::npos)
    ```
    
-   `s.compare(t)`：比较字符串，相等返回 0
-   `s[ i ]` 按下标访问字符，可读写

### 特性

1.  可以直接 `cin >> s`、`cout << s`；`getline(cin,s)` 读整行（注意换行残留）
2.  可以直接比较：`s1 < s2` 字典序比较
3.  可以 sort：`sort(s.begin(),s.end())` 给字符排序

### 坑

-   `getline` 和 cin 混用，cin 读完后缓冲区有换行，要用 `cin.ignore()`
-   `string::npos` 是很大的无符号数，不要用 int 存返回值

----------

## 三、queue 队列（BFS 必背，先进先出 FIFO）

```
queue<int> q;
```

### 成员函数

-   `q.push(x)`：入队（队尾）
-   `q.pop()`：队首出队，**无返回值！！大坑**
-   `q.front()`：队首元素
-   `q.back()`：队尾元素
-   `q.empty()` 判断空
-   `q.size()` 元素数量

### 特性

1.  BFS 迷宫、最短路标准容器
2.  不支持随机访问！不能 `q[i]`，不能 sort

### 坑

pop () 不返回元素，正确写法：

```
int u = q.front(); q.pop();
```

----------

## 四、stack 栈（后进先出 LIFO）

```
stack<int> st;
```

### 成员函数

-   `st.push(x)` 入栈
-   `st.pop()` 弹出栈顶，**无返回值**
-   `st.top()` 栈顶元素
-   `st.empty()`、`st.size()`

### 特性

1.  表达式求值、括号匹配、单调栈（CSP-J 单调栈低频）
2.  同样**不能随机访问**，不能下标、不能 sort

### 坑

pop () 不返回值！

----------

## 五、set 集合（自动有序、去重）

```
set<int> st;
```

### 成员函数

-   `st.insert(x)` 插入 x，重复自动忽略
-   `st.erase(x)` 删除值 x；`st.erase(it)` 删除迭代器位置
-   `st.count(x)`：存在返回 1，不存在返回 0（判断存在神器）
-   `st.find(x)`：找到返回迭代器，找不到 `st.end()`
-   `st.begin()` 最小元素；`st.rbegin()` 反向迭代器（最大值）
-   `st.empty()`、`st.size()`

### 特性

1.  **默认从小到大自动升序**，内部红黑树，O (log n)
2.  元素**唯一，自动去重**
3.  不能直接修改元素，只能删了再插
4.  遍历只能迭代器 / 范围 for，不能下标访问

### 常用场景：离散化去重、判断某个数是否出现过

```
if(st.count(x))
```

----------

## 六、map 映射（键值对 key -> val）

```
map<int,int> mp;
```

### 成员函数

-   `mp[key] = val` 赋值；**key 不存在会自动创建，初始值 0**（重点特性！）
-   `mp.insert( {k,v} )`
-   `mp.erase(key)`
-   `mp.count(key)` 判断 key 是否存在（1 存在，0 不存在）
-   `mp.find(key)` 迭代器，找不到返回 end ()
-   `mp.begin()` 最小 key，有序

### 特性

1.  key 自动从小到大排序，key 唯一
2.  `mp[x]` 访问如果没有 x，会凭空插入 x，很多 bug 来源！ ✅ 安全判断写法：

```
if(mp.count(x)) cout << mp[x];
```

❌ 不推荐直接 `if(mp[x])`，不存在就新建了 3. 可以存字符串映射 `map<string,int>`

### 用途：统计、离散化、编号映射

> 补充：CSP-J 一般不用 unordered_map（哈希，容易卡常数、卡评测），优先 map

----------

# 七、algorithm 库万能函数（不属于容器，但复赛天天用！⭐重中之重）

```
#include <algorithm>
```

### 1. sort 排序

```
sort(a+1,a+n+1);    // 数组 [1,n]
sort(v.begin(),v.end());
// 自定义比较函数（从大到小）
bool cmp(int x,int y){return x>y;}
sort(v.begin(),v.end(),cmp);
```

特性：**快速排序，不稳定**；复杂度 \(O(n\log n)\)；支持数组、vector、string

### 2. reverse 翻转

```
reverse(v.begin(),v.end());
reverse(s.begin(),s.end());
```

### 3. unique 去重（⚠️ 先排序！！）

> unique**不会删除元素**，只是把重复元素挪到后面，返回去重之后的尾迭代器

```
sort(v.begin(),v.end());
auto it = unique(v.begin(),v.end());
v.erase(it, v.end()); // 真正删掉重复
```

必考套路：排序 + unique+erase 去重

### 4. lower_bound /upper_bound（二分，前提：有序！！）

-   `lower_bound(l,r,x)`：第一个 **≥x** 的位置
-   `upper_bound(l,r,x)`：第一个 **>x** 的位置 返回迭代器；找不到返回 end ()

```
int pos = lower_bound(v.begin(),v.end(),x) - v.begin();
```

👉 用来二分查找、二分答案配套、求排名

### 5. min / max

```
min(a,b); max(a,b);
min({a,b,c}); max({a,b,c}); // C++11，多值最值，CSP可用
```

### 6. swap (a,b) 交换

`swap(v[i],v[j])` 容器、变量都能用

----------

# 📌 快速对比表（方便考前翻）

表格

容器

顺序

是否可重复

随机访问

常用场景

vector

插入顺序

✅ 可重复

✅ [i]

存数据、模拟、排序

queue

FIFO

✅

❌

BFS

stack

LIFO

✅

❌

括号匹配

set

自动升序

❌ 唯一

❌

去重、查询存在性

map

key 升序

key 唯一

❌

键值映射、计数

string

字符顺序

✅

✅ [i]

字符串处理

# 📌 CSP-J STL 避坑总清单（高频丢分点）

1.  `queue/stack` 的 pop () **没有返回值**！
2.  `map[key]`，key 不存在会自动插入
3.  `unique` 必须先 sort，本身不删除元素
4.  lower_bound/upper_bound 必须保证区间有序
5.  `.end()` 是尾后，不是最后一个元素
6.  `size()` 返回 size_t（无符号），减法容易溢出
7.  set/map 是 O (logn)，大量数据比 vector 慢一点，不要无脑用
8.  getline 和 cin 混用时，换行符残留问题

# 📌 补充：什么时候不用 STL？

简单计数优先**数组 / 桶**，比 set/map 快很多；只有值域很大、离散化的时候才上 set/map。

如果你想要，我可以给你一份：**STL 配套默写模板（直接复制进代码，复赛直接用）**。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMzgyNDQwNTYsLTY3NjE4OTIwOV19
-->