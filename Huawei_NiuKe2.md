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

