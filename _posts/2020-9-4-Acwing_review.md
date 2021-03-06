---
layout: post
title: 9.4 acwing基础算法题回顾
---


- 785.快速排序

  - 思路：快排基本思想，l指针先找左边部分大于中值的元素，然后在r指针找右边小于种植元素，交换位置，直到l>r；对此时的左右部分继续做快速排序的划分。最差O(N^2),平均O(NlogN)

  - 代码

    ```C++
    void quick_sort(int q[], int l, int r)
    {
        if ( l >= r) return;
        int x = q[l+r >>1], i= l-1, j = r+1;
        while(i<j)
        {
        	do i++; while (q[i]<x);
     		do j++; while (q[j]>X);
            if(i<j) swap(q[i], q[j]);
        }
    	quick_sort(q,l,j);
        quick_sort(q,j+1,r);
    }
    ```

- 787.归并排序

  - 思路：使用O(N)辅助数组，自底向上，每次递归处理的两个子序列均为已完成排序的子序列。总共logN次处理子序列，每次O(N)，时间复杂度O(NlogN)

  - 代码：

    ```C++
    void merge_sort(int q[], int l, int r)
    {
        if(l >= r) return;
        int mid = l+r >>1;
        merge_sort(q,l,mid);
        merge_sort(q,mid+1,r);
        int k=0, i=l, j =mid+1;
        while(i <= mid && j <= r)
        {
            if(q[i] <= q[j]) tmp[k++] = q[i++];
            else tmp[k++]= q[j++]
        }
        while (i <= mid) tmp[k++] = q[i++];
        while ( j<=r ) tmp[k++] = q[j++];
        // which is small than the r
        for (i=l,j=0;i <= r; i++,j++)  q[i] = tmp[j];
    }
    
    ```

- 789.数的范围

  - 题意：根据所给的序列，给出输入的数是否在原数组中出现，可能存在重复的数字。

  - 思路：使用两次二分法，先找左端的数字，再找右端数字。

  - 代码

    ```C++
    bool search(int q, int x,int n)
    {
    	int l=0, r = n-1;
        //find left one 
        while(l<r)
        {
            int mid = l+r >>1;
            if(a[mid]<x) l = mid+1；
            else r = mid;
        }
        if(a[l] != x)
        	return false;
        //find the right;
        int l1 = l, r1 = n;// notice = n
        while（l1 + 1< r1)
        {
            int mid = l1+r1 >>1;
            if(a[mid] <= x) l1 = mid;
            else r1 = mid;
        }
        printf("%d %d\n", l, l1);
        return true;
    }
    ```

- 791.高精度加法

  - 思路：使用string保存大数，由于string保存的数字较大端为实际数字的较小的位置，因此遍历时需要从大到小的进行遍历，然后将所得串reverse一次。

  - 代码

    ```C++
    string add(const string &A, const string &B)
    {
        string C;
        int t=0;
        for(int i=A.size()-1, j=B.size()-1; i>=0 || j>= 0 || t>0; i--,j--)
        {
            //要避免某一串过长的情况
            if(i>=0) t+= (A[i] -'0');
            if(j>=0) t+= (B[j] -'0');
            C += ((t%10) + '0');
            t /=10;//直接处理进位
        }
        reverse(C.begin(),C.end());
        return C;
    }
    ```

- 795.前缀和

  - 思路：从1开始，将数组中的元素记录，并从1开始计算前缀和。

  - 代码：

    ```C++
    for(int i=1; i<=n; i++) cin>>a[i];
    for(int i=1; i<=N; i++) s[i] = s[i-1] + a[i];
    cout<<(s[i] - s[i-1]);
    
    ```

- 799.最长连续不重复子序列

  - 思路：所输入的序列为递增序列，使用辅助空间s记录每个位置的数字，如果重复则需要使用慢指针向前移动，同时对遍历过程中的s数组--

  - 代码：

    ```C++
    int longunrepeatedsubseq(int a[], int s[],int n)
    {
        for(int i=0 j=0; i<n; i++)
        {
            s[a[i]]++;
            while(j<=1 && s[a[i]] >1)
            {
                s[a[j]]--;
                j++；
            }
            res = max(res, i-j +1)
        }
        return res;
    }
    ```

- 801.二进制中1的个数

  - 思路：和1相与，然后右移

  - 代码：

    ```C++
    k = (k+(a&1);
    a = a>>1;
    ```

  - 



- 802 区间和

  - 题意：每次操作让某一位置x上的数字加c，给出两个整数l,r求出区间[l,r]之间的所有数字的和

  - 思路：由于存储下标过大，需要进行映射；而使用hash表又会造成时间复杂度过大。

    1. 使用vector\<PII\>的*add*和*query*记录每次的(位置，数字)，查询区间*(l,r)*，使用vector\<int\>的*alls*保存插入位置x和查询区间(l,r)，**保存(l,r)的目的是为了在后续求区间，能直接使用前缀和进行处理**。

    2. *alls*进行排序，目的是要让其单调；使用去重，是为了避免出现再后续离散化保存数字的数组a中出现下标未定义的情况。

       ![区间和题解.png](https://cdn.acwing.com/media/article/image/2019/11/11/13021_4a9746ee04-区间和题解.png)

    3. 遍历*add*，完成数组离散到*a*数组中进行加上c的操作（使用find函数）

    4. 初始化*s*数组

    5. 遍历查询数组*query*，完成求区间[l,r]的和。

  - 代码：

    ```C++
    //输入为映射前的下标x，返回映射后的位置+1。+1为了求区间和的时候减少下标为0的判断。
    int find(int x)
    {
        int l = 0, r = alls.size()-1;
        while(l<r)
        {
            int mid = l+r >>1;
            if(alls[mid] >= x) r=mid;
            else l = mid+1;
        }
        return r+1;
    }
    
    //迭代器访问vector中的元素,对任何容器的访问都适用
    vector<int>::iterator unique(vector<int> &a)
    {
        int j=0;
        for( int i=0; i<a.size; i++)
        {
            //a[0]到a[j-1]为所有a中不重复的数字
            if(!i|| a[i]!=a[i-1])
            {
                a[j++] = a[i];
            }
        }
        return a.begin() + j;
    }
    
    int main()
    {
        cin>> n>>m;
        for(int i=0; i<n; i++)
        {
            int x,c;
            // pos and accumulate number
            cin>>x>>c;
            // subscript are the pos
            add.push_back({x,c});
            alls.push_back(x);
        }
        for(int i=0; i<m; i++)
        {
            // input the section
            int l,r;
            cin>>l>>r;
            query.push_back({l , r});
    
            alls.push_back(l);
            alls.push_back(r);
    
        }
        
        // 去重
        sort(alls.begin(), alls.end());
        alls.erase(unique(alls),alls.end());
        
        // 处理输入
        for( auto item: add)
        {
            //使用add数组，找的是去重后alls中的对应位置
            int x = find(item.first);
            a[x] += item.second;
        }
        
        //预处理前缀和
        for(int i=1; i<= alls.size(); i++) s[i] = s[i-1] +a[i];
        
        //处理
