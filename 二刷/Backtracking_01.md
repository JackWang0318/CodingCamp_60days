## 回溯章节
![image](https://github.com/user-attachments/assets/b95f0f26-da5b-44b2-84a3-ba957b33b34d)
- 回溯本质就是 穷举 枚举所有满足条件的可能性，但是是通过递归的方式纵向遍历 深度以及 for循环的方式横向遍历(画成树的方式就很好理解),

- 回溯三部曲：1.回溯函数的返回值和参数 2.回溯时的终止条件和结果存放 3. 回溯遍历的

```c++
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}

```
- 2.4.2025: [组合](https://leetcode.cn/problems/combinations/submissions/596797418/) 
