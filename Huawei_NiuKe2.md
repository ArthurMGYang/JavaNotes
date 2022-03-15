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

## 63.DNA序列√

其实也不难，就是麻烦

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String gene = null;
        while((gene = br.readLine()) != null){
            int length = Integer.parseInt(br.readLine());
            int start = 0,end = start + length;
            double maxGC = 0;
            String gc = "";
            while(end <= gene.length()){
                String current = gene.substring(start,end);
                double gcRatio = getGCRatio(current);
                if(gcRatio > maxGC){
                    maxGC = gcRatio;
                    gc = current;
                }
                start++;
                end++;
            }
            System.out.println(gc);
        }
    }
    
    private static double getGCRatio(String str){
        char[] strs = str.toCharArray();
        double count = 0;
        for(char c : strs){
            if(c == 'G' || c == 'C') count++;
        }
        return count/str.length();
    }
}
```

## 64.MP3光标位置√

简单的模拟，不知道中途我错在哪儿了

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		// Scanner sc = new Scanner(System.in);
		String str = null;
		while ((str = br.readLine()) != null) {
			String str2 = br.readLine();
			int num = Integer.parseInt(str);

			char[] array = str2.toCharArray();
			int current = 1;
			int start = 1;
			for (char one : array) {
				if (num <= 4) {
					if(one == 'U') {
						if(current == 1) {
							current = num;
						}else {
							current--;
						}
						
					}
					else if(one == 'D') {
						if(current == num) {
							current = 1;
						}else {
							current++;
						}
						
					}
				}
				if(num > 4) {
					if(one == 'U') {
						if(current == 1) {
							current = num;
							start = num-3;
						}else if(current == start) {
							current--;
							start--;
						}else {
							current--;
						}
					}else if(one == 'D') {
						if(current == num) {
							current = 1;
							start = 1;
						}else if(current == start+3) {
							current++;
							start++;
						}else {
							current++;
						}
					}
				}
			}
			StringBuilder sb = new StringBuilder();
			for(int i=1;i<=4;i++) {
				if(num >=i) {
					sb.append(start + i -1).append(" ");
				}
			}
			System.out.println(sb.toString().trim());
			System.out.println(current);
		}
	}
}
```

## 65.两个字符串的最长公共子串！

虽然做出来了，但是复杂度高，且全是字符串运算，所以时间花费太多

动态规划

```java
import java.io.*;
import java.util.*;
public class Main{
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str;
        while((str = br.readLine())!=null){
            String ss = br.readLine();
            if(str.length()<ss.length()){
                System.out.println(res(str,ss));
            }else{
                System.out.println(res(ss,str));
            }
        }
    }
    public static String res(String s,String c){
        char[] ch1 = s.toCharArray();
        char[] ch2 = c.toCharArray();
        int[][] ins = new int[ch1.length + 1][ch2.length + 1];
        int max = 0;
        int start = 0;
        for (int i = 0; i < ch1.length; i++) {
            for (int j = 0; j < ch2.length; j++) {
                if(ch1[i]==ch2[j]){
                    ins[i+1][j+1] = ins[i][j]+1;
                    if(ins[i+1][j+1]>max){
                        max = ins[i+1][j+1];
                        start = i-max;
                    }
                }
            }
        }
        return s.substring(start+1,start+max+1);
    }
}
```

## 66.配置文件恢复√

典型的麻烦但是不难，我的速度不快，但是其实主要是全程采用了字符串操作，而排行榜上的操作，属实是有点穷举了，反正6条，穷举肯定更快，但是显然我这个具有普遍性

```java
import java.io.*;

public class Main{
    static String[][] structions = new String[][]{{"reset","","reset what"},
                                                  {"reset","board","board fault"},
                                                  {"board","add","where to add"},
                                                  {"board","delete","no board at all"},
                                                  {"reboot","backplane","impossible"},
                                                  {"backplane","abort","install first"},
                                                  {"he","he","unknown command"}};
    
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String struction = null;
        while((struction = br.readLine())  != null){
            String[] strs = struction.split(" ");
            int n = -1;
            if(strs.length == 1){
                n = getMatch(strs[0]);
            }else {
                n = getMatch(strs[0],strs[1]);
            }
            System.out.println(structions[n][2]);
        }
    }
    
    private static int getMatch(String str){
        int n = 6;
        for(int i = 0;i < 6;++i){
            if(structions[i][1].equals("") && structions[i][0].startsWith(str)){
                n = i;
                break;
            }
        }
        return n;
    }
    
    private static int getMatch(String str1,String str2){
        int n = 6;
        for(int i = 0;i < 6;++i){
            if(!structions[i][0].equals("") && !structions[i][1].equals("")){
                if(structions[i][0].startsWith(str1) && structions[i][1].startsWith(str2)){
                    if(n == 6){
                        n = i;
                    }else {
                        n = 6;
                        break;
                    }
                }
            }
        }
        return n;
    }
}
```

## 67.24点游戏算法！

dfs啊dfs啊！！！！

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int[] nums = new int[4];
            for(int i = 0;i < 4;++i){
                nums[i] = Integer.parseInt(strs[i]);
            }
            boolean[] visited = new boolean[4];
            System.out.println(dfs(nums,0,visited,0));
        }
        br.close();
    }
    
    private static boolean dfs(int[] nums,double sum,boolean[] visited,int k){
        if(sum == 24 && k == 4){
            return true;
        }
        for(int i = 0;i < 4;++i){
            if(!visited[i]){
                visited[i] = true;
                if(dfs(nums,sum+nums[i],visited,k+1) ||
                   dfs(nums,sum-nums[i],visited,k+1) ||
                   dfs(nums,sum*nums[i],visited,k+1) ||
                   dfs(nums,sum/nums[i],visited,k+1)){
                    return true;
                }
                visited[i] = false;
            }
        }
        return false;
    }
}
```

## 69.矩阵乘法√

可恶，要是最后我是这种考题就好了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br= new BufferedReader(new InputStreamReader(System.in));
        String xS = null;
        while((xS = br.readLine()) != null){
            int x = Integer.parseInt(xS);
            int y = Integer.parseInt(br.readLine());
            int z = Integer.parseInt(br.readLine());
            int[][] A = new int[x][y];
            int[][] B = new int[y][z];
            for(int i = 0;i < x;++i){
                String[] strs = br.readLine().split(" ");
                for(int j = 0;j < y;++j){
                    A[i][j] = Integer.parseInt(strs[j]);
                }
            }
            for(int i = 0;i < y;++i){
                String[] strs = br.readLine().split(" ");
                for(int j = 0;j < z;++j){
                    B[i][j] = Integer.parseInt(strs[j]);
                }
            }
            int[][] ans = AB(A,B);
            StringBuilder sb = new StringBuilder();
            for(int i = 0;i < x;++i){
                for(int j = 0;j < z;++j){
                    if(j!=z-1){
                        sb.append(ans[i][j] + " ");
                    }else{
                        sb.append(ans[i][j]);
                    }
                }
                if(i!=x-1) sb.append("\n");
            }
            System.out.println(sb);
        }
        br.close();
    }
    
    private static int[][] AB(int[][] A,int[][] B){
        int x = A.length;
        int y = B.length;
        int z = B[0].length;
        int[][] ans = new int[x][z];
        for(int i = 0;i < x;++i){
            for(int j = 0;j < z;++j){
                int sum = 0;
                for(int k = 0;k < y;++k){
                    sum += (A[i][k] * B[k][j]);
                }
                ans[i][j] = sum;
            }
        }
        return ans;
    }
}
```

## 70.矩阵乘法计算量估算×

我已经开始为下周的考试而感到担心了

这个题为什么可以不用递归，首先右括号挨着肯定只有一次计算，其次，这个的算法是一样的，不用像运算那个题的加减乘除都来一遍

```java
import java.io.*;
import java.util.LinkedList;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int[][] juzheng = new int[n][2];
            for(int i = 0;i < n;++i){
                String[] strs = br.readLine().split(" ");
                juzheng[i][0] = Integer.parseInt(strs[0]);
                juzheng[i][1] = Integer.parseInt(strs[1]);
            }
            String expression = br.readLine();
            System.out.println(getCalCount(juzheng,expression));
        }
        br.close();
    }
    
    private static int getCalCount(int[][] juzheng,String expression){
        LinkedList<int[]> stack = new LinkedList<>();
        int count = 0;
        char[] chars = expression.toCharArray();
        for(int i = 0;i < chars.length;++i){
            char c = chars[i];
            if(c >= 'A' && c <= 'Z'){
                stack.addLast(juzheng[c-'A']);
            }else if(c == '('){
                continue;
            }else if(c == ')' || i == chars.length-1){
                int[] juzheng2 = stack.pollLast();
                int[] juzheng1 = stack.pollLast();
                count += juzheng1[0] * juzheng1[1] * juzheng2[1];
                int[] result = new int[]{juzheng1[0],juzheng2[1]};
                stack.addLast(result);
            }
        }
        return count;
    }
}
```

## 71.字符串通配符×

什么勾八题啊？

动态规划

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String s1 = null;
        while((s1 = br.readLine()) != null){
            String s2 = br.readLine();
            char[] rule = s1.toCharArray();
            char[] str = s2.toCharArray();
            System.out.println(isMatch(str,rule));
        }
        br.close();
    }
    
    private static boolean isMatch(char[] str,char[] rule){
        int n1 = str.length;
        int n2 = rule.length;
        boolean[][] dp = new boolean[n1+1][n2+1];
        dp[0][0] = true;
        for(int i = 0;i <= n1;++i){
            for(int j = 1;j <= n2;++j){
                if(rule[j-1] == '*'){
                    dp[i][j] = dp[i][j-1];
                    if(i == 0) break;
                    for(int k = i-1;k>=0;--k){
                        if(Character.isLetter(str[k]) || Character.isDigit(str[k])){
                            dp[i][j] = dp[i][j] || dp[k][j];
                        }else{
                            dp[i][j] = dp[i][j] || dp[k][j-1];
                            break;
                        }
                    }
                }else{
                    if(i == 0) break;
                    if(twoMatch(str[i-1],rule[j-1])){
                        dp[i][j] = dp[i-1][j-1];
                    }
                }
            }
        }
        return dp[n1][n2];
    }
    
    private static boolean twoMatch(char str,char rule){
        return str == rule || 
            (Character.isLetter(str) && Character.isLetter(rule) && Math.abs(str-rule) == Math.abs('A'-'a')) ||
            (rule == '?' && (Character.isLetter(str) || Character.isDigit(str)));
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

## 74.参数分析√

虽然做出来了，但是细节还是差了点，调试了几次才弄对，所以细节决定成败啊

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int start = 0,end = 0;
            ArrayList<String> list = new ArrayList<>();
            char[] chars = line.toCharArray();
            while(end < chars.length){
                if(chars[end] == ' '){
                    list.add(line.substring(start,end));
                    start = end+1;
                    end = start;
                }else if(chars[end] == '"'){
                    start = end+1;
                    end++;
                    while(end < chars.length && chars[end] != '"'){
                        end++;
                    }
                    list.add(line.substring(start,end));
                    end += 2;
                    start = end;
                }else if(end == chars.length-1){
                    list.add(line.substring(start,end+1));
                    break;
                }else{
                    end++;
                }
            }
            System.out.println(list.size());
            for(String s : list){
                System.out.println(s);
            }
        }
    }
}
```

## 75.公共子串计算！

因为数据量小，所以时间复杂度可以达到O(n^3)，但是即使是这样，我的方法不是最简的，是一个即使不用做笔记也能想到的，所以笔记采取了排行榜上更牛逼的解法

```java
import java.io.*;

public class Main{
    public static void main(String[] args)throws Exception{
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String str1 = "";
        String str2 = "";

        while((str1 = bf.readLine())!= null && (str2 = bf.readLine())!= null){
            int max = 0;
            char[] cha1 = str1.toCharArray();
            char[] cha2 = str2.toCharArray();
            for(int i = 0; i < str1.length(); i++){
                for(int j = 0; j < str2.length(); j++){
                    int t1 = i;
                    int t2 = j;
                    int count = 0;
                    while(cha1[t1] == cha2[t2]){
                        t1++;
                        t2++;
                        count++;
                        max = Math.max(count,max);
                        if(t1 == cha1.length || t2 == cha2.length) break;
                    }
                }
            }
            System.out.println(max);
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

## 77.火车进站×⭐️

傻逼吧我是个，dfs做过多少遍了，还是不会做，记得最后把所有的dfs全部找来看一遍

```java
import java.io.*;
import java.util.LinkedList;
import java.util.ArrayList;
import java.util.Collections;

public class Main{
    private static LinkedList<Integer> queue = new LinkedList<>();
    private static LinkedList<Integer> stack = new LinkedList<>();
    private static ArrayList<String> ans = new ArrayList<>();
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            String[] strs = br.readLine().split(" ");
            for(int i = 0;i < n;++i){
                queue.addLast(Integer.parseInt(strs[i]));
            }
            dfs("");
            Collections.sort(ans);
            StringBuilder sb = new StringBuilder();
            for(String str : ans){
                sb.append(str + "\n");
            }
            System.out.println(sb.toString().substring(0,sb.length()-1));
        }
        br.close();
    }
    
    private static void dfs(String outQueue){
        if(queue.isEmpty() && stack.isEmpty()){
            ans.add(outQueue.trim());
            return;
        }
        //模拟火车出站，火车进站以后，可以选择出或者不出
        //第一次弹出，表示火车出站了，然后开始进行下一次递归
        //然后放回去，表示没出站的情况
        if(!stack.isEmpty()){
            int num = stack.pollLast();
            String tempOutQueue = outQueue + num + " ";
            dfs(tempOutQueue);
            stack.addLast(num);
        }
        //模拟火车进站的情况
        //如果还有火车没进站，那么可以选择让其进站
        if(!queue.isEmpty()){
            int current = queue.pollFirst();
            stack.addLast(current);
            dfs(outQueue);
            stack.pollLast();
            queue.addFirst(current);
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

## 82.真分数分解为埃及分数×

属实是吃了没文化的亏，完全不会数学相关的，求求了，别出数学相关的问题

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String num = null;
        while((num = br.readLine()) != null){
            String[] strs = num.split("\\/");
            int fenZi = Integer.parseInt(strs[0]);
            int fenMu = Integer.parseInt(strs[1]);
            System.out.println(getIG(fenZi,fenMu));
        }
        br.close();
    }
    
    private static String getIG(int a,int b){
        StringBuilder sb = new StringBuilder();
        if(a > b || a < 1 || b < 2) return "";
        while(a != 1){
            if(b%a == 0){
                b = b/a;
                a = 1;
                continue;
            }
            if(b % (a-1) == 0){
                sb.append("1/" + b/(a-1) + "+");
                a = 1;
            }else {
                int c = b/a + 1;
                a = a-b%a;
                b = b*c;
                sb.append("1/" + c + "+");
            }
        }
        sb.append("1/" + b);
        return sb.toString();
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

## 88.扑克牌大小√

同样的，注意细节啊！！！

```java
import java.io.*;
import java.util.HashMap;

public class Main{
    static HashMap<String,Integer> map = new HashMap<>();
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        map.put("3",0);
        map.put("4",1);
        map.put("5",2);
        map.put("6",3);
        map.put("7",4);
        map.put("8",5);
        map.put("9",6);
        map.put("10",7);
        map.put("J",8);
        map.put("Q",9);
        map.put("K",10);
        map.put("A",11);
        map.put("2",12);
        map.put("joker",13);
        map.put("JOKER",14);
        while((line = br.readLine()) != null){
            String[] cards = line.split("\\-");
            String[] cards1 = cards[0].split(" ");
            String[] cards2 = cards[1].split(" ");
            if(!isValidCompare(cards1,cards2)){
                System.out.println("ERROR");
                continue;
            }
            String[] win = null;
            boolean isWin = false;
            if(cardsType(cards1).equals("boom") && !cardsType(cards2).equals("boom")){
                win = cards1;
                isWin = true;
            }else if(!cardsType(cards1).equals("boom") && cardsType(cards2).equals("boom")){
                win = cards2;
                isWin = true;
            }
            if(!isWin){
                if(cardsType(cards1).equals("single") || cardsType(cards1).equals("pair") ||
                   cardsType(cards1).equals("triple") || cardsType(cards1).equals("ShunZi")){
                    win = singlePairTripleShunZiCompare(cards1,cards2);
                }else{
                    win = boomCompare(cards1,cards2);
                }
            }
            StringBuilder sb = new StringBuilder();
            for(String s : win){
                sb.append(s + " ");
            }
            System.out.println(sb.toString().substring(0,sb.length()-1));
        }
        br.close();
    }
    
    private static String cardsType(String[] cards){
        if(cards.length == 1) return "single";
        if(cards.length == 2 && cards[0].equals(cards[1])) return "pair";
        if(cards.length == 3 && cards[0].equals(cards[1]) && cards[0].equals(cards[2])) return "triple";
        if(cards.length == 4 || (cards.length == 2 && (cards[0].equals("joker") || cards[1].equals("joker"))))
            return "boom";
        if(cards.length == 5) return "ShunZi";
        return null;
    }
    
    private static boolean isValidCompare(String[] cards1,String[] cards2){
        if(cardsType(cards1).equals(cardsType(cards2))) return true;
        if(cardsType(cards1).equals("boom") || cardsType(cards2).equals("boom")) return true;
        return false;
    }
    
    private static String[] singlePairTripleShunZiCompare(String[] cards1,String[] cards2){
        int n1 = map.get(cards1[0]);
        int n2 = map.get(cards2[0]);
        return n1 > n2 ? cards1 : cards2;
    }
    
    private static String[] boomCompare(String[] cards1,String[] cards2){
        if(cards1[0].equals("joker") || cards1[1].equals("joker")) return cards1;
        if(cards2[0].equals("joker") || cards1[2].equals("joker")) return cards2;
        int n1 = map.get(cards1[0]);
        int n2 = map.get(cards2[0]);
        return n1 > n2 ? cards1 : cards2;
    }
}
```

## 89.24点运算×

第一次知道原来还真能用穷举法做题的啊，不过也是，扑克牌只有这么多，那么n^5，也算是常数级的运算

```java
import java.io.*;
import java.util.LinkedList;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] cards = line.split(" ");
            if(!isValidCards(cards)){
                System.out.println("ERROR");
                continue;
            }
            int num1 = card2Int(cards[0]);
            int num2 = card2Int(cards[1]);
            int num3 = card2Int(cards[2]);
            int num4 = card2Int(cards[3]);
            System.out.println(compute(num1,num2,num3,num4));
        }
        br.close();
    }
    
    private static boolean isValidCards(String[] cards){
        String base = "345678910JQKA2jokerJOKER";
        for(String card : cards){
            if(card.equals("joker") || card.equals("JOKER")) return false;
            if(!base.contains(card)) return false;
        }
        return true;
    }
    private static String compute(int num1,int num2,int num3,int num4){
        int[] nums = new int[]{num1,num2,num3,num4};
        String[][] symbols = symbol();
        StringBuilder sb = new StringBuilder();
        for(int a = 0;a < 4;++a){
            for(int b = 0;b < 4;++b){
                for(int c = 0;c < 4;++c){
                    for(int d = 0;d < 4;++d){
                        if((a!=b)&&(a!=c)&&(a!=d)&&(b!=c)&&(b!=d)&&(c!=d)){
                            for(String[] op : symbols){
                                int sum = calculate(nums[a],nums[b],op[0]);
                                sum = calculate(sum,nums[c],op[1]);
                                sum = calculate(sum,nums[d],op[2]);
                                if(sum == 24){
                                    sb.append(int2Card(nums[a])+op[0]+int2Card(nums[b])+op[1]+
                                              int2Card(nums[c])+op[2]+int2Card(nums[d]));
                                    return sb.toString();
                                }
                            }
                        }
                    }
                }
            }
        }
        return "NONE";
    }
    
    //穷举所有的运算符可能情况
    private static String[][] symbol(){
        //四个数运算，会有三个运算符，运算符
        String[] symbols = new String[]{"+","-","*","/"};
        String[][] symbol = new String[64][3];
        int p = 0;
        for(int i = 0;i < 4;i++){
            for(int j = 0;j < 4;j++){
                for(int k = 0;k < 4;k++){
                    symbol[p++] = new String[]{symbols[i],symbols[j],symbols[k]};
                }
            }
        }
        return symbol;
    }
    
    private static int calculate(int a,int b,String op){
        if(op.equals("+")) return a+b;
        if(op.equals("-")) return a-b;
        if(op.equals("*")) return a*b;
        if(op.equals("/")) return a/b;
        return 0;
    }
    
    private static int card2Int(String card){
        switch(card){
            case "J" : return 11;
            case "Q" : return 12;
            case "K" : return 13;
            case "A" : return 1;
            case "joker" : return -1;
            case "JOKER" : return -1;
        }
        return Integer.parseInt(card);
    }
    private static String int2Card(int num){
        switch(num){
            case 11 : return "J";
            case 12 : return "Q";
            case 13 : return "K";
            case 1 : return "A";
        }
        return num+"";
    }
}
```



## 90.合法IP√

恶心！需要考虑到的情况太多了！

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String ip = null;
        while((ip = br.readLine()) != null){
            if(isValid(ip)) System.out.println("YES");
            else System.out.println("NO");
        }
        br.close();
    }
    
    private static boolean isValid(String ip){
        String[] ips = ip.split("\\.");
        if(ips.length != 4) return false;
        for(String str : ips){
            if(str.length() == 0 || str.length() > 3) return false;
            if(str.length() > 1 && str.startsWith("0")) return false;
            char[] chars = str.toCharArray();
            for(char c : chars){
                if(c < '0' || c > '9') return false;
            }
            int n = Integer.parseInt(str);
            if(n > 255 || n < 0) return false;
        }
        return true;
    }
}
```



## 91.走方格的方案数×

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

## 92.在字符串中找出连续最长的数字串√

注意细节啊！

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(getLonggestInteger(line));
        }
        br.close();
    }
    
    private static String getLonggestInteger(String line){
        StringBuilder sb = new StringBuilder();
        int start = 0,end = 0, max = 0;
        char[] chars = line.toCharArray();
        while(end < chars.length){
            if(chars[end] >= '0' && chars[end] <= '9'){
                start = end;
                while(end < chars.length && Character.isDigit(chars[end])){
                    end++;
                }
                if(end - start > max){
                    max = end - start;
                    sb = new StringBuilder();
                    sb.append(line.substring(start,end));
                }else if(end - start == max){
                    sb.append(line.substring(start,end));
                }
            }
            end++;
        }
        sb.append("," + max);
        return sb.toString();
    }
}
```

## 93.数组分组×

dfs的题，预先的处理和思路，算是思考对了路线，但是最后还是没想出来

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int[] nums = new int[n];
            String[] strs = br.readLine().split(" ");
            for(int i = 0;i < n;++i){
                nums[i] = Integer.parseInt(strs[i]);
            }
            System.out.println(isSplitAble(nums));
        }
        br.close();
    }
    
    private static boolean isSplitAble(int[] nums){
        int sum3 = 0, sum5 = 0, sum = 0;
        ArrayList<Integer> list = new ArrayList<>();
        for(int n : nums){
            if(n % 5 == 0) sum5 += n;
            else if(n % 3 == 0) sum3 += n;
            else list.add(n);
            sum += n;
        }
        if(sum % 2 != 0) return false;
        int target = sum/2 - sum3;
        return dfs(list,0,target);
    }
    
    private static boolean dfs(ArrayList<Integer> list,int i,int target){
        if(i == list.size()) return target == 0;
        return dfs(list,i+1,target) || dfs(list,i+1,target-list.get(i));
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

## 95.人民币转换×

对于这一类题，现在基本上有了个方法，比如人民币是以一千为一个单位，也就是每4位一个循环，然后处理好这个循环，加个单位，再处理下一个循环

```java
import java.io.*;

public class Main{
    private static String[] ten = {"零","壹","贰","叁","肆","伍","陆","柒","捌","玖"};
    private static String[] power = {"万","亿"};
    private static String[] daiwei = {"元","角","分","整"};

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split("\\.");
            StringBuilder sb = new StringBuilder("人民币");
            if(strs[1].equals("00") || strs[1].equals("0")){
                sb.append(solveLeft(strs[0]) + "元整");
            }else if(strs[0].equals("0")){
                sb.append(solveRight(strs[1]));
            }else{
                sb.append(solveLeft(strs[0]) + "元" + solveRight(strs[1]));
            }
            System.out.println(sb);
        }
        br.close();
    }
    
    private static String solveRight(String str){
        StringBuilder sb = new StringBuilder();
        int jiao = Integer.parseInt(str.substring(0,1));
        int fen = Integer.parseInt(str.substring(1,2));
        if(jiao != 0){
            sb.append(ten[jiao] + "角");
        }
        if(fen != 0){
            sb.append(ten[fen] + "分");
        }
        return sb.toString();
    }
    
    private static String solveLeft(String str){
        StringBuilder sb = new StringBuilder();
        int pow = 0;
        int money = Integer.parseInt(str);
        while(money != 0){
            if(pow != 0){
                sb.append(power[pow-1]);
            }
            int current4 = money % 10000;
            int geWei = current4  % 10;
            int shiWei = (current4 / 10) % 10;
            int baiWei = (current4 / 100) % 10;
            int qianWei = (current4 / 1000) % 10;
            if(geWei != 0){
                sb.append(ten[geWei]);
            }
            
            if(shiWei != 0){
                sb.append("拾");
                if(shiWei != 1){
                    sb.append(ten[shiWei]);
                }
            }else{
                if(geWei != 0 && (current4 > 99 || money > 10000)){
                    sb.append(ten[0]);
                }
            }
            
            if(baiWei != 0){
                sb.append("佰");
                sb.append(ten[baiWei]);
            }else{
                if(shiWei != 0 && (current4 > 999 || money > 10000)){
                    sb.append(ten[0]);
                }
            }
            if(qianWei != 0){
                sb.append("仟");
                sb.append(ten[qianWei]);
            }else{
                if(baiWei != 0 && money > 10000){
                    sb.append(ten[0]);
                }
            }
            money /= 10000;
            pow++;
            if(pow > 2) pow = 1;
        }
        return sb.reverse().toString();
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

## 98.自动售货系统×

因为是个很复杂的题，但是本身过程看起来不是很复杂，所以我就没做了，反正不会出现原题

```java
import java.util.*;
import java.io.*;
public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br  = new BufferedReader(new InputStreamReader(System.in));
        String str;
        while((str=br.readLine())!=null){
            String[] arr = str.split(";");
            int[] price = {2,3,4,5,8,6};
            int[] amount = {1,2,5,10};
            int[] money = new int[4];
            int[] goods = new int[6];
            int sum = 0;
            for(String s:arr){
                if(s.startsWith("r ")){
                    System.out.println("S001:Initialization is successful");
                    String[] arr1 = s.split(" ");
                    String[] arr2 = arr1[1].split("-");
                    for(int i = 0;i < arr2.length;i++){
                        goods[i] = Integer.parseInt(arr2[i]);
                    }
                    arr2 = arr1[2].split("-");
                    for(int i = 0;i < arr2.length;i++){
                        money[i] = Integer.parseInt(arr2[i]);
                    }
                }else if(s.startsWith("p ")){
                    int input = Integer.parseInt(s.substring(2));
                    if(input!=1 && input!=2 && input!=5 && input!=10){
                        System.out.println("E002:Denomination error");
                        continue;
                    }
                    if((input==5 || input==10) && (money[0]+money[1]*2<input)){
                        System.out.println("E003:Change is not enough, pay fail");
                        continue;
                    }
                    boolean flag = true;
                    for(int item:goods){
                        if(item!=0){
                            flag = false;
                            break;
                        }
                    }
                    if(flag){
                        System.out.println("E005:All the goods sold out");
                    }else{
                        sum += input;
                        for(int i = 0;i < amount.length;i++){
                            if(amount[i] == input){
                                money[i]++;
                                break;
                            }
                        }
                        System.out.println("S002:Pay success,balance=" + sum);
                    }
                }else if(s.startsWith("b ")){
                    String good = s.substring(2);
                    if(!good.equals("A1") && !good.equals("A2") && !good.equals("A3") && !good.equals("A4") && !good.equals("A5") && !good.equals("A6")){
                        System.out.println("E006:Goods does not exist");
                        continue;
                    }
                    int name = Integer.parseInt(good.substring(1)) - 1;
                    if(goods[name] == 0){
                        System.out.println("E007:The goods sold out");
                        continue;
                    }
                    if(sum < price[name]){
                        System.out.println("E008:Lack of balance");
                    }else{
                        sum -= price[name];
                        goods[name]--;
                        System.out.println("S003:Buy success,balance=" + sum);
                    }
                }else if(s.equals("c")){
                    if(sum == 0){
                        System.out.println("E009:Work failure");
                    }else{
                        StringBuilder sb = new StringBuilder();
                        if(sum/10 >= money[3]){
                            sum -= 10*money[3];
                            sb.append("10 yuan coin number=").append(money[3]);
                            money[3] = 0;
                        }else{
                            sb.append("10 yuan coin number=").append(sum/10);
                            money[3] -= sum/10;
                            sum %= 10;
                        }
                        if(sum/5 >= money[2]){
                            sum -= 5*money[2];
                            sb.insert(0,"\n").insert(0,money[2]).insert(0,"5 yuan coin number=");
                            money[2] = 0;
                        }else{
                            sb.insert(0,"\n").insert(0,sum/5).insert(0,"5 yuan coin number=");
                            money[2] -= sum/5;
                            sum %= 5;
                        }
                        if(sum/2 >= money[1]){
                            sum -= 2*money[1];
                            sb.insert(0,"\n").insert(0,money[1]).insert(0,"2 yuan coin number=");
                            money[1] = 0;
                        }else{
                            sb.insert(0,"\n").insert(0,sum/2).insert(0,"2 yuan coin number=");
                            money[1] -= sum/2;
                            sum %= 2;
                        }
                        if(sum >= money[0]){
                            sum -= money[0];
                            sb.insert(0,"\n").insert(0,money[0]).insert(0,"1 yuan coin number=");
                            money[0] = 0;
                        }else{
                            sb.insert(0,"\n").insert(0,sum).insert(0,"1 yuan coin number=");
                            money[0] -= sum;
                            sum = 0;
                        }
                        System.out.println(sb);
                        sum = 0;
                    }
                }else if(s.startsWith("q")){
                    if("q 0".equals(s)){
                        for(int i = 0;i < goods.length;i++){
                            System.out.println("A" + (i+1) + " " + price[i] + " " + goods[i]);
                        }
                    }else if("q 1".equals(s)){
                        for(int i = 0;i < money.length;i++){
                            System.out.println(amount[i] + " yuan coin number=" + money[i]);
                        }
                    }else{
                        System.out.println("E010:Parameter error");
                    }
                }
            }
        }
    }
    
}
```



## 99.自守数√

虽然我做出来了，但是因为有String的参与，速度非常慢，所以根据排行榜上的题解进行了调整

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int count = 0;
            for(int i = 0;i <= n;++i){
                if(isZiShou(i)) count++;
            }
            System.out.println(count);
        }
        br.close();
    }
    
    private static boolean isZiShou(int n){
//         int n2 = n * n;
//         return (n2+"").endsWith((n+""));
        int temp = n;
        int j = 1;
        while(temp != 0){
            temp /= 10;
            j *= 10;
        }
        return (n*n-n)%j == 0;
    }
}
```

## 100.等差数列√

。。。。。

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int end = 3 * n - 1;//3 * (n-1) + 2
            int sum = (2 + end) * n /2;
            System.out.println(sum);
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

## 102.字符统计√

利用HashMap进行统计，顺便练习了一下怎么对HashMap里面的Entry进行排序

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            HashMap<Character,Integer> map  = new HashMap<>();
            char[] strs = line.toCharArray();
            for(char c : strs){
                map.put(c,map.getOrDefault(c,0) + 1);
            }
            ArrayList<Map.Entry<Character,Integer>> list = new ArrayList<>(map.entrySet());
            Collections.sort(list,new Comparator<Map.Entry<Character,Integer>>(){
                public int compare(Map.Entry<Character,Integer> o1, Map.Entry<Character,Integer> o2){
                    if(o1.getValue().equals(o2.getValue())){
                        return o1.getKey() - o2.getKey();
                    }else {
                        return o2.getValue() - o1.getValue();
                    }
                }
            });
            StringBuilder sb = new StringBuilder();
            for(Map.Entry<Character,Integer> entry : list){
                sb.append(entry.getKey());
            }
            System.out.println(sb);
        }
    }
}
```

但是，因为全是小写字母和数字，所以还是有办法的

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int[] abc = new int[128];
            char[] strs = line.toCharArray();
            for(char c : strs){
                abc[(int)c]++;
            }
            int max = 0;
            for(int num : abc){
                if(max < num) max = num;
            }
            StringBuilder sb = new StringBuilder();
            while(max > 0){
                for(int i = 0;i < abc.length;++i){
                    if(abc[i] == max){
                        sb.append((char)i);
                    }
                }
                max--;
            }
            System.out.println(sb);
        }
        br.close();
    }
}
```

## 103.Redraiment的走法×

动态规划，根据题意，我们需要找到的是，以num[i]结尾的最长增序列

初始变量：因为每个东西自己算一步，所以每个位置初始都是1

然后开始遍历每个nums[i]，对于每个nums[i]，都是找前面所有比他小的nums[j]，然后更新dp[i]，对于dp[i],是每一个比它小的nums[j]对应的dp[j]+1之后的最大值

同时，因为最长的序列不一定是以最后一个数结尾，所以需要再遍历所有的dp，得到最大的值进行输出

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int[] nums = new int[n];
            String[] strs = br.readLine().split(" ");
            for(int i = 0;i < n;++i){
                nums[i] = Integer.parseInt(strs[i]);
            }
            System.out.println(getMaxStep(nums));
        }
    }
    
    private static int getMaxStep(int[] nums){
        int n = nums.length;
        int[] dp = new int[n];
        for(int i = 0;i < n;++i){
            dp[i] = 1;
        }
        for(int i = 1;i < n;++i){
            for(int j = 0;j < i;++j){
                if(nums[j] < nums[i]){
                    dp[i] = Math.max(dp[i],dp[j] + 1);
                }
            }
        }
        int max = 1;
        for(int num : dp){
            max = Math.max(max,num);
        }
        return max;
    }
}
```

## 105.记负均正II√

???

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        int negCount = 0,posCount = 0;
        long posSum = 0;
        while((line = br.readLine()) != null && line.length() > 0){
            int n = Integer.parseInt(line);
            if(n < 0) negCount++;
            else{
                posCount++;
                posSum += n;
            }
        }
        System.out.println(negCount);
        if(posCount == 0) System.out.println("0.0");
        else{
            double ava = (double)posSum /  (double) posCount;
            System.out.println(String.format("%.1f",ava));
        }
        br.close();
    }
}
```



## 106.字符串逆序√

......

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            char[] origin = line.toCharArray();
            StringBuilder sb = new StringBuilder();
            for(int i = origin.length-1;i >= 0;--i){
                sb.append(origin[i]);
            }
            System.out.println(sb);
        }
    }
}
```

## 107.求解立方根×

想到了二分法求，但是没想到怎么控制误差，新知识get

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            double n = Double.parseDouble(line);
            System.out.println(String.format("%.1f",getResult(n)));
        }
        br.close();
    }
    
    private static double getResult(double n){
        double left = 0,right = 0;
        boolean neg = false;
        if(n < 0){
            neg = true;
            n = -n;
        }
        if(n < 1) right = 1;
        else right = n;
        while(true){
            double target = left + (right - left)/2;
            double distance = n/(target* target) -target;
            if(distance >= -0.1 && distance <= 0.1){
                if(neg) return -target;
                else return target;
            }else if(distance > 0.1) left = target;
            else right = target;
        }
    }
}
```

## 108.求最小公倍数√

求最大公约数的方法值得学习

```java
import java.io.*;

//最小公倍数即两个数的乘积再除以最大公约数

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine())!= null){
            String[] strs = line.split(" ");
            int n = Integer.parseInt(strs[0]);
            int m = Integer.parseInt(strs[1]);
            int mn = n * m;
            if(n > m){
                int temp = n;
                n = m;
                m = temp;
            }
            while(n != 0){//求最大公约数
                int r = m % n;
                m = n;
                n = r;
            }
            System.out.println(mn/m);
        }
    }
}
```

