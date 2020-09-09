---
layout: post
title: 9.4 acwing基础算法题回顾
---


#### 9.8 Acwing review

- 826 单链表

  - 思路：模拟题，和先前不同，主要使用数组模拟链表

  - 代码

    ```C
    int head = -1, e[N], ne[N], idx;//e中存储节点值，ne存储当前next的坐标
    void init(int a)
    {
        head = -1;
        idx = 0;
    }
    
    void insert(int a)
    {
        e[idx] = a, ne[idx] = head, head = idx++;
    }
    void insert_k(int k, int x)
    {
        // 插入k后面
        e[idx] = x;
        ne[idx] = ne[k]；
        ne[k] = idx;
        idx++;
    }
    
    void remove(int k)
    {
    	ne[k] = ne[ne[k]]
    }
    ```

- 827.双链表

  - 思路：和单链表一致，使用`l[N]`,`r[N]`存储左右指针

  - 代码：

    ```C
    int e[N], l[N], r[N], idx;
    
    void init()
    {
        r[0] = 1,l[1] = 0;
        idx = 2;
    }
    
    void insert(int a, int x)
    {
        // 插入a右边
        e[idx] = x;
        l[idx] = a,r[idx] = ra;
        l[r[a]] = idx, r[a] = idx++;
    }
    
    void remove(int a)
    {
        l[r[a]] = l[a];
        r[l[a]] = r[a];
    }
    ```

    

- 8.29.模拟栈

  - 思路：数组存储，top保存栈顶元素

  - 代码：

    ```C
    int stack[N],top;
    ...
    if(op == "push")
    {
        cin>>x;
        stack[top++]=x;
    }
    else if(op == "pop")
        --top;
    else  if(op == "empty")
        if(top) printf("NO\n");-
        else printf("YES\n");
    else
        cout<<stack[top-1]<<endl;
    ...
    ```

    

- 830.单调栈

  - 思路：仅需要保存已输入的数字中的最小值即可

  - 代码：

    ```c
    //不使用s[0]元素
    for(int i=0; i<n; i++)
    {
        int x;
        cin>>x;
        while(tt && s[tt] >= x) tt--;
        if(tt) cout<< s[tt]<<" ";
        else cout<<""-1"<<" ";
        s[++tt] = x;
    }
    ```

- **<u>*154.滑动窗口*</u>**

  - 题意：确定滑动窗口位于每个位置时，窗口中的最大值和最小值。

  - 思路：使用队列存储原数组的下标

  - 代码：t

    ```C
    int a[N], q[N], hh, tt = -1; // q[N]中为原数组的下标，
    int n,k;//k为滑动窗口的大小\
    //以最小值为例
    for(int i=0; i<n; i++)
    {
        if(i-k+1 > q[hh]) ++hh;					 // 此时队首超出窗口
        while(hh<= tt && a[i] <= a[q[tt]]) --tt; // 此时的队尾不单调，tt-1
        q[++tt] = i;							//当前下标加到队尾
        if(i+1 >= k) printf("%d ", a[q[hh]]);
    }
    ```

- 831.KMP字符串

  - 思路：求匹配串的next数组，**next数组用来存模式串中每个前缀最长的能匹配前缀子串的结尾字符的下标。** 

  - 代码：

    ```C
    char p[M]， s[N];//输入时从p+1位置开始输入，不使用0位置
    int ne[M];
    
    //next数组
    for(int i=2, j=0;i<=m; i++)
    {
        while(j &&p[i] != p[j+1]) j = ne[j];
        if(p[i] == p[j+1]) j++;
        ne[i] =j;
    }
    
    //kmp匹配
    for(int i=1, j=0; i<=n; i++)
    {
        while(j && s[i]!= p[j+1]) j = ne[j];
        if(s[i] == p[j+1]) j++;
        if(j == n)
        {
            printf("%d", i-n);
            j = ne[j];
        }
    }
    ```

- 835.Trie字符串统计

  - 思路：使用son的二位数组存放，第一维存放的是父节点对应的idx，第二维用于记录26字母对应，数组内存放子节点对应idx。

  - 代码

    ```C
    int son[P][26];
    int cnt[N];//存放字符串最后一个字符对应idx作为cnt数组的下标。数组的值是该idx对应个数
    int idx;	//将字符串分配到一个树结构，以下标记录每个字符的位置
    char str[N];
    
    void insert(char *str)
    {
        int p=0;
        for(int i=0; str[i]; i++)
        {
            int u = str[i] - '0';
            if(!son[p][u]) son[p][u] = ++idx;
            p = son[p][u];
        }
        //此时p为str最后一个字符对应的trie树位置idx
        //中间字符串匹配到时cnt[p]==0
        cnt[p]++;
    }
    
    int query(char *str)
    {
        int p=0;
        for(int i=0; str[i]; i++)
        {
            int u = str[i] - '0';
            if(!son[p][u]) return 0;
            p = son[p][u];
    	}
        return cnt[p];
    }
    ```

- 836.合并集合

  - 思路：就是并查集，使用父指针数组记录每个元素的父指针，一开始父指针为其本身。

  - 代码：

    ```C
    int find(int x)
    {
    	if(p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
    ...
    if(op == "modify") p[find(a)] == find(b);
    ```

    

