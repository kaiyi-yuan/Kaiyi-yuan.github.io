[toc]

# 动态规划

### 题目描述

给你一个整数数组 `nums` ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

#### 提示

```
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

解题思路：动态规划   分为max  min。

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int len=nums.size();
        int max1=INT_MIN,imax=1,imin=1;
        for(int i=0;i<len;i++){
            if(nums[i]<0){
                int temp=imax;
                imax=imin;
                imin=temp;
            }
            imax=max(imax*nums[i],nums[i]);
            imin=min(imin*nums[i],nums[i]);
            max1=max(imax,max1);
        }
        return max1;
    }
};
```



## 题目198. 打家劫舍（leetcode）

### 题目描述

```
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。
```

#### 示例

```
输入: [1,2,3,1]
输出: 4
解释: 偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

#### 解题思路

利用动态规划求出前面房间金额来判断是否偷当前房间。

#### 代码

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int len=nums.size();
        if(len==0) return 0;
        if(len==1) return nums[0];
        vector<int> dp=vector<int>(len,0); 
        dp[0]=nums[0];
        dp[1]=max(nums[1],nums[0]);
        for(int i=2;i<len;i++){
            dp[i]+=max(dp[i-1],dp[i-2]+nums[i]);
        }
        return dp[len-1];
    }
};
```

# 二分法

### 二分法经典模板

```c++
while(left<=right){  //**条件
    mid=(left+right)/2;
    if(mid>result){
    	right=mid-1;    
    }
    else if(mid<result){
        left=mid+1;
	}
    else{				//相等
        break;
    }
}
```



### 题目描述

传送带上的包裹必须在 D 天内从一个港口运送到另一个港口。

传送带上的第 i 个包裹的重量为 weights[i]。每一天，我们都会按给出重量的顺序往传送带上装载包裹。我们装载的重量不会超过船的最大运载重量。

返回能在 D 天内将传送带上的所有包裹送达的船的最低运载能力。



##### 示例

```输入：weights = [1,2,3,4,5,6,7,8,9,10], D = 5
输出：15
解释：
船舶最低载重 15 就能够在 5 天内送达所有包裹，如下所示：
第 1 天：1, 2, 3, 4, 5
第 2 天：6, 7
第 3 天：8
第 4 天：9
第 5 天：10
请注意，货物必须按照给定的顺序装运，因此使用载重能力为 14 的船舶并将包装分成 (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) 是不允许的
```

##### 本题代码

```c++
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int D) {
        int mid=0,sum=0,max=0;
        vector<int> v;
        int len=weights.size();
        for(int i=0;i<len;i++){
            sum+=weights[i];
            if(max<weights[i]){
                max=weights[i];
            }
        }
        int low=max,high=sum;
        int days=0,temp=0;
        while(low<=high){
            days=0;
            temp=0;
            mid=(low+high)/2;
            for(int i=0;i<len;i++){//本题关键计算当前需要天数
                temp+=weights[i];
                if(temp>mid){
                    days++;
                    temp=weights[i];
                }
            }
            days++;
            if(days>D){
                low=mid+1;
            }
            else if(days<=D){
                high=mid-1;
                v.push_back(mid);
            }
        }
        return *min_element(v.begin(),v.end());
    }
};
```

# 递归调用

递归需要找到的条件有：递归边界，递归条件。

### 题目描述

给定一个非空字符串 `s`，**最多**删除一个字符。判断是否能成为回文字符串。

##### 示例

```
输入: "aba"
输出: True
```

##### 代码

```c++
class Solution {
public:
    bool validPalindrome(string s) {
        return validPalindrome(s, 0, s.size()-1, 0);
    }

    bool validPalindrome(string& s, int left, int right, int count) {
        if (count > 1) return false;   //边界
        while (left < right) {         //条件
            if (s[left] == s[right]) {
                left ++, right --;
            } else {
                return validPalindrome(s, left+1, right, count+1) || 
                       validPalindrome(s, left, right-1, count+1);
            }
        }
        return true;
    }
};
```
#双指针解题
###题目描述
给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的连续子数组，并返回其长度。如果不存在符合条件的连续子数组，返回 0。
####示例
```
输入: s = 7, nums = [2,3,1,2,4,3]
输出: 2
解释: 子数组 [4,3] 是该条件下的长度最小的连续子数组。
```
####思路:

通过两个指针指向左(x)和右(y)，先移动右端。

####代码
```c++
class Solution {
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        int len=INT_MAX;
        int x=0,y=x;
        int sum=0;
        bool flag=true;
        if(nums.size()==0){
            return 0;
        }
        while(y<=nums.size()-1){
            sum+=nums[y];
            if(sum<s){
                y++;
            }
            else{
                flag=false;
                if(len>y-x+1){
                    len=y-x+1;
                }
                sum=0;
                x++;
                y=x;
            }
        }
        if(flag){
            return 0;
        }
        return len;
    }
};

```
#利用栈解题
##题目：394. 字符串解码（leetcode）
###题目描述
```
给定一个经过编码的字符串，返回它解码后的字符串。
编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。
你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。
此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。
```
####示例
```
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```
####解题思路
创建两个栈，分别存放数字，字符串,设置str字符串来记录当前字符串。遍历字符串如果是数字需要先计算当前准确数字大小，如果是字符用str+=s[i]更新当前字符串,如果是'['需要将当前数字和str压入相应的栈。如果是"]"那么就需要更新str将[]中的字符解放出来。
####代码
```c++
class Solution {
public:
    string decodeString(string s) {
        int len=s.size();
        if(len==0) return s;
        string str="";
        stack<string> sss;//创建字符串栈
        stack<int> n;//创建数字栈
        int num=0;
        for(int i=0;i<len;i++){
            if(isalpha(s[i])){//是字符那么更新当前字符串
                str+=s[i];
            }
            else if(isdigit(s[i])){//计算准确的数字
                num=num*10+s[i]-'0';
            }
            else if(s[i]=='['){//遇到[说明一个子串的开始
                sss.push(str);//更新
                n.push(num);
                num=0;
                str="";
            }
            else if(s[i]==']'){//一个子串的结束   将子串解放出来
                for(int i=0;i<n.top();i++){
                    sss.top()+=str;
                }
                n.pop();
                str=sss.top();
                sss.pop();
            }
        }
        return str;
    }
};
```

# 用前缀和加哈希表解题

## 题目： 974. 和可被 K 整除的子数组（leetcode）

### 题目描述

给定一个整数数组 `A`，返回其中元素之和可被 `K` 整除的（连续、非空）子数组的数目。

#### 示例

```
输入：A = [4,5,0,-2,-3,1], K = 5
输出：7
解释：
有 7 个子数组满足其元素之和可被 K = 5 整除：
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
```

#### 解题思路

看到子串的和应该想起前缀和的方式来简化算法，presum(前缀和),sum(当前和) ,mod(前面的余数)  如果有sum%k==mod那么说明这个子串肯定能被k整除,利用哈希表来记录每个mod出现的次数。

```
补充：前缀和 为负数 的情况
举例：K = 4，求得一个前缀和为 -1 ， -1 % K = -1 ，3 % K = 3
看似模的结果不相等，一个为 -1 一个为 3 ，但它们应该记到一组
因为它们前缀和之差：3 - (-1) 为 4 。 4 % K = 0
所以 mod K 的结果 -1 ，要加上 K ，转成正数的 3
```

#### 代码

```c++
class Solution {
public:
    int subarraysDivByK(vector<int>& A, int K) {
        int len=A.size();
        if(len==0) return 0;
        int count=0,sum=0;
        map<int,int> m;
        m[0]=1;
        for(int i=0;i<len;i++){
            sum+=A[i];
            int mod=sum%K<0?sum%K+K:sum%K;//将负数转换成正数
            count+=m[mod];
            m[mod]++;
        }
        return count;
    }
};
```







