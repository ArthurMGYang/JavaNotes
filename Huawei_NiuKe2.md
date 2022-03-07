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

