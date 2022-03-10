## 55.挑7！

字符串很慢，千万别用字符串写题！！

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            System.out.println(find7(n));
        }
    }
    
    private static int find7(int n){
        if(n < 7) return 0;
        int count = 0;
        for(int i = 7;i <= n;++i){
            if(i % 7 == 0 || contains7(i)){
                count++;
            }
        }
        return count;
    }
    
    private static boolean contains7(int n){
        while(n > 0){
            if(n % 10 == 7) return true;
            n /= 10;
        }
        return false;
    }
}
```

## 56.完全数计算！

方法确实是一个个的算，但是我中途有些东西没搞对，导致一直得不到最终结果

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            System.out.println(getPerfect(n));
        }
    }
    
    private static int getPerfect(int n){
        if(n < 6) return 0;
        int count = 0;
        for(int i = 6;i <= n;++i){
            int sum = 0;
            for(int j = 1;j <= i/2;++j){
                if(i % j == 0) sum += j;
            }
            if(sum == i) count++;
        }
        return count;
    }
}
```

## 57.高精度整数加法√

其实这个题，甚至比链表那个要简单一丢丢

在看了排行榜上的解法之后，有的人考虑了减法的情况，但是题干没有要求所以我就没有做了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str1 = null;
        while((str1 = br.readLine()) != null){
            String str2 = br.readLine();
            System.out.println(add2String(str1,str2));
        }
    }
    
    private static String add2String(String s1,String s2){
        if(s1.length() < s2.length()){
            String temp = s1;
            s1 = s2;
            s2 = temp;
        }
        if(s2.equals("0")) return s1;
        char[] c1 = s1.toCharArray();
        char[] c2 = s2.toCharArray();
        char[] result = new char[c1.length+1];
        int ten = 0;
        int i = 0, j = 0, k = 0;
        for(i = c1.length-1,j=c2.length-1,k = result.length-1;j >= 0;--i,--j,--k){
            int sum = (c1[i]-'0') + (c2[j] - '0') + ten;
            if(sum >= 10){
                sum -= 10;
                ten = 1;
            }else{
                ten = 0;
            }
            result[k] = (char)(sum + '0');
        }
        while(i >= 0){
            int sum = (c1[i] - '0') + ten;
            if(sum >= 10){
                sum -= 10;
                ten = 1;
            }else{
                ten = 0;
            }
            result[k] = (char)(sum + '0');
            i--;
            k--;
        }
        if(ten == 1){
            result[0] = '1';
            return new String(result);
        }else{
            result[0] = '0';
            String ans = new String(result);
            return ans.substring(1);
        }
//         StringBuilder sb = new StringBuilder();
//         result[0] = (char)(ten + '0');
//         if(ten == 1) k = 0;
//         else k = 1;
//         while(k < result.length) sb.append(result[k++]);
//         return sb.toString();
    }
}
```



## 58.输入n个整数，输出其中最小的k个√

虽然是个入门题，但是我也引发了一些思考，真正出题遇到了，真的可以直接用sort吗？

所以我想，要是真在笔试中遇到了这种用API一下能做出来的，就用API做一个，然后注释掉，用复杂的方法再来一次

所以我先用API直接`Arrays.sort`了一下，而后面用优先队列来完成

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int n = Integer.parseInt(strs[0]);
            int k = Integer.parseInt(strs[1]);
            strs = br.readLine().split(" ");
            int[] nums = new int[n];
            for(int i = 0;i < n;i++){
                nums[i] = Integer.parseInt(strs[i]);
            }
            int[] minK = getMinK(nums,k);
            for(int i : minK){
                System.out.print(i + " ");
            }
            System.out.println();
        }
    }
    
//     private static int[] getMinK(int[] nums, int k){
//         if(k >= nums.length) return nums;
//         Arrays.sort(nums);
//         int[] minK = new int[k];
//         for(int i = 0;i < k;i++){
//             minK[i] = nums[i];
//         }
//         return minK;
//     }
    private static int[] getMinK(int[] nums, int k){
        if(k >= nums.length) return nums;
        int[] minK = new int[k];
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i : nums){
            pq.add(i);
        }
        for(int i = 0;i < k;i++){
            minK[i] = pq.poll();
        }
        return minK;
    }
}
```

## 59.找出字符串第一个只出现一次的字符×

非常巧妙的方法，没次看到一个数，就看他第一次出现和最后一次出现的位置是不是一样的，如果是一样的，那么这个数就只出现了一次，某则就出现了很多次，当第一个找到位置一样的，那么这个数就是要找的那个数，打印出来就好了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            if(line.length() == 0) continue;
            boolean found = false;
            for(int i = 0;i < line.length();++i){
                char current = line.charAt(i);
                if(line.lastIndexOf(current) == line.indexOf(current)){
                    System.out.println(current);
                    found = true;
                    break;
                }
            }
            if(!found){
                System.out.println(-1);
            }
        }
        br.close();
    }
}
```

## 59.5关于BufferedReader的进一步思考

一般流在使用完后，调用一下close()

## 60.查找组成一个偶数最接近的两个素数！

我想的方法是，找到所有的，然后找最小的，但是可以优化。

首先我已经想到了，这样的两个素数，必然是一个大于n/2，一个小于n/2；但是我没想到，要找最小，那么从这里出发，找到的第一个满足的不就是差值最小的吗！？

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            if(n % 2 == 1){
                System.out.println("0" + "\n" + "0");
                continue;
            }
            System.out.println(getAns(n) + "\n" + (n-getAns(n)));
        }
    }
    
    private static int getAns(int n){
        int[] ans = new int[2];
        int i = n/2, j = n-i;
        while(!(isPrime(i)) || !isPrime(j)){
            i--;
            j++;
        }
        return i;
    }
    
    private static boolean isPrime(int num){
        for(int i = 2;i <= Math.sqrt(num);++i){
            if(num % i == 0) return false;
        }
        return true;
    }
}
```

## 61.放苹果

把m个苹果放在n个盘子里，允许盘子空着，有多少种放法。

01背包不一样的是，01背包除了放，还有重量和价值的限定

所以这个采用递归

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int m = Integer.parseInt(strs[0]);
            int n = Integer.parseInt(strs[1]);
            System.out.println(count(m,n));
        }
        br.close();
    }
    private static int count(int m,int n){
        if(m == 0 || n == 1) return 1;
        else if(n > m) return count(m,m);
        else return count(m,n-1) + count(m-n,n);
    }
}
```

## 62.查找整数二进制中1的个数

n & (n-1)：将末位1变成0

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int count = 0;
            while(n > 0){
                count++;
                n &= (n-1);
            }
            System.out.println(count);
        }
        br.close();
    }
}
```

## 72.百钱买百鸡！

虽然我做出来了，但是简单的数学我居然没有化简

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(buy());
        }
    }
    
    private static String buy(){
        StringBuilder sb = new StringBuilder();
        //5x+3y+z/3 = 100
        //x+y+z = 100
        //消除z后7x+4y = 100
        int x,y,z;
        for(x = 0;x<=14;++x){
            if((100-7*x) % 4 == 0){
                y = (100 - 7 * x) / 4;
                z = 100 - x - y;
                sb.append(x + " " + y + " " + z + "\n");
            }
        }
        return sb.toString();
    }
}
```

## 73.计算日期到天数的转换√

真希望到时候就是这种简单题啊啊啊啊啊啊

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int year = Integer.parseInt(strs[0]);
            int month = Integer.parseInt(strs[1]);
            int day = Integer.parseInt(strs[2]);
            System.out.println(getDay(year,month,day));
        }
    }
    
    private static int getDay(int year,int month,int day){
        int[] days = new int[]{31,28,31,30,31,30,31,31,30,31,30,31};
        int[] daysInRun = new int[]{31,29,31,30,31,30,31,31,30,31,30,31};
        int[] current = days;
        if(isRun(year)){
            current = daysInRun;
        }
        int count = 0;
        for(int i = 0;i < month - 1;++i){
            count += current[i];
        }
        return count + day;
    }
    
    private static boolean isRun(int year){
        if((year % 4== 0&& year % 100 != 0) || year%400 == 0){
            return true;
        }else{
            return false;
        }
    }
}
```

## 76.尼科彻斯定理√

其实还是找规律，找到规律了就简单了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int m = Integer.parseInt(line);
            //因为m个，所以当m是奇数的时候，m^2必定是中间那个
            //m为偶数的时候，m^2左右各m/2个数就是要找的数
            int[] ans = new int[m];
            if(m % 2 == 1){
                int m2 = m * m;
                for(int i = m/2;i >= 0;--i){
                    ans[i] = m2;
                    m2 -= 2;
                }
                m2 = m * m;
                for(int i = m/2;i < m;++i){
                    ans[i] = m2;
                    m2 += 2;
                }
            }else{
                int m2 = m * m + 1;
                for(int i = m/2;i < m;++i){
                    ans[i] = m2;
                    m2 += 2;
                }
                m2 = m * m -1;
                for(int i = m/2 - 1;i >= 0;--i){
                    ans[i] = m2;
                    m2 -= 2;
                }
            }
            StringBuilder sb = new StringBuilder();
            for(int i = 0;i < m-1;++i){
                sb.append(ans[i] + "+");
            }
            sb.append(ans[m-1]);
            System.out.println(sb);
        }
    }
}
```

## 80.整型数组合并√

又是一个利用API和不用API的区别

利用TreeSet

```java
import java.io.*;
import java.util.TreeSet;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n1 = Integer.parseInt(line);
            String[] strs1 = br.readLine().split(" ");
            int[] nums1 = string2IntArray(strs1);
            int n2 = Integer.parseInt(br.readLine());
            String[] strs2 = br.readLine().split(" ");
            int[] nums2 = string2IntArray(strs2);
            System.out.println(merge(nums1,nums2));
        }
    }
    
    private static int[] string2IntArray(String[] strs){
        int length = strs.length;
        int[] nums = new int[length];
        for(int i = 0;i < length;++i){
            nums[i] = Integer.parseInt(strs[i]);
        }
        return nums;
    }
    
    
    private static String merge(int[] nums1,int[] nums2){
        TreeSet<Integer> set = new TreeSet<>();
        for(int i = 0;i < nums1.length;++i){
            set.add(nums1[i]);
        }
        for(int i = 0;i < nums2.length;++i){
            set.add(nums2[i]);
        }
        StringBuilder sb = new StringBuilder();
        for(int num : set){
            sb.append(num);
        }
        return sb.toString();
    }
}
```

不用TreeSet

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Arrays;
  
public class Main {
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line;
        while ((line = br.readLine()) != null && line.length() > 0) {
            String[] strs = br.readLine().split(" ");
            int[] array1 = new int[strs.length];
            for (int i = 0; i < array1.length; ++i)
                array1[i] = Integer.parseInt(strs[i]);
  
            line = br.readLine();
            strs = br.readLine().split(" ");
            int[] array2 = new int[strs.length];
            for (int i = 0; i < array2.length; ++i)
                array2[i] = Integer.parseInt(strs[i]);
  
            System.out.println(combineBySort(array1, array2));
  
        }
    }
  
    static String combineBySort(int[] array1, int[] array2) {
        int[] outPut = new int[array1.length + array2.length];
        Arrays.sort(array1);
        Arrays.sort(array2);
  
        int M = array1.length, R = array2.length;
        int idx = 0, i = 0, j = 0;
        if (array1[i] > array2[j]) {
            outPut[idx++] = array2[j++];
        } else if (array1[i] < array2[j]) {
            outPut[idx++] = array1[i++];
        } else {
            outPut[idx++] = array1[i++];
            j++;
        }
        while (i < M && j < R) {
            if (array1[i] > array2[j]) {
                if (outPut[idx - 1] != array2[j])
                    outPut[idx++] = array2[j];
                ++j;
            } else if (array1[i] < array2[j]) {
                if (outPut[idx - 1] != array1[i])
                    outPut[idx++] = array1[i];
                ++i;
            } else {
                if (outPut[idx - 1] != array1[i])
                    outPut[idx++] = array1[i];
                ++i;
                ++j;
            }
        }
  
        if (i == M) {
            while( j < R){
                if (outPut[idx - 1] != array2[j])//去重
                    outPut[idx++] = array2[j];
                j++;
            }
                
        } else {
            for (; i < M; ++i)
                if (outPut[idx - 1] != array1[i])
                    outPut[idx++] = array1[i];
        }
  
        StringBuilder sb = new StringBuilder();
        for (i = 0; i < idx; ++i)
            sb.append(outPut[i]);
  
        return sb.toString();
    }
  
}
```

## 81.字符串字符匹配√

一看到26个小写字母，马上想到数组

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String s1 = null;
        while((s1 = br.readLine()) != null){
            String s2 = br.readLine();
            System.out.println(judge(s1,s2));
        }
    }
    
    private static boolean judge(String s1,String s2){
        if(s1.length() > s2.length()){
            String temp = s1;
            s1 = s2;
            s2 = temp;
        }
        int[] s2Freq = new int[26];
        char[] strs2 = s2.toCharArray();
        for(char c : strs2) s2Freq[c-'a']++;
        char[] strs1 = s1.toCharArray();
        for(char c : strs1){
            if(s2Freq[c-'a'] == 0){
                return false;
            }
        }
        return true;
    }
}
```

## 83.二维数组操作√

充分体现了华为的题，不难，但是烦

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int m = Integer.parseInt(strs[0]);
            int n = Integer.parseInt(strs[1]);
            if((m > 0 && m <= 9) && (n > 0 && n <= 9)){
                System.out.println(0);
            }else {
                System.out.println(-1);
            }
            strs = br.readLine().split(" ");
            int x1 = Integer.parseInt(strs[0]);
            int y1 = Integer.parseInt(strs[1]);
            int x2 = Integer.parseInt(strs[2]);
            int y2 = Integer.parseInt(strs[3]);
            if((x1>=0 && x1 < m) && (y1 >=0 && y1 < n) &&
               (x2>=0 && x2 < m) && (y2 >=0 && y2 < n)){
                System.out.println(0);
            }else {
                System.out.println(-1);
            }
            int x = Integer.parseInt(br.readLine());
            if(x >= 0 && x < m && m+1 <=9){
                System.out.println(0);
            }else{
                System.out.println(-1);
            }
            int y = Integer.parseInt(br.readLine());
            if(y >= 0 && y < n && n+1 <= 9){
                System.out.println(0);
            }else {
                System.out.println(-1);
            }
            strs = br.readLine().split(" ");
            x = Integer.parseInt(strs[0]);
            y = Integer.parseInt(strs[1]);
            if((x>=0 && x < m) && (y >= 0 && y < n)){
                System.out.println(0);
            }else {
                System.out.println(-1);
            }
        }
    }
}
```

## 84.统计大写字母个数√

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(getUpperCase(line));
        }
    }
    
    private static int getUpperCase(String line){
        int count = 0;
        char[] chars = line.toCharArray();
        for(char c : chars){
            if(c >= 'A' && c <= 'Z'){
                count++;
            }
        }
        return count;
    }
}
```

## 85.最长回文子串!

这个方法倒是记得滚瓜烂熟了，但是这个方法居然是O(n)复杂度？

查询了力扣，这个方法是O(n^2)的时间复杂度，但是O(1)的空间复杂度

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int count = 0;
            char[] chars = line.toCharArray();
            for(int i = 0;i < chars.length;++i){
                count = Math.max(count,getLonggestHuiWen(chars,i,i));
                if(i > 0){
                    count = Math.max(count,getLonggestHuiWen(chars,i-1,i));
                }
            }
            System.out.println(count);
        }
    }
    
    private static int getLonggestHuiWen(char[] chars,int left,int right){
        while(left >= 0 && right < chars.length && chars[left] == chars[right]){
            left--;
            right++;
        }
        return right - left - 1;
    }
}
```

而时间和空间都是O(n)复杂度的是曼彻斯特算法，复习的时候看看这个方法

[曼彻斯特算法][https://leetcode-cn.com/problems/longest-palindromic-substring/solution/zui-chang-hui-wen-zi-chuan-by-leetcode-solution/]

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(longestPalindrome(line));
        }
    }
    
    private static int longestPalindrome(String s) {
        int start = 0, end = -1;
        StringBuffer t = new StringBuffer("#");
        for (int i = 0; i < s.length(); ++i) {
            t.append(s.charAt(i));
            t.append('#');
        }
        t.append('#');
        s = t.toString();

        List<Integer> arm_len = new ArrayList<Integer>();
        int right = -1, j = -1;
        for (int i = 0; i < s.length(); ++i) {
            int cur_arm_len;
            if (right >= i) {
                int i_sym = j * 2 - i;
                int min_arm_len = Math.min(arm_len.get(i_sym), right - i);
                cur_arm_len = expand(s, i - min_arm_len, i + min_arm_len);
            } else {
                cur_arm_len = expand(s, i, i);
            }
            arm_len.add(cur_arm_len);
            if (i + cur_arm_len > right) {
                j = i;
                right = i + cur_arm_len;
            }
            if (cur_arm_len * 2 + 1 > end - start) {
                start = i - cur_arm_len;
                end = i + cur_arm_len;
            }
        }

        StringBuffer ans = new StringBuffer();
        for (int i = start; i <= end; ++i) {
            if (s.charAt(i) != '#') {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString().length();
    }

    private static int expand(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            --left;
            ++right;
        }
        return (right - left - 2) / 2;
    }
}
```

## 86.求最大连续bit数×

乍一看，以为是很简单的题，实际上，因为要求时间复杂度O(logn)，空间复杂度O(1)，所以犯难了，看了解析恍然大户。

利用规则：n & 1，用来检验n的末位是不是1，以此来看连续的1的个数

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            System.out.println(longgest1(n));
        }
    }
    
    private static int longgest1(int n){
        int max = 0;
        int count = 0;
        while(n != 0){
            if((n & 1) == 1){
                count++;
                max = Math.max(count,max);
            }else{
                count = 0;
            }
            n >>= 1;
        }
        return max;
    }
}
```

## 87.密码强度等级√

恶心！

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String password = null;
        while((password = br.readLine()) != null){
            int score = getScore(password);
            System.out.println(score2String(score));
        }
        br.close();
    }
    
    private static int getScore(String password){
        char[] chars = password.toCharArray();
        int integerNum = 0,lowerNum = 0, upperNum = 0, signNum = 0;
        int score = 0;
        for(char c : chars){
            if(c >= '0' && c <= '9') integerNum++;
            else if(c >= 'a' && c <= 'z') lowerNum++;
            else if(c >= 'A' && c <= 'Z') upperNum++;
            else signNum++;
        }
        //1.长度
        int n = chars.length;
        if(n >= 8) score += 25;
        if(n >= 5 && n <= 7) score += 10;
        if(n >= 0 && n <= 4) score += 5;
        
        //2.字母
        if(lowerNum == 0 && upperNum == 0) score+=0;
        else if((lowerNum >0 && upperNum == 0)|| (lowerNum ==0 && upperNum > 0 )) score += 10;
        else score += 20;
        
        //3.数字
        if(integerNum > 1) score += 20;
        else if(integerNum == 1) score += 10;
        else if(integerNum == 0) score += 0;
        
        //4.符号
        if(signNum > 1) score += 25;
        else if(signNum == 1) score += 10;
        else if(signNum == 0) score += 0;
        
        //5.奖励
        if(integerNum >0 && lowerNum > 0 && upperNum > 0 && signNum > 0) score += 5;
        else if(integerNum > 0 && signNum > 0 && (lowerNum > 0 || upperNum > 0)) score += 3;
        else if(integerNum > 0 && signNum == 0 && (lowerNum > 0 || upperNum > 0)) score += 2;
        return score;
    }
    
    private static String score2String(int score){
        if(score >= 90) return "VERY_SECURE";
        if(score >= 80) return "SECURE";
        if(score >= 70) return "VERY_STRONG";
        if(score >= 60) return "STRONG";
        if(score >= 50) return "AVERAGE";
        if(score >= 25) return "WEAK";
        return "VERY_WEAK";
    }
}
```

## 91.走方格的方案数

基础的动态规划

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int n = Integer.parseInt(strs[0]);
            int m = Integer.parseInt(strs[1]);
            System.out.println(getWays(n,m));
        }
    }
    
    private static int getWays(int n,int m){
        int[][] dp = new int[n + 1][m + 1];
        for(int i = 0;i <= n;++i){
            dp[i][0] = 1;
        }
        for(int j = 0;j <= m;++j){
            dp[0][j] = 1;
        }
        for(int i = 1;i <= n;++i){
            for(int j = 1;j <= m;++j){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[n][m];
    }
}
```

## 94.记票统计√

很明显用Map，只是因为是需要顺序遍历，所以要用到LinkedHashMap

```java
import java.io.*;
import java.util.LinkedHashMap;
import java.util.Map;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            LinkedHashMap<String,Integer> map = new LinkedHashMap<>();
            int n = Integer.parseInt(line);
            String[] strs = br.readLine().split(" ");
            for(int i = 0;i < n;++i){
                map.put(strs[i],0);
            }
            map.put("Invalid",0);
            int votes = Integer.parseInt(br.readLine());
            strs = br.readLine().split(" ");
            for(int i = 0;i < votes;++i){
                if(map.containsKey(strs[i])){
                    map.put(strs[i],map.get(strs[i]) + 1);
                }else{
                    map.put("Invalid",map.get("Invalid") + 1);
                }
            }
            StringBuilder sb = new StringBuilder();
            for(Map.Entry<String,Integer> entry : map.entrySet()){
                sb.append(entry.getKey() + " : " + entry.getValue() + "\n");
            }
            System.out.println(sb.substring(0,sb.length()-1));
        }
    }
}
```

## 96.表示数字√

很简单的遍历

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            StringBuilder sb = new StringBuilder();
            char[] chars = line.toCharArray();
            for(int i = 0;i < chars.length;++i){
                char c = chars[i];
                if(!Character.isDigit(c)){
                    sb.append(c);
                    continue;
                }
                int end = i;
                while(end < chars.length && Character.isDigit(chars[end])){
                    end++;
                }
                sb.append("*" + line.substring(i,end) + "*");
                i = end-1;
            }
            System.out.println(sb);
        }
        br.close();
    }
}
```

## 97.记负均正√

这里需要注意的一点是如何使输出保留一位小数`String.format("%.1f",f)`其中1表示保留一位

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            String[] strs = br.readLine().split(" ");
            int negCount = 0;
            double posSum = 0;
            int posCount = 0;
            for(int i = 0;i < n;++i){
                int num = Integer.parseInt(strs[i]);
                if(num > 0){
                    posCount++;
                    posSum += num;
                }else if(num < 0){
                    negCount++;
                }
            }
            double posAva = (posSum/posCount);
            System.out.println(negCount + " " + String.format("%.1f",posAva));
        }
    }
}
```

## 101.输入整型数组和排序标识，对其元素按照升序或降序进行排序√

同理，用了API和没用API都搞了

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            String[] strs = br.readLine().split(" ");
            int[] nums = new int[n];
            for(int i = 0;i < n;++i){
                nums[i] = Integer.parseInt(strs[i]);
            }
            int order = Integer.parseInt(br.readLine());
            StringBuilder sb = new StringBuilder();
            if(order == 0){
                for(int i : ascOrder(nums)){
                    sb.append(i + " ");
                }
            }else{
                for(int i : dscOrder(nums)){
                    sb.append(i + " ");
                }
            }
            System.out.println(sb.toString());
        }
    }
    
    private static int[] ascOrder(int[] nums){
        int n = nums.length;
        int[] ordered = new int[n];
//         Arrays.sort(nums);
//         ordered = nums;
//         return ordered;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i : nums) pq.add(i);
        for(int i = 0;i < n;i++) ordered[i] = pq.poll();
        return ordered;
    }
    private static int[] dscOrder(int[] nums){
        int n = nums.length;
        int[] ordered = new int[n];
//         Arrays.sort(nums);
//         for(int i = 0;i < n;i++){
//             ordered[i] = nums[n-1-i];
//         }
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i : nums) pq.add(i);
        for(int i = 0;i < n;i++) ordered[n-1-i] = pq.poll();
        return ordered;
    }
}
```

