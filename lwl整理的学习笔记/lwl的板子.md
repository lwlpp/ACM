

> [!WARNING]
>
> 正式比赛的评测机会很慢，最好用快读和快写
>
> 要么用scanf和printf，要么用cin,cout,ios::sync_with_stdio(0),cin.tie(0),cout.tie(0)，浮点数的输出
>
> 别他妈用#define int long long 会爆的，要思考什么时候用long long
>
> return 0;评测机会自己优化

```c++
ios::sync_with_stdio(0),cin.tie(0),cout.tie(0)
    
//关闭了同步流，就不能用scanf和printf，不能用getchar()函数，不能再用cout<<endl，而应该改用cout<<'\n'
```

```c++
cout << fixed << setprecision(7) << res;

//fixed ：这是一个操纵符，用于设置浮点数输出格式为固定小数点格式。这意味着不管浮点数的大小如何，输出时都会以普通的小数形式显示，而不是科学计数法。
//setprecision(n) ：保留n位小数

fabs(res - 11.1111) < 0.0001
//浮点数不能判断等于，只能检查误差

double x=a*1.0
//浮点数最好*1.0
```

```c++
int n;
cin>>n;
cin.ignore();
string s;
getline(cin,s);//会读取用户输入的一整行内容（包括空格），直到遇到换行符（\n），并将换行符从输入缓冲区中移除。
//如果在使用 getline 之前使用了 cin >>，可能会导致 getline 读取到多余的换行符。解决方法是使用 cin.ignore() 清除输入缓冲区中的换行符。
```

devc++：工具 -> 编译选项

<img src="D:\repo1\算法\photo\屏幕截图 2025-03-08 192850.png" alt="屏幕截图 2025-03-08 192850" style="zoom: 50%;" />

windows：命令行编译

g++ 1.cpp -o 1，编译生成.exe

1，运行.exe

<img src="D:\repo1\算法\photo\屏幕截图 2025-04-01 102259.png" alt="屏幕截图 2025-04-01 102259" style="zoom:50%;" />

<img src="D:\repo1\算法\photo\屏幕截图 2025-04-01 102255.png" alt="屏幕截图 2025-04-01 102255" style="zoom:50%;" />

```c++
#include<bits/stdc++.h>
using namespace std;
#define ff first 
#define ss second
#define pb push_back
using ll = long long; 
using PII = pair<int,int>;
using PLL = pair<ll,ll>;
using i128 = __int128;

const int mod=998244353;

void solve()
{
	
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
	int t;
	cin>>t;
	while(t--)
	{
		solve();
	}
    return 0;
}
```



# 快读快写

普通快读：

```c++
inline int read()
{
    int x=0,f=1;
    char ch=getchar();
    while(ch<'0'||ch>'9')
    {
        if(ch=='-')
            f=-1;
        ch=getchar();
    }
    while(ch>='0' && ch<='9')
        x=x*10+ch-'0',ch=getchar();
    return x*f;
}
```

超级快读：

```c++
char *p1,*p2,buf[100000];
#define nc() (p1==p2 && (p2=(p1=buf)+fread(buf,1,100000,stdin),p1==p2)?EOF:*p1++)
int read()
{
    int x=0,f=1;
    char ch=nc();
    while(ch<48||ch>57)
    {
        if(ch=='-')
            f=-1;
        ch=nc();
    }
    while(ch>=48&&ch<=57)
        x=x*10+ch-48,ch=nc();
    return x*f;
}
```

快写

```c++
void write(int x)
{
    if(x<0)
        putchar('-'),x=-x;
    if(x>9)
        write(x/10);
    putchar(x%10+'0');
    return;
}
```

# 二分（⭐）

①  二分答案：把答案当作已知，来找到符合题目的最小值/最大值

②  二分查找：在一群数中以logn的时间找到符合条件（第一个/最后一个）的数

题单

- [ ] 二分板块：[【237题】算法基础精选题单_ACM竞赛_ACM/CSP/ICPC/CCPC/比赛经验/题解/资讯_牛客竞赛OJ_牛客网](https://ac.nowcoder.com/discuss/926597)

**用法1:lower_bound()和upper_bound()**

```c++
vector<int>::iterator it=lower_bound(a.begin(),a.end(),3) //找到第一个大于等于3的数
vector<int>::iterator it=upper_bound(a.begin(),a.end(),3) //找到第一个大于3的数
值：*it
下标：it-a.begin()
//好处：难怕数组找不到想要的，会返回end(),就是跟数组最后下标的下一位(if it==v.end()代表无)
    
vector<int>::iterator it=lower_bound(a.begin(),a.end(),3,cmp) //找到第一个无法被3整除
bool cmp(const int & e,const int & val)
{
	return （e%val）==0;
}
```

**用法2：Set自带二分函数**

```c++
set<int> s;
int main() {
	s.insert(1);
	s.insert(3);
	s.insert(5);
	s.insert(7);
	s.insert(9);
	cout << *s.lower_bound(4) << endl;//输出5
    //只能找值，不能找下标
}
```

**用法3：手写二分**

适合二分答案

```c++
整数二分
1.找到第一个想要的数
int l=0,r=1e9;
while(l<r)
{
	int mid=(l+r)>>1;
	if(check(mid))
		r=mid;
	else
		l=mid+1;
}
return l;
2.找到最后一个想要的数
int l=0,r=1e9;
while(l<r)
{
	int mid=(l+r+1)>>1;
	if(check(mid))
		l=mid;
	else
		r=mid-1;
}
return l;
浮点数二分
一般用于平均数
double l = -100, r = 100;
while (r - l > 1e-8)//精度取决于题目对精度的要求，比要求多两位
{
    double mid = (l + r) / 2;
    if (check(mid)) r = mid;
    else l = mid;
}
printf("%.6lf\n", l);
```

**实战演练1：二分查找 lower_bound()**

[Problem - C - Codeforces](https://codeforces.com/contest/2032/problem/C)

问题：给定一个数组，可以进行操作：选择两个数，赋值相等，使得最后数组变成任意三个数，都可以组成非退化三角形，求最小操作数

思路：排序，只要找到最小的两个数，都能大于最大数，这个范围内都是符合非退化三角形的数。最小操作数就是前面的个数和后面的个数。枚举最小两个数，二分找到最大数，记录最小值。

```c++
signed main()
{
	int t;
	cin>>t;
	while(t--)
	{
		int n;
		cin>>n;
		vector<int> a(n);
		for(int i=0;i<n;i++)
			cin>>a[i];
		int ans=0x3f3f3f3f;
		sort(a.begin(),a.end());
		for(int i=1;i<n;i++)
		{
			int p=lower_bound(a.begin(),a.end(),a[i]+a[i-1])-a.begin();
			ans=min(ans,n-p+i-1);
		}
		cout<<ans<<endl;
	} 
}
```

**实战演练2：二分查找 set自带二分**

[C-猪猪养成计划1_牛客小白月赛109](https://ac.nowcoder.com/acm/contest/99785/C)

问题：有编号为1~n的猪。有q次操作，操作1：跟l~r的猪玩，玩过跳过。操作2：询问编号为x的猪是第几只玩的，没玩过输出0

思路：先把1~n只猪放在集合set里面，代表都没玩过，当出现操作1时，在set里面二分找到大于等于l和小于等于r的数，每次找到的数记上顺序，然后从集合去掉。所以时间复杂度为线性O（n）

```c++
signed main()
{
    int n,q;
    cin>>n>>q;
    set<int> s;
    for(int i=1;i<=n;i++)
    {
        s.insert(i);
    }
    int cnt=1;
    int ans[n+10];
    memset(ans,0,sizeof ans);
    while(q--)
    {
        int op;
        cin>>op;
        if(op==1)
        {
            int l,r;
            cin>>l>>r;
            while(1)
            {
                auto p=s.lower_bound(l);
                if(p==s.end() || *p>r)
                    break;
                ans[*p]=cnt;
                cnt++;
                s.erase(p);
            }
        }
        else if(op==2)
        {
            int x;
            cin>>x;
            cout<<ans[x]<<endl;
        }
    }
}
```

**实战演练3：二分答案**

[C-小红打怪_牛客小白月赛104](https://ac.nowcoder.com/acm/contest/94879/C)

问题：有一组怪物的血量。小红的攻击是全体攻击，所有怪物血量-1。队友1的攻击是单体攻击，单体怪物血量-1。队友2的攻击是范围攻击，相邻两只怪物血量-1（死了也可以）。每个回合，每人一次操作，最少需要多少回合

思路：答案具有单调性，已知回合答案，写check函数判断是否符合，找最小数

```c++
const int N=200100;
int n,a[N],b[N];
bool check(int t)
{
    //小红整体攻击
    for(int i=0;i<n;i++)
    {
        b[i]=max(a[i]-t,0ll);
    }
    //队友2攻击
    int c=t;
    for(int i=1;i<n;i++)
    {
        int mi=min(min(b[i],b[i-1]),c);
        c-=mi;
        b[i]-=mi,b[i-1]-=mi;
    }
    c+=t;
    for(int i=0;i<n;i++)
    {
        c-=b[i];
    }
    return c>=0;
    
}
signed main()
{
    cin>>n;
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    int l=0,r=1000000007;
    while(l<r)
    {
        int mid=(l+r)>>1;
        if(check(mid))
            r=mid;
        else
            l=mid+1;
    }
    cout<<l<<endl;
}
```

**实战演练4：二分答案**

[G-小红的数组操作(A组、B组)_2024年第七届传智杯程序设计挑战赛初赛](https://ac.nowcoder.com/acm/contest/98215/G)

问题：一个数组。操作：选一个数，减去x。k次操作后，让最大值尽可能小。求尽可能小的最大值

思路：二分答案，答案具有单调性，已知最大值是什么，那么循环跑一遍数组，凡是超过最大值的，我们就相应扣除操作次数，让这个数小于等于最大值，若操作次数<0，就不符合。

```c++
const int N=100100;
int n,k,x;
int a[N];
bool check(int t)
{
    int c=k;
    for(int i=0;i<n;i++)
    {
        if(a[i]<=t)
            continue;
        c-=(a[i]-t)/x;
        if( (a[i]-t) %x )
            c-=1;
        if(c<0)
            return false;
    }
    return true;
}
signed main()
{
    cin>>n>>k>>x;
    for(int i=0;i<n;i++)
        cin>>a[i];
    int l=-1e18,r=1e9;
    while(l<r)
    {
        int mid=(l+r)>>1;
        if(check(mid))
            r=mid;
        else
            l=mid+1;
    }
    cout<<l<<endl;
}
```

**实战演练5：二分范围**

[Problem - E - Codeforces](https://codeforces.com/contest/2044/problem/E)

问题：
$$
五个整数 k
 ， l1
 ， r1
 ， l2
 和 r2
 。计算出满足条件有序数对 (x,y)
 的个数：l1≤x≤r1
 .
l2≤y≤r2
 .

存在一个非负整数 n ，使得 \frac{y}{x} = k^n .
$$
思路：y=x*k^n,先定下k^n，只要枚举就可以，因为2^64就差不多超过1e18，那么只要枚举1~64就够了，然后再二分x的范围，找到第一个大于y范围的x，再找到最后一个小于y的范围的x，两者相减+1就是有序对的个数，注意特判一下，有没有超过x的范围！！！

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
int qpow(int a,int b)
{
    int ret=1;
    while(b)
    {
        if(b&1)
            ret=ret*a;
        a=a*a;
        b>>=1;
    }
    return ret;
}
signed main()
{
    int t;
    cin>>t;
    while(t--)
    {
        int k,l1,r1,l2,r2;
        cin>>k>>l1>>r1>>l2>>r2;
        int n; 
        for(int i=0;i<65;i++)
        {
            if((qpow(k,i))>r2)
            {
                n=i;
                break;
            }
        }
        int ans=0;
        for(int i=0;i<n;i++)
        {
            int l=0,r=1000000007;//右边界l 
            while(l<r)
            {
                int mid=l+r>>1;
                if(mid*qpow(k,i)>r2)
                    r=mid;
                else
                    l=mid+1;
            }
            int l0=0,r0=1000000007;//左边界l0 
            while(l0<r0)
            {
                int mid=l0+r0+1>>1;
                if(mid*qpow(k,i)<l2)
                    l0=mid;
                else
                    r0=mid-1;
            }
            if(l-1<l1 || l0+1>r1)
                continue;
            else
                ans += min(l-1,r1)-max(l1,l0+1)+1;
        }
        cout<<ans<<endl;
    }
}

```

**实战演练6：浮点数二分**

[102. 最佳牛围栏 - AcWing题库](https://www.acwing.com/problem/content/104/)

问题：给一组N田地的牛数，要围起连续的地（至少F地），使得围起来的平均数最大，输出最大平均数×1000再向下取整

思路：二分浮点数答案，已知平均数答案，每个数减去平均数，有区间和（至少F地）为正数，就说明该答案符合题意。区间和先用前缀和预处理，循环先割出F地，再慢慢割，记录负数，以求当前区间中最大区间和。

```c++
const int N = 100010;
int n, F;
double a[N], s[N];
bool check(double avg)
{
    for (int i = 1; i <= n; i ++ ) s[i] = s[i - 1] + a[i] - avg;
    double mins = 0;
    for (int k = F; k <= n; k ++ )
    {
        mins = min(mins, s[k - F]);
        if (s[k] >= mins) return true;
    }
    return false;
}
int main()
{
    scanf("%d%d", &n, &F);
    double l = 0, r = 0;
    for (int i = 1; i <= n; i ++ )
    {
        scanf("%lf", &a[i]);
        r = max(r, a[i]);
    }
    while (r - l > 1e-5)
    {
        double mid = (l + r) / 2;
        if (check(mid)) l = mid;
        else r = mid;
    }
    printf("%d\n", (int)(r * 1000));
}
```

# 前缀和（⭐）

适合统计区间某个字母/数字的个数。好处是预处理，查询快

```c++
sum(l~r)=sum(r)-sum(l-1)
int sum[N]
for(int i=1;i<=n;i++)
{
	sum[i]=sum[i-1]+a[i];
}
need=sum[r]-sum[l-1];
```

**实战演练1**

[Problem - C - Codeforces](https://codeforces.com/contest/1996/problem/C)

问题：给两个字符串a,b，每次给查询区间【l,r】，可以选择ai=任意字母，使得sorted(a[l...r])=sorted(b[l...r])，求最小操作数

思路：预处理字符串a和b的前缀和，枚举26个字母统计区间个数。cnt1[i][字母a】代表前i个区间内有多少个字母a。查询时只要计算出字符串ab区间内相差多少字母。

```c++
signed main()
{
	int t;
    cin>>t;
    while(t--) 
	{
		int n,q;
		cin>>n>>q;
		string a,b;
		cin>>a>>b;
		int cnt1[n+10][50],cnt2[n+10][50];
		memset(cnt1,0,sizeof(cnt1));
		memset(cnt2,0,sizeof(cnt2));
		cnt1[0][a[0]-'a']++;
		cnt2[0][b[0]-'a']++;
		for(int i=1;i<n;i++)
		{
			for(int j=0;j<26;j++)
			{
				cnt1[i][j]=cnt1[i-1][j];
				cnt2[i][j]=cnt2[i-1][j];
			}
			cnt1[i][a[i]-'a']++;
			cnt2[i][b[i]-'a']++;
		}
		for(int i=0;i<q;i++)
		{
			int l,r;
			cin>>l>>r;
			l-=1,r-=1;
			int sum=0;
			for(int j=0;j<26;j++)
			{
				if(l)
					sum+=abs(cnt1[r][j]-cnt1[l-1][j]-cnt2[r][j]+cnt2[l-1][j]);
				else
					sum+=abs(cnt1[r][j]-cnt2[r][j]);
			}
			cout<<sum/2<<endl;
		}
	}
}
```

**实战演练2**

[Problem - B - Codeforces](https://codeforces.com/contest/2053/problem/B)

问题：给定n个数的范围，求每个数是否唯一，是1否0

思路：最确定的情况：l==r只能选一个数，只有一次就是唯一，多的就不是唯一了。用bool数组sum[i]标记下标为i是否唯一，再计算前缀和，统计区间内是否都是1。

```c++
signed main()
{
	int t;
	cin>>t;
	while(t--)
	{
		int n;
		cin>>n;
		int l[n],r[n];
		map<int,int> mp;
		int sum[2*n+10]; 
		for(int i=0;i<=2*n;i++)
			sum[i]=0;
		for(int i=0;i<n;i++)
		{
			cin>>l[i]>>r[i];
			if(l[i]==r[i])
				sum[l[i]]=1,mp[l[i]]++;
			
		}
		for(int i=2;i<=2*n;i++)
			sum[i]+=sum[i-1];
		for(int i=0;i<n;i++)
		{
			if(l[i]==r[i] && mp[l[i]]>1)
				cout<<0;
			else if(l[i]==r[i] && mp[l[i]]==1)
				cout<<1;
			else if(sum[r[i]]-sum[l[i]-1]==r[i]-l[i]+1)
				cout<<0;
			else
				cout<<1;
		}
		cout<<endl;
	}
 }
```

**实战演练3**

[[CQOI2009\]中位数图](https://ac.nowcoder.com/acm/problem/19913)

问题：给出1~n的排列和数b，统计该排列有多少个长度为奇数的连续子序列的中位数是b。中位数是指把所有元素从小到大排列后，位于中间的数。

思路：中位数：有一半以上的数便可以确定中位数。把大于b的数记录为+1，等于b的数记录为0，小于b的数记录为-1。因为是排列，所以只有一个b。统计区间（包含b）和为0的个数。分类讨论：1.b在左边界 2.b在右边界 3.b在中间（借助12找抵消)

```c++
signed main()
{
    int n,k;
    cin>>n>>k;
    int a[n+10];
    for(int i=1;i<=n;i++)
        cin>>a[i];
    int index;
    for(int i=1;i<=n;i++)
    {
        if(a[i]<k)
            a[i]=-1;
        else if(a[i]==k)
            index=i,a[i]=0;
        else if(a[i]>k)
            a[i]=1;
    }
    map<int,int> mp;
    map<int,int> mp2;
    mp[index]=0;
    int cnt=1;
    for(int i=index+1;i<=n;i++)
    {
        mp[i]=a[i]+mp[i-1];
        mp2[mp[i]]++;
        if(mp[i]==0)
            cnt++;
    }
    for(int i=index-1;i>=1;i--)
    {
        mp[i]=a[i]+mp[i+1];
        if(mp[i]==0)
            cnt++;
        cnt += mp2[-mp[i]];
    }
    cout<<cnt<<endl;
}
```

**实战演练4**

中位数：有种操作，把小于mid的数变成-1，把大于mid的数变成1，把等于mid的数变成0，求区间和用前缀和预处理，也可能用滑动窗口。其实中位数就数个数，只要有一半以上的数符合，便可以确定中位数，数个数便是求区间和

[Problem - C - Codeforces](https://codeforces.com/contest/2103/problem/C)

问题：割三段，三段的中位数的中位数能不能小于k。

思路：线性求和，当前区间的最大子区间（非空），就是减去当前区间的最小区间和，就是当前区间的最大区间和

```c++
const int N=200010;
int n,k;
int a[N];
bool check_pre_suf()
{
    int s=0,in1=-1,in2=-1;
    for(int i=1;i<=n;i++)
    {
        s += a[i];
        if(s>=0)
        {
            in1=i;
            break;
        }
    }
    s=0;
    for(int i=n;i>=1;i--)
    {
        s += a[i];
        if(s>=0)
        {
            in2=i;
            break;
        }
    }
    if(in1!=-1 && in2!=-1 && in2-in1-1>=1)
        return true;
    else
        return false;
}
bool check_pre_mid()
{
    int suf[n+10],minsuf[n+10];
    suf[n]=minsuf[n]=a[n];
    for(int i=n-1;i>=1;i--)
    {
        suf[i]=suf[i+1]+a[i];
        minsuf[i]=min(suf[i],minsuf[i+1]);
    }
    int s=0;
    for(int i=1;i<=n;i++)
    {
        s += a[i];
        if(s<0)
            continue;
        if(suf[i+1]-minsuf[i+2]>=0)
        {
            return true;
        }
    }
    return false;
}
void solve()
{
    cin>>n>>k;
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
        if(a[i]<=k)
            a[i]=1;
        else
            a[i]=-1;
    }
    if(check_pre_suf() || check_pre_mid())
        cout<<"YES\n";
    else
    {
        reverse(a+1,a+n+1);
        if(check_pre_mid())
            cout<<"YES\n";
        else
            cout<<"NO\n";
    }
}
```



# 差分

区间操作转两点操作。适合先统一修改区间，最后给一个结果。

```c++
int a[N],b[N];
Void insert(int l, int r, int c)
{
	b[l] += c;
	b[r+1] -= c;
}
for(int i=1;i<=n;i++)
{
    cin>>a[i];
	insert(i,i,a[i]);//构造差分数组
}
for(int i=1;i<=m;i++)
{
	int x,y,c;
	Cin>>x>>y>>c;
	insert(x,y,c);
}
for(int i=1;i<=n;i++)
{
	a[i]=a[i-1]+b[i];
}

```

**实战演练1**

https://ac.nowcoder.com/acm/problem/24636

问题：给定L，代表数轴上0~L都有人，踹走M区域的人，马路上还有多少个人。1≤L≤100000000，1<=M<=1000000

思路：差分数组，初始化为0，赶人-1

```c++
const int N=100000100;
int a[N];
signed main()
{
    int l,m;
    cin>>l>>m;
    for(int i=0;i<m;i++)
    {
        int x,y;
        cin>>x>>y;
        a[x] += 1;
        a[y+1] -= 1; 
    }
    int sum[l+10];
    int ans=0;
    sum[0]=a[0];
    for(int i=1;i<=l;i++)
    {
        sum[i] = sum[i-1]+a[i];
        if(sum[i]==0)
            ans++;
    }
    if(a[0]==0)
        ans++;
    cout<<ans<<endl;
}
```

**实战演练2** 

https://ac.nowcoder.com/acm/problem/207053

问题：猜数字，有多少位裁判回答是对的。“+”猜的数比答案大，“-”比答案小，“.”猜到了答案。（所有数的大小都小于int类型最大值。）

思路：区间操作转两点操作——差分。猜数比答案大，那么在猜数的左区间统一+1；猜数比答案小，那么在猜数的右区间统一+1；猜数对了，在那个点+1。求前缀和，复原出原来的数组，找出最大元素的点。注意到本题区间很大，但是*n*≤100000，可以利用map实现离散化

```c++
int inf=0x3f3f3f3f;
signed main()
{
    int n;
    cin>>n;
    map<int,int> b;
    for(int i=0;i<n;i++)
    {
        int a;
        char c;
        cin>>a>>c;
        if(c=='.')
        {
            b[a]+=1;
            b[a+1]-=1;
        }
        else if(c=='+')
        {
            b[-inf]++;
            b[a]--;
        }
        else if(c=='-')
        {
            b[a+1]++;
        }
    }
    int res=0;
    int temp=0;
    for(auto i:b)//map实现离散化
    {
        temp += i.second;
        res=max(temp,res);
    }
    cout<<res<<endl;
}
```

# 位运算

### 1.异或

**题目描述**

给定一个数组[a1,a2,a3…an] ，定义f(i,j) 为 ai^aj，求任意 i,j , f(i,j) 的和

**更优的算法**

众所周知，异或是一个[位运算](https://zhida.zhihu.com/search?content_id=222397602&content_type=Article&match_order=1&q=位运算&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3MzgxMzA0MzYsInEiOiLkvY3ov5DnrpciLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMjIzOTc2MDIsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.vhjSguT_o7m6ylokr8_Ea-JzNLukfPk3ATdlXqeIYdY&zhida_source=entity)，所以我们可以将要异或的数展开来看：

拿 [1,2,3,4,5,6,7,8,9,10] 举例：

> 1 [ 0 0 0 1 ]   2 [ 0 0 1 0 ]
> 3 [ 0 0 1 1 ]   4 [ 0 1 0 0 ]
> 5 [ 0 1 0 1 ]   6 [ 0 1 1 0 ]
> 7 [ 0 1 1 1 ]   8 [ 1 0 0 0 ]
> 9 [ 1 0 0 1 ] 10 [ 1 0 1 0 ]

之后我们统计每一个位上 1 的个数：[ 3 4 5 5 ]

我们可以知道，这些数异或和的值取决于每个位(1,0)对的个数，根据[乘法法则](https://zhida.zhihu.com/search?content_id=222397602&content_type=Article&match_order=1&q=乘法法则&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3MzgxMzA0MzYsInEiOiLkuZjms5Xms5XliJkiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyMjIzOTc2MDIsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.zB_HAzU8cN0pqFgclhWBOEh1BYF9QiJVMD2bAP7LxLY&zhida_source=entity)得：[ (3×7) (4×6) (5×5) (5×5) ]

所以他们的异或和为：
$$
21×2^3+24×2^2+25×2^1+25×2^0=339
$$
**代码**

```cpp
#include<bits/stdc++.h>
using namespace std;
int arr[10] = {1,2,3,4,5,6,7,8,9,10};
int Count[10];
signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    for(int temp = 0 ; temp < 10 ; temp++){
        for(int temp2 = 0 ; temp2 <= 5 ; temp2++){
            if((arr[temp] >> temp2) & 1)
                Count[temp2] ++;
        }
    }
    int Sum = 0;
    for(int temp = 0 ; temp <= 5 ; temp++)
        Sum += ((Count[temp] * (10 - Count[temp])) * (1 << temp));
    cout << Sum;
    return 0;
}
```

**XOR补充知识**

> x^y=z⇒x^z=y
> (x⊕y)⊕z=x⊕(y⊕z)
> x⊕0=x，x⊕x=0
> x⊕b⊕b=x⊕0=x

常用性质：

> a+b=a^b+2*(a&b)
>
> 从 1 开始的长度为 k 的排列异或和为 0 等价于 k % 4 = 3

例如：后面每四对异或和为0，每一组异或情况：1^2^3=0，还可以1^3=2，1^2=3等等

> 1 [ 0 0 0 1 ]   4 [ 0 1 0 0 ]   8 [ 1 0 0 0 ]
>
> 2 [ 0 0 1 0 ]   5 [ 0 1 0 1 ]   9 [ 1 0 0 1 ]
>
> 3 [ 0 0 1 1 ]   6 [ 0 1 1 0 ] 10 [ 1 0 1 0 ]
>
> ​                      7 [ 0 1 1 1 ]  11[ 1 0 1 1 ] 







**实战演练**1

[Problem - C - Codeforces](https://codeforces.com/contest/2057/problem/C)

题目：给定范围l~r，求三个数两两异或之和最大值，输出三个数。

思路：贪心，最好尽量多满足同一位为不同的，比如三个数，个位分别是110或001，满足1×2对数。找到最大值r和最小值l，从左开始第一次不一样的那一位上。答案就是前面一样的都是一样，从不一样开始，一个数后面全0，另一个数-1使得全1，剩下一个数随便取。比如0100~1111，取1000，0111，已经满足最大了，剩下一个数随便取

```c++
signed main()
{
	int n;
    cin >> n;
    while (n--) {
        int l, r;
        cin >> l >> r;
        int ans=0,c;
        for(int i=31;i>=0;i--)
        {
        	if((l>>i&1) != (r>>i&1))
        	{
        		c=i;
        		break;
			}
		}
		for(int i=31;i>=c;i--)
		{
			if(i==c)
				ans += (1<<i);
			if((l>>i&1)==1)
				ans += (1<<i);
		}
		if(r-l==2)
			cout<<l<<" "<<l+1<<" "<<r<<endl;
		else if(ans-1!=l)
			cout<<ans<<" "<<ans-1<<" "<<ans-2<<endl;
		else
			cout<<ans<<" "<<ans-1<<" "<<ans+1<<endl;
    }
}
```

**实战演练**2

[兔子的区间密码](https://ac.nowcoder.com/acm/problem/20860)

题目：给定范围l~r，求两个数异或最大值，输出最大值，注意数据范围，要快速读入

思路：贪心，最好尽量多满足同一位为不同的，找到最大值r和最小值l，从左开始第一次不一样的那一位上。答案就是前面全0，后面全1。注意数据范围，要1LL！！！！！！

```c++
char *p1,*p2,buf[100000];
#define nc() (p1==p2 && (p2=(p1=buf)+fread(buf,1,100000,stdin),p1==p2)?EOF:*p1++)
int read()
{
    int x=0,f=1;
    char ch=nc();
    while(ch<48||ch>57)
    {
        if(ch=='-')
            f=-1;
        ch=nc();
    }
    while(ch>=48&&ch<=57)
        x=x*10+ch-48,ch=nc();
    return x*f;
}
signed main()
{
    int t;
    t=read();
    while(t--)
    {
        int l=read();
        int r=read();
        if(l==r)
        {
            cout<<0<<endl;
            continue;
        }
        int i;
        for(i=63;i>=0;i--)
        {
            if((l>>i&1)!=(r>>i&1))
            {
                break;
            }
        }
        cout<<(1ll<<(i+1))-1<<endl;
    }
}
```

**实战演练3**

[[NOI2014\]起床困难综合症](https://ac.nowcoder.com/acm/problem/17857)

问题：给一个数m，代表范围0~m，选一个数，使得经过给定一系列操作之后结果最大，输出最大值结果

思路：每一位都是独立的，我们只要考虑每一位0或1的情况即可。于是直接1111（-1）全1和0000（0）全0的两种情况。如果当前位结果为1，贡献为2^i

```c++
signed main()
{
    int n,m;
    cin>>n>>m;
    int a=0,b=-1;
    for(int i=0;i<n;i++)
    {
        string op;
        int t;
        cin>>op>>t;
        if(op[0]=='A')
        {
            a &= t;
            b &= t;
        }
        else if(op[0]=='O')
        {
            a |= t;
            b |= t;
        }
        else if(op[0]=='X')
        {
            a ^= t;
            b ^= t;
        }
    }
    int sum=0;
    for(int i=(1ll<<32);i;i>>=1)
    {
        if(a&i)
        {
            sum |= i;
        }
        else if((b&i) && i<=m)
        {
            sum |= i;
            m -= i;
        }
    }
    cout<<sum<<endl;
}
```

# 数据结构应用

## 1.vector

支持比较运算（>=<），按字典序，所以二维数组可以排序

```c++
vector<int> a,b;
a={12,1},b={11,1};
//a>b
reverse(a.begin(),a.end());
//a={1,12}
vector<vector<int>> a;
sort(a.begin(),a.end());
//{2,2,0}
//{3,1,0}
//{3,5,0}
//{7,14,1}
//......

//二维数组输入
// 创建一个二维向量并初始化
vector<vector<int>> matrix(rows, vector<int>(cols));
vector<vector<int>> a(3,vector<int>(2,3));
for (int i = 0; i < rows; i++) 
    for (int j = 0; j < cols; j++)
        cin >> matrix[i][j];
// 动态创建二维向量
vector<vector<int>> matrix；
for (int i = 0; i < rows; i++) {
    vector<int> row(cols);
    for (int j = 0; j < cols; j++) {
        cin >> row[j];
    }
    matrix.push_back(row);
}
```

## 2.pair<int,int>

固定一对，比如值和下标是一对。支持比较运算，以first为第一关键字，以second为第二关键字（字典序）

```c++
//敲多了繁琐，以下是规范简写模式
#define ff first 
#define ss second
#define pb push_back
using ll = long long; 
using PII = pair<ll,ll>;
int main()
{
   PII a={20,20};
   vector< PII > a;
   for(auto [i,j]:a)
   {
       ......
   }
}
```

## 3.string

很耗时，别用来标记，用bool数组标记

```c++
to_string(数字)  //int->string
stoll(字符串)    //string->long long int
.substr(起始坐标,长度) //子串
s.find("1")     //首次找到字符串的下标并返回下标，若无则返回-1
s1.compare(s2)  //a>b:"a.compare(b)>0";a=b:"a.compare(b)==0";a<b:"a.compare(b)<0"
reverse(s.begin(),s.end()) //翻转
stoi(字符串,起始坐标,n进制)   //将n进制转十进制。eg:string s="100";int x=stoi(s,0,2);将二进制“100”转十进制x

```

## 4.stack/queue/deque

```c++
stack<int> s;
while(!s.empty())
{
    int x=s.top();
    s.pop();
}
queue<int> q;
while(!q.empty())
{
    int x=q.front();
    q.pop();
}
deque<int> dq;
dq.push_front(x);dq.push_back(x); //入
dq.pop_front();dq.pop_back();     //出
dq.front();dq.back();             //访问
//像数组
sort(dq.begin(), dq.end());
reverse(dq.begin(), dq.end());
for (int i = 0; i < dq.size(); i++)
     cout << dq[i] << " ";
```

##### 模型：单调栈

找出每个数左边/右边离它最近的第一大/第一小的数字

```c++
stack<int> stk;
stk.push(-1);
for(int i=0;i<n;i++)
{
    int x;
    cin>>x;
    while(stk.size()&&stk.top()>=x)  //检查栈
        stk.pop();
    cout<<stk.top()<<endl;           //输出栈顶
    stk.push(x);
}
```

##### 模型：单调队列（滑动窗口）

找出滑动窗口中的最大值/最小值

```c++
deque<int> q;  //存储下标
for(int i=0;i<n;i++)
{
    while(q.size()&&a[q.back()]>a[i])
        q.pop_back();
    q.push_back(i);
    if(i-k>=0 && q.front()<i-k+1)
    	q.pop_front();
    if(i>=k-1)
        cout<<q.front()<<endl;
}
```

## 5.优先队列

```c++
priority_queue<int> pq;  //大根堆
priority_queue<int,vector<int>,greater<int> > pq; //小根堆
q.push(1);//o(logn)
while(q.size())
{
    auto t=q.top();//o(1)
    q.pop();//o(logn)
}
```

## 6.set/multiset

set、multiset能做的事情：可以维护有序序列，二分查找，但是不能维护大于/小于某个数的数量

```c++
set<int> s;
s.insert(i);//o(logn)
s.erase(i);//o(logn)
auto p=s.lower_bound(i);//找到第一个大于等于i的迭代器o(logn)
s.erase(p);*p;
s.clear();//o(n)
int mi=*s.begin();//min就是set第一个位置的迭代器元素begin() o(1)
int ma=*prev(s.end());//max就是set最后一个位置的迭代器元素prev(end()) o(1)
multiset<int> ms;//从小到大
ms.erase(3)//删掉所有3
ms.count(3);//返回3的数量
auto p=ms.find(3);//找到第一个3就返回迭代器，没有就end()
for(auto i=ms.begin();i!=ms.end();i++)
{
    cout<<*i<<"\n";
}

1、得到大于等于 x 的第一个数的迭代器。
auto t = se.lower_bound(x); 
if(t==se.end() or *t < x) 无解; 
2、得到大于 x 的第一个数的迭代器。
auto t = se.upper_bound(x); 
if(t==se.end() or *t<=x) 无解;  
3、找到最大的小于当前数值数字
无解返回-1
ll check(ll va, set<ll> se) {
    auto t = se.lower_bound(va); 
    t=prev(t); 
    if(t==se.end() and *t>=va) return -1;
    return *t;
}
4、 找到最大的小于等于当前数值数字：
ll check(ll va, set<ll> se) {
    if(se.count(va))return va;
    
    auto t = se.lower_bound(va); 
    t=prev(t); 
    if(t==se.end() and *t>=va) return -1;
    return *t;
}
```

## 7.map/multimap

```c++
//遍历map,可以实现离散化
//求前缀和
map<int,int> b;
int res=0,temp=0;
for(auto i:b)
{
    temp += i.second;
    res=max(temp,res);
}
cout<<res<<endl;
```

## 8.bitset 压位

```c++

```

## 9.树状数组

```c++
int n, tr[200001];
int lowbit(int x)
{
	return x&-x;
}
void modify(int x,int c)//修改树状数组x位置的值
{
	for(int i=x;i<=n;i+=lowbit(i)) 
        tr[i]+=c;
}
int query(int x)//查询区间1~x的区间和；
{
	int res=0;
	for(int i=x;i>=1;i-=lowbit(i)) 
        res+=tr[i];
	return res; 
}
```

## 10.线段树

线段树用来维护区间信息（区间和，区间最值，区间GCD），可以在logn的时间内执行区间修改和区间查询


```c++
//最大连续子段和（单点修改）
 
int w[N];//区间里的数 
int n,m;
 
struct node
{
	int l,r;   //当前结点所处区间 
	int sum;   //当前区间的权值和 
	int lmax;  //当前区间的最大前缀和 
	int rmax;  //当前区间的最大后缀和 
	int tmax;  //当前区间的最大连续子序列和 
}tr[N*4];//注意对题中所给操作数量或者数据要开4倍大 
 
void pushup(node &u,node &l,node &r)
{
	u.sum=l.sum+r.sum;//父节点的和等于 左节点+右节点 
	u.lmax=max(l.lmax,l.sum+r.lmax); //父节点的最大前缀和等于max(左孩子最大前缀和，左孩子的和+右孩子的最大前缀和） 
	u.rmax=max(r.rmax,r.sum+l.rmax);//和上面同理 
	u.tmax=max(max(l.tmax,r.tmax),l.rmax+r.lmax);//包含三种情况，属于左孩子，属于右孩子，或者跨区间左边和右边 
}
 
void pushup(int u)// 由子节点更新父节点 
{
	pushup(tr[u],tr[u<<1],tr[u<<1|1]);
}
 
void build(int u,int l,int r)
{
	if(l==r) tr[u]={l,r,w[r],w[r],w[r],w[r]};//如果处理到叶结点了，就保存叶结点的信息 
	else
	{	tr[u]={l,r};	//保存当前节点的区间信息 
		int mid=l+r>>1;	
		build(u<<1,l,mid);	//递归左节点 
		build(u<<1|1,mid+1,r);	//	递归右节点 
		pushup(u);	//每次根据子节点更新父节点 
	}
}
 
void modify(int u,int x,int v)
{
	if(tr[u].l==x&&tr[u].r==x)  tr[u] = {x,x,v,v,v,v}; 	//   //如果处理到叶结点了，就保存叶结点的信息
	else 
	{
		int mid=tr[u].l+tr[u].r>>1;	//当前节点区间的中点 
		if(x<=mid) modify(u<<1,x,v);  //如果要修改的地方处于中点的左端，则递归其左儿子 
		else modify(u<<1|1,x,v);	// 如果要修改的地方处于中点的右端，则递归其右儿子 
		pushup(u);	//修改完之后由子节点更新父节点的信息 
	}
}
 
node query(int u,int l,int r)   //在区间l，r里面查询 
{
	if(tr[u].l>=l&&tr[u].r<=r) return tr[u]; // 如果当前区间在l~r里面，则直接返回想要的信息 
	else
	{	
		int mid=tr[u].l+tr[u].r>>1;  //取当前节点的区间中点 
		if(r<=mid) return query(u<<1,l,r);	//	如果当前查询区间在当前区间的中点左端，则递归左儿子 
		else if(l>mid) return query(u<<1|1,l,r);	//如果当前查询区间在当前节点区间的右端，则递归右儿子； 
		else //如果一部分在mid左边，一部分在mid右边 
		{
			auto left=query(u<<1,l,r);   //递归左儿子 
			auto right=query(u<<1|1,l,r);	//递归右儿子 
			node res;
			pushup(res,left,right);   //由左儿子和右儿子的信息来更新当前父节点的信息 
			return res;
		}	
	}
}

//区间整体加和乘（区间修改）
int w[N];//区间里的数 
int n,m,p;
 
struct node
{
	ll l,r;   //当前结点所处区间 
	ll sum;   //当前区间的权值和 
	ll add;	  //当前区间所具有加权值的懒标记 
	ll mul;	  //当前区间所具有倍数权值的懒标记 
}tr[N*4];//注意对题中所给操作数量或者数据要开4倍大 
void eval(node &root,int add,int mul)
{
	//更新公式： （root.mul * root.sum + root.add）*mul+add 
    root.sum=(root.sum*mul+(root.r-root.l+1)*add)%p;
    root.mul=root.mul*mul%p;
    root.add=(root.add*mul+add)%p;
}
void pushup(int u)// 由子节点更新父节点 
{
	tr[u].sum=(tr[u<<1].sum+tr[u<<1|1].sum)%p;
}
void pushdown(int u)
{
    eval(tr[u<<1],tr[u].add,tr[u].mul);
    eval(tr[u<<1|1],tr[u].add,tr[u].mul);
    tr[u].add=0,tr[u].mul=1;//恢复懒标记 
}
void build(int u,int l,int r)
{
	if(l==r) tr[u]={l,r,w[r],0,1};//如果处理到叶结点了，就保存叶结点的信息 
	else
	{	tr[u]={l,r,0,0,1};	//保存当前节点的信息 
		int mid=l+r>>1;	
		build(u<<1,l,mid);	//递归左节点 
		build(u<<1|1,mid+1,r);	//	递归右节点 
		pushup(u);	//每次根据子节点更新父节点 
	}
}
 
void modify(int u,int l,int r,int add,int mul)
{
	if(tr[u].l>=l&&tr[u].r<=r)
	{
		eval(tr[u],add,mul);//当前树中区间被包含在修改区间时，直接修改即可； 
	}
	else 
	{
		pushdown(u);
		int mid=tr[u].l+tr[u].r>>1;	//当前节点区间的中点 
		if(l<=mid) modify(u<<1,l,r,add,mul);  //如果要修改的地方处于中点的左端，则递归其左儿子 
		if(r>mid) modify(u<<1|1,l,r,add,mul);	// 如果要修改的地方处于中点的右端，则递归其右儿子 
		pushup(u);	//修改完之后由子节点更新父节点的信息 
	}
}
 
node query(int u,int l,int r)   //在区间l，r里面查询 
{
	if(tr[u].l>=l&&tr[u].r<=r) return tr[u]; // 如果当前区间在l~r里面，则直接返回想要的信息 
	else
	{	
	    pushdown(u);
		node res;
		res.sum=0;
		int mid=tr[u].l+tr[u].r>>1;  //取当前节点的区间中点 
		if(l<=mid) res.sum+=query(u<<1,l,r).sum%p;//查询区间的和等于左右两个子树区间的和 
		if(r>mid) res.sum+=query(u<<1|1,l,r).sum%p;
		pushup(u);
		return res;
	}
}
```

**实战演练1：括号匹配栈**

[B-tb的字符串问题_牛客小白月赛101](https://ac.nowcoder.com/acm/contest/90072/B)

问题：选择子串 ''fc'' 或者子串 ''tb'' ，将其从字符串中删去。求最后剩下字符串的最短长度。 子串：原字符串中下标连续的一段字符串。

```c++
signed main()
{
    int n;
    cin>>n;
    string s;
    cin>>s;
    stack<char> stk;
    for(int i=0;i<n;i++)
    {
        stk.push(s[i]);
        if(stk.size()>1)
        {
            int p1=stk.top();
            stk.pop();
            int p2=stk.top();
            stk.pop();
            if(p1=='b'&&p2=='t' || p1=='c'&&p2=='f')
            {
                continue;
            }
            stk.push(p2);
            stk.push(p1);
        }
    }
    cout<<stk.size();
}
```

**实战演练2：前缀和+滑动窗口**

问题：给定长度为n的整数序列，请找出长度不超过m的连续子序列的最大和。注意： 子序列的长度至少是 1。

思路：注意到数字有正有负

```c++
int sum[N];
sum[0]=0;
for(int i=1;i<=n;i++)
{
    sum[i]=a[i]+sum[i-1];
}
deque<int> dq;
int ans=sum[1];
for(int i=1;i<=n;i++)
{
    if(dq.size()&&dq.front()<i-m)
        dq.pop_front();
    ans=max(ans,sum[i]-sum[dq.front()]);
    while(dq.size()&&sum[i]<=sum[dq.back()])
        dq.pop_back();
    dq.push_back(i);
}
cout<<ans<<endl;
```

**实战演练3：贪心+栈，贪心+堆**

- [ ] [栈和排序](https://ac.nowcoder.com/acm/problem/14893)
- [ ] [[JSOI2010\]缓存交换](https://ac.nowcoder.com/acm/problem/20185)

问题：①给定入栈顺序，求字典序最大的出栈序列。②容量为m的CPU处理n个数，求最少Cache缺失次数

思路：①一个简单的贪心做法，设maxe[i]表示i~n的元素的最大值。当前栈顶的元素比maxe[i+1]大，当前栈顶比后面的最大值还大，就该出栈了。②当满容量的时候，删除下一次最晚出现的数字。

```c++
signed main()
{
    int n;
    cin>>n;
    int a[n+10],ma[n+10];
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    ma[n]=0;
    for(int i=n-1;i>=0;i--)
    {
        ma[i]=max(ma[i+1],a[i]);
    }
    stack<int> st;
    vector<int> ans;
    for(int i=0;i<n;i++)
    {
        st.push(a[i]);
        while(!st.empty() && st.top()>ma[i+1])
        {
            ans.push_back(st.top());
            st.pop();
        }
    }
    while(!st.empty())
    {
        ans.push_back(st.top());
        st.pop();
    }
    for(auto j:ans)
        cout<<j<<" ";
    cout<<endl;
}

```

```c++
void solve()
{
	int n,m;
    cin>>n>>m;
    int a[n];
    for(int i=0;i<n;i++)
        cin>>a[i];
    map<int,int> mp;//mp[a[i]]=i,记录数字下标
    int next[n+10];//第i个数的下一次出现位置
    for(int i=n-1;i>=0;i--)
    {
        if(mp[a[i]]==0)
            next[i]=n+10;
        else
            next[i]=mp[a[i]];
        mp[a[i]]=i;
    }
    int cnt=0;
    map<int,int> mp2;//标记在不在
    priority_queue<int> pq;
    int ans=0;
    for(int i=0;i<n;i++)
    {
        if(mp2[i]==0)
        {
            ans++;
            if(ans>m)
            {
                auto j=pq.top();
                pq.pop();
                mp2[j]=0;
            }
        }
        mp2[next[i]]=1;
        pq.push(next[i]);
    }
    cout<<ans<<"\n";
}
```

**实战演练4：multiset**

[3.子矩阵 - 蓝桥云课](https://www.lanqiao.cn/problems/3521/learning/?page=1&first_category_id=1&sort=students_count&second_category_id=3&name=子矩阵)

问题：给定一个 *n*×*m* 的矩阵。设一个矩阵的价值为其所有数中的最大值和最小值的乘积。求给定矩阵的所有大小为 *a*×*b* 的子矩阵的价值的和。答案可能很大，你只需要输出答案对 998244353 取模后的结果。

思路：求ax1每一块的最小值最大值，用multiset维护有序序列，可以增删。再求axb每一块的最小值最大值

```c++
#include<bits/stdc++.h>
using namespace std;
using ll=long long;
#define pb push_back
const int mod=998244353;
int main()
{
    int n,m,a,b;
    cin>>n>>m>>a>>b;
    ll s[n+10][m+10];
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            cin>>s[i][j];
        }
    }
    //求a*1最小值
    multiset<ll> x;
    ll lmin[n+10][m+10];
    for(int i=1;i<=m;i++)
    {
        x.clear();
        for(int j=1;j<=n;j++)
        {
            x.insert(s[j][i]);
            if(j>a)
            {
                x.erase(x.find(s[j-a][i]));
            }
            if(j>=a)
            {
                lmin[j-a+1][i]=*x.begin();
            }
        }
    }
    //求a*1最大值
    multiset<ll,greater<ll> > y;
    ll lmax[n+10][m+10];
    for(int i=1;i<=m;i++)
    {
        y.clear();
        for(int j=1;j<=n;j++)
        {
            y.insert(s[j][i]);
            if(j>a)
            {
                y.erase(y.find(s[j-a][i]));
            }
            if(j>=a)
            {
                lmax[j-a+1][i]=*y.begin();
            }
        }
    }
    //求a*b最小值
    multiset<ll> z;
    vector<ll> ansmin;
    for(int i=1;i<=n;i++)
    {
        z.clear();
        for(int j=1;j<=m;j++)
        {
            z.insert(lmin[i][j]);
            if(j>b)
            {
                z.erase(z.find(lmin[i][j-b]));
            }
            if(j>=b)
            {
                ansmin.pb(*z.begin());
            }
        }
    }
    //求a*b最大值
    multiset<ll,greater<ll> > c;
    vector<ll> ansmax;
    for(int i=1;i<=n;i++)
    {
        c.clear();
        for(int j=1;j<=m;j++)
        {
            c.insert(lmax[i][j]);
            if(j>b)
            {
                c.erase(c.find(lmax[i][j-b]));
            }
            if(j>=b)
            {
                ansmax.pb(*c.begin());
            }
        }
    }
    ll ans=0;
    for(int i=0;i<ansmin.size();i++)
    {
        ans = (ans+((ansmin[i]%mod)*(ansmax[i]%mod)%mod))%mod;
    }
    cout<<ans<<"\n";
    return 0;
}
```

**实战演练 5：小根堆+大根堆=对顶堆**

- [ ] [第 k 小](https://ac.nowcoder.com/acm/problem/214362)
- [ ] [106. 动态中位数 - AcWing题库y](https://www.acwing.com/problem/content/108/)

问题：①动态求第k小：有一组数组，操作1插入x，操作2求第k小。②动态求中位数：输入数组，奇数时输出中位数。

思路：①用下面的大根堆维护前k个数，答案就是大根堆的堆顶。②求中位数时：下面的大根堆的大小比小根堆的大小多1

![屏幕截图 2025-03-19 185521](D:\repo1\算法\photo\屏幕截图 2025-03-19 185521.png)

```c++
void solve()
{
	int n,m,k;
    cin>>n>>m>>k;
    int a[n];
    priority_queue<int> down;
    priority_queue<int,vector<int>,greater<int> > up;
    for(int i=0;i<n;i++)
         cin>>a[i];
    sort(a,a+n);
    for(int i=0;i<n;i++)
    {
        if(down.size()<k)
            down.push(a[i]);
        else
            up.push(a[i]);    
    }   
    while(m--)
    {
        int op;
        cin>>op;
        if(op==1)
        {
            int x;
            cin>>x;
            if(down.size()<k || down.top()>x)
            {
                down.push(x);
            }
            else
                up.push(x);
            if(down.size()>k)
            {
                int z=down.top();down.pop();
                up.push(z);
            }
        }
        else if(op==2)
        {
            if(down.size()==k)
                cout<<down.top()<<"\n";
            else
                cout<<"-1\n";
        }
    }
}
```

```c++
void solve()
{
	int id,n;
    cin>>id>>n;
    int a[n];
    priority_queue<int> down;
    priority_queue<int,vector<int>,greater<int> > up;
    cout<<id<<" "<<(n+1)/2<<"\n";
    int cnt=0;
    for(int i=0;i<n;i++)
        cin>>a[i];
    for(int i=0;i<n;i++)
    {
        if(!down.size() || down.top()>a[i])
            down.push(a[i]);
        else
            up.push(a[i]);
        if(down.size()>up.size()+1)
        {
            int x=down.top();down.pop();
            up.push(x);
        }
        if(down.size()<up.size()+1)
        {
            int x=up.top();up.pop();
            down.push(x);
        }
        if(i%2==0)
        {
            cnt++;
            cout<<down.top()<<" ";
        }
        if(cnt==10)
        {
            cout<<"\n";
            cnt=0;
        }
    }
    if(cnt!=0)
        cout<<"\n";
}
```

**实战演练6：树状数组+前缀异或**

[#3195. 「eJOI2019」异或橙子 - 题目 - LibreOJ](https://loj.ac/p/3195)

问题：给定长度为n的数组，有两种操作：
$$
①修改第i个元素为j，即 a_i = j。
②查询 [l,r] 区间内所有子区间的异或和的异或和。
$$
思路：

![屏幕截图 2025-03-08 214728](D:\repo1\算法\photo\屏幕截图 2025-03-08 214728.png)

对于r-l+1为偶数：异或结果为0
对于r-l+1为奇数：与l奇偶性一样的数的异或和
我们可以开二维树状数组来维护下，第一行维护奇数位上的前缀异或和，第二行维护偶数位上的前缀异或和

------

小知识：前缀异或 相当于 前缀和，所以修改也要用异或操作，查询是求前缀异或
$$
使用树状数组，
单点修改：O(logn)，
区间查询：O(logn)
$$

$$
朴素算法，单点修改：O(1)，区间查询：O(n)
$$

```c++
#include <bits/stdc++.h>
using namespace std;
int n, a[200001], t[2][200001];
int lowbit(int x)
{
    return x & -x;
}
void update(int b, int x, int d)
{
    for(int i = x; i <= n; i += lowbit(i)) 
		t[b][i] ^= d;
}
int query(int b, int x)
{
    int sum = 0;
    for(int i = x; i; i -= lowbit(i)) 
		sum ^= t[b][i];
    return sum;
}
int main()
{
	ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);
    int m;
    cin >> n >> m;

    for (int i = 1; i <= n; i++)
        cin >> a[i], update(i & 1, i, a[i]);

    while (m--)
	{
        int op, x, y;
        cin >> op >> x >> y;

        if (op == 1)
            update(x & 1, x, y ^ a[x]), a[x] = y;
        else
            cout << (x + y & 1 ? 0 : query(x & 1, y)^query(x & 1, x - 1)) << endl;
    }

    return 0;
}
```


# 博弈论

**必胜态：a1^a2^a3……!=0**

说明必定存在二进制某一位的1个数为奇数

先手必胜，先手只要保持平衡态即可

a1^a2^a3……=s

a1^a2^a3^s……=s^s=0

（ai）变成（ai^s），从大到小即可，就取（ai）-(ai^s) 块石头

**必败态：a1^a2^a3……=0**

说明二进制每一位的1个数为偶数

- [ ] 实战：https://ac.nowcoder.com/acm/contest/549/I

- [ ] 规律：https://codeforces.com/gym/105491/problem/A 

**sg(x) = mex(sg(x可以到达的状态))**

**“海盗分金”模型**：5个[海盗](https://baike.baidu.com/item/海盗/161939?fromModule=lemma_inlink)抢得100枚金币，他们按抽签的顺序依次提方案：首先由1号提出分配方案，然后5人表决，投票要超过半数同意方案才被通过，否则他将被扔入大海喂鲨鱼，2号再提出分配方案，以此类推。

反向推理：

第五号知道：杀到最后若剩下第四号和第五号，那么不管五号怎么办，四号支持自己拿下所有金币，五号就拿不到金币了，所以聪明的五号知道不能让三号死去，聪明的四号知道要让三号死去。

若杀到最后剩下三人，三号四号五号，三号就得拉拢五号，给五号一枚金币，保全性命；

若杀到最后剩下四人，二号三号四号五号，三号会让二号死，这样就拿的分配权（分配权的意义就是可以拿大量金币），所以二号不能拉拢三号，可以拉拢四号，因为四号知道若二号死了，剩下三人局面，四号自己是拿不到金币的，因为三号会拉拢五号！五号会争取出现三人局面，这样可以被确保拉拢，所以二号就得拉拢四号，给四号一枚金币，保全性命。

一开始五人局面，一号就得拉拢三号和五号。二号不可能没结果，三号知道二号不会理他，四号跟二号关系好，五号跟四号对着干。

十人结果：96、0、1、0、1、0、1、0、1、0。

**实战演练**

[1007 分配宝藏](https://acm.hdu.edu.cn/contest/problem?cid=1150&pid=1007)

问题：

<img src="D:\repo1\算法\photo\屏幕截图 2025-03-08 192549.png" alt="屏幕截图 2025-03-08 192549" style="zoom: 33%;" />

思路：

<img src="D:\repo1\算法\photo\屏幕截图 2025-03-08 192059.png" alt="屏幕截图 2025-03-08 192059" style="zoom:33%;" />

```C++
using ll = long long; 
const ll mod=1000000007;
void solve()
{
	ll n;
	cin>>n;
	ll ans=(2+n)/2*(n/2);//吃了天大的亏，没开#define int long long的话，最好公式算出来的答案放在longlong类型的变量，最好mod一下
	ans %= mod;
	cout<<ans<<"\n";
}
```

# 贪心

**实战演练1：排序**

- [ ] [Problem - A - Codeforces](https://codeforces.com/contest/2023/problem/A)
- [ ] [[NOIP2012\]国王的游戏](https://ac.nowcoder.com/acm/problem/16561)

思路：贪心，比较难证明，要有经验

```c++
用pair存起来，固定顺序，pair数组串联起来。
1.和之比
bool cmp(pair<int,int> a,pair<int,int> b)
{
  return a.first+a.second < b.first+b.second;
}
2.积之比
bool cmp(pair<int,int> a,pair<int,int> b)
{
  return a.first*a.second < b.first*b.second;
}
```

**实战演练2：两两相消**

- [ ] [C-小L的位运算_2025牛客寒假算法基础集训营5](https://ac.nowcoder.com/acm/contest/95337/C)
- [ ] [L-变鸡器_2025牛客寒假算法基础集训营6](https://ac.nowcoder.com/acm/contest/95338/L)

思路：两两相消，①最大值大于等于剩下之和，可以直接消掉，剩下max-sum；②最大值小于剩下之和，要么全都可以两两抵消，要么只剩下一个（总数是奇数)。

C题花费数记得特判一下，L题记得判断是否存在“CHICKEN”子串

```c++
if (cnt[3] > cnt[0] + cnt[1] + cnt[2])
	......
```

**实战演练3：中位数贪心**

- [ ] [E-双生双宿之错_2025牛客寒假算法基础集训营1](https://ac.nowcoder.com/acm/contest/95323/E)
- [ ] [D - Swap to Gather](https://atcoder.jp/contests/abc393/tasks/abc393_d)
- [ ] [104. 货仓选址 - AcWing题库](https://www.acwing.com/problem/content/106/)

思路：选中位数最优解

对于一个序列a，可以执行以下操作任意次：

> 将某个元素的值+1或-1。

求使得所有元素相同的最小操作次数。

可以证明存在一种解，是**将所有的元素变为原始序列a的中位数**。

如：序列[1, 2, 4, 5, 8]，其中位数为4，则将所有的元素变为4，操作次数最小，最小为3+1+0+1+3=8。 当序列为偶数时，存在两个中位数，则选择任意一个中位数，或是两个中位数中间的某个数，操作次数取到最小。

**实战演练4：所有区间两两都有交集（可以取得一个数，左端点最大值<=右端点最小值）**

- [ ] [M.有效算法](https://codeforces.com/gym/105158/attachments/download/25503/problemset_codeforces.pdf)

问题：给出长度为n的数组a和数组b，将ai 变成满足|ai−x|≤k×bi 的任意整数x。请你求出最小的非负整数k，使得存在至少一种方法使得操作后数组a所有数都相等。

思路：把a所有数变成x。发现k越小越不符合，越大越符合，具有单调性。考虑二分答案。设答案为k，x的范围为 [ai−k×bi,ai+k×bi]，有解当且仅当这n个取值范围区间有交，可以取得一个数，那么只需判断左端点的最大值是否不大于右端点的最小值即可。

```c++
bool check(int t)
{
	int ma=0,mi=0x3f3f3f3f;
	for(int i=0;i<n;i++)
	{
		int zhi1=a[i]-t*b[i];
		int zhi2=a[i]+t*b[i];
		ma=max(ma,zhi1);
		mi=min(mi,zhi2);
	}
	return ma<=mi;
}
signed main()
{
	int t;
	cin>>t;
	while(t--)
	{
		cin>>n;
		for(int i=0;i<n;i++)
			cin>>a[i];
		for(int i=0;i<n;i++)
			cin>>b[i];
		int l=0,r=1e9;
		while(l<r)
		{
			int mid=(l+r)/2;
			if(check(mid))
				r=mid;
			else
				l=mid+1;
		}
		cout<<l<<endl;
	}
}
```

**实战演练5：反悔贪心**

套路就是：排序+用堆维护

[tokitsukaze and Soldier](https://ac.nowcoder.com/acm/problem/50439)

问题：n个士兵，有战斗力和限制人数。求团总和的最大战斗力。

思路：把限制人数从大到小排序，用小根堆当成团，扫一遍士兵，加入，每超过人数，就踢出去最弱的战斗力，记录最大值。

```c++
bool cmp(PII a,PII b)
{
    return a.ss>=b.ss;
}
void solve()
{
	int n;
    cin>>n;
    vector<PII > a(n);
    for(int i=0;i<n;i++)
        cin>>a[i].ff>>a[i].ss;
    sort(a.begin(),a.end(),cmp);
    priority_queue<int,vector<int>,greater<int> > pq;
    ll sum=0,res=0;
    for(int i=0;i<n;i++)
    {
        sum += a[i].ff;
        pq.push(a[i].ff);
        while(pq.size()>a[i].ss)
        {
            int zhi=pq.top();
            pq.pop();
            sum -= zhi;
        }
        res=max(res,sum);
    }
    cout<<res<<"\n";
}
```

**实战演练6：区间选点，最大不相交区间数量**

问题1：给定 N 个闭区间 [ai,bi]，请你在数轴上选择尽量少的点，使得每个区间内至少包含一个选出的点。

输出选择的点的最小数量。

问题2：最大不相交区间数量=最少覆盖所有区间的点数

```c++
bool cmp(PII a,PII b)
{
	return a.ss<b.ss;
}
void solve()
{
	int n;
	cin>>n;
	PII p[n];
	for(int i=0;i<n;i++)
	{
		cin>>p[i].ff>>p[i].ss;
	}
	sort(p,p+n,cmp);
	int res=0,ed=-2e9;
	for(int i=0;i<n;i++)
	{
		if(p[i].ff>ed){
			res++;
			ed=p[i].ss;
		}
	}
	cout<<res<<"\n";
}
```

**实战演练7：区间分组**

问题：给定 N个闭区间 [ai,bi]，请你将这些区间分成若干组，使得每组内部的区间两两之间（包括端点）没有交集，并使得组数尽可能小。（同一组的小孩不能打架） 

[C. Colorful Segments 2](https://codeforces.com/gym/105385/problem/C)

思路：转化为区间分组问题，因为每一组的涂色种类递减，所以组数越少越好

```c++
void solve()
{
	int n;
	cin>>n;
	PII p[n];
	for(int i = 0;i < n; i ++)
	{
		cin >> p[i].ff >> p[i].ss;
	}
	sort(p,p+n);
	priority_queue<int,vector<int>,greater<int> > pq;
	for(int i=0;i<n;i++)
	{
		if(pq.empty() || pq.top() >= p[i].ff )//开新组
		{
			pq.push(p[i].ss);
		}
		else {//放在有的组
			pq.pop();
			pq.push(p[i].ss);
		}
	}
	cout<<pq.size()<<"\n";
}
```

**实战演练8：区间覆盖**

问题：给定 N 个区间 [ai,bi] 以及一个区间 [s,t]，请你选择尽量少的区间，将指定区间完全覆盖。

输出最少区间数，如果无法完全覆盖则输出 −1。

```c++
void solve()
{
	int st,ed;
	cin >> st >> ed;
	int n;
	cin >> n;
	PII p[n];
	for(int i = 0;i < n; i ++)
	{
		cin >> p[i].ff >> p[i].ss;
	}
	sort(p,p + n);
	int ans = 0 , i = 0;
	bool ok = false;
	while(i < n)
	{
		int j = i,r = -2e9;
		while(j < n &&  p[j].ff <= st)
		{
			r=max(r,p[j].ss);
			j ++;
		}
		if(r>=st)
		{
			st = r;
			ans ++;
			i = j;
			if(st >= ed)
			{
				ok = true;
				break;
			}
		}
		else
		 	break;
	}
	if(ok)
		cout<<ans<<"\n";
	else 
		cout<<"-1\n";
}
```



# 模拟排序

思路：根据题意重写排序cmp

**实战演练1**

[拼数](https://ac.nowcoder.com/acm/problem/16783)

问题：有n个正整数（n ≤ 20）组成一个最大的多位整数。例如：n=3时，3个整数13，312，343联接成的最大整数为：343 312 13

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
bool cmp(string x,string y)
{
    return x+y > y+x;
}
signed main()
{
    int n;
    cin>>n;
    string a[n];
    for(int i=0;i<n;i++)
    {
        cin>>a[i];
    }
    sort(a,a+n,cmp);
    for(int i=0;i<n;i++)
        cout<<a[i];
}
```

**实战演练2**

[Problem - B - Codeforces](https://codeforces.com/contest/2056/problem/B)

问题：给矩阵，对于每一对整数 1≤i<j≤n ，当且仅当 pi<pj 时，在顶点 pi 和顶点 pj 之间有一条无向边。求排列n

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
char a[1100][1100];
bool cmp(int x,int y)
{
	if(a[x][y]=='1')
		return x<y;
	return x>y;
}
signed main()
{
	int t;
	cin>>t;
	while(t--)
	{
		int n;
		cin>>n;
		int ans[n];
		for(int i=0;i<n;i++)
		{
			for(int j=0;j<n;j++)
			{
				cin>>a[i][j];
			}
			ans[i]=i;
		}
		sort(ans,ans+n,cmp);
		for(int i=0;i<n;i++)
			cout<<ans[i]+1<<" ";
		cout<<endl;
	}
}
```

**实战演练3**

[113. 特殊排序 - AcWing题库](https://www.acwing.com/problem/content/115/)

问题：给矩阵,compare(x,y)=1代表x<y，求排列n（从小到大排序），n的范围是1000

思路：双循环不超时，主要是题目是交互题目，提问次数不能超过10000，所以我们用归并排序或者二分

归并排序的时间复杂度为 *O*(*n*log*n*)，在最坏情况下调用 `compare` 函数的次数为 *O*(*n*log*n*)，对于 *n*=1000 的情况，大约需要 10,000 次调用，符合题目要求

```c++
//可以用std::stable_sort实现
class Solution {
public:
    vector<int> specialSort(int N) {
        vector<int>v;
        for(int i=1;i<=N;++i)v.push_back(i);
        stable_sort(v.begin(),v.end(),compare);
        return v;
    }
};
```



# 数论

## 1.两点距离

```c++
精益求精版本
long double distance(long double x1,long double y1,long double x2,long double y2)
{
	return (long double)(sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2)));
}
普通保险版本
ll distance(ll x1,ll y1,ll x2,ll y2)
{
	return (ll)((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));
} 
```

## 2.找对称点

```c++
中心公式,可以判断三点是否共线
int x1=2*x0-x2;
int y1=2*y0-y2;
//结合圆，圆的特性就是圆心到圆上的点的距离相等，即半径
```

## 3.三角形

三角形特性：两边之和大于第三边
做法：排序，枚举最大边，找左边小的两条边，只要两条边大于最大边，一定能组成三角形

## 4.曼哈顿距离

定义：两点 (x1,y1) 和 (x2,y2) 距离 |x1-x2|+|y1-y2|。

思路：求距离最大值，就是两个绝对值之和转化为一个绝对值。|x1-x2|+|y1-y2|转化为|(x1+y1)-(x2+y2)|或|(x1-y1)-(x2-y2)|,求max转成求最大极差|最大值-最小值|，求x+y最大值和最小值，x-y最大值和最小值。

- [ ] [B-赛跑_牛客挑战赛75](https://ac.nowcoder.com/acm/contest/84888/B)
- [ ] [1010 选择配送](https://acm.hdu.edu.cn/contest/problem?cid=1152&pid=1010)

问题：①n个坐标和m个坐标，找出其中一对最大的曼哈顿距离，可以求一个点到所有点的最大曼哈顿距离最大。②n个客户点，m个配送点，找一个配送点到所有客户的最大曼哈顿距离最小。

```c++
void solve()
{
	int n,m;
    cin>>n>>m;
    ll a,b,c,d;
    for(int i=0;i<n;i++)
    {
        ll x,y;
        cin>>x>>y;
        if(i==0)
        {
            a=x+y;
            b=x+y;
            c=x-y;
            d=x-y;
        }
        else 
        {
            a=max(a,x+y);
            b=min(b,x+y);
            c=max(c,x-y);
            d=min(d,x-y);
        }
    }
    ll ans=3e9;
    for(int i=0;i<m;i++)
    {
        ll x,y;
        cin>>x>>y;
        ans=min(ans,max({x+y-b,a-x-y,c-x+y,x-y-d}));
    }
    cout<<ans<<"\n";
}
```

## 5.筛素数

**线性筛选素数**（蓝桥杯出过，考前背一背）

```c++
const int N= 100000000;(可开大)
 
int primes[N], cnt;
int st[N];//正常的st表是表示是否被筛过了，这里的st表是【素数是本身，合数是最小质因子】

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        if (!st[i])
        {
            primes[cnt ++ ] = i;
            st[i]=i;//原版：没有
        }
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            st[primes[j] * i] = primes[j];//原版：st[...]=true
            if (i % primes[j] == 0) break;
        }
    }
}
int main()
{
    get_primes(1000000000);//筛选出一亿范围内的素数
}
```

## 6.组合数

```c++
const int mod=1e9+7;
const int N=1e6+5;
ll fac[N],ifac[N];
ll qpow(ll a,ll b)
{
    ll t=1,y=a;
    while(b)
    {
        if(b&1)t=t*y%mod;
        y=y*y%mod;
        b>>=1;
    }
    return t;
}
void init()
{
    int n=1e6;
    fac[0]=ifac[0]=1;
    for(int i=1;i<=n;i++)
        fac[i]=fac[i-1]*i%mod;
    ifac[n]=qpow(fac[n],mod-2);
    for(int i=n-1;i>=1;i--)
        ifac[i]=ifac[i+1]*(i+1)%mod;
}
ll C(ll n,ll m)//C（总，占）
{
    return n>=m&&m>=0?fac[n]*ifac[m]%mod*ifac[n-m]%mod:0;
}
```

## 7.快速幂和龟速乘，乘法逆元，int128的readprint

```c++
//①快速幂——快速计算形如a^b mod c的式子，效率O（logN）
//②龟速乘——常与快速幂搭配使用，应对数据相乘会爆longlong的情况

//longlong快速幂
const int mod=998244353;
ll qpow(ll a,ll b)
{
    ll t=1,y=a;
    while(b)
    {
        if(b&1)t=t*y%mod;
        y=y*y%mod;
        b>>=1;
    }
    return t;
}
//__int128快速幂
__int128 qpow(__int128 a, __int128 n, __int128 mod) 
{
    __int128 ret = 1;
    while (n) 
    {
        if (n & 1)
            ret = ret * a % mod;
        a = a * a % mod;
        n >>= 1;
    }
    return ret;
}
int main()
 {
    int T;
    cin>>T;
    while (T--) {
        ll a, b, p;
        cin>>a>>b>>p;
        cout<<(ll)qpow((__int128)a, (__int128)b, (__int128)p)<<endl;
    }
    return 0;
}
//龟速乘
ll quick_mul(ll x,ll y,ll mod) 
{
	ll ans=0;
	while(y)
    {
		if(y&1)
            ans+=x,ans%=mod;
		x=x+x,x%=mod;
		y>>=1; 
	}
	return ans;
}
ll quick_pow(ll x,ll y,ll mod)
{
	ll sum=1;
    while(y)
    {
         if(y&1)
             sum=quick_mul(sum,x,mod),sum%=mod;
    	 x=quick_mul(x,x,mod),x%=mod;
         y>>=1;
    }
    return sum;
}
```

```c++
//乘法逆元
const ll mod=998244353;
ll qpow(ll a,ll b,ll p)
{
    ll t=1,y=a;
    while(b)
    {
        if(b&1)t=t*y%p;
        y=y*y%p;
        b>>=1;
    }
    return t;
}
ll inv(ll x)//逆元
{
    return qmi(x, mod - 2, mod);
}
```

```c++
//__int128的read和print
inline __int128 read()
{
    __int128 x=0,f=1;
    char ch=getchar();
    while(ch<'0'||ch>'9')
    {
        if(ch=='-')
            f=-1;
        ch=getchar();
    }
    while(ch>='0'&&ch<='9')
    {
        x=x*10+ch-'0';
        ch=getchar();
    }
    return x*f;
}
inline void print(__int128 x)
{
    if(x<0)
    {
        putchar('-');
        x=-x;
    }
    if(x>9)
        print(x/10);
    putchar(x%10+'0');
}
int main()
{
    __int128 a = read();
    __int128 b = read();
    print(a + b);
    cout<<endl;
    return 0;
}
```

## 8.lowbit

O(1)判断是不是2的n次方

```c++
int lowbit(int x)
{
	return x&(-x);
}
//a是不是2的n次幂
if(a-lowbit(a)==0)
    cout<<"yes"<<endl;
else
    cout<<"no"<<endl;
```

## 9.lcm最小公倍数和gcd最大公约数

结论：

$$
LCM(a,b)= a×b/GCD(a,b)
$$

常见技巧：枚举因子，个数为n^5，数值为n^5，时间复杂度便是O（n^5 * n^2）

结论：对于一个数而言，它和任意一个数的gcd一定是它的某个因子。

$$
y=ax^2+bx+c，△>0有两个实数解，△=0有一个实数解，△<0无解,韦达定理：x1+x2=-b/a,x1x2=c/a
$$
a    b

c     d

交叉相乘ad+bc=B

ax^2+bx+c=(ax+b)(cx+d)



# 动态规划dp

开dp数组，记得初始化呀！

## 1.递推dp

[Problem - C - Codeforces](https://codeforces.com/contest/2069/problem/C)

问题：求12......(2)3子序列个数

思路：递推，dp[1]记录1的个数；每当出现2的时候，会与前面的1结合，还可以与前面12序列结合，dp[2]=dp[1]+2*dp[2]，当出现3的时候给答案加贡献

```c++
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int mod=998244353;
signed main()
{
 	int t;
	 cin>>t;
	 while(t--)
	 {
	 	int n;
	 	cin>>n;
	 	int a[n];
	 	for(int i=0;i<n;i++)
	 		cin>>a[i];
	 	int dp[3];
	 	dp[1]=dp[2]=0;
		int ans=0;
		for(int i=0;i<n;i++)
		{
			if(a[i]==1)
			{
				dp[1] ++;
				dp[1] %= mod;
			}
			else if(a[i]==2)
			{
				dp[2] = dp[1]+2*dp[2];
				dp[2] %= mod;
			}
			else
			{
				ans += dp[2];
				ans %= mod;
			}
				
		 } 
		 cout<<ans<<endl;
	}   
}
```

[爪哇部落|Online Judge | 积木游戏](http://oj.javatribe.org/contest/41/problem/C)

问题：前面积木只能与后面积木拼接，尾巴和头部相同字母可以连接在一起，求连接后最长字符串的长度

思路：递推，扫一遍积木，看出现的末尾字母，每当出现一个积木，考虑要不要拼接该积木。

```c++
void solve()
{
	int n;
    cin>>n;
    int dp[30];
    for(int i=0;i<30;i++)
        dp[i]=0;
    int res=0;
    for(int i=0;i<n;i++)
    {
        string s;
        cin>>s;
        int m=s.size();
        dp[s[m-1]-'a']=max(dp[s[m-1]-'a'],dp[s[0]-'a']+m);
        res=max(res,dp[s[m-1]-'a']);
    }
    cout<<res<<"\n";
}
```

## 2.背包

**01背包模型**
$$
dp[i][j]其中i为物品，j为限制条件，如果可以退回前一行，那么可以降维，降维时考虑正序（多重）还是倒序（一次）
$$
**实战演练1**

[E-小苯的Polygon_牛客周赛 Round 86](https://ac.nowcoder.com/acm/contest/104637/E)

问题：n根木棍，每根木棍只能用一次，要组成封闭凸多边形，求最小周长。

思路：贪心组成三角形最优，三角形两边之和大于第三边，所以排序+枚举最大边，保证前面有两个或以上的数大于枚举的最大边，便能保证组成三角形。

然后转换成背包问题，（混淆体积与价值），最大边前面的sum总和，要保证有a[i]+1（肯定是两个木棍之和，因为排序过了），剩下的尽可能去掉。也就是容量为sum-a[i]-1，能装多少东西。
$$
容量为i的最多周长，dp[i]=max(dp[i],dp[i-a[j]]+a[j]);
$$
递推，遍历每件物品，考虑装不装，不装就是不变，装就是，+a[j]，退回前i-a[j]的状态

**实战演练2**

[D-小苯的ovo(A组、B组、C组)_2024年第七届传智杯程序设计挑战赛复赛（第二场）](https://ac.nowcoder.com/acm/contest/103864/D)

问题：给长度为n的字符串，最多修改次数为m次，修改o为v，修改v为o，求最多不相交“ovo”的数量

思路：
$$
dp[ i ][ j ] 表示前 i 个位置，花费 j 次操作的最大数,转移方程dp[i][j]=max(dp[i-1][j],dp[i-3][j-k]+1);
$$
递推，遍历每个位置，每出现一个位置，考虑与前两个位置结合，不装就是前一个位置的状态，装就是，+1，并且看退回前三个位置的状态剩下j-k次最多可以装多少

```c++
void solve()
{
	int n,m;
    cin>>n>>m; 
    string s;
    cin>>s;
    int dp[n+10][m+10];
    for(int i=0;i<3;i++)
        for(int j=0;j<=m;j++)
            dp[i][j]=0;
    for(int i=3;i<=n;i++)
    {
        int k=0;
        if(s[i-1]!='o')
            k++;
        if(s[i-2]!='v')
            k++;
        if(s[i-3]!='o')
            k++;
        for(int j=0;j<=m;j++)
        {
            if(j<k)
                dp[i][j]=dp[i-1][j];
            else
                dp[i][j]=max(dp[i-1][j],dp[i-3][j-k]+1);
        }
    }
    cout<<dp[n][m]<<"\n";
}
```

**实战演练3**

[失衡天平](https://ac.nowcoder.com/acm/problem/19158)

问题：有一个长度为n的序列，从中选择任意个数字将其分成两堆使两堆的和最大（且两堆的差值不超过m）

思路：列一下动态转移方程，不装/装（左天平还是右天平）,放重的一边退回前状态：abs(d-a[i])，放轻的一边放退回前状态：d+a[i]

最重要的就是：状态转移方程是从原有的数转移过来的，所以一开始初始化dp为-0x3f3f3f3f（负inf为负无穷），避免转移到没有的数，dp00初始化为0，为后面的转移铺垫
$$
dp[i][j]=max(dp[i-1][j],dp[i-1][j+a[i]]+a[i],dp[i-1][abs(j-a[i])]+a[i])
$$

```c++
void solve()
{
	int n,m;
    cin>>n>>m;
    int dp[n+10][50000];
    memset(dp, -0x3f, sizeof(dp));
    dp[0][0]=0;
    for(int i=1;i<=n;i++)
    {
        int x;
        cin>>x;
        for(int j=0;j<10100;j++)
        {
            dp[i][j]=max({dp[i-1][j],dp[i-1][j+x]+x,dp[i-1][abs(j-x)]+x});
        }
    }
    int ma=dp[n][0];
    for(int i=1;i<=m;i++)
        ma=max(ma,dp[n][i]);
    cout<<ma<<"\n";
}
```

## 3.线性DP

**实战演练1：不相邻取数**

[D - Forbidden Difference](https://atcoder.jp/contests/abc403/tasks/abc403_d)

问题：最少删掉距离为D的数

思路：最少转最大，方便dp数组初始化和转移，dp数组初始化为-1，dp[i]=max(dp[i-1],dp[i-2]+a[i])，代表前i个数 不选a[i] / 选a[i]

**实战演练2：预处理26个字母**

[月月查华华的手机](https://ac.nowcoder.com/acm/problem/23053)

问题：给一个字符串，多个询问：快速知道某串是不是字符串的子串

思路：设 dp【i】【 j 】 表示第 i 个位置的字母后第一个 j 字母的位置，提前预处理母串s，查询子串 s 就可以简化到O( n )

```c++
#include<bits/stdc++.h>
using namespace std;
int main()
{
    string s;
    cin>>s;
    int size=s.size();
    s=" "+s;
    vector<vector<int>> dp(s.size()+5,vector<int>(30,0));
    for(int i=size-1;i>=0;i--)
    {
        for(int j=0;j<26;j++)
        {
            dp[i][j]=dp[i+1][j];
        }
        dp[i][s[i+1]-'a']=i+1;
    }
    int n;
    cin>>n;
    while(n--)
    {
        string g;
        cin>>g;
        int size2=g.size();
        g=" "+g;
        int flag=1;
        int k=0;
        for(int i=1;i<=size2;i++)
        {
            if(dp[k][g[i]-'a'])
                k=dp[k][g[i]-'a'];
            else
            {
                flag=0;
                break;
            }
        }
        if(flag)
            cout<<"Yes"<<endl;
        else
            cout<<"No"<<endl;
    }
}
```



# 搜索

## 1.图的存储

##### ①邻接矩阵

二维数组g[u] [v]存储从点u到点v的边的权值，时间复杂度为O（n^2）,空间复杂度O（n^2）

应用：只在点数不多的稠密图（例如n=1e3,m=1e6）上使用

```c++
const int N=1010;
int g[N][N];
int vis[N];
void dfs(int u)
{
    vis[u]=true;
    for(int v=1;v<=n;v++)
    {
        if(g[u][v])
            cout<<g[u][v]<<"\n";
        if(!vis[v])
        	dfs(v);
    }
}
g[u][v]=w;//建图
dfs(1);//搜
```

##### ②边集数组

e[i]存储第i条边的{起点u，终点v，边权w}，时间复杂度O（nm），空间复杂度O（m）

应用：在Kruskai算法中，需要将边按边权排序，直接存边

```c++
struct edge{
    int u,v,w;
}e[M];
int vis[N];
void dfs(int u)
{
    vis[u]=true;
    for(int i=1;i<=m;i++)
    {
        if(e[i].u==u)
        {
            int v=e[i].v,w=e[i].w;
            cout<<u<<" "<<v<<" "<<w<<"\n";
            if(!vis[v])
                dfs(e[i].v);
        }
    }
}
e[i]={u,v,w};//建图
dfs(1); //搜
```

##### ③邻接表

1->shu shu shu

2->shu shu shu

.......

```c++
const int N=100;
vector< vector<int> > e(n+1);//下标从1开始,有1~n行的空数组，即邻接表
bool vis[n+1];
void dfs(int u)
{
    vis[u]=true;
    //1.入
    for(auto v:e[u])
    {
        //2.下
        if(!vis[v])
            dfs(v);
        //3.回
    }
    //4.离
}
e[u].pb(v);//建图
dfs(1);//搜
```

##### ④链式向前星

```c++
int h[N],e[N],ne[N],idx=0;
void add(int a,int b)
{
    e[idx]=b,ne[idx]=h[a],h[a]=idx++;
}
int dfs(int u)
{
    st[u]=true;
    //1.入
    for(int i=h[u];i!=-1;i=ne[i])
    {
        int j=e[i];
        //2.下
        if(!st[j])
            dfs(j);
        //3.回
    }
    //4.离
}
memset(h,-1,sizeof h);//初始化
add(u,v);//建图
```

## 2.深搜

本质：栈，压入，弹出

##### ①网格图的搜索

时间复杂度O（2^格子数）

小知识：马走日，有八个方向

```c++
int g[N][N];//图
bool vis[N];//标记
int d[4][2]={{-1,0},{0,-1},{1,0},{0,1}};//方向
int ans=0;
void dfs(int x,int y)
{
    if(x==fx&&y==fy)//终止条件
    {
        ans++;
        return;
    }
    for(int i=0;i<4;i++)//递归
    {
        int nx=x+d[i][0],ny=y+d[i][1];
        if(1<=nx&&nx<=n&&1<=ny&&ny<=m&&vis[nx][ny]==false)
        {
            vis[nx][ny]=true;
            dfs(nx,ny);
            vis[nx][ny]=false;//全局数组，回溯
        }
    }
}
//八皇后问题，每行每列每主对角线每副对角线只能有一个棋子
int pos[N];//存储答案
bool c[N],p[N],q[N];//标记
void dfs(int i)
{
    if(i>n)//递归终止条件
    {
        ans++;
        print();
        return;
    }
    for(int j=1;j<=n;j++)//递归
    {
        if(!c[j]&&!p[i+j]&&!q[i-j+n])
        {
            pos[i]=j;
            c[j]=p[i+j]=q[i-j+n]=true;
            dfs(i+1);
            c[j]=p[i+j]=q[i-j+n]=false;//回溯
            pos[i]=0;
        }
    }
}
```

##### ②爆搜

```c++
//sum在[l,r]的方案
void dfs(int x,ll sum)
{
    if(sum>r)//1.剪枝
        return;
    if(l<=sum&&sum<=r)//2.贡献答案
        ans++;
    for(int i=x+1;i<=n;i++)//3.递归
        dfs(i,sum+a[i]);
}
dfs(0,0);

//全排列爆搜,平替：permutation
void dfs(int k)
{
    if(k==n)
    {
        print();
        return;
    }
    for(int i=1;i<=n;i++)
    {
        if(!vis[i])
        {
            res.pb(i);
            vis[i]=1;
            dfs(k+1);
            res.pop_back();//4.回溯
            vis[i]=0;
        }
    }
}
dfs(0);
```

##### **③树的搜索**

一般考子树的权值

树的重心：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。

```c++
//子树的权值
int dfs(int u)
{
    int sum=0;//1.求u的子树大小
    for(auto j:e[u])
    {
        if(!vis[j])
        {
            int s=dfs(j);//2.子节点所在的连通块大小
            sum += s;//3.各个连通块大小之和
        }
    }
    return sum+1;//4.返回给u的父节点，代表u父节点的子树大小
}
dfs(1);
```

## 3.宽搜

```c++
queue<int> q;
q.push(1);//1.入点+标记
vis[i]=true;
while(q.size())//2.层次遍历
{
    auto t=q.front();q.pop();//3.取点
    for(auto j:e[t])
    {
        if(!vis[j])
        {
            q.push(j);//4.入点+标记
            vis[j]=true;
        }
    }
}
//八数码
//思路：宽搜到每个状态，第一次搜到的状态就是最短距离
//字符串用哈希表记录状态
string z="12345678x";
void solve()
{
    unordered_map<string,int> dis;//哈希表，有时候超时了，可以trytry
    int flag=0;
    queue<string> q;
    q.push(s);
    dis[s]=0;
    while(q.size())
    {
        auto t=q.front();
        q.pop();
        if(t==z)
        {
            flag=1;
            break;
        }
        int p=t.find('x');
        int x=p/3,y=p%3;
        for(int i=0;i<4;i++)
        {
            string temp=t;
            int nx=x+d[i][0];
            int ny=y+d[i][1];
            if(0<=nx && nx<3 && 0<=ny && ny<3)
            {
                swap(temp[nx*3+ny],temp[p]);
                if(!dis[temp])
                {
                    q.push(temp);
                    dis[temp]=dis[t]+1;
                }   
            }
        }
    }
    if(flag)
      cout<<dis[z]<<"\n";
    else
      cout<<"-1\n";
}
```

**实战演练1：数连通块（上下左右斜对角算连在一起）**

<img src="photo/屏幕截图%202025-04-08%20175522.png" style="zoom: 33%;" />
思路：数w相连的块，修改成跟剩下.一样

```c++
char g[N][N];
int d[8][2]={{-1,-1},{-1,0},{-1,1},{0,-1},{0,1},{1,-1},{1,0},{1,1}};
//1.dfs
void dfs(int x,int y)
{
    g[x][y]='.';
    for(int i=0;i<8;i++)
    {
        int nx=x+d[i][0],ny=y+d[i][1];
        if(1<=nx&&nx<=n&&1<=ny&&ny<=m&&g[x][y]=='w')
        {
            dfs(nx,ny);
        }
    }
}
//2.bfs(防爆栈)
void bfs(int x,int y)
{
    queue< pair<int,int> > q;
    q.push({x,y});
    g[x][y]=".";
    while(q.size())
    {
        auto t=q.front();q.pop();
        for(int i=0;i<8;i++)
        {
            int nx=t.ff+d[i][0],ny=t.ss+d[i][1];
            if(1<=nx&&nx<=n&&1<=ny&&ny<=m&&g[nx][ny]=='w')
            {
                g[nx][ny]='.';
                q.push({nx,ny});
            }
        }
    }
}
int ans=0;
for(int i=1;i<=n;i++)
{
    for(int j=1;j<=m;j++)
    {
        if(g[i][j]=='w')
        {
            ans++;
            dfs(i,j);
        }
    }
}
```

**实战演练2：爆搜枚举**

<img src="photo/屏幕截图%202025-04-08%20175431.png" style="zoom: 33%;" />
问题：单词接龙，每个单词最多用两次，相邻不能包含关系，例如at和atide，给定字母开头，求最长字符串。(n<=20)

细节：①枚举前后缀长度不要超过单词和当前串的长度，保证相邻的两部分不包含。②在龙头字母的前面加*，就不用特判了。③开个标记数组used代表用了多少次

```c++
const int N=25;
int n,ans;
int used[N];
string word[N];
void dfs(string s)
{
    int ls=s.size();
    ans=max(ans,ls);//更新答案
    for(int i=0;i<n;i++)//枚举每个单词
    {
        string w=word[i];
        int lw=w.size();
        for(int j=1;j<ls&&j<lw;j++)
        {
            if(used[i]<2&&s.substr(ls-j)==w.substr(0,j))
            {
                used[i]++;
                dfs(s+w.substr(j));
                used[i]--;
                break;
            }
        }
    }
}
int main()
{
    cin>>n;
    for(int i=0;i<n;i++)
       	cin>>word[i];
    string start;cin>>start;
    start="*"+start;
    dfs(start);
    cout<<ans-1<<endl;
}
```

**实战演练3：剪枝**

问题：

①n个数，分组，每组中任意两个数互质，至少要分成多少组（n<=10，数字<=1e4）

```c++

```

**实战演练4：子树**

[E-小红的陡峭值（四）_牛客周赛 Round 84](https://ac.nowcoder.com/acm/contest/103152/E)

问题：定义一棵树的陡峭值为：每一条边连接的两个相邻节点，编号差值的绝对值之和。一棵由 n 个节点组成的树，断掉一条树边，使得形成两棵树，希望这两棵树的陡峭值尽可能的接近。求出这两棵树陡峭值之差的绝对值的最小值。

思路：对于一棵树，找一下他的子树的权值，假设时候子树权值是temp，那么记录一下断开父节点和这颗子树之间的边后答案是多少。已知所有边的和是sum，边权是v，那么父节点在的树的值是sum-v-temp，子树是temp，算一下绝对值之差。

```c++
#include<bits/stdc++.h>
using namespace std;
#define pb push_back
using ll = long long; 
const int N=1e5+15;
vector<int> g[N];
bool vis[N];
ll ans=1e18,cnt=0;
ll dfs(int x)
{
    vis[x]=true;
    //1.入
    ll sum=0;
    for(auto u:g[x])
    {
        if(vis[u])
            continue;
        //2.下
        ll temp=dfs(u);//子树的权值
        //3.离
        ll zhi=abs(temp-(cnt-temp-abs (x-u)));
        ans = min(ans,zhi);
        sum += ll(temp+abs(x-u));
    }
    //4.回
    return sum;
}
void solve()
{
    int n;
    cin>>n;
    for(int i=0;i<n-1;i++)
    {
        int x,y;
        cin>>x>>y;
        g[x].pb(y);
        g[y].pb(x);
        cnt += abs(x-y);
    }
    dfs(1);
    cout<<ans<<"\n";
}
```

# 图论

## 1.最短路

dis数组最好初始化为（题目要求走不到就要输出的数字）（eg:-1,0x3f3f3f3f）

##### ①权值为1

```c++
//1.有向图，存在重边和自环，求1号点到 n 号点的最短距离，无法走到 n，输出 −1。
const int N=1e5+10;
vector<vector<int>> e(N);
int dis[N];//i到1的距离
void solve()
{
    int n,m;
    cin>>n>>m;
    for(int i=0;i<m;i++)
    {
        int u,v;
        cin>>u>>v;
        e[u].pb(v);
    }
    memset(dis,-1,sizeof dis);
    queue<int> q;
    q.push(1);
    dis[1]=0;
    while(q.size())
    {
        auto t=q.front();q.pop();
        for(auto j:e[t])
        {
            if(dis[j]==-1)
            {
                q.push(j);
                dis[j]=dis[t]+1;
            }
        }
    }
    cout<<dis[n];
}
//2.网格图，有障碍1，走一步花一时间，（1，1）到（n，m）最短距离
const int N=110;
int dis[N][N];//i,j到1,1的距离
int d[4][2]={{-1,0},{0,-1},{1,0},{0,1}};
void solve()
{
    int n,m;
    cin>>n>>m;
    memset(dis,-1,sizeof dis);
    int a[n][m];
    for(int i=0;i<n;i++)
        for(int j=0;j<m;j++)
            cin>>a[i][j];
    queue<PII> q;
    q.push({0,0});
    dis[0][0]=0;
    while(q.size())
    {
        auto t=q.front();
        q.pop();
        for(int i=0;i<4;i++)
        {
            int nx=t.ff+d[i][0];
            int ny=t.ss+d[i][1];
            if(0<=nx && nx<n && 0<=ny && ny<m && a[nx][ny]==0 && dis[nx][ny]==-1)
            {
                q.push({nx,ny});
                dis[nx][ny]=dis[t.ff][t.ss]+1;
            }
        }
    }
    cout<<dis[n-1][m-1]<<"\n";
}
//走迷宫升级版：走一格花费1，杀掉怪物花掉1
struct node{
    int dis,x,y;
};
void bfs()
{
    priority_queue<node,vector<node>,greater<node>> q;
    q.push({0,sx,sy});
    maze[sx][sy]="#";
    while(q.size())
    {
        auto a=q.top();q.pop();
        if(a.x==tx&&a.y==ty)
        {
            cout<<a.dis<<"\n";
            return;
        }
        for(int i=0;i<4;i++)
        {
            int nx=a.x+dx[i],ny=a.y+dy[i];
            int ndis=0;
            if(0<=nx&&nx<n&&0<=ny&&ny<m&&maze[nx][ny]!='#')
            {
                if(maze[nx][ny]=='x')
                    ndis=a.dis+2;
                else
                    ndis=a.dis+1;
                maze[nx][ny]='#';
                q.push({ndis,nx,ny});
            }
        }
    }
    cout<<"impossible\n";
}
```

##### ②权值不为1，且是正数

```c++
//1.有向图，存在重边和自环，求1号点到 n 号点的最短距离，无法走到 n，输出 −1。
const int N=1e5+600;
vector<vector<PII>> e(N);
bool vis[N];
int dis[N];//i点到源点的距离

void solve()
{
	int n,m;
    cin>>n>>m;
    
    for(int i=0;i<m;i++)
    {
        int u,v,w;
        cin>>u>>v>>w;
        e[u].pb({w,v});
    }
    memset(vis,false,sizeof vis);
    memset(dis,0x3f,sizeof dis);
    priority_queue<PII,vector<PII>,greater<PII>> pq;
    pq.push({0,1});
    dis[1]=0;
    while(pq.size())
    {
        auto t=pq.top();pq.pop();
        int dian=t.ss;
        if(!vis[dian])
        {
            vis[dian]=true;
            for(auto [w,dian2]:e[dian])
            {
                if(dis[dian2]>dis[dian]+w)
                {
                    dis[dian2]=dis[dian]+w;
                    pq.push({dis[dian2],dian2});
                }
            }
        }
    }
    if(dis[n]==0x3f3f3f3f)
        cout<<"-1\n";
    else
        cout<<dis[n];
}
```

## 2.拓扑图

有向无环图，一定能拓扑排序！

若一个由图中所有点构成的序列 A 满足：对于图中的每条边 (x,y)，x 在 A 中都出现在 y 之前，则称 A 是该图的一个拓扑序列。

问题：给定一个 n 个点 m 条边的有向图，点的编号是 1 到 n，图中可能存在重边和自环。

请输出任意一个该有向图的拓扑序列，如果拓扑序列不存在，则输出−1。

```c++
const int N=1e5+10;
vector<vector<int>> e(N);
vector<int> du(N,0);
vector<int> ans;
int n,m;
void tuopu()
{
	queue<int> q;
	for(int i=1;i<=n;i++)
	{
		if(du[i]==0)
			q.push(i);
	}
	while(q.size())
	{
		auto t=q.front();
		q.pop();
		ans.pb(t);
		for(auto j:e[t])
		{
			du[j]--;
			if(du[j]==0)
				q.push(j);
		}
	}
	if(ans.size()!=n)
	{
		cout<<"-1\n";
		return;
	}
	for(auto j:ans)
		cout<<j<<" ";
}
void solve()
{
	cin>>n>>m;
	for(int i=0;i<m;i++)
	{
		int x,y;
		cin>>x>>y;
		e[x].pb(y);
		e[y].pb(x);
		du[y]++;
	}
	tuopu();
}
```



## 3.LCA最近公共祖先

**①倍增法**

倍增法通过预处理ST表实现快速查询，预处理时间为O(nlogn)，查询时间O(logn)；

```C++
const int N = 40010, M = N * 2;
int n,m,depth[N],fa[N][16],dist[N],x[N];
vector<vector<PII>> e(M+1);
//depth[i]记录节点i的深度，方便找lca，同时可以记录该节点是否被搜索过
//fa[i][0]很好用！为i的父节点
void bfs(int root)//预处理打出ST表
{
    memset(depth,0x3f,sizeof depth);
    depth[0] = 0,depth[root] = 1;
    
    queue<int> q;
    q.push(root);
    while(q.size())
    {
        int t = q.front();
        q.pop();
        
        for(auto [j,w]:e[t])
        //枚举节点t的儿子们
        {
            if(depth[j]>depth[t]+1)
            //说明j还没被搜索过
            {
                depth[j] = depth[t]+1;
                dist[j]=dist[t]+w;//区别
                q.push(j);//把儿子j加进队列
                fa[j][0] = t;//j往上跳2^0步后就是父节点t
                for(int k=1;k<=15;k++)
                //k的最大范围取决于题目的节点编号，2^15=32768
                    fa[j][k] = fa[fa[j][k-1]][k-1];//倍增递推
            }
        }
    }
}
int lca(int a, int b)//倍增法
{
    if (depth[a] < depth[b])
        swap(a, b); //为方便处理，a在下面，b在上面
        
    //下面的a往上跳到b同一层
    for (int k = 15; k >= 0; k -- )
        if (depth[fa[a][k]] >= depth[b])
            a = fa[a][k];
        //当a第一次跳完2^k在b下面，就进入该点，继续跳，直至同一层
        //二进制拼凑法，总会跳到同一层的    
    
    if (a == b) return a;//如果刚好a跳到了点b，b就是lca
    
    //a,b同层但不同节点
    for (int k = 15; k >= 0; k -- )
        if (fa[a][k] != fa[b][k])
        {
            a = fa[a][k];
            b = fa[b][k];
        }
    //循环结束，到达lca下一层，lca(a,b) = 再往上跳1步即可
    return fa[a][0];
}
void solve()
{
    cin>>n>>m;
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        cin>>a>>b>>c;
        e[a].pb({b,c});
        e[b].pb({a,c});
        
    }
    bfs(1);
    for(int i=0;i<m;i++)
    {
        cin>>x[i];
    }
    ll ans=0;
    for(int i=0;i<m-1;i++)
    {
        int y=lca(x[i],x[i+1]);
        ans += dist[x[i]]+dist[x[i+1]]-2*dist[y];
    }
}
```

**②tarjan**

问题：给出 n 个点的一棵树，多次询问两点之间的最短距离。

Tarjan算法则是一种离线算法，通过并查集优化，处理时间为O(n)，查询时间O(1)。

```c++
const int N = 10010, M = N * 2;
vector<vector<PII>> e(M+1);
vector<vector<PII>> query(N+1);
int n,m,dist[N],st[N],p[N],res[M];//dist为根节点距离

// query[i],i存查询的第一个点，first存查询的另外一个点，second存查询编号（即第几组查询）
void queries()
{
    for (int i = 0; i < m; i ++ )
    {
        int a, b;
        cin>>a>>b;
        if (a != b)
        {
            query[a].push_back({b, i});
            query[b].push_back({a, i});
        }
    }
}
//并查集初始化，查找父节点
void init(int n)
{
    for (int i = 1; i <= n; i ++ ) 
        p[i] = i;
}
int find(int x)
{
    if (p[x] != x) 
        p[x] = find(p[x]);
    return p[x];
}
//搜索,记录根节点的距离
void dfs(int u, int fa)
{
    for(auto [j,w]:e[u])
    {
        if (j == fa)
            continue;
        dist[j] = dist[u] + w;
        dfs(j,u);
    }
}
void tarjan(int u)
{
    st[u] = 1;//当前正在搜索的点标记为1
    for(auto [j,w]:e[u])//枚举u的儿子们
    {
        if (!st[j])//若儿子j没搜索过
        {
            tarjan(j);//递归儿子j
            p[j] = u;//回溯后把儿子j合并到父节点u
        }
    }
    for (auto item : query[u])// 对于当前点u 搜索所有u的查询
    {
        int y = item.ff, id = item.ss;
        if (st[y] == 2)//如果查询的这个点是已经搜索过且回溯过
        {
            int anc = find(y);//第id组查询的结果
            res[id] = dist[u] + dist[y] - dist[anc] * 2;
        }
    }
    st[u] = 2;//点u已经搜索完且要回溯了 就标记为2 
}
void solve()
{
	cin>>n>>m;
    for (int i = 0; i < n - 1; i ++ )
    {
        int a, b, c;
        cin>>a>>b>>c;
        e[a].pb({b,c});
        e[b].pb({a,c});
        
    }
    queries();
	init(n);
    dfs(1, -1);
    tarjan(1);
    for (int i = 0; i < m; i ++ ) 
        cout<<res[i]<<"\n";
}
```



# 好用的库和函数

## 1.全排列函数

```c++
int a[4]={1,2,3,4};
sort(a,a+4);
do{
	for(int i=0;i<4;i++)
        cout<<a[i]<<" ";
	cout<<endl;
}while(next_permutation(a,a+4));
```

## 2.对数函数

```c++
//log 函数，以 e 为底的对数
double log(double x);
float log(float x);
long double log(long double x);
//log10 函数，以 10 为底的对数
double log10(double x);
float log10(float x);
long double log10(long double x);
//log2 函数，以 2 为底的对数
double log2(double x);
float log2(float x);
long double log2(long double x);
```

## 3.排序函数和lambda表达式

```c++
//归并排序，稳定性好，调用compare次数为nlogn，时间复杂度通常为 O(nlogn)
stable_sort(v.begin(),v.end(),compare);
stable_sort(v.begin(),v.end(),[&](int a, int b) {
    return cache[a][b];
});
//lambda 表达式是一种匿名函数，它允许你在代码中直接定义一个简单的函数，而不需要给它起一个名字
//[捕获列表](参数列表) -> 返回类型 { 函数体 }
/*
捕获列表决定了 lambda 表达式可以访问哪些外部变量。常见的捕获方式包括：
[=]：按值捕获所有外部变量（创建副本）。
[&]：按引用捕获所有外部变量（直接使用外部变量）。
[var]：按值捕获指定变量。
[&var]：按引用捕获指定变量。
[]：不捕获任何变量。
*/
int x = 10;
int y = 20;
// 按值捕获 x，按引用捕获 y
auto lambda = [x, &y]() {
    std::cout << "x: " << x << std::endl; // x 是副本，值为 10
    std::cout << "y: " << y << std::endl; // y 是引用，值为 20
    y = 30; // 修改 y 的值
};
```

## 4.最值函数

```c++
int ma=*max_element(dp.begin(),dp.end());
int mi=*min_element(dp.begin(),dp.end());
```

## 5.总和函数

```c++
int sum = accumulate(v.begin() , v.end() , 0);
```

## 6.向上取整函数

```c++
int shu=ceil((l*1.0)/r);//2.2->3.0
```



# 常见的知识和技巧

## 1.进制转换

|    十进制转M进制     | **除M倒取余数法** | **eg:<br/>10<br/>10 ÷ 2 = 5 余 0<br/>5 ÷ 2 = 2 余 1<br/>2 ÷ 2 = 1 余 0<br/>1 ÷ 2 = 0 余 1<br/>余数倒序排列：1010₂<br/>255<br/>255 ÷ 16 = 15 余 15 → F<br/>15 ÷ 16 = 0 余 15 → F<br/>余数倒序排列：FF₁₆** |
| :------------------: | ----------------- | ------------------------------------------------------------ |
|  **M进制转十进制**   | **按权展开**      | **FF₁₆<br/>F×16¹ + F×16⁰ = 15×16 + 15×1 = 240 + 15 = 255₁₀** |
| **二进制转十六进制** | **分组法**        | **1010 1100 → A C → AC₁₆**                                   |
| **十六进制转二进制** | **直译法**        | **A → 1010<br/>C → 1100<br/>结果：10101100₂**                |
|  **二进制转八进制**  | **分组法**        | **10 101 100 → 2 5 4 → 254₈**                                |
|  **八进制转二进制**  | **直译法**        | **2 → 010<br/>5 → 101<br/>4 → 100<br/>结果：010101100₂**     |

## 2.二叉树的遍历

先序遍历：先根节点后左右子树

中序遍历：先左子树，再根节点，最后右子树。

后序遍历：先左右子树后根节点。

```c++
//①中序+后序，求先序。（大写字母表示，长度 ≤ 8）。
//后续最后一个点为根节点，先根遍历便是先输出根节点。tree(中，后)
void tree(string x,string y)
{
    int len1=x.size();
    int len2=y.size();
    if(len2>0)
    {
        cout<<y[len2-1];
        int p=x.find(y[len2-1]);
        tree(x.substr(0,p),y.substr(0,p));
        tree(x.substr(p+1),y.substr(p,len2-p-1));
    }
}
tree(s1,s2);
//②先序+后序，求中序。（数字表示）
//不能唯一确定树，但题目说有一个儿子必定是左儿子

```

## 3.离散化

区间和：

假定有一个无限长的数轴，数轴上每个坐标上的数都是 0。

现在，我们首先进行 n 次操作，每次操作将某一位置 x 上的数加 c。

接下来，进行 m 次询问，求出在区间 [l,r]之间的所有数的和。

思路：映射+二分

```c++
void solve()
{
	int n,m;
	cin >> n >> m;
	set<int> d;
	vector<int> dian;
	vector<PII> add;
	vector<PII> query;
	for(int i=0;i<n;i++)
	{
		int x,c;
		cin >> x >> c;
		add.pb({x,c});
		d.insert(x);
	}
	for(int i=0;i<m;i++)
	{
		int l,r;
		cin >> l >> r;
		query.pb({l,r});
		d.insert(l);
		d.insert(r);
	}
	for(auto j:d)
		dian.pb(j);
	sort(dian.begin(),dian.end());
	vector<int> zhi(dian.size()+10,0);
	for(int i=0;i<n;i++)
	{
		int index=lower_bound(dian.begin(),dian.end(),add[i].ff)-dian.begin();
		zhi[index+1] += add[i].ss;
	}
	vector<int> pre(zhi.size()+10,0);
	for(int i=1;i<zhi.size();i++)
	{
		pre[i]=pre[i-1]+zhi[i];
	}
	for(int i=0;i<m;i++)
	{
		int index=lower_bound(dian.begin(),dian.end(),query[i].ff)-dian.begin();
		int index2=lower_bound(dian.begin(),dian.end(),query[i].ss)-dian.begin();
		cout<<pre[index2+1]-pre[index]<<"\n";
	}
}
```

## 4.顺时针螺旋

[D. I Love 1543](https://codeforces.com/contest/2036/problem/D)

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
signed main()
{
	int t;
	cin>>t;
	while(t--)
	{
		int n,m;
		cin>>n>>m;
		int a[n+10][m+10];
		for(int i=0;i<n+10;i++)
		{
			for(int j=0;j<m+10;j++)
			{
				a[i][j]=-1;
			}
		}
		int b[n+10][m+10];
		getchar();
		for(int i=0;i<n;i++)
		{
			for(int j=0;j<m;j++)
			{
				char ch;
				cin>>ch;
				a[i][j]=ch-'0';
				b[i][j]=1;
			}
			getchar();
		}
		int N=0,s=n*m;
		int i=0,j=0;
		int res=0;
		while(N<s)
		{
			vector<int> ans;
			while(a[i][j]!=-1&&b[i][j])//右
			{
				b[i][j]=0;
				ans.push_back(a[i][j]);
				j++;
				N++;
			}
			i++;j--;
			while(a[i][j]!=-1&&b[i][j])//下
			{
				b[i][j]=0;
				ans.push_back(a[i][j]);
				i++;
				N++;
			}
			i--;j--;
			while(a[i][j]!=-1&&b[i][j])//左
			{
				b[i][j]=0;
				ans.push_back(a[i][j]);
				j--;
				N++;
			}
			i--;j++;
			while(a[i][j]!=-1&&b[i][j])//上
			{
				b[i][j]=0;
				ans.push_back(a[i][j]);
				i--;
				N++;
			}
			i++;j++;
			int mod=2*(n+m)-4;
			for(int g=0;g<mod;g++)
			{
				if(ans[g]==1 && ans[(g+1)%mod]==5 && ans[(g+2)%mod]==4 && ans[(g+3)%mod]==3)
				{
					res++;
				}
			}
			n-=2;
			m-=2;
		}
		cout<<res<<endl;
	}
} 
```

4.知1定1，常见枚举

