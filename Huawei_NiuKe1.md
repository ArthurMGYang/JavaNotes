因为有些简单和入门的题是真的很简单，所以：

使用“√”表示太简单了，一眼就会；

使用“！”表示尝试做了，做出来了，但是方法不简便，还是得看最简的方法；

使用“×”表示尝试做了，没做出来，要多看；

## 1.字符串最后一个单词长度√

如果不能用String自带的方法，那么就倒过来遍历；如果可以用，则用lastIndexOf

```java
import java.util.Scanner;
public class Main{
    
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String s = scanner.nextLine();
        System.out.println(s.length()-s.lastIndexOf(" ")-1);
    }
}
```

## 2.计算某字符出现次数√

如果不使用String自带的方法，就一个个数，如果要使用则替换所有的要找的字符为""，然后用总长度减换了之后的长度

```java
import java.util.Scanner;
public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.nextLine().toLowerCase();
        String c = scanner.nextLine().toLowerCase();
        System.out.println(str.length() - str.replace(c,"").length());
    }
}
```

## 3.明明的随机数√

按照题意，一个简单的去重和排序，以及一个简单的对输入的处理。对于后者，不用说，用一个`int`就能搞定，对于前者，去重需要`Set`，排序则是`TreeSet`，于是答案就出来了

```java
import java.util.*;
public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        while(scanner.hasNextLine()){
            int n = Integer.parseInt(scanner.nextLine());
            TreeSet<Integer> set = new TreeSet<>();
            int count = 0;
            while(count < n){
                set.add(Integer.parseInt(scanner.nextLine()));
                count++;
            }
            Iterator iterator = set.iterator();
            while(iterator.hasNext()){
            System.out.println(iterator.next());
            }
        }
    }
}
```

## 4.字符串分隔√

简单的分隔

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        while(scanner.hasNextLine()){
            String current = scanner.nextLine();
            if(current.length() <=8){
                int length0 = 8 - current.length();
                sb.append(current);
                for(int i = 0;i < length0;i++){
                    sb.append("0");
                }
                sb.append("\n");
            }else{
                while(current.length() > 8){
                    sb.append(current.substring(0,8));
                    sb.append("\n");
                    current = current.substring(8);
                }
                int length0 = 8 - current.length();
                sb.append(current);
                for(int i = 0;i < length0;i++){
                    sb.append("0");
                }
                sb.append("\n");
            }
        }
        
        System.out.print(sb.toString());
    }
    
}
```

## 5.进制转换√

一个16进制向10进制的转换，进制概念和基础循环

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        StringBuilder sb = new StringBuilder();
        while(scanner.hasNextLine()){
            String current = scanner.nextLine();
            int currentInt = 0;
            if(current.startsWith("0x")) current = current.substring(2);
            int p = 1;
            for(int i = current.length()-1;i >= 0;i--){
                int q = charToInt(current.charAt(i));
                currentInt += (p*q);
                p *= 16;
            }
            sb.append(currentInt+"\n");
        }
        System.out.println(sb.toString());
    }
    
    private static int charToInt(char c){
        if(c >= '0' && c <= '9'){
            return c - '0';
        }else{
            return c - 'A' + 10;
        }
    }
}
```

## 6.质数因子×

首先，因为要找质数因子，得到一个质数集合，然后在里面遍历，似乎是最简的，但是很明显，质数集合很难获得。

所以线性的从2开始找，在找到过程中，每个数不是一次除一次就看下一个，而是循环的看，这个是不是可以整除，因为某些数，比如4，是由两个2。

但是如果线性的找，那么时间复杂度会很高，当这个数特别大的时候，就会超时，所以需要做一定的优化。对于一个数而言，分解成质数因子之后，他的任何一个质数因子，肯定要比这个数的开方要小。例如16，开方是4，那么他不可能有比4大的质数因子，所以在循环中，可以只判断到这个数的开方。

但是存在一种情况，这个数是个由比它开方大的质因子以及一些小因子组成，那么循环之后，最后这个数没有除数，本身是一个质数，即最后的n不是1，那么要将这个n也打印在最后

```java
import java.util.*;

public class Main{
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
 
        long num = scanner.nextLong();
        long k = (long) Math.sqrt(num);
 
        for (long i = 2; i <= k; ++i) {
            while (num % i == 0) {
                System.out.print(i + " ");
                num /= i;
            }
        }
        System.out.println(num == 1 ? "": num+" ");
    }
}
```

## 7.取近似值√

简单的判断，看小数点位置，判断进还是退，就看小数点之后那一位大于等于5还是小于5；然后前面的数字用Integer的parseInt就可以得到数值

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String num = scanner.nextLine();
        int n = 0;
        int judge = 0;
        for(int i = 0;i < num.length();i++){
            if(num.charAt(i) == '.' && i+1 < num.length()){
                if(i > 0){
                    n = Integer.parseInt(num.substring(0,i));
                }
                judge = num.charAt(i+1) - '0';
            }
        }
        System.out.println(judge >= 5 ? (n+1) : n);
    }
}
```

## 8.合并表记录！

这个题看似很简单，但是有两个地方有点坑。

首先，这个题居然是要key升序排列，这也就意味着，不能单纯的 用HashMap来做，而是需要用TreeMap

第二，这个题，在输入表的数据之前，会有一个n，这个n表示后面有多少数据，我是一次来读取这个n，但是忽略了一个情况，这个n可能有很多个。（虽然题解没有，但是实际上得考虑到）

最后，算是一个小点，对于打印Map，我是用set来保存了entry，然后再用iterator去打印，但是实际上，可以直接获取`keySet()`，然后通过遍历里面的所以key来打印

```java
import java.util.*;
public class Main {
        public static void main(String[] args){
            Scanner sc = new Scanner(System.in);
            TreeMap<Integer,Integer>  map = new TreeMap<>();
            while(sc.hasNext()){
                int n = sc.nextInt();
                for(int i =0;i<n;i++){
                    int key = sc.nextInt();
                    int value = sc.nextInt();
                    map.put(key,map.getOrDefault(key,0)+value);
                }
                for(Integer i : map.keySet()){
                    System.out.println(i+" "+map.get(i));
                }
            }
       }
}
```

## 9.提取不重复的整数√

一个简单的反向遍历，去重我采取的方式是用set判断是否遇到过，但是其实也可以不用Set而是用String自带的来判断（Set的我自己很容易想到，所以下面写的是用String的，但是其实运行时间和空间差别不大）

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String n = scanner.nextLine();
        String ans = "";
        for(int i = n.length() -1 ;i >=0;--i){
            char c = n.charAt(i);
            if(ans.contains(c+"")) continue;
            ans += c;
        }
        System.out.println(Integer.parseInt(ans));
    }
}
```

## 10.字符个数统计√

跟上题差不多，都是考虑出现没出现的问题，甚至都没考虑出现次数。最简单的，还是使用Set来判断，还有一种方法，使用数组，这样相对来说，空间复杂度要少一点，但是时间复杂度其实反而增加了。

```java
import java.util.*;
 
public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.nextLine();
        int[] freq = new int[128];
        int ans = 0;
        for(int i = 0;i < str.length();++i){
            char c = str.charAt(i);
            if(freq[c] == 0){
                ans++;
                freq[c] +=1;
            }
        }
        System.out.println(ans);
    }
}
```

## 11.数字颠倒√

和下面一个题几乎一模一样，所以放到下面说

## 12.字符串颠倒√

倒序遍历，正序生成答案

```java
import java.util.*;

public class Main{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        String str = scanner.nextLine();
        char[] ans = new char[str.length()];
        for(int i = str.length()-1;i >=0;--i){
            ans[str.length()-i-1] = str.charAt(i);
        }
        System.out.println(new String(ans));
    }
}
```

## 12.5关于意识到BufferedReader后的相关知识

在读取的时候，我之前一直是喜欢用Scanner，这样其实是很慢的，所以要进行改变。

1. 导包，除了以往的导入`java.util.*`以外，还要导入`java.io.*`
2. 创建对象`new BufferedReader(new InputStreamReader(System.in))`
3. 抛出异常，在使用到BufferReader的方法上，要抛出IOException的异常
4. BufferReader相关的方法

| 方法                                       | 返回值类型 | 作用         |
| ------------------------------------------ | ---------- | ------------ |
| read(),read(char[] cbuf, int off, int len) | int        | 读取一个char |
| readLine()                                 | String     | 读一行       |

5. BufferReader判断还有没有输入的方法

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String str = "";
while((str=br.readLine()) != null){
    //TO DO
}
```

## 13.句子逆序√

题的思路很简单，就是一个单纯的读取，分解，重组，如果可以用split，那么分解重组就会很简单，如果不让用，则自己写一个实现split的方法，不过使用了BufferedReader之后，确实快了好多啊

```java
import java.util.*;
import java.io.*;
public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();
        String[] strs = str.split(" ");
        StringBuilder sb = new StringBuilder();
        for(int i = strs.length-1;i>=0;--i){
            sb.append(strs[i] + " ");
        }
        System.out.println(sb.toString().substring(0,sb.length()-1));
    }
}
```

## 14.字符串排序√

这个也是读，然后排序，不过需要注意的是，我最开始用了TreeSet，但是这样出现了一个问题，相同的字符会被忽略掉，所以不能用Set，应该用数组存，然后sort

```java
import java.util.*;
import java.io.*;
 
public class Main{
     
     
 public static void main(String[] args) throws IOException{
     BufferedReader bf=new BufferedReader(new InputStreamReader(System.in));
     int count=Integer.parseInt(bf.readLine());
     String[] result=new String[count];
     for(int i=0;i<count;i++)result[i]=bf.readLine();
     StringBuilder sb=new StringBuilder();
     Arrays.sort(result);
     for (String w : result) sb.append(w).append('\n');
     System.out.println(sb.toString());
    }
}
```

## 15.求int型正整数在内存中存储时1的个数√

经典位运算的题，将最后一位1变成0的方法`n &= (n-1)`。所以不停的变就完事儿了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int count = 0;
        while(n > 0){
            n &= (n-1);
            count++;
        }
        System.out.println(count);
    }
}
```

## 16.购物单×

经典的01背包问题的变种。

01背包问题，背包重量是W，总共可以装N个东西，然后有一堆物品重量分别是w，价值是v，用背包装东西，怎么装能装的价值最大。

使用动态规划来进行解答，设`dp[i][j]`表示前i个物品，背包重量为j的情况下能装的最大的价值，那么很明显对于任意的`dp[i][j]`都有

`dp[i][j] = Math.max(dp[i-1][j] + dp[i-1][j-w[i]] + v[i])`其中，`j-w[i]`要大于0

即表示，此时这个物品选或者不选，对总价值的影响

而这个题，每个要装进去的东西，除了有自身相关的东西以外，还有最多两个附件，那么对动态规划进行一点修改就可以开始作答。

首先是数据的读取，用什么来装。如果用对象装，可行，但是存在问题附件先读，那么可能会出现空指针异常，所以改为用二维数组。第一行用来表示编号和自身的价格，其他的行用来表示其附件，因为这个题最多两个附件，所以二维数组最多3行。

其次就是动态规划过程，对于这种题，因为每个东西有附件，所以在动态规划的时候，有四种情况，主件，主件加附件1，主件加附件2，主件加附件1和附件2，分别用对应的方法计算就可以得到最终的结果。

优化：在题目中说到，价格都是10的倍数，所以可以把所有的结果都除以10，这样可以减少空间复杂度，但是别忘了每个地方都要除以10且最后要乘上

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] line = br.readLine().split(" ");
        int money = Integer.parseInt(line[0]);
        int m = Integer.parseInt(line[1]);
        //因为题目说了价格都是10的倍数，如果没有的话，就不能这么做
        money /= 10;
        
        int[][] prices = new int[m+1][3];//自身，2个附件，所以行数是3
        int[][] values = new int[m+1][3];
        
        for(int i = 1;i <= m;++i){
            line = br.readLine().split(" ");
            int price = Integer.parseInt(line[0]) / 10;
            int value = Integer.parseInt(line[1]);
            int q = Integer.parseInt(line[2]);
            if(q == 0){
                prices[i][0] = price;
                values[i][0] = price * value;
            }else if(prices[q][1] == 0){
                prices[q][1] = price;
                values[q][1] = price * value;
            }else {
                prices[q][2] = price;
                values[q][2] = price * value;
            }
        }
        
        int[][] dp = new int[m+1][money+1];
        for(int i = 1;i <= m;++i){
            for(int j = 1;j <=money;++j){
                int p1 = prices[i][0];
                int p2 = prices[i][1];
                int p3 = prices[i][2];
                int v1 = values[i][0];
                int v2 = values[i][1];
                int v3 = values[i][2];
                
                dp[i][j] = j - p1 >=0 ? Math.max(dp[i-1][j],dp[i-1][j-p1] + v1) : dp[i-1][j];
                dp[i][j] = j - p1 - p2 >=0? Math.max(dp[i][j],dp[i-1][j-p1-p2] + v1+v2) : dp[i][j];
                dp[i][j] = j - p1 - p3 >=0? Math.max(dp[i][j],dp[i-1][j-p1-p3] + v1+v3) : dp[i][j];
                dp[i][j] = j - p1 - p2 - p3 >=0? Math.max(dp[i][j],dp[i-1][j-p1-p2-p3] + v1+v2+v3) : dp[i][j];
            }
        }
        System.out.println(dp[m][money] * 10);
    }
}
```

## 17.坐标移动√

我不知道凭什么这个题能和上个题一样是中等难度的题

读取之后，按照`;`分开，然后按照逻辑进行判断以及移动

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] structions = br.readLine().split(";");
        int x = 0,y = 0;
        for(String str : structions){
            if(isValid(str)){
                char direction = str.charAt(0);
                int position = Integer.parseInt(str.substring(1));
                if(direction == 'A'){
                    x -= position;
                }else if(direction == 'D'){
                    x += position;
                }else if(direction == 'W'){
                    y += position;
                }else{
                    y -= position;
                }
            }
        }
        
        System.out.println(x + "," + y);
    }
    
    public static boolean isValid(String str){
        if(str.length() < 2 || str.length() > 3) return false;
        if(str.charAt(0) != 'A' && str.charAt(0) != 'D' && str.charAt(0) != 'W' && str.charAt(0) != 'S'){
            return false;
        }
        try{
            int a = Integer.parseInt(str.substring(1));
            if(a >= 100 || a < 0) return false;
        }catch(NumberFormatException e){
            return false;
        }
        return true;
    }
}
```

## 18识别有效的IP地址并分类!

对于这个题，其实总体来说，没有算法上的难点，拆分，存好，然后一个个判断就完事儿了，但是有几个细节需要注意

1. 在拆分的时候，要注意split方法参数是一个正则表达式，也就是说，直接`split(.)`并不会得到想要的结果，要使用`split(\\.)`才行
2. 在判断掩码的时候，不能直接使用`Integer.toBinaryString(i)`，因为在转化成掩码的时候，还需要补全0,

```java
import java.util.*;
import java.io.*;
 
public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = "";
        String[] ipAndMask = new String[2];
        String[] ipArr = new String[4];
        int A = 0,B = 0,C = 0,D =0,E = 0,errIpOrMask = 0,privateIp = 0;
        while ((line=br.readLine()) != null) {
            ipAndMask = line.split("\\~");
            if(ipAndMask[0].equals("end")){
                break;
            }
            ipArr = ipAndMask[0].split("\\.");
            //类似于【0.*.*.*】和【127.*.*.*】忽略，这个条件要放在最前面，否则错误掩码会统计多
            if(ipArr[0].equals("0") || ipArr[0].equals("127")){
                continue;//跳过
            }
            if(!isValidMask(ipAndMask[1])){ //判断掩码是否有效
                errIpOrMask++;
            }else{
                if(!isValidIp(ipAndMask[0])){
                    errIpOrMask++;
                }else{
                    if(Integer.parseInt(ipArr[0])>=1 && Integer.parseInt(ipArr[0])<=126){
                        if(Integer.parseInt(ipArr[0])==10){
                            privateIp++;
                            A++;
                        }else{
                            A++;
                        }
                    }
                    if(Integer.parseInt(ipArr[0])>=128 && Integer.parseInt(ipArr[0])<=191){
                        if(Integer.parseInt(ipArr[0])==172 && (Integer.parseInt(ipArr[1]) >=16 && Integer.parseInt(ipArr[1])<=31)){
                            privateIp++;
                            B++;
                        }else{
                            B++;
                        }
                    }
                    if(Integer.parseInt(ipArr[0])>=192 && Integer.parseInt(ipArr[0])<=223){
                        if(Integer.parseInt(ipArr[0])==192 && Integer.parseInt(ipArr[1]) ==168){
                            privateIp++;
                            C++;
                        }else{
                            C++;
                        }
                    }
                }
                if(Integer.parseInt(ipArr[0])>=224 && Integer.parseInt(ipArr[0])<=239){
                    D++;
                }
                if(Integer.parseInt(ipArr[0])>=240 && Integer.parseInt(ipArr[0])<=255){
                    E++;
                }
            }
        }
        System.out.println(A + " " + B + " " + C + " " + D + " " + E + " " + errIpOrMask + " " + privateIp);
    }
    public static boolean isValidMask(String mask){
        if(!isValidIp(mask)){
            return false;
        }
        String[] maskTable = mask.split("\\.");
        //mask转为32位2进制字符串
        StringBuilder sb = new StringBuilder();
        for(int i=0;i<maskTable.length;i++){
            maskTable[i] = Integer.toBinaryString(Integer.parseInt(maskTable[i]));
            if(maskTable[i].length() < 8){//不足8位补齐0
                for(int j=0;j < 8- maskTable[i].length();j++){
                    sb.append("0");//补完零
                }
                sb.append(maskTable[i]);//再添加转换的2进制串
            }else{
                sb.append(maskTable[i]);//刚好8位直接添加，因为之前已经判断过ip的有效性，所以不可能超过8位
            }
        }
        //最后一个1在第一个0之前，有效，否则无效
        return sb.toString().lastIndexOf("1") < sb.toString().indexOf("0");
    }
    public static boolean isValidIp(String ip){
        String[] ipTable = ip.split("\\.");
        if(ipTable.length != 4){
            return false;
        }
        for(String s : ipTable){
            if(Integer.parseInt(s) < 0 || Integer.parseInt(s) > 255){
                return false;
            }
        }
        return true;
    }
}
```

## 19.简单错误记录×

首先，第一次读的时候没有理解到题目，所以直接去题解了，所以算是这个题没有自己做，但是其实这个题非常的简单。

简言之，就是读取所有的记录，然后放入HashMap中，但是有一个问题是，只需要最后8个，前面的就不要了，这也就意味着，这个Map需要时有序的，而LinkedHashMap正好满足这个

然后就是拆分，记录，以及输出，不过在拆分的时候，题解的这个方法非常的简便

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        HashMap<String,Integer> map = new LinkedHashMap<String,Integer>();
        String str=null;
        while((str = br.readLine()) != null){      //&& !tstr.equals(""))没有性能影响
            String[] split = str.split("\\s+");
            String FileName = split[0].substring(split[0].lastIndexOf("\\")+1);
            String key = FileName.substring(Math.max(FileName.length()-16 ,0))+" "+ split[1];  //max 最快推荐 ？：也可以 if太麻烦
            
            map.put(key,map.getOrDefault(key,0)+1);
        }
        int cnt=0;
        for(HashMap.Entry<String,Integer> it:map.entrySet()){
            if(map.size()-cnt<=8)
                System.out.println(it.getKey()+" "+it.getValue());
            cnt++;
        }
    }
}
```

## 20.密码验证合格程序!

逻辑上是很简单的，一个个的验证就行了。

但是关于重复，我刚开始没理解到题意，但是即使理解了，我感觉自己也不能第一时间想到这种方法，所以还是得多练习

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = "";
        while((line=br.readLine()) != null){
            if(line.length() <=8 ){
                System.out.println("NG");
                continue;
            }
            int need = 0;
            if(hasUpper(line)) need++;
            if(hasLower(line)) need++;
            if(hasInteger(line)) need++;
            if(hasOtehr(line)) need++;
            if(need < 3){
                System.out.println("NG");
                continue;
            }
            
            if(repeat(line,0,3)){
                System.out.println("NG");
                continue;
            }
            
            System.out.println("OK");
        }
    }
    
    private static boolean hasUpper(String line){
        for(char c : line.toCharArray()){
            if(c >='A' && c <='Z') return true;
        }
        return false;
    }
    
    private static boolean hasLower(String line){
        for(char c : line.toCharArray()){
            if(c >='a' && c <='z') return true;
        }
        return false;
    }
    
    private static boolean hasInteger(String line){
        for(char c : line.toCharArray()){
            if(c >='0' && c <='9') return true;
        }
        return false;
    }
    
    private static boolean hasOtehr(String line){
        for(char c : line.toCharArray()){
            if(c == ' ' || c == '\n') return false;
            boolean isUpper = (c >='A' && c <='Z');
            boolean isLower = (c >='a' && c <='z');
            boolean isInteger = (c >='0' && c <='9');
            if(!isUpper && !isLower && !isInteger){
                return true;
            }
        }
        return false;
    }
    
    private static boolean repeat(String line,int left,int right){
        if(right > line.length()){
            return false;
        }
        if(line.substring(right).contains(line.substring(left,right))){
            return true;
        }else{
            return repeat(line,left+1,right+1);
        }
    }
}
```

## 21.简单密码√

没啥好说的

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = "";
        while((line = br.readLine()) != null){
            char[] chars = line.toCharArray();
            StringBuffer sb = new StringBuffer();
            for(int i = 0;i < chars.length;i++){
                if(chars[i] >= 'A' && chars[i] <= 'Z'){
                    if(chars[i] == 'Z') chars[i] = 'A';
                    else chars[i] += 1;
                    sb.append((chars[i]+"").toLowerCase());
                }
                if(chars[i] >='a' && chars[i] <= 'z'){
                    if(chars[i] == 'a' || chars[i] == 'b' || chars[i] == 'c') sb.append("2");
                    if(chars[i] == 'd' || chars[i] == 'e' || chars[i] == 'f') sb.append("3");
                    if(chars[i] == 'g' || chars[i] == 'h' || chars[i] == 'i') sb.append("4");
                    if(chars[i] == 'j' || chars[i] == 'k' || chars[i] == 'l') sb.append("5");
                    if(chars[i] == 'm' || chars[i] == 'n' || chars[i] == 'o') sb.append("6");
                    if(chars[i] == 'p' || chars[i] == 'q' || chars[i] == 'r' || chars[i] == 's') sb.append("7");
                    if(chars[i] == 't' || chars[i] == 'u' || chars[i] == 'v') sb.append("8");
                    if(chars[i] == 'w' || chars[i] == 'x' || chars[i] == 'y' || chars[i] == 'z') sb.append("9");
                }
                if(chars[i] >= '0' && chars[i] <='9') sb.append(chars[i]);
            }
            System.out.println(sb.toString());
        }
    }
}
```

## 22.汽水瓶√

简单的递归，但是注意一个点，他是允许借一个的，所以当瓶子数是2的时候，可以借一个喝了还，所以是可以喝1的

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = "";
        while(!(line = br.readLine()).equals("0")){
            int n = Integer.parseInt(line);
            System.out.println(getDrink(n));
        }
    }
    
    private static int getDrink(int n){
        if(n < 2) return 0;
        if(n == 2) return 1;
        int drink = n / 3;
        int left = n - 3*drink;
        return drink + getDrink(left+drink);
    }
}
```

## 23.删除字符串出现次数最小的字符√

没什么好说的，map存，找最小，然后删除以及输出。删除有两种办法，第一种是遍历所有的字符，如果他次数是最小的就替换；第二种是新建一个StringBuffer，然后不停的加那些出现次数不是最小的字符

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = "";
        while((line=br.readLine()) != null){
            int min = Integer.MAX_VALUE;
            Map<Character,Integer> map = new HashMap<>();
            for(int i = 0;i < line.length();i++){
                map.put(line.charAt(i),map.getOrDefault(line.charAt(i),0)+1);
            }
            for(int i : map.values()){
                if(i < min) min = i;
            }
            StringBuilder sb = new StringBuilder();
            for (char ch : line.toCharArray()) {
                if (map.get(ch) != min) {
                    sb.append(ch);
                }
            }
//            for(char c : map.keySet()){
//                if(map.get(c) == min){
//                    line = line.replace(c+"","");
//                }
//            }
            System.out.println(sb.toString());
        }
    }
}
```

## 24.合唱队×

这个题，主体思路是寻找到对每一个数，找到能使其满足题目要求所需要踢出的人的个数，然后再算最小。

### 方法一：动态规划

对于任何一个数，我们用left数组表示其左边小于它的数的个数（包括它自身），然后用right数组表示其右边小于它的数的个数（同样包括自身），得到这两个数组之后，我们就可以得到以这个数为中心点构成的合唱队的长度（对应的left+right-1）。要踢出的队员最少，即这个结果最长，找到这个最长，也就能得到最少踢出人的个数

缺点是，这个方法的复杂度是O(n^2)，如果数据太多容易超时

```java
import java.io.*;
 
public class Main {
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = null;
        while ((str = br.readLine()) != null) {
            int n = Integer.parseInt(str);
            int[] arr = new int[n];
            String[] strs = br.readLine().split(" ");
            for (int i = 0; i < n; i++) {
                arr[i] = Integer.parseInt(strs[i]);
            }
 
            int[] left = new int[n]; //存储每个数左边小于其的数的个数
            int[] right = new int[n];//存储每个数右边小于其的数的个数
            left[0] = 1;            //最左边的数设为1
            right[n - 1] = 1;        //最右边的数设为1
            //计算每个位置左侧的最长递增
            for (int i = 0; i < n; i++) {
                left[i] = 1;
                for (int j = 0; j < i; j++) {
                    if (arr[i] > arr[j]) {   //动态规划
                        left[i] = Math.max(left[j] + 1, left[i]);
                    }
                }
            }
            //计算每个位置右侧的最长递减
            for (int i = n - 1; i >= 0; i--) {
                right[i] = 1;
                for (int j = n - 1; j > i; j--) {
                    if (arr[i] > arr[j]) {   //动态规划
                        right[i] = Math.max(right[i], right[j] + 1);
                    }
                }
            }
            // 记录每个位置的值
            int max = 1;
            for (int i = 0; i < n; i++) {
                max = Math.max(left[i] + right[i] - 1,max);
            }
            System.out.println(n - max);
        }
 
    }
}
```

### 方法二：二分查找

说实话我有点理解不能

```java
import java.io.*;
 
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = null;
        while ((str = br.readLine()) != null) {
            if (str.equals("")) continue;
            int n = Integer.parseInt(str);
            int[] heights = new int[n];
            String[] str_heights = br.readLine().split(" ");
            // 当仅有一个人时，其自己组成一个合唱队，出列0人
            if (n <= 1) System.out.println(0);
            else {
                for (int i = 0; i < n; i++) heights[i] = Integer.parseInt(str_heights[i]);
                // 记录从左向右的最长递增子序列和从右向左的最长递增子序列
                int[] seq = new int[n], rev_seq = new int[n];
                int[] k = new int[n];  // 用于记录以i为终点的从左向右和从右向走的子序列元素个数
                seq[0] = heights[0];  // 初始化从左向右子序列首元素为第一个元素
                int index = 1; // 记录当前子序列的长度
                for (int i = 1; i < n; i++) {
                    if (heights[i] > seq[index-1]) {  // 当当前元素大于递增序列最后一个元素时
                        k[i] = index;  // 其左边元素个数
                        seq[index++] = heights[i];  // 更新递增序列
                    } else {  // 当当前元素位于目前维护递增序列之间时
                        // 使用二分搜索找到其所属位置
                        int l = 0, r = index - 1;
                        while (l < r) {
                            int mid = l + (r - l) / 2;
                            if (seq[mid] < heights[i]) l = mid + 1;
                            else r = mid;
                        }
                        seq[l] = heights[i];  // 将所属位置值进行替换
                        k[i] = l;  // 其左边元素个数
                    }
                }
 
                // 随后，再从右向左进行上述操作
                rev_seq[0] = heights[n-1];
                index = 1;
                for (int i = n - 2; i >= 0; i--) {
                    if (heights[i] > rev_seq[index-1]) {
                        k[i] += index;
                        rev_seq[index++] = heights[i];
                    } else {
                        int l = 0, r = index - 1;
                        while (l < r) {
                            int mid = l + (r - l) / 2;
                            if (rev_seq[mid] < heights[i]) l = mid + 1;
                            else r = mid;
                        }
                        rev_seq[l] = heights[i];
                        k[i] += l;
                    }
                }
 
                int max = 1;
                for (int num: k)
                    if (max < num) max = num;
                // max+1为最大的k
                System.out.println(n - max - 1);
            }
        }
    }
}
```

## 25.数据分类处理

其实这个题不难，主要是绕来绕去的很烦躁

有两个注意的点：

1. 注意审题，最终要怎么输出
2. 因为全部都是字符串，所以在装入TreeSet进行比较的时候，要先自定排序

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line=br.readLine()) != null){
            String L = line;
            String R = br.readLine();
            String[] first = L.split(" ");
            String[] second = R.split(" ");
            Set<String> set = new TreeSet<>(new Comparator<String>(){
                public int compare(String s1,String s2){
                    return Integer.parseInt(s1) - Integer.parseInt(s2);
                }
            });
            for(int i = 1;i < second.length;++i){
                set.add(second[i]);
            }
            StringBuffer sb = new StringBuffer();
            int total = 0;
            for(String str : set){
                List<String> list = new ArrayList<>();
                for(int i = 1;i < first.length;++i){
                    if(first[i].contains(str)){
                         list.add((i-1) + " " + first[i] + " ");
                    }
                }
                if(list.size() == 0) continue;
                sb.append(str + " ");
                total += 2;
                total += list.size() * 2;
                sb.append(list.size() + " ");
                for(String s : list) sb.append(s);
            }
            
            System.out.println(total+ " " + sb.toString());
        }
    }
}
```

## 26.字符串排序×

### 方法一：利用API

因为这个题，需要将字母进行排序，其他不变，那么很明显，可以将字母分离出来，然后排序，排序完成后，再次遍历原字符串，如果是字母，那么从排序好的集合中取一个，如果不是字母，那么保留这个特殊字符

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            List<Character> list = new ArrayList<>();
            for(int i = 0;i < line.length();i++){
                if(Character.isLetter(line.charAt(i))){
                    list.add(line.charAt(i));
                }
            }
            list.sort(new Comparator<Character>() {
                public int compare(Character c1, Character c2) {
                    return Character.toLowerCase(c1) - Character.toLowerCase(c2);
                }
            });
            
            int count = 0;
            StringBuffer sb = new StringBuffer();
            for(int i = 0;i < line.length();i++){
                if(Character.isLetter(line.charAt(i))){
                    sb.append(list.get(count++));
                }else {
                    sb.append(line.charAt(i));
                }
            }
            System.out.println(sb.toString());
        }
    }
}
```

### 方法二：不使用API，双重遍历

获取到字符串之后，创建一个letters数组，从字母A（这里大小写无所谓）开始遍历到Z（同样大小写无所谓，但是必须和之前是一样的）。然后内部遍历整个字符串，当字符串等于外层字符（大小写均算等于），则放入数组中。这样当遍历完成后，letters内部就是已经排序好的字符串，然后再遍历字符串，把非字符定在原位即可。

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
 
public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line;
        while((line = br.readLine()) != null){
            char[] letters = new char[line.length()];
            int count = 0;
            for(int i = (int)'a';i <= (int)'z';++i){
                for(int j = 0;j < line.length();++j){
                    if(Character.toLowerCase(line.charAt(j)) == i){
                        letters[count++] = line.charAt(j);
                    }
                }
            }
            
            count = 0;
            StringBuilder sb = new StringBuilder();
            for(int i = 0;i < line.length();i++){
                if(Character.isLetter(line.charAt(i))){
                    sb.append(letters[count++]);
                }else{
                    sb.append(line.charAt(i));
                }
            }
            System.out.println(sb.toString());
        }
    }
}
```

## 27.查找兄弟单词！

理论上是个很简单的题，找到兄弟单词，排序，找到低K个，但是依然有坑以及找兄弟单词时候的诀窍

坑：找到的兄弟单词，可能比K要小，也就是说，只能打出size，并不能找到想要找的那一个

诀窍：在找兄弟的时候，遍历两个数组，由原本的作为参考，遍历需要找的单词，遇到一样的字符，就把字符换掉。如果互为兄弟单词，那么再找完之后，这个单词的char数组，应该全部都被换掉了，由此判断其是否为兄弟单词

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
 
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf=new BufferedReader(new InputStreamReader(System.in));
        String s=null;
        while((s=bf.readLine())!=null){
            // 将输入的字符串分割成字符串数组
            String[] words=s.split(" ");
            // 待查找单词
            String str=words[words.length-2];
            char[] chStr=str.toCharArray();
            // 兄弟单词表里的第k个兄弟单词
            int k=Integer.parseInt(words[words.length-1]);
            // 存放兄弟单词表
            ArrayList<String> broWords=new ArrayList<>();
            // 遍历输入的单词
            for (int i = 1; i < words.length-2; i++) {
                // 不相等且长度相同
                if((!words[i].equals(str)) && words[i].length()==str.length()) {
                    char[] word=words[i].toCharArray();
                    for (int j = 0; j < chStr.length; j++) {
                        for (int j2 = 0; j2 < word.length; j2++) {
                            if (word[j2]==chStr[j]) {
                                word[j2]='0';
                                break;
                            }
                        }
                    }
                    boolean flag = true;
                    for(char c : word){
                        if(c != '0'){
                            flag = false;
                            break;
                        } 
                    }
                    if(flag) broWords.add(words[i]);
                }
            }
            System.out.println(broWords.size());
            if(k>0 && k<=broWords.size()) {
                Collections.sort(broWords);
                System.out.println(broWords.get(k-1));
            }
        }
    }
}
```

## 28.素数伴侣×

匈牙利算法。

1. 首先定义两个容器，分别用来装偶数和奇数
2. 利用匈牙利算法找到素数伴侣，核心思路：先到先得，能让就让
3. 最后输出素数伴侣最多时的对数

```java
import java.io.*;
import java.util.ArrayList;

public class Main{
    
    static int max=0;
    public static void main(String[] args) throws IOException{
        //io输入
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));
        String s;
        while((s=br.readLine())!=null){
            //输入正偶数n
            int n=Integer.parseInt(s);
            //输入n个整数
            String[] arr=br.readLine().split(" "); 
            //用于存储所有的奇数
            ArrayList<Integer> odds=new ArrayList<>();
            //用于存储所有的偶数
            ArrayList<Integer> evens=new ArrayList<>();
            for(int i=0;i<n;i++){
                int num=Integer.parseInt(arr[i]);
                //将奇数添加到odds
                if(num%2==1){
                    odds.add(num);
                }
                //将偶数添加到evens
                if(num%2==0){
                    evens.add(num);
                }
            }
            //下标对应已经匹配的偶数的下标，值对应这个偶数的伴侣
            int[] matcheven=new int[evens.size()];
            //记录伴侣的对数
            int count=0;
            for(int i=0;i<odds.size();i++){
                //用于标记对应的偶数是否查找过
                boolean[] v=new boolean[evens.size()];
                //如果匹配上，则计数加1
                if(find(odds.get(i),matcheven,evens,v)){
                    count++;
                }
            }
            System.out.println(count);
        }   
    }
    
    //判断奇数x能否找到伴侣
    private static boolean find(int x,int[] matcheven,ArrayList<Integer> evens,boolean[] v){
        for(int i=0;i<evens.size();i++){
            //该位置偶数没被访问过，并且能与x组成素数伴侣
            if(isPrime(x+evens.get(i))&&v[i]==false){
                v[i]=true;
                /*如果i位置偶数还没有伴侣，则与x组成伴侣，如果已经有伴侣，并且这个伴侣能重新找到新伴侣，
                则把原来伴侣让给别人，自己与x组成伴侣*/
                if(matcheven[i]==0||find(matcheven[i],matcheven,evens,v)){
                    matcheven[i]=x;
                    return true;
                }
            }
        }
        return false;
    }
    //判断x是否是素数
    private static boolean isPrime(int x){
        if(x==1) return false;
        //如果能被2到根号x整除，则一定不是素数
        for(int i=2;i<=(int)Math.sqrt(x);i++){
            if(x%i==0){
                return false;
            }
        }
        return true;
    }
}

```



## 29.字符串加密√

简单的加密和解密，照着模板逻辑也很简单的来

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String origin = line;
            String password = br.readLine();
            System.out.println(encrypt(origin));
            System.out.println(decrypt(password));
        }
    }
    
    private static String encrypt(String origin){
        char[] chars = origin.toCharArray();
        for(int i = 0;i < chars.length;++i){
            if(chars[i] >= 'a' && chars[i] <= 'z'){
                chars[i] = chars[i] == 'z' ? 'A' : (char)(Character.toUpperCase(chars[i])+1);
            }else if(chars[i] >= 'A' && chars[i] <= 'Z'){
                chars[i] = chars[i] == 'Z' ? 'a' : (char)(Character.toLowerCase(chars[i])+1);
            }else if(chars[i] >= '0' && chars[i] <= '9'){
                chars[i] = chars[i] == '9' ? '0' : (char)(chars[i]+1);
            }
        }
        return new String(chars);
    }
    
    private static String decrypt(String password){
        char[] chars = password.toCharArray();
        for(int i = 0;i < chars.length;++i){
            if(chars[i] >= 'a' && chars[i] <= 'z'){
                chars[i] = chars[i] == 'a' ? 'Z' : (char)(Character.toUpperCase(chars[i])-1);
            }else if(chars[i] >= 'A' && chars[i] <= 'Z'){
                chars[i] = chars[i] == 'A' ? 'z' : (char)(Character.toLowerCase(chars[i])-1);
            }else if(chars[i] >= '0' && chars[i] <= '9'){
                chars[i] = chars[i] == '0' ? '9' : (char)(chars[i]-1);
            }
        }
        return new String(chars);
    }
}
```



## 30.字符串合并处理！

逻辑上，不是特别麻烦，但是还是有很多技巧的东西

首先是合并上，因为对应位置要进行排序，所以可以一边找一边排

其次是关于二进制倒序后转成十六进制，其实对于一个字符，转成二进制之后，最多也就4位，所以与运算得到的是某一个位，然后再进行相应的左移和右移就行了

```java
import java.util.*;
import java.io.*;
 
public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        String s = null;
        while((s = bf.readLine())!=null){
            String[] str = s.split(" ");
            s = str[0] + str[1];
            char[] array = sort(s);
            System.out.println(transform(array));
        }
    }
     
    public static char[] sort(String s){
        char[] array = s.toCharArray();
        int i,j;
        for(i=2;i<array.length;i+=2){
            if(array[i] < array[i-2]){
                char tmp = array[i];
                for(j=i;j>0 && array[j-2] > tmp; j-=2){
                    array[j] = array[j-2];
                }
                array[j] = tmp;
            }
        }
        for(i=3;i<array.length;i+=2){
            if(array[i] < array[i-2]){
                char tmp = array[i];
                for(j=i;j>1 && array[j-2]>tmp;j-=2){
                    array[j] = array[j-2];
                }
                array[j] = tmp;
            }
        }
        return array;
    }
 
    public static String transform(char[] array){
        for(int i=0;i<array.length;i++){
            int num = -1;
            if(array[i] >= 'A' && array[i] <= 'F'){
                num = array[i]-'A'+10;
            }else if(array[i] >= 'a' && array[i] <= 'f'){
                num = array[i]-'a'+10; 
            }else if(array[i] >= '0' && array[i] <= '9'){
                num = array[i]-'0';
            }
 
            if(num != -1){ // 需要转换
                num = ((num&1) << 3) + ((num&2) << 1) + ((num&4) >> 1) + ((num&8) >> 3);
                if(num<10){
                    array[i] = (char)(num+'0');
                }else if(num<16){
                    array[i] = (char)(num-10+'A');
                }
            }
        }
        return new String(array);
    }
}
```



## 31.单词倒排√

相比于普通的倒排，这个只是分割符变成了所有不是单词的东西，所以需要用循环，先找到所有的字母，构成一个单词，添加到集合中，再利用循环，跳过所有的分隔符，再进行下一次查找

最后倒序遍历集合即可

```java
import java.util.*;
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            ArrayList<String> words = new ArrayList<>();
            int start = 0,end = 0;
            while(end < line.length()){
                while(end < line.length() && Character.isLetter(line.charAt(end))){
                    end++;
                }
                words.add(line.substring(start,end));
                while(end < line.length() && !Character.isLetter(line.charAt(end))){
                    end++;
                }
                start = end;
            }
            
            StringBuilder sb = new StringBuilder();
            for(int i = words.size()-1;i > 0;--i){
                sb.append(words.get(i) + " ");
            }
            sb.append(words.get(0));
            System.out.println(sb.toString());
        }
    }
}
```

## 32.密码截取！

等同于最长回文子串，采用中心扩散法，不过要记住，不禁要考虑奇数的回文，还要考虑偶数的回文！！！

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(getLongestPassword(line));
        }
    }
    
    private static int getLongestPassword(String line){
        int max = 0;
        char[] strs = line.toCharArray();
        for(int i = 0;i < strs.length;++i){
            int m1 = currentLongest(strs,i,i);
            int m2 = currentLongest(strs,i,i+1);
            max = Math.max(max,Math.max(m1,m2));
        }
        return max;
    }
    
    private static int currentLongest(char[] strs,int left, int right){
        while(left >= 0 && right < strs.length && strs[left] == strs[right]){
            left--;
            right++;
        }
        return right - left - 1;
    }
}
```

## 33.整数与IP地址之间的转换√

一个简单的转换，就是绕来绕去容易绕晕，注意几个点，比如转成二进制字符串之后，长度的补全，以及因为有32位，所以可能超过int的界限等问题

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split("\\.");
            if(strs.length == 4){
                StringBuilder sb = new StringBuilder();
                for(String str : strs){
                    sb.append(str2Binary(str));
                }
                System.out.println(binary2Long(sb.toString()));
            }else {
                long n = Long.parseLong(strs[0]);
                int[] ips = long2IP(n);
                System.out.println(ips[0] + "." + ips[1] + "." + ips[2] + "." + ips[3]);
            }
        }
    }
    
    private static String str2Binary(String str){
        int n = Integer.parseInt(str);
        str = Integer.toBinaryString(n);
        if(str.length() < 8){
            while(str.length() < 8){
                str = "0" + str;
            }
        }else if(str.length() > 8){
            str = str.substring(str.length()-8);
        }
        return str;
    }
    
    private static long binary2Long(String str){
        long ans = 0;
        long p = 1;
        for(int i = str.length()-1;i >= 0;--i){
            if(str.charAt(i) == '1'){
                ans += p;
            }
            p *= 2;
        }
        return ans;
    }
    
    private static int[] long2IP(long n){
        int[] ips = new int[4];
        String s = Long.toBinaryString(n);
        if(s.length() < 32){
            while(s.length() < 32){
                s = "0" + s;
            }
        }
        int length = s.length();
        String s1 = s.substring(length-32,length-24);
        String s2 = s.substring(length-24,length-16);
        String s3 = s.substring(length-16,length-8);
        String s4 = s.substring(length-8);
        ips[0] = (int)(binary2Long(s1));
        ips[1] = (int)(binary2Long(s2));
        ips[2] = (int)(binary2Long(s3));
        ips[3] = (int)(binary2Long(s4));
        return ips;
    }
}
```

## 34.图片整理（字符串字符排序）√

这个题当然可以用排序的思路做，但是因为是按照ASCII码排序，数组也可以做。同时因为只有数字和字符，可以加一个if来减少遍历的长度

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int[] chars = new int[128];
            for(int i = 0;i < line.length();++i){
                chars[(int)line.charAt(i)]++;
            }
            StringBuilder sb = new StringBuilder();
            for(int i = 0;i <128;++i){
                if((i>='0'&&i<='9') || (i>='a'&&i<='z') || (i>='A' &&i<='Z')){
                    while(chars[i] > 0){
                        sb.append((char)i);
                        chars[i]--;
                    }
                }
            }
            System.out.println(sb.toString());
        }
    }
}
```

## 35.蛇形矩阵√

没啥复杂的，就按照一定的规律去打印就完事儿了，不过StringBuilder确实在处理字符串的时候更快

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            int start = 1,gap = 2,startGap = 1;
            StringBuilder sb = new StringBuilder();
            for(int i = 0;i<n;++i){
                int current = start;
                int currentGap = gap;
                sb.append(current + " ");
                for(int j = 0;j < n-i-1;++j){
                    current += currentGap;
                    if(j == n-i-2){
                        sb.append(current);
                    }else {
                        sb.append(current + " ");
                    }
                    currentGap++;
                }
                sb.append("\n");
                start += startGap;
                startGap++;
                gap++;
            }
            System.out.print(sb.toString());
        }
    }
}
```

## 36.字符串加密!

中途想到怎么做了，但是却又没完全想明白。借助LinkedHashSet的特性，把key(或者叫password)添加进去，然后循环添加`'a'~'z'`，这样最后LinkedHashSet其实就保存的是对应的密码信息，为了方便选择，所以放入list中

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String password = line;
            line = br.readLine();
            System.out.println(encrypt(line,password));
        }
    }
    
    private static String encrypt(String line, String password){
        char[] chars = password.toLowerCase().toCharArray();
        LinkedHashSet<Character> set = new LinkedHashSet<>();
        for(int i = 0;i < chars.length;++i){
            set.add(chars[i]);
        }
        int k = 0;
        while(set.size() < 26){
            set.add((char)('a' + k));
            k++;
        }
        ArrayList<Character> list = new ArrayList<>(set);
        StringBuilder sb = new StringBuilder();
        char[] strs = line.toCharArray();
        for(int i = 0;i < strs.length;++i){
            //因为字符串保证了只有小写，所以不用做判断，否则需要判断空格之类的
            char c = (char)list.get(strs[i] - 'a');
            sb.append(c);
        }
        return sb.toString();
    }
}
```



## 37.统计兔子总数×

这个问题，可以递归，也可以递推

首先，兔子分四种，新生的，一月大，两个月大，三个月及以上，推进到下一个月之后，会出现的变化

三月及以上 = 原本的三月及以上（题目假设兔子不会死） + 原本两个月大的（长大了）

两个月大 = 一个月大的（长大了）

一个月大的 = 新生的

新生的 = 下个月即将出生的兔子，所以是现在三个月大的 + 两个月大的（因为下个月它们也三月了）

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(getRabbit(Integer.parseInt(line)));
        }
    }
    
    private static int getRabbit(int month){
        int oneMonth = 1;
        int twoMonth = 0;
        int threeOrMore = 0;
        int newRabbit = 0;
        for(int i = 2;i <= month;i++){
            threeOrMore += twoMonth;
            twoMonth = oneMonth;
            oneMonth = newRabbit;
            newRabbit = threeOrMore+twoMonth;
        }
        
        return oneMonth+twoMonth+threeOrMore;
    }
}
```

## 38.求小球落地5次后距离和高度√

很简单的一个题，随便算算就好了。而且这里是5，如果换成n次，就把循环的 5变成n就行了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            double init = Double.parseDouble(line);
            double height = 0;
            double sum = 0;
            for(int i = 1;i <= 5;++i){
                sum = sum + init + init/2;
                height = init/2;
                init = height;
            }
            System.out.println(sum-height);
            System.out.println(height);
        }
    }
}
```

## 39.判断两个IP是否属于同一子网√

没什么难度，一个个去判断就好了

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String ip1 = br.readLine();
            String ip2 = br.readLine();
            if(!isValidMask(line) || !isValidIP(ip1) || !isValidIP(ip2)){
                System.out.println(1);
                continue;
            }
            if(isSameSub(line,ip1,ip2)){
                System.out.println(0);
            }else{
                System.out.println(2);
            }
        }
    }
    
    private static boolean isValidMask(String mask){
        String[] strs = mask.split("\\.");
        int[] ints = new int[4];
        if(strs.length != 4) return false;
        for(int i = 0;i < 4;++i){
            ints[i] = Integer.parseInt(strs[i]);
            if(ints[i] <0 || ints[i] > 255) return false;
        }
        String str = string2Binary(mask);
        int i = 0;
        while(str.charAt(i) == '1'){
            i++;
        }
        for(;i<32;++i){
            if(str.charAt(i) == '1') return false;
        }
        return true;
    }
    
    private static boolean isValidIP(String ip){
        String[] strs = ip.split("\\.");
        int[] ints = new int[4];
        if(strs.length != 4) return false;
        for(int i = 0;i < 4;++i){
            ints[i] = Integer.parseInt(strs[i]);
            if(ints[i] <0 || ints[i] > 255) return false;
        }
        return true;
    }
    
    private static boolean isSameSub(String mask,String ip1,String ip2){
        mask = string2Binary(mask);
        ip1 = string2Binary(ip1);
        ip2 = string2Binary(ip2);
        ip1 = ipAndMask(mask,ip1);
        ip2 = ipAndMask(mask,ip2);
        return ip1.equals(ip2);
    }
    
    private static String string2Binary(String ip){
        String[] strs = ip.split("\\.");
        int[] ips = new int[4];
        for(int i = 0;i < strs.length;++i){
            ips[i] = Integer.parseInt(strs[i]);
        }
        StringBuilder sb = new StringBuilder();
        for(int i = 0;i < 4;++i){
            String s = Integer.toBinaryString(ips[i]);
            while(s.length() < 8){
                s = "0" + s;
            }
            sb.append(s);
        }
        return sb.toString();
    }
    
    private static String ipAndMask(String mask,String ip){
        StringBuilder sb = new StringBuilder();
        for(int i = 0;i < 32;++i){
            if(mask.charAt(i) == '1' && ip.charAt(i) == '1') sb.append("1");
            else sb.append("0");
        }
        return sb.toString();
    }
}
```

## 40.统计字符√

也不知道出现的意义是什么

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int numC = 0, numS= 0, numI = 0, numO = 0;
            for(int i = 0;i < line.length();++i){
                char c = line.charAt(i);
                if((c>='a'&&c<='z') || (c>='A'&&c<='Z')){
                    numC++;
                }else if(c >= '0' && c <= '9'){
                    numI++;
                }else if(c == ' '){
                    numS++;
                }else{
                    numO++;
                }
            }
            System.out.println(numC);
            System.out.println(numS);
            System.out.println(numI);
            System.out.println(numO);
        }
    }
}
```

## 41.称砝码！

本来我是考虑到了这种情况，但是我想，复杂度这么高，可能是错了，于是没有继续往下，结果好多人都是用O(n^3)做的啊

先加入0，然后开始取砝码，取第一个砝码，有x种情况，全部存入set，然后取下一个砝码，这个砝码所能造成的重量，是前面所有情况，然后放入x个它之后存在的情况

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null && !line.equals("")){
            int n = Integer.parseInt(line);
            String[] strs = br.readLine().split(" ");
            String[] strs1 = br.readLine().split(" ");
            int[] ms = new int[n];
            int[] xs = new int[n];
            for(int i = 0;i < n;++i){
                ms[i] = Integer.parseInt(strs[i]);
                xs[i] = Integer.parseInt(strs1[i]);
            }
            System.out.println(getPossible(n,ms,xs));
        }
    }
    
    private static int getPossible(int n,int[] ms,int[] xs){
        HashSet<Integer> set = new HashSet<>();
        set.add(0);
        for(int i = 0;i < n;++i){//每一种砝码
          //获得之前的情况
            ArrayList<Integer> list = new ArrayList<>(set);
            for(int j = 1;j <= xs[i];++j){//这次的可能性就是放入x个砝码
                for(int k = 0;k < list.size();++k){
                    set.add(list.get(k) + ms[i] * j);//放入x个和对应之前对应的所有情况，即k*x种情况
                }
            }
        }
        return set.size();
    }
}
```

## 42.学英语×

这个题和蔚来笔试差不多啊，但是好像英语更方便一点？

一个比较傻逼的问题，题解里面的19，是nighteen，我感觉这是一个以前用牛客考试的防作弊点？

方法：每次取后三个，因为每三个读法是一样的，然后单位用power来控制。

```java
import java.io.*;
import java.util.*;

public class Main{
    static String[] NUMS = {"zero", "one", "two", "three", "four", "five", "six", "seven",
                            "eight", "nine", "ten", "eleven", "twelve", "thirteen", "fourteen",
                            "fifteen", "sixteen", "seventeen", "eighteen", "nineteen", "twenty"};
    static String[] TENS = {"zero", "ten", "twenty", "thirty", "forty", "fifty",
                            "sixty", "seventy", "eighty", "ninety"};
    static String[] POWER = {"", "hundred", "thousand", "million", "billion"};
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int num = Integer.parseInt(line);
            System.out.println(num2String(num));
        }
    }
    
    private static String num2String(int num){
        
        ArrayList<String> list = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        int power = 1;//单位
        while(num != 0){
            if(power != 1){
                list.add(POWER[power]);
            }
            int t = num % 1000;//取后三位
            if(t % 100 <= 20){//如果后两位小于20，需要直接读
                if(t % 100 != 0){//但是如果是0就不用读了
                    list.add(NUMS[t%100]);
                }
                if(t/100 != 0){
                    if(t % 100 != 0){
                        list.add("and");
                    }
                    list.add(POWER[1]);
                    list.add(NUMS[t/100]);
                }
            }else {
                if(t%10 != 0){//有个位
                    list.add(NUMS[t%10]);
                }
                t/=10;
                if(t%10 != 0){//有十位
                    list.add(TENS[t%10]);
                }
                t/=10;
                if(t%10 != 0){
                    list.add("and");
                    list.add(POWER[1]);
                    list.add(NUMS[t%10]);
                }
            }
            num /= 1000;
            power++;
        }
        for(int i = list.size()-1;i >=0 ;--i){
            if(i != 0){
                sb.append(list.get(i) + " ");
            }else{
                sb.append(list.get(i));
            }
        }
        return sb.toString();
    }
}
```

## 43.迷宫问题！

这个题，属于是最简单的dfs之一，但是我却因为久了没有练习，中间频繁出问题

不过做过一次之后，或许会有更深的印象了吧

````java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int m = Integer.parseInt(strs[0]);
            int n = Integer.parseInt(strs[1]);
            int[][] maze = new int[m][n];
            for(int i = 0;i < m;++i){
                strs = br.readLine().split(" ");
                for(int j = 0;j < n;++j){
                    maze[i][j] = Integer.parseInt(strs[j]);
                }
            }
            Deque<int[]> deque = new LinkedList<>();
            dfs(deque,maze,0,0);
            StringBuilder sb = new StringBuilder();
            while(!deque.isEmpty()){
                int[] current = deque.pollFirst();
                if(deque.size() > 0){
                    sb.append("(" + current[0] + "," + current[1] + ")" + "\n");
                }else{
                    sb.append("(" + current[0] + "," + current[1] + ")");
                }
            }
            System.out.println(sb.toString());
        }
    }
    
    private static boolean dfs(Deque<int[]> deque,int[][] maze,int i,int j){
        deque.addLast(new int[]{i,j});
        maze[i][j] = 1;
        if(i == maze.length - 1 && j == maze[0].length - 1){
            return true;
        }
        if(j+1 < maze[0].length && maze[i][j+1] == 0){
            if(dfs(deque,maze,i,j+1)){
                return true;
            }
        }
        if(j-1 >= 0 && maze[i][j-1] == 0){
            if(dfs(deque,maze,i,j-1)){
                return true;
            }
        }
        if(i+1 < maze.length && maze[i+1][j] == 0){
            if(dfs(deque,maze,i+1,j)){
                return true;
            }
        }
        if(i-1>=0 && maze[i-1][j] == 0){
            if(dfs(deque,maze,i-1,j)){
                return true;
            }
        }
        deque.removeLast();
        maze[i][j] = 0;
        return false;
    }
}
````

## 44.数独×

和上一道题有相似的思路，甚至其实更简单，只是这个方法并不是最简便的

遍历数独，对于每一个位置，如果是0，那么尝试从1到9填入一个数字，然后进入下一个递归

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] strs = new String[9];
        int[][] sudoku = new int[9][9];
        for(int i = 0;i < 9;++i){
            strs = br.readLine().split(" ");
            for(int j = 0;j < 9;++j){
                sudoku[i][j] = Integer.parseInt(strs[j]);
            }
        }
        StringBuilder sb = new StringBuilder();
        makeFull(sudoku);
        for(int i = 0;i < 9;++i){
            for(int j = 0;j < 9;++j){
                if(j != 8){
                    sb.append(sudoku[i][j] + " ");
                }else{
                    sb.append(sudoku[i][j]);
                }
            }
            if(i != 8) sb.append("\n");
        }
        System.out.println(sb.toString());
    }
    
    private static boolean makeFull(int[][] sudoku){
        for(int i = 0;i < 9;++i){
            for(int j = 0;j < 9;++j){
                if(sudoku[i][j] != 0){
                    continue;
                }
                for(int k = 1;k <=9;++k){
                    if(isValid(i,j,k,sudoku)){
                        sudoku[i][j] = k;
                        if(makeFull(sudoku)){
                            return true;
                        }
                        sudoku[i][j] = 0;
                    }
                }
                return false;
            }
        }
        return true;
    }
    
    private static boolean isValid(int i,int j,int k,int[][] sudoku){
        for(int row = 0;row < 9;++row){
            if(k == sudoku[row][j]) return false;
        }
        for(int col = 0;col < 9;++col){
            if(k == sudoku[i][col]) return false;
        }
        int startRow = (i / 3) * 3;
        int startCol = (j / 3) * 3;
        for(int row = startRow;row < startRow + 3;++row){
            for(int col = startCol;col < startCol + 3;++col){
                if(k == sudoku[row][col]) return false;
            }
        }
        return true;
    }
}
```

## 45.名字的漂亮度√

细节决定成败啊，总之就是要注意细节啊，虽然自己做出来了，但第一次审题不对，导致读取出错，第二次p忘了减，第三次power忘了减，注意细节啊！！！

```java
import java.io.*;
import java.util.Arrays;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        for(int i = 1;i <= n;++i){
            String str = br.readLine();
            System.out.println(piaoLiang(str));
        }
    }
    private static int piaoLiang(String str){
        int[] cs = new int[26];
        int sum = 0;
        char[] strs = str.toLowerCase().toCharArray();
        for(int i = 0;i < strs.length;++i){
            int current = strs[i] - 'a';
            cs[current]++;
        }
        Arrays.sort(cs);
        int power = 26;
        int p = 25;
        while(p >= 0 && cs[p] != 0){
            sum += (cs[p] * power);
            p--;
            power--;
        }
        return sum;
    }
}
```



## 46.截取字符串√

好希望笔试的时候全是入门题啊

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String str = line;
            int k = Integer.parseInt(br.readLine());
//             System.out.println(str.substring(0,k));
            StringBuffer sb = new StringBuffer();
            for(int i = 0;i < k;i++){
                sb.append(str.charAt(i));
            }
            System.out.println(sb.toString());
        }
    }
}
```

## 48.从单向链表中删除指定值节点√(本身确实没47)

这个题，与其说是删除指定节点，不如说考的是链表的合成

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String[] strs = line.split(" ");
            int n = Integer.parseInt(strs[0]);
            int k = Integer.parseInt(strs[strs.length-1]);
            ListNode head = new ListNode(Integer.parseInt(strs[1]));
            ListNode tail = head;
            for(int i = 2;i <= strs.length-2;i += 2){
                int val = Integer.parseInt(strs[i]);
                int prev = Integer.parseInt(strs[i+1]);
                ListNode current = new ListNode(val);
                while(tail != null){
                    if(tail.val == prev){
                        if(tail.next == null){
                            tail.next = current;
                        }else {
                            current.next = tail.next;
                            tail.next = current;
                        }
                    }
                    tail = tail.next;
                }
                tail = head;
            }
            while(head.val == k){
                head = head.next;
            }
            tail = head;
            ListNode temp = tail.next;
            while(temp != null){
                if(temp.val == k){
                    tail.next = temp.next;
                }
                tail = tail.next;
                temp = temp.next;
            }
            System.out.println(head);
        }
    }
}

class ListNode{
    int val;
    ListNode next;
    public ListNode(int val){
        this.val = val;
    }
    public String toString(){
        if(this == null) return null;
        ListNode tail = this;
        StringBuilder sb = new StringBuilder();
        while(tail.next != null){
            sb.append(tail.val + " ");
            tail = tail.next;
        }
        sb.append(tail.val);
        return sb.toString();
    }
}
```

## 50.四则运算×

做过一次，还是不会，你有什么出息啊

递归法：采用一个栈来存储数，遇到符号后进行相应的计算，遇到括号后，对括号内的内容进行递归

```java
import java.io.*;
import java.util.LinkedList;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(getResult(line));
        }
    }
    
    private static int getResult(String line){
        LinkedList<Integer> nums = new LinkedList<>();
        char sign = '+';
        char[] chars = line.toCharArray();
        int num = 0;
        for(int i = 0;i < line.length();++i){
            char current = chars[i];
            if(current >= '0' && current <= '9'){
                num = num * 10 + (current - '0');
            }
            if(current == '('){
                int p = i+1;
                int count = 1;
                while(count>0){
                    if(chars[p] == '(') count++;
                    if(chars[p] == ')') count--;
                    p++;
                }
                num = getResult(line.substring(i+1,p-1));
                i = p -1;
            }
            if(current == '+' || current == '-' || current == '*' || current == '/' || i == chars.length-1){
                if(sign == '+') nums.addLast(num);
                else if(sign == '-') nums.addLast(0-num);
                else if(sign == '*') nums.addLast(nums.pollLast() * num);
                else if(sign == '/') nums.addLast(nums.pollLast() / num);
                sign = current;
                num = 0;
            }
        }
        int ans = 0;
        while(!nums.isEmpty()){
            ans += nums.pollLast();
        }
        return ans;
    }
}
```



## 51.输出链表的倒数第K个节点√

经典的快慢指针，不过这个题还有个坑，即K可能为0，导致最终不合法，所以需要进行判断

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
            int k = Integer.parseInt(br.readLine());
            ListNode head = new ListNode(Integer.parseInt(strs[0]));
            ListNode tail = head;
            for(int i = 1;i < n;++i){
                ListNode temp = new ListNode(Integer.parseInt(strs[i]));
                tail.next = temp;
                tail = temp;
            }
            System.out.println(getLastK(head,k));
        }
    }
    
    private static int getLastK(ListNode head,int k){
        if(k == 0) return 0;
        ListNode first = head;
        ListNode second = head;
        for(int i = 0;i < k;++i){
            second = second.next;
        }
        while(second != null){
            first = first.next;
            second = second.next;
        }
        return first.val;
    }
}

class ListNode{
    int val;
    ListNode next;
    public ListNode(int val){
        this.val = val;
    }
}
```

## 52.计算字符串的编辑距离×

经典的动态规划，力扣上也做过，但是还是不会，刷这么多题有啥意义呢？

对于字符串A[0,.....i-1]和B[0,.....j-1]，要将其中一个变形成为另一个，有两种情况

1. A[i-1] == B[j-1]，那么这个位置是不用懂的，只用考虑A[i-2]以前和B[j-2]以前的元素，即`dp[i][j] = dp[i-1][j-1]`

2. 当不等的时候，那么可以分三种情况

   1. 从A[0,...i-2]编辑到B[0,...j-1]再删除A[i-1]
   2. 从A[0,....i-1]到B[0,...j-2]再删除B[j-1]
   3. 从A[0,...i-1]到B[0,....j-2]再把A[i-1]替换成B[j-1]

   即从上述三种情况得到最小的可能性

3. 边界条件`dp[0][0] = 0`，同时两个边界`dp[0][j] = j, dp[i][0] = i`

```java
import java.io.*;
import java.util.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            String str = br.readLine();
            System.out.println(getStringDistance(line,str));
        }
    }
    
    private static int getStringDistance(String s1,String s2){
        if(s1.equals(s2)) return 0;
        int[][] dp = new int[s1.length()+1][s2.length()+1];
        dp[0][0] = 0;
        for(int i = 1;i < dp.length;++i){
            dp[i][0] = i;
        }
        for(int j = 1;j < dp[0].length;++j){
            dp[0][j] = j;
        }
        for(int i = 1;i < dp.length;++i){
            for(int j = 1;j < dp[0].length;++j){
                if(s1.charAt(i-1) == s2.charAt(j-1)) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = Math.min(dp[i-1][j-1]+1,Math.min(dp[i-1][j]+1,dp[i][j-1]+1));
            }
        }
        return dp[s1.length()][s2.length()];
    }
}
```

## 53.杨辉三角的变形×

这个题，看似是简单题，但是如果想要做，就需要找规律，找到了规律确实是简单题，否则，这个问题就是非常复杂的了

根据规律，对于每个行，每4个会有一个循环，如果对4求余得到了1或者3，那么在第二个位置就是偶数，如果得到了0，那么第三个位置是，如果得到2，那么第四个位置是

```java
import java.io.*;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            int n = Integer.parseInt(line);
            System.out.println(getFirstEven(n));
        }
    }
    
    private static int getFirstEven(int n){
        if(n == 1 || n == 2) return -1;
        int ans = -1;
        switch(n%4){
            case 1: ans = 2;break;
            case 2: ans = 4;break;
            case 3: ans = 2;break;
            case 0: ans = 3;break; 
        }
        return ans;
    }
}
```

## 54.表达式求值×

可恶，这真的是个简单题吗？

很明显，首先需要计算括号，对于括号内的计算，可以采用递归的方法。而对于每个数的存储，可以用stack来进行，而计算的方法，则是根据符号来的

```java
import java.io.*;
import java.util.LinkedList;

public class Main{
    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(getResult(line));
        }
    }
    
    private static int getResult(String line){
        LinkedList<Integer> nums = new LinkedList<>();
        char sign = '+';
        char[] chars = line.toCharArray();
        int num = 0;
        for(int i = 0;i < line.length();++i){
            char current = chars[i];
            if(current >= '0' && current <= '9'){
                num = num * 10 + (current - '0');
            }
            if(current == '('){
                int p = i+1;
                int count = 1;
                while(count>0){
                    if(chars[p] == '(') count++;
                    if(chars[p] == ')') count--;
                    p++;
                }
                num = getResult(line.substring(i+1,p-1));
                i = p -1;
            }
            if(current == '+' || current == '-' || current == '*' || current == '/' || i == chars.length-1){
                if(sign == '+') nums.addLast(num);
                else if(sign == '-') nums.addLast(0-num);
                else if(sign == '*') nums.addLast(nums.pollLast() * num);
                else if(sign == '/') nums.addLast(nums.pollLast() / num);
                sign = current;
                num = 0;
            }
        }
        int ans = 0;
        while(!nums.isEmpty()){
            ans += nums.pollLast();
        }
        return ans;
    }
}
```
