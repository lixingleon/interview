#### [393. UTF-8 编码验证](https://leetcode-cn.com/problems/utf-8-validation/)

难度中等117

给定一个表示数据的整数数组 `data` ，返回它是否为有效的 **UTF-8** 编码。

**UTF-8** 中的一个字符可能的长度为 **1 到 4 字节**，遵循以下的规则：

1. 对于 **1 字节** 的字符，字节的第一位设为 0 ，后面 7 位为这个符号的 unicode 码。
2. 对于 **n 字节** 的字符 (n > 1)，第一个字节的前 n 位都设为1，第 n+1 位设为 0 ，后面字节的前两位一律设为 10 。剩下的没有提及的二进制位，全部为这个符号的 unicode 码。

这是 UTF-8 编码的工作方式：

```
   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

```

**注意：**输入是整数数组。只有每个整数的 **最低 8 个有效位** 用来存储数据。这意味着每个整数只表示 1 字节的数据。

 

**示例 1：**

```
输入：data = [197,130,1]
输出：true
解释：数据表示字节序列:11000101 10000010 00000001。
这是有效的 utf-8 编码，为一个 2 字节字符，跟着一个 1 字节字符。

```

**示例 2：**

```
输入：data = [235,140,4]
输出：false
解释：数据表示 8 位的序列: 11101011 10001100 00000100.
前 3 位都是 1 ，第 4 位为 0 表示它是一个 3 字节字符。
下一个字节是开头为 10 的延续字节，这是正确的。
但第二个延续字节不以 10 开头，所以是不符合规则的。

```

 

**提示:**

- `1 <= data.length <= 2 * 104`
- `0 <= data[i] <= 255`

通过次数

17,831

提交次数

43,051

```java
//不用位运算，转化成字符串处理问题，可以做但效率很低。
class Solution {
    public boolean validUtf8(int[] data) {
        int idx = 0;
        while(idx<data.length){
           int cnt = checkHead(data[idx]);
           if(cnt == 5){
               return false;
           }
           if(checkTail(cnt, data, idx)){
               idx+= cnt;
           }
           else{
               return false;
           }
        }
        return true;

    }
    //查看head所在的group有几个
    private int checkHead(int data){
        String s = String.format("%8s", Integer.toBinaryString(data)).replaceAll(" ", "0");
        char[] chars = s.toCharArray();
        int idx = 0;
        while(idx<chars.length && chars[idx] == '1'){
            idx++;
        }
        int cnt = idx;
        if(idx == 0){
            cnt++;
        }
        if(idx == 1 || idx>=5){
            return 5;
        }
        return cnt;
    }
    //10010001
    private boolean checkTail(int cnt, int[] data, int idx){
        if(cnt == 1){
            return true;
        }
        //优化：提前判断tail数量够不够
        if(idx+cnt-1>=data.length){
            return false;
        }
        for(int i = 1; i<cnt; i++){
            int cur = data[idx+i];
            String s = String.format("%8s", Integer.toBinaryString(cur)).replaceAll(" ", "0");
            String two = s.substring(0, 2);
            if(!two.equals("10")){
                return false;
            }
        }
        return true;
    }
}


//位运算
class Solution {
    public boolean validUtf8(int[] data) {
        int idx = 0;
        while(idx<data.length){
           int cnt = checkHead(data[idx]);
           if(cnt == -1){
               return false;
           }
           if(checkTail(cnt, data, idx)){
               idx+= cnt;
           }
           else{
               return false;
           }
        }
        return true;

    }
    //查看head所在的group有几个
    private int checkHead(int data){
        int idx = 7;
        while(((data>> idx) &1) == 1){
            idx--;
        }
        int cnt = 7-idx;
        if(cnt == 1 || cnt>=5){
            return -1;
        }
        if(cnt == 0){
            cnt++;
        }
        return cnt;
    }
    //10010001
    private boolean checkTail(int cnt, int[] data, int idx){
        if(cnt == 1){
            return true;
        }
        //优化：提前判断tail数量够不够
        if(idx+cnt-1>=data.length){
            return false;
        }
        for(int i = 1; i<cnt; i++){
            if(((data[idx+i]>>7 &1) == 1) && ((data[idx+i]>>6 &1) == 0)){
                continue;
            }
            return false;
        }
        return true;
    }
}
```



#### [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

难度中等582收藏分享切换为英文接收动态反馈

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

 

**示例 1：**

```
输入：nums = [4,1,4,6]
输出：[1,6] 或 [6,1]
```

**示例 2：**

```
输入：nums = [1,2,10,4,1,4,3,3]
输出：[2,10] 或 [10,2]
```

 

**限制：**

- `2 <= nums.length <= 10000`

 

通过次数143,009

提交次数206,336

```java
class Solution {
    public int[] singleNumbers(int[] nums) {
        // 假设x，y是只出现一次的两个数
        //1. 把nums的所有数异或一遍。最终结果result = x^y
        //2. 既然x，y不同，那result必然有一位是1，找到那一位。
        //3. 那一位是0的分为1组，是1的分为1组。如何判断那一位是0呢？让m的那一位为1，其余位为0，跟m做位与运算，等于0的那一位就是0。（注意，那一位是1的跟m位与运算后不一定为1，但那一位是0的跟m位与运算后一定是0）
        //x和y就被分在了两组里，每组元素都异或一遍，最终得到x和y
        //时间复杂度：两遍O(n)加一次遍历num1&num2的二进制位O(32) == O(1)
        //空间复杂度：O(1) 使用了c

        int result = 0;
        for(int num:nums){
            result ^=num;
        }
        int m = 1;
        //目的是找到x和y不同的那一位
        while((m&result) == 0) m<<=1;

        int x = 0, y = 0;
        for(int num:nums){
            if((num&m) == 0) x^=num;
            else y^=num;
        }
        return new int[]{x,y};
    }
}
```

