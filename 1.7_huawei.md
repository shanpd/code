### 算法

![1651554973256](dp_img\1651554973256.png)

### 一、分而冶之



###                                                二、动态规划

##### 01、 数塔取数问题

一个高度为N的由正整数组成的三角形，从上走到下，求经过的数字和的最大值。 
每次只能走到下一层相邻的数上，例如从第3层的6向下走，只能走到第4层的2或9上。

该三角形第n层有n个数字，例如：

第一层有一个数字：5

第二层有两个数字：8 4

第三层有三个数字：3 6 9

第四层有四个数字：7 2 9 5

最优方案是：5 + 8 + 6 + 9 = 28

注意:上面应该是排列成一个三角形的样子不是竖向对应的，排版问题没有显示成三角形。

状态定义: Fi，j是第i行j列项最大取数和，求第n行Fn，m（0 < m < n）中最大值。

状态转移方程：Fi，j = max{Fi-1,j-1,Fi-1,j}+Ai,jt

```java
import java.util.Scanner;

public class Dp01 {

    /**
     * 数塔取数问题
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        while (sc.hasNext()){

            int n = sc.nextInt();
            int[][] dp = new int[n][n];
            dp[0][0] = sc.nextInt();
            long max = dp[0][0];
            for (int i = 1; i < n; i++) {
                for (int j = 0; j <= i; j++) {
                    int num = sc.nextInt();
                    if(j == 0)
                        dp[i][j] = dp[i-1][j] + num;
                    else
                        dp[i][j] = Math.max(dp[i-1][j-1],dp[i-1][j]) + num;
                    max = Math.max(dp[i][j],max);
                }

            }

            System.out.println(max);
        }

        sc.close();
    }

}

```



##### 02、编辑距离

编辑距离，又称Levenshtein距离（也叫做Edit Distance），是指两个字串之间，由一个转成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。 
例如将kitten一字转成sitting： 
sitten （k->s） 
sittin （e->i） 
sitting （->g） 
所以kitten和sitting的编辑距离是3。俄罗斯科学家Vladimir Levenshtein在1965年提出这个概念。 
给出两个字符串a,b，求a和b的编辑距离。

状态定义:Fi,j表示第一个字符串的前i个字母和第二个字符串的前j个字母需要编辑的次数，求Fn,m，n和m分别是两个字符串的长度。

状态转移方程： 
当Fi,j-1=Fi-1,j时，Fi,j=Fi,j-1； 
当Fi,j-1！=Fi-1,j时，Fi,j=min{Fi-1,j-1,Fi,j-1,Fi-1,j}+1.

```java
import java.util.Scanner;

public class Dp02 {

    /**
     * 编辑距离
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            dp02(sc.nextLine(), sc.nextLine());
        }

        sc.close();
    }

    public static void dp02(String s1, String s2) {

        int[][] dp = new int[s1.length() + 1][s2.length() + 1];
        dp[0][0] = 0;
        for (int i = 1; i < dp.length; i++) {

            dp[i][0] = i;
        }

        for (int i = 0; i < dp[0].length; i++) {

            dp[0][i] = i;
        }

        for (int a = 1; a < dp.length; a++) {

            for (int b = 1; b < dp[0].length; b++) {

                int c = 1;
                if (s1.charAt(a - 1) == s2.charAt(b - 1))
                    c = 0;
                dp[a][b] = Math.min(dp[a - 1][b - 1] + c, Math.min(dp[a - 1][b] + 1, dp[a][b - 1] + 1));
            }

        }

        System.out.println(dp[s1.length()][s2.length()]);
    }

}

```



##### 03、矩阵取数问题

一个N*N矩阵中有不同的正整数，经过这个格子，就能获得相应价值的奖励，从左上走到右下，只能向下向右走，求能够获得的最大价值。例如：3 * 3的方格。

1 3 3 
2 1 3 
2 2 1

能够获得的最大价值为：11。

```java
import java.util.Scanner;

public class Dp03 {

    /**
     * 矩阵取数问题
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()){

            int n = sc.nextInt();
            int[][] dp = new int[n][n];
            for (int a = 0; a < dp.length; a++) {

                for (int b = 0; b < dp[0].length; b++) {

                    int num = sc.nextInt();
                    if(a == 0 && b == 0){
                        dp[a][b] = num;
                    }else if(a == 0 && b > 0){
                        dp[a][b] = dp[a][b-1] + num;
                    }else if(a > 0 && b == 0){
                        dp[a][b] = dp[a-1][b] + num;
                    }else {
                        dp[a][b] = Math.max(dp[a-1][b],dp[a][b-1]) + num;
                    }

                }

            }

            System.out.println(dp[n-1][n-1]);
        }

        sc.close();
    }

}
```



##### 04、背包问题

 在N件物品取出若干件放在容量为W的背包里，每件物品的体积为W1，W2……Wn（Wi为整数），与之相对应的价值为P1,P2……Pn（Pi为整数）。求背包能够容纳的最大价值。 

```java
import java.util.Scanner;

public class Dp04 {

    /**
     * 背包问题
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        while (sc.hasNext()) {

            int n = sc.nextInt();
            int v = sc.nextInt();
            int[] dp = new int[v + 1];
            int[] price = new int[n + 1];
            int[] weight = new int[n + 1];
            long max = 0;
            for (int i = 0; i < n + 1; i++) {
                for (int j = v; j > 0; j--) {
                    if (j - weight[i] > 0)
                        dp[j] = Math.max(dp[j], dp[j - weight[i]] + price[i]);
                    else
                        dp[j] = dp[i];
                }

            }

            for (int i = 0; i < v + 1; i++) {

                max = max > dp[i] ? max : dp[i];
            }

            System.out.println(max);
        }

        sc.close();
    }
}
```



##### 05、最长递增子序列

 给出长度为N的数组，找出这个数组的最长递增子序列。 
(递增子序列是指，子序列的元素是递增的） 
例如：5 1 6 8 2 4 5 10，最长递增子序列是1 2 4 5 10。 

```java
import java.util.Arrays;
import java.util.Scanner;

public class Dp05 {

    /**
     * 最长递增子序列
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            int n = sc.nextInt();
            int[] arr = new int[n];
            for (int i = 0; i < n; i++) {

                arr[i] = sc.nextInt();
            }
            dp05(arr);
        }

        sc.close();
    }

    public static void dp05(int[] arr) {

        int[] dp = new int[arr.length];
        for (int i = 0; i < dp.length; i++) {

            dp[i] = 1;
            for (int j = 0; j < i; j++) {

                if (arr[i] > arr[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }

            }

        }

        Arrays.sort(dp);
        System.out.println(dp[dp.length - 1]);
    }

}
```



##### 06、最大子段和

 N个整数组成的序列a[1],a[2],a[3],…,a[n]，求该序列如a[i]+a[i+1]+…+a[j]的连续子段和的最大值。 
当所给的整数均为负数时和为0。 
例如：-2,11,-4,13,-5,-2，和最大的子段为：11,-4,13。和为20。 

```java
import java.util.Scanner;

public class Dp06 {

    /**
     * 最大连续子串和
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        while (sc.hasNext()) {

            int n = sc.nextInt();
            int[] arr = new int[n];
            for (int i = 0; i < n; i++) {
                arr[i] = sc.nextInt();
            }

            dp06(arr);
        }

        sc.close();
    }

    public static void dp06(int[] arr) {

        int maxSum = Integer.MIN_VALUE;
        int nowSum = 0;
        for (int i : arr) {

            nowSum = nowSum + i;
            if (nowSum > maxSum)
                maxSum = nowSum;
            if (nowSum < 0)
                nowSum = 0;
        }

        System.out.println(maxSum);
    }

}
```



##### 07、最长公共子序列Lcs

给出两个字符串A B，求A与B的最长公共子序列（子序列不要求是连续的）。 
比如两个串为：

abcicba 
abdkscab

ab是两个串的子序列，abc也是，abca也是，其中abca是这两个字符串最长的子序列。

```java
import java.util.Scanner;

public class Dp07 {

    public static void main(String[] args) {

        /**
         * 最长公共子序列Lcs
         */
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            String s1 = sc.nextLine();
            String s2 = sc.nextLine();

            //最长公共子序列Lcs
            dp07(s1, s2);
        }

        sc.close();
    }


    public static void dp07(String s1, String s2) {

        int[][] dp = new int[s1.length() + 1][s2.length() + 1];
        dp[0][0] = 0;
        for (int i = 1; i < dp.length; i++) {
            dp[i][0] = 0;
        }

        for (int i = 0; i < dp[0].length; i++) {
            dp[0][i] = 0;
        }

        for (int a = 1; a < dp.length; a++) {

            for (int b = 1; b < dp[0].length; b++) {

                int c = 0;
                if (s1.charAt(a - 1) == s2.charAt(b - 1))
                    c = 1;
                dp[a][b] = Math.max(dp[a - 1][b - 1] + c, Math.max(dp[a - 1][b], dp[a][b - 1]));
            }
        }

        System.out.println(dp[s1.length()][s2.length()]);
    }
}
```



##### 08、正整数分组

 将一堆正整数分为2组，要求2组的和相差最小。 
例如：1 2 3 4 5，将1 2 4分为1组，3 5分为1组，两组和相差1，是所有方案中相差最少的。 

```java
import java.util.Scanner;

public class Dp08 {

    /**
     * 正整数分组要求两组差最小
     */
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            int n = sc.nextInt();
            long sum = 0;
            int max = 0;
            int[] nums = new int[n];

            for (int i = 0; i < n; i++) {
                nums[i] = sc.nextInt();
                sum += nums[i];
            }

            int[] dp = new int[(int) (sum / 2 + 1)];
            for (int i = 0; i < n; i++) {
                for (int j = (int) (sum / 2); j > 0; j--) {
                    if (j >= nums[i]) {
                        dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
                    }
                }
            }

            //获取数据的大小
            for (int i = 1; i < sum / 2 + 1; i++) {
                max = max > dp[i] ? max : dp[i];
            }
            System.out.println(Math.abs((sum - max) - max));
        }
        sc.close();
    }
}
```

### 三、贪心算法

***

### 四、考试

### 001、【最大N个数与最小N个数的和】

给定一个数组，编写一个函数来计算它的最大N个数与最小N个数的和。你需要对数组进行去重。
说明：
*数组中数字范围[0, 1000]
*最大N个数与最小N个数不能有重叠，如有重叠，输入非法返回-1
*输入非法返回-1
输入描述：
输出描述：
示例1：
输入
5
95 88 83 64 100
2
输出
342

```java
   public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int m = sc.nextInt();
            int[] arr = new int[m];
            for (int i = 0; i < m; i++) {
                arr[i] = sc.nextInt();
            }
            int n = sc.nextInt();
            System.out.println(sp04(arr, n));
        }
        sc.close();
    }

    public static int sp04(int[] arr, int n) {
        if (2 * n > arr.length)
            return -1;
        Arrays.sort(arr);
        int minSum = 0;
        int maxSum = 0;
        for (int i = 0; i < n; i++) {
            minSum += arr[i];
        }
        for (int i = arr.length - 1; i > arr.length - 1 - n; i--) {
            maxSum += arr[i];
        }
        return minSum + maxSum;
    }
```

###002、【事件推送】

同一个数轴X上有两个点的集合A={A , A , …, A }和B={B , B , …, B }，A 和B 均为正整数，A、B已经按照从小到大排好序，A、B均不为空，给定一
个距离R(正整数)，列出同时满足如下条件的所有（A , B ）数对：
1）A <= B
2）A , B 之间的距离小于等于R
3）在满足1）2）的情况下，每个A 只需输出距离最近的B
4）输出结果按A 从小到大的顺序排序
输入描述：
输出描述：
示例1：
输入
4 5 5
1 5 5 10
1 3 8 8 20
输出
1 1
5 8
5 8

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int m = sc.nextInt();
       int n = sc.nextInt();
       int r = sc.nextInt();
       int[] arr1 = new int[m];
       int[] arr2 = new int[n];
       for (int i = 0; i < m; i++) {
           arr1[i] = sc.nextInt();
       }
       for (int i = 0; i < n; i++) {
           arr2[i] = sc.nextInt();

       }
       sp01(arr1, arr2, r);
   }
}

public static void sp01(int[] arr1, int[] arr2, int r) {

   for (int i = 0; i < arr1.length; i++) {
       for (int j = 0; j < arr2.length; j++) {
           if (arr2[j] >= arr1[i] && arr2[j] - arr1[i] <= r) {
               System.out.println(arr1[i] + " " + arr2[j]);
               break;
           }
       }
   }
}
```

###003、【找车位】

停车场有一横排车位，0代表没有停车，1代表有车。至少停了一辆车在车位上，也至少有一个空位没有停车。
为了防剐蹭，需为停车人找到一个车位，使得距停车人的车最近的车辆的距离是最大的，返回此时的最大距离。
输入描述：
1、一个用半角逗号分割的停车标识字符串，停车标识为0或1，0为空位，1为已停车。
2、停车位最多100个。
输出描述：
输出一个整数记录最大距离。
示例1：
输入
1,0,0,0,0,1,0,0,1,0,1
输出
2

```java
public static void sp02(String s) {
   String[] ss = s.split(",");
   int[] dp = new int[ss.length];
   int sum = 0;
   for (int i = 0; i < ss.length; i++) {
       if ("1".equals(ss[i]))
           sum = 0;
       else
           sum += 1;
       dp[i] = sum;
   }
   int max = Arrays.stream(dp).max().getAsInt();
   for (int i = 0; i < dp.length; i++) {
       if (dp[i] == max)
           System.out.println(i - (max / 2));
   }
}
```

### 004、【磁盘容量排序】

磁盘的容量单位常用的有M，G，T这三个等级，它们之间的换算关系为1T = 1024G，1G = 1024M，现在给定n块磁盘的容量，请对它们按从小
到大的顺序进行稳定排序，例如给定5块盘的容量，1T，20M，3G，10G6T，3M12G9M排序后的结果为20M，3G，3M12G9M，1T，10G6T。注意单位可以重复
出现，上述3M12G9M表示的容量即为3M+12G+9M，和12M12G相等。
输入描述：
输入第一行包含一个整数n(2 <= n <= 100)，表示磁盘的个数，接下的n行，每行一个字符串(长度大于2，小于30)，表示磁盘的容量，由一个或多个格式为mv的
子串组成，其中m表示容量大小，v表示容量单位，例如20M，1T，30G，10G6T，3M12G9M。
磁盘容量m的范围为1到1024的正整数，容量单位v的范围只包含题目中提到的M，G，T三种，换算关系如题目描述。
输出描述：
输出n行，表示n块磁盘容量排序后的结果。
示例1：
输入
3
1G
2G
1024M
输出
1G
1024M
2G

```java
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            String[] arr = new String[n];
            for (int i = 0; i < n; i++) {
                arr[i] = sc.next();
            }
            sp03(arr);
        }
        sc.close();
    }

    public static void sp03(String[] arr) {

        int len = arr.length;
        int[] rec = new int[len];
        Pattern p1 = Pattern.compile("\\d*M");
        Pattern p2 = Pattern.compile("\\d*G");
        Pattern p3 = Pattern.compile("\\d*T");
        for (int i = 0; i < len; i++) {
            int sum_m = 0;
            int sum_g = 0;
            int sum_t = 0;
            Matcher m1 = p1.matcher(arr[i]);
            while (m1.find()) {
                String g1 = m1.group();
                String s1 = g1.substring(0, g1.length() - 1);

                sum_m += Integer.parseInt(s1);
            }

            Matcher m2 = p2.matcher(arr[i]);
            while (m2.find()) {
                String g2 = m2.group();
                String s2 = g2.substring(0, g2.length() - 1);
                sum_g += (Integer.parseInt(s2) * 1024);
            }

            Matcher m3 = p3.matcher(arr[i]);
            while (m3.find()) {
                String g3 = m3.group();
                String s3 = g3.substring(0, g3.length() - 1);
                sum_t += (Integer.parseInt(s3) * 1024 * 1024);
            }

            rec[i] = sum_m + sum_g + sum_t;
        }

        List<Integer> list = new ArrayList<>();
        for (int i : rec) {
            if (!list.contains(i))
                list.add(i);
        }

        for (int i : list) {
            for (int j = 0; j < rec.length; j++) {
                if (i == rec[j])
                    System.out.println(arr[j]);
            }
        }
}
```

### 005、【数字涂色】

疫情过后，希望小学终于又重新开学了，三年二班开学第一天的任务是将后面的黑板报重新制作。黑板上已经写上了N个正整数，同学们需要给这每个
数分别上一种颜色。为了让黑板报既美观又有学习意义，老师要求同种颜色的所有数都可以被这种颜色中最小的那个数整除。现在请你帮帮小朋友们，算算最少需要
多少种颜色才能给这N个数进行上色。
输入描述：
第一行有一个正整数N，其中 。
第二行有N个int型数(保证输入数据在[1,100]范围中)，表示黑板上各个正整数的值。
输出描述：
输出只有一个整数，为最少需要的颜色种数
示例1：
输入
3
2 4 6
输出
1
342

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }
       sp05(arr);
   }
   sc.close();
}

public static void sp05(int[] arr) {

   List<Integer> list = new ArrayList<>();
   list.add(arr[0]);
   for (int i = 1; i < arr.length; i++) {
       for (int j = 0; j < list.size(); j++) {
           if (arr[i] % list.get(j) == 0)
               break;
           if (j == list.size() - 1 && arr[i] % list.get(j) != 0)
               list.add(arr[i]);
       }
   }
   System.out.println(list.size());
}
```

### 006、【两数之和绝对值最小】

给定一个从小到大的有序整数序列（存在正整数和负整数）数组 nums ，请你在该数组中找出两个数，其和的绝对值(|nums[x]+nums[y]|)
为最小值，并返回这个绝对值。
每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
输入描述：
一个通过空格分割的有序整数序列字符串，最多1000个整数，且整数数值范围是 -65535~65535。
输出描述：
两数之和绝对值最小值
示例1：
输入
-3 -1 5 7 11 15
输出
2

```java
   public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String[] arr = sc.nextLine().split(" ");
       sp06(arr);
   }
   sc.close();
}

public static void sp06(String[] arr) {

   int min = Integer.MAX_VALUE;
   for (int i = 0; i < arr.length - 1; i++) {
       for (int j = i + 1; j < arr.length; j++) {
           int temp = Math.abs(Integer.parseInt(arr[i]) + Integer.parseInt(arr[j]));
           if (temp < min)
               min = temp;
       }
   }
   System.out.println(min);
}
```

### 007、【玩牌高手】

给定一个长度为n的整型数组，表示一个选手在n轮内可选择的牌面分数。选手基于规则选牌，请计算所有轮结束后其可以获得的最高总分数。选择规则如下：
1、在每轮里选手可以选择获取该轮牌面，则其总分数加上该轮牌面分数，为其新的总分数。
2、选手也可不选择本轮牌面直接跳到下一轮，此时将当前总分数还原为3轮前的总分数，若当前轮次小于等于3（即在第1、2、3轮选择跳过轮次），则总分数置为
0。
3、选手的初始总分数为0，且必须依次参加每一轮。
输入描述：
第一行为一个小写逗号分割的字符串，表示n轮的牌面分数，1<= n <=20。
分数值为整数，-100 <= 分数值 <= 100。
不考虑格式问题。
输出描述：
所有轮结束后选手获得的最高总分数。
示例1：
输入
1,-5,-6,4,3,6,-2
输出
11

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.nextLine();
       sp07(s);
   }
   sc.close();
}

public static void sp07(String s) {

   String[] arr = s.split(",");
   int nowSum = 0, count = 0, sum_3 = 0;
   for (String s1 : arr) {
       nowSum += Integer.parseInt(s1);
       count++;

       if (nowSum < 0 && count < 4) {
           nowSum = 0;
           count = 0;
       } else if (nowSum < 0 && count >= 4) {
           nowSum = sum_3;
           count = 3;
       }

       if (count == 3) {
           sum_3 = nowSum;
       }
   }
   System.out.println(nowSum);
}
```

### 008、【考勤信息】

公司用一个字符串来表示员工的出勤信息：
absent：缺勤
late：迟到
leaveearly：早退
present：正常上班
现需根据员工出勤信息，判断本次是否能获得出勤奖，能获得出勤奖的条件如下：
缺勤不超过一次；没有连续的迟到/早退；任意连续7次考勤，缺勤/迟到/早退不超过3次
输入描述：
用户的考勤数据字符串，记录条数 >= 1；输入字符串长度<10000；不存在非法输入
如：
2
present
present absent present present leaveearly present absent
输出描述：
根据考勤数据字符串，如果能得到考勤奖，输出"true"；否则输出"false"，对于输入示例的结果应为：
true false
示例1：
输入
2
present
present present
输出
true true

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String[] arr = sc.nextLine().split(" ");
       System.out.println(sp08(arr));
   }
   sc.close();
}

public static boolean sp08(String[] arr) {

   int a = 0;
   for (int i = 0; i < arr.length; i++) {
       if ("absent".equals(arr[i]))
           a++;
       if (a > 1)
           return false;
   }
   for (int i = 0; i < arr.length - 6; i++) {
       int b = 0;
       for (int j = i; j < i + 7; j++) {
           if ("late".equals(arr[j]) || "leaveearly".equals(arr[j]) || "absent".equals(arr[j]))
               b++;
           if (b > 3)
               return false;
       }
   }
   return true;
}
```

###009 服务失效判断

a8-a1,a1-a2,a5-a6,a2-a3
a5,a2

```java
public static void main(String[] args) {
   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String[] s1 = sc.nextLine().split(",");
       String[][] ss = new String[s1.length][2];
       for (int i = 0; i < s1.length; i++) {
           String[] si = s1[i].split("-");
           for (int j = 0; j < 2; j++) {
               ss[i][j] = si[j];
           }
       }
       String[] s = sc.nextLine().split(",");
       sp09(ss, s);
   }
   sc.close();
}

public static void sp09(String[][] ss, String[] s) {
   ArrayList<String> list = new ArrayList<>();
   for (String s1 : s) {
       list.add(s1);
   }
   //这里有问题
   for (int i = 0; i < ss.length; i++) {
       if (list.contains(ss[i][1]) && !list.contains(ss[i][0])) {
           list.add(ss[i][0]);
           i = -1;
       }
   }
   ArrayList<String> pass = new ArrayList<>();
   for (int i = 0; i < ss.length; i++) {
       for (int j = 0; j < ss[i].length; j++) {
           if (!list.contains(ss[i][j]))
               pass.add(ss[i][j]);
       }
   }
   if (pass.isEmpty()) {
       System.out.print(",");
   } else {
       for (int i = 0; i < pass.size(); i++) {
           if (i == (pass.size() - 1))
               System.out.print(pass.get(i));
           else
               System.out.print(pass.get(i) + ",");
       }
   }
}
```

###010 、【字符匹配】

给你一个字符串数组（每个字符串均由小写字母组成）和一个字符规律（由小写字母和.和*组成），识别数组中哪些字符串可以匹配到字符规律上。
'.' 匹配任意单个字符，'*' 匹配零个或多个任意字符；判断字符串是否匹配，是要涵盖整个字符串的，而不是部分字符串。
输入描述：
第一行为空格分割的多个字符串，1<单个字符串长度<100，1<字符串个数<100
第二行为字符规律，1<=字符规律长度<=50
不需要考虑异常场景
输出描述：
匹配的字符串在数组中的下标（从0开始），多个匹配时下标升序并用,分割，若均不匹配输出-1
示例1：
输入
ab aab abacd
.*
输出
0,1,2

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String[] arr = sc.nextLine().split(" ");
       String reg = sc.nextLine();
       sp10(arr, reg);
   }
   sc.close();
}

public static void sp10(String[] arr, String reg) {

   for (int i = 0; i < arr.length; i++) {
       Pattern p = Pattern.compile(reg);
       Matcher m = p.matcher(arr[i]);
       if (m.find() && i == (arr.length - 1)) {
           System.out.print(i);
       } else if (m.find()) {
           System.out.print(i + ",");
       }
   }
}
```

### 011、【分苹果】

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }
       sp(arr);
   }
   sc.close();
}

public static void sp(int[] arr) {

   int temp = 0;
   int sum = 0;
   for (int i : arr) {
       temp ^= i;
       sum += i;
   }
   if (temp != 0) {
       System.out.println(-1);
   } else {
       Arrays.sort(arr);
       System.out.println(sum - arr[0]);
   }
}
```

### 012、打印机任务排序

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String[] arr = sc.nextLine().split(",");
       sp12(arr);
   }
   sc.close();
}

public static void sp12(String[] arr) {

   ArrayList<Integer> list = new ArrayList<>();
   for (String s : arr) {
       int temp = Integer.parseInt(s);
       if (!list.contains(temp))
           list.add(temp);
   }
   Collections.sort(list);
   int count = 0;
   for (int a = list.size() - 1; a >= 0; a--) {
       for (int i = 0; i < arr.length; i++) {

           if (list.get(a) == Integer.parseInt(arr[i]) && count == (list.size() - 1)) {
               System.out.print(i);
           } else if (list.get(a) == Integer.parseInt(arr[i])) {
               ++count;
               System.out.print(i + ",");
           }
       }
   }
}
```

### 013、【找朋友】

在学校中，N个小朋友站成一队， 第i个小朋友的身高为height[i]，
第i个小朋友可以看到的第一个比自己身高更高的小朋友j，那么j是i的好朋友(要求j > i)。
请重新生成一个列表，对应位置的输出是每个小朋友的好朋友位置，如果没有看到好朋友，请在该位置用0代替。
小朋友人数范围是 [0, 40000]。
输入描述：
第一行输入N，N表示有N个小朋友
第二行输入N个小朋友的身高height[i]，都是整数
输出描述：
输出N个小朋友的好朋友的位置
示例1：
输入
2
100 95
输出
0 0

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }
       sp14(arr);
   }
   sc.close();
}

public static void sp14(int[] arr) {

   int[] rec = new int[arr.length];
   for (int i = 0; i < arr.length - 1; i++) {
       for (int j = i + 1; j < arr.length; j++) {
           if (j == arr.length - 1 && arr[j] <= arr[i])
               rec[i] = 0;
           else
               rec[i] = j;
       }
   }
   for (int i : rec) {
       System.out.print(i + " ");
   }
}
```

### 014、【寻找身高相近的小朋友】

小明今年升学到小学一年级，来到新班级后发现其他小朋友们身高参差不齐，然后就想基于各小朋友和自己的身高差对他们进行排序，请
帮他实现排序。
输入描述：
第一行为正整数H和N，0<H<200，为小明的身高，0<N<50，为新班级其他小朋友个数。
第二行为N个正整数H1-HN，分别是其他小朋友的身高，取值范围0<Hi<200（1<=i<=N），且N个正整数各不相同。
输出描述：
输出排序结果，各正整数以空格分割。和小明身高差绝对值最小的小朋友排在前面，和小明身高差绝对值最大的小朋友排在最后，如果两个小朋友和小明身高差一
样，则个子较小的小朋友排在前面。
输入
100 10
95 96 97 98 99 101 102 103 104 105
输出
99 101 98 102 97 103 96 104 95 105

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int h = sc.nextInt();
       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }
       sp15(h, arr);
   }
   sc.close();
}

public static void sp15(int h, int[] arr) {

   List<Integer> list = new ArrayList<Integer>();
   for (int i : arr) {
       if (!list.contains(Math.abs(i - h)))
           list.add(Math.abs(i - h));
   }
   Collections.sort(list);
   List<Integer> rec = new ArrayList<Integer>();
   for (Integer integer : list) {
       List<Integer> temp = new ArrayList<Integer>();
       for (int i : arr) {
           if (Math.abs(i - h) == integer)
               temp.add(i);
       }
       Collections.sort(temp);
       rec.addAll(temp);
   }
   for (Integer integer : rec) {
       System.out.print(integer + " ");
   }
}
```

### 015、【数组组成的最小数字】

给定一个整型数组，请从该数组中选择3个元素组成最小数字并输出（如果数组长度小于3，则选择数组中所有元素来组成最小数字）。
输入描述：
一行用半角逗号分割的字符串记录的整型数组，0 < 数组长度 <= 100，0 < 整数的取值范围 <= 10000。
输出描述：
由3个元素组成的最小数字，如果数组长度小于3，则选择数组中所有元素来组成最小数字。
备注：
示例1：
输入
21,30,62,5,31
输出
21305

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String[] arr = sc.nextLine().split(",");
       System.out.println(sp16(arr));
   }
   sc.close();
}

public static int sp16(String[] arr) {
   int temp1 = 0, temp2 = 0, temp3 = 0;
   if (arr.length < 3) {
       if (arr.length == 1) {
           temp1 = Integer.parseInt(arr[0]);
           return temp1;
       } else {
           temp1 = Integer.parseInt(arr[0] + arr[1]);
           temp2 = Integer.parseInt(arr[1] + arr[0]);
           return temp1 < temp2 ? temp1 : temp2;
       }
   } else {
       int[] arri = new int[arr.length];
       for (int i = 0; i < arr.length; i++) {
           arri[i] = Integer.parseInt(arr[i]);
       }
       Arrays.sort(arri);
       int[] rec = new int[6];
       rec[0] = Integer.parseInt("" + arri[0] + arri[1] + arri[2]);
       rec[1] = Integer.parseInt("" + arri[0] + arri[2] + arri[1]);
       rec[2] = Integer.parseInt("" + arri[1] + arri[0] + arri[2]);
       rec[3] = Integer.parseInt("" + arri[1] + arri[2] + arri[0]);
       rec[4] = Integer.parseInt("" + arri[2] + arri[1] + arri[0]);
       rec[5] = Integer.parseInt("" + arri[2] + arri[0] + arri[1]);
       Arrays.sort(rec);
       return rec[0];
   }
}
```

### 016、【一种字符串压缩表示的解压】---未完成

有一种简易压缩算法：针对全部由小写英文字母组成的字符串，将其中连续超过两个相同字母的部分压缩为连续个数加该字母，其他
部分保持原样不变。例如：字符串“aaabbccccd”经过压缩成为字符串“3abb4cd”。 请您编写解压函数，根据输入的字符串，判断其是否为合法压缩过的字符
串，若输入合法则输出解压缩后的字符串，否则输出字符串“!error”来报告错误。
输入描述：
输入一行，为一个ASCII字符串，长度不会超过100字符，用例保证输出的字符串长度也不会超过100字符
输出描述：
若判断输入为合法的经过压缩后的字符串，则输出压缩前的字符串；若输入不合法，则输出字符串“!error”。
示例1：
输入
4dff
输出
ddddff

```java
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            String s = sc.next();
            sp17(s);
        }

        sc.close();

    }

    public static void sp17(String s) {

        boolean matches = s.matches("[a-z0-9]*[a-z]$");
        System.out.println(matches);

    }
```

### 017、【乱序整数序列两数之和绝对值最小】

给定一个随机的整数（可能存在正整数和负整数）数组 nums ，请你在该数组中找出两个数，其和的绝对值
(|nums[x]+nums[y]|)为最小值，并返回这个两个数（按从小到大返回）以及绝对值。
每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
输入描述：
一个通过空格分割的有序整数序列字符串，最多1000个整数，且整数数值范围是 [-65535, 65535]。
输出描述：
两数之和绝对值最小值
示例1：
输入
-1 -3 7 5 11 15
输出
-3 5 2

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String[] arr = sc.nextLine().split(" ");
       sp18(arr);
   }
   sc.close();
}

public static void sp18(String[] arr) {

   long sum = 0;
   ArrayList<Long> list = new ArrayList<>();
   for (int i = 0; i < arr.length - 1; i++) {
       for (int j = i + 1; j < arr.length; j++) {
           sum = Integer.parseInt(arr[i]) + Integer.parseInt(arr[j]);

           if (!list.contains(Math.abs(sum)))
               list.add(Math.abs(sum));
       }
   }
   Collections.sort(list);
   for (int i = 0; i < arr.length - 1; i++) {
       for (int j = i + 1; j < arr.length; j++) {
           sum = Integer.parseInt(arr[i]) + Integer.parseInt(arr[j]);
           if (Math.abs(sum) == list.get(0)) {
               System.out.println(arr[i] + " " + arr[j] + " " + list.get(0));
               return;
           }
       }
   }
}
```

### 018、【停车场车辆统计】

特定大小的停车场，数组cars[]表示，其中1表示有车，0表示没车。车辆大小不一，小车占一个车位（长度1），货车占两个车位（长度2），卡
车占三个车位（长度3），统计停车场最少可以停多少辆车，返回具体的数目。
输入描述：
整型字符串数组cars[]，其中1表示有车，0表示没车，数组长度小于1000。
输出描述：
整型数字字符串，表示最少停车数目。
示例1：
输入
1,0,1
输出
2

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String[] arr = sc.nextLine().split(",");
       sp19(arr);
   }
   sc.close();
}
private static void sp19(String[] arr) {
   int min = 0;
   int count = 0;
   for (int i = 0; i < arr.length; i++) {
       int temp = Integer.parseInt(arr[i]);
       if (temp == 1) {
           ++count;
       } else if (temp == 0) {
           if (count != 0 && count % 3 == 0)
               min = min + (count / 3);
           if (count != 0 && count % 3 != 0)
               min = min + (count / 3) + 1;
           count = 0;
       }
       if (i == arr.length - 1) {
           if (count != 0 && count % 3 == 0)
               min = min + (count / 3);
           if (count != 0 && count % 3 != 0)
               min = min + (count / 3) + 1;
       }
   }
   System.out.println(min);
}
```

### 019、【字符串序列判定】

输入两个字符串S和L，都只包含英文小写字母。S长度<=100，L长度<=500,000。判定S是否是L的有效字串。
判定规则：S中的每个字符在L中都能找到（可以不连续），且S在Ｌ中字符的前后顺序与S中顺序要保持一致。（例如，S="ace"是L="abcde"的一个子序列且有效字
符是a、c、e，而"aec"不是有效子序列，且有效字符只有a、e）
ace
abcde
4

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s1 = sc.nextLine();
       String s2 = sc.nextLine();
       sp20(s1, s2);
   }
   sc.close();
}

public static void sp20(String s1, String s2) {

   int nowIndex = 0;
   for (int i = 0; i < s1.length(); i++) {
       for (int j = nowIndex; j < s2.length(); j++) {
           if (s1.indexOf(i) == s2.indexOf(j)) {
               nowIndex = j;
           }
       }
   }
   System.out.println(nowIndex);
}
```

### 020、【游戏规则】

游戏规则：输入一个只包含英文字母的字符串，字符串中的两个字母如果相邻且相同，就可以消除。
在字符串上反复执行消除的动作，直到无法继续消除为止，此时游戏结束。
输出最终得到的字符串长度。
输入描述：
输出描述：
备注：
示例1：
输入:gg
输出:0

```java
public static void main(String[] args) {
   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.next();
       sp21(s);
   }
   sc.close();
}
public static void sp21(String s) {
   Pattern p = Pattern.compile("(.)\\1");
   while (true) {
       boolean flag = false;
       Matcher m = p.matcher(s);
       while (m.find()) {
           flag = true;
           String g = m.group();
           System.out.println(g);
           s = s.replace(g, "");
       }
       if (!flag)
           break;
   }
   System.out.println(s.length());
}
```

### 021、【矩形相交的面积】---未完成

在坐标系中，给定3个矩形，求相交区域的面积。

### 022、【单词接龙】

单词接龙的规则是：可用于接龙的单词首字母必须要前一个单词的尾字母相同；当存在多个首字母相同的单词时，取长度最长的单词，如果长度也相

等，则取字典序最小的单词；已经参与接龙的单词不能重复使用。
输入描述：
输出描述：
备注：
示例1：
输入
0
6
word
dd
da
dc
dword
d
输出
worddwordda

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int index = sc.nextInt();
       int n = sc.nextInt();
       ArrayList<String> list = new ArrayList<String>();
       for (int i = 0; i < n; i++) {
           list.add(sc.next());
       }
       sp23(list, index);
   }
   sc.close();
}

public static void sp23(ArrayList<String> list, int index) {
   StringBuffer sb = new StringBuffer(list.get(index));
   list.remove(index);
   while (true) {
       boolean flag = false;
       int i = 0;
       String temp = "";
       int del = 0;
       while (i < list.size()) {
           if (sb.charAt(sb.length() - 1) == list.get(i).charAt(0)) {
               flag = true;
               if (list.get(i).length() > temp.length()) {
                   temp = list.get(i);
                   del = i;
               } else if (list.get(i).length() == temp.length()) {
                   if (temp.compareTo(list.get(i)) > 0)
                       temp = list.get(i);
                   del = i;
               }
           }
           i++;
       }
       if (!flag)
           break;
       sb.append(temp);
       list.remove(del);
   }
   System.out.println(sb);
}
```

### 023、【日志排序】

运维工程师采集到某产品现网运行一天产生的日志N条，现需根据日志时间按时间先后顺序对日志进行排序。
日志时间格式为：
H:M:S.N
H表示小时(0-23)，M表示分钟(0-59)，S表示秒(0-59)，N表示毫秒(0-999)
时间可能并没有补齐，也就是说: 01:01:01.001，也可能表示为1:1:1.1
输入描述：
第一行输入一个整数N，表示日志条数，1<=N<=100000
接下来N行输入N个时间
输出描述：
按时间升序排序之后的时间
如果有两个时间表示的时间相同，则保持输入顺序
示例1：
输入
2
01:41:8.9
1:1:09.211
输出
1:1:09.211
01:41:8.9

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       String[] arr = new String[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.next();
       }
       sp24(arr);
   }
   sc.close();
}

public static void sp24(String[] arr) {

   Pattern p = Pattern.compile("[1-9][0-9]*");
   long[] arr0 = new long[arr.length];
   for (int i = 0; i < arr.length; i++) {
       String[] arr1 = arr[i].split("\\.");
       String[] arr2 = arr1[0].split(":");
       long temp = 0;
       for (int j = 0; j < arr2.length; j++) {
           Matcher m = p.matcher(arr2[j]);
           while (m.find()) {
               String g = m.group();
               if (j == 0)
                   temp += Long.parseLong(g) * 3600000;
               if (j == 1)
                   temp += Long.parseLong(g) * 60000;
               if (j == 2)
                   temp += Long.parseLong(g) * 1000;
           }
       }
       Matcher m = p.matcher(arr1[1]);
       while (m.find())
           temp += Long.parseLong(arr1[1]);
       arr0[i] = temp;
   }

   ArrayList<Long> list = new ArrayList<>();
   for (long l : arr0) {
       if (!list.contains(l))
           list.add(l);
   }
   Collections.sort(list);

   for (Long aLong : list) {
       for (int i = 0; i < arr0.length; i++) {
           if (aLong == arr0[i])
               System.out.println(arr[i]);
       }
   }
}
```

### 024、【猴子爬山】

一天一只顽猴想去从山脚爬到山顶，途中经过一个有个N个台阶的阶梯，但是这猴子有一个习惯： 每一次只能跳1步或跳3步，试问猴子通过这个阶梯
输入描述：
输入只有一个整数N（0<N<=50）此阶梯有多少个阶梯
输出描述：
输入只有一个整数N（0<N<=50）此阶梯有多少个阶梯
示例1：
输入
50
输出
122106097

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       System.out.println(sp25(n));
   }
   sc.close();
}

private static int sp25(int n) {

   if (n == 1 || n == 2)
       return 1;
   if (n == 3)
       return 2;
   return sp25(n - 1) + sp25(n - 3);
}
```

### 025、【字符统计及重排】

给出一个仅包含字母的字符串，不包含空格，统计字符串中各个字母（区分大小写）出现的次数，并按照字母出现次数从大到小的顺序输出各个
字母及其出现次数。如果次数相同，按照自然顺序进行排序，且小写字母在大写字母之前。
输入描述：
输入一行，为一个仅包含字母的字符串。
输出描述：
按照字母出现次数从大到小的顺序输出各个字母和字母次数，用英文分号分隔，注意末尾的分号；字母和次数间用英文冒号分隔。
示例1：
输入
xyxyXX
输出
x:2;y:2;X:2;

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String s = sc.next();
       sp26(s);
   }
   sc.close();
}

private static void sp26(String s) {

   LinkedHashMap<Character, Integer> map = new LinkedHashMap<>();
   for (int i = 0; i < s.length(); i++) {
       if (!map.containsKey(s.charAt(i)))
           map.put(s.charAt(i), 1);
       else
           map.put(s.charAt(i), map.get(s.charAt(i)) + 1);
   }

   Set<Map.Entry<Character, Integer>> entries = map.entrySet();

   ArrayList<Integer> list = new ArrayList<>();
   for (Map.Entry<Character, Integer> entry : entries) {
       if (!list.contains(entry.getValue()))
           list.add(entry.getValue());

   }
   Collections.sort(list);
   for (int i = list.size() - 1; i >= 0; i--) {
       for (Map.Entry<Character, Integer> entry : entries) {
           if (list.get(i) == entry.getValue())
               System.out.print(entry.getKey() + ":" + list.get(i) + ";");
       }
   }
}
```

### 026、【数字序列】

输入描述：
输入一个字符串仅包含大小写字母和数字，输入的字符串最大不超过255个字符。
输出描述：
最长的非严格递增连续数字序列的长度
示例1：
输入
abc2234019A334bc
输出
4

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.next();

       sp27(s);
   }

   sc.close();
}

private static void sp27(String s) {

   Pattern p = Pattern.compile("\\d+");
   Matcher m = p.matcher(s);
   int maxLen = 0;
   while (m.find()) {
       String s1 = m.group();
       int nowMax = s1.charAt(0);
       int nowLen = 1;
       for (int i = 1; i < s1.length(); i++) {
           if (nowMax <= s1.charAt(i)) {
               nowMax = s1.charAt(i);
               ++nowLen;
           } else {
               nowMax = s1.charAt(i);
               nowLen = 1;
           }
           if (nowLen > maxLen)
               maxLen = nowLen;
       }
   }
   System.out.println(maxLen);
}
```

### 027、【字符串分割】

给定一个非空字符串S，其被N个‘-’分隔成N+1的子串，给定正整数K，要求除第一个子串外，其余的子串每K个字符组成新的子串，并用‘-’分
隔。对于新组成的每一个子串，如果它含有的小写字母比大写字母多，则将这个子串的所有大写字母转换为小写字母；反之，如果它含有的大写字母比小写字母多，
则将这个子串的所有小写字母转换为大写字母；大小写字母的数量相等时，不做转换。
输入描述：
输入为两行，第一行为参数K，第二行为字符串S。
输出描述：
输出转换后的字符串。
示例1：
输入
3
12abc-abCABc-4aB@
输出
12abc-abc-ABC-4aB-@

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       String s = sc.next();
       sp28(n, s);
   }
   sc.close();

}

public static void sp28(int n, String s) {

   String[] arr = s.split("-");
   ArrayList<String> list = new ArrayList<>();
   list.add(arr[0]);

   for (int i = 1; i < arr.length; i++) {
       String sb = arr[i];
       while (sb.length() > 2) {
           list.add(sb.substring(0, 3));
           sb = sb.substring(3);
       }
       if (sb.length() % 3 != 0)
           list.add(sb);
   }

   System.out.print(list.get(0));
   for (int i = 1; i < list.size(); i++) {
       int up = 0;
       int low = 0;
       for (int j = 0; j < list.get(i).length(); j++) {
           if (65 <= list.get(i).charAt(j) && list.get(i).charAt(j) <= 90)
               up++;
           else if (97 <= list.get(i).charAt(j) && list.get(i).charAt(j) <= 122)
               low++;
       }
       if (up > low)
           System.out.print("-" + list.get(i).toUpperCase());
       else if (up < low)
           System.out.print("-" + list.get(i).toLowerCase());
       else
           System.out.print("-" + list.get(i));
   }
}
```

### 028、【寻找相同子串】

给你两个字符串 t 和 p ，要求从 t 中找到一个和 p 相同的连续子串，并输出该字串第一个字符的下标。
输入描述：
输入文件包括两行，分别表示字符串 t 和 p ，保证 t 的长度不小于 p ，且 t 的长度不超过1000000，p 的长度不超过10000。
输出描述：
如果能从 t 中找到一个和 p 相等的连续子串，则输出该子串第一个字符在t中的下标（下标从左到右依次为1,2,3,…）；如果不能则输出”No”；如果含有多个这样
的子串，则输出第一个字符下标最小的。
示例1：
输入
AVERDXIVYERDIAN
RDXI
输出
4

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String s1 = sc.nextLine();
       String s2 = sc.nextLine();
       sp29(s1, s2);
   }
   sc.close();
}

private static void sp29(String s1, String s2) {

   for (int a = 0, b = a + s2.length(); b < s1.length(); a++, b++) {
       if (s2.equals(s1.substring(a, b))) {
           System.out.println(a + 1);
           return;
       }
   }
   System.out.println("No");
}
```

### 029、【数组去重和排序】

给定一个乱序的数组，删除所有的重复元素，使得每个元素只出现一次，并且按照出现的次数从高到低进行排序，相同出现次数按照第一次出现顺序进行先后排序。
输入描述：
一个数组
输出描述：
去重排序后的数组
备注：
示例1：
输入
1,3,3,3,2,4,4,4,5
输出
3,4,1,2,5

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String[] arr = sc.nextLine().split(",");
       sp30(arr);
   }
   sc.close();
}

private static void sp30(String[] arr) {

   LinkedHashMap<String, Integer> map = new LinkedHashMap<>();

   for (String s : arr) {
       if (!map.containsKey(s))
           map.put(s, 1);
       else
           map.put(s, map.get(s) + 1);
   }
   ArrayList<Integer> list = new ArrayList<>();
   for (Integer value : map.values()) {
       if (!list.contains(value))
           list.add(value);
   }
   Collections.sort(list);
   for (int i = list.size() - 1; i >= 0; i--) {
       int len = 0;
       for (String s : map.keySet()) {
           if (len == map.size() - 1 && map.get(s) == list.get(i)) {
               System.out.print(s);
           } else if (map.get(s) == list.get(i)) {
               System.out.print(s + ",");
           }
           len++;
       }
   }
}
```

### 030、【根据某条件聚类最少交换次数】

给出数字K,请输出所有结果小于K的整数组合到一起的最少交换次数。
组合一起是指满足条件的数字相邻，不要求相邻后在数组中的位置。
数据范围
-100 <= K <= 100
-100 <= 数组中数值<= 100
输入描述：
第一行输入数组：1 3 1 4 0
第二行输入K数值：2
输出描述：
第一行输出最少较好次数：1
备注：
示例1：
输入
1 3 1 4 0
2
输出
1

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);

   while (sc.hasNext()) {
       int m = sc.nextInt();
       int[] arr = new int[m];
       for (int i = 0; i < m; i++) {
           arr[i] = sc.nextInt();
       }
       int n = sc.nextInt();
       sp32(arr, n);
   }
   sc.close();
}

private static void sp32(int[] arr, int n) {

   ArrayList<Integer> list = new ArrayList<>();
   for (int s : arr) {
       if (!list.contains(s))
           list.add(s);
   }
   Collections.sort(list);
   if (2 * n > list.size()) {
       System.out.println(-1);
       return;
   }
   int sum = 0;
   for (int i = 0; i < n; i++) {
       sum = sum + list.get(i) + list.get(list.size() - i - 1);
   }
   System.out.println(sum);
}
```

### 031、【最大花费金额】

双十一众多商品进行打折销售，小明想购买自己心仪的一些物品，但由于受购买资金限制，所以他决定从众多心仪商品中购买三件，而且想尽可能
的花完资金，现在请你设计一个程序帮助小明计算尽可能花费的最大资金数额。
输入描述：
输入第一行为一维整型数组M，数组长度小于100，数组元素记录单个商品的价格，单个商品价格小于1000。
输入第二行为购买资金的额度R，R小于100000。
输出描述：

备注：
示例1：
输入
23,26,36,27
78
输出
76

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.nextLine();
       int limit = sc.nextInt();
       sp37(s, limit);
   }
   sc.close();
}

private static void sp37(String s, int limit) {

   String[] temp = s.split(",");
   int[] arr = new int[temp.length];
   for (int i = 0; i < arr.length; i++) {
       arr[i] = Integer.parseInt(temp[i]);
   }

   ArrayList<Integer> list = new ArrayList<>();

   for (int i = 0; i < arr.length - 2; i++) {
       for (int j = i + 1; j < arr.length - 1; j++) {
           for (int k = 0; k < arr.length; k++) {
               int sum = arr[i] + arr[j] + arr[k];
               if (sum < limit)
                   list.add(sum);
           }
       }
   }

   Collections.sort(list);
   System.out.println(list.get(list.size() - 1));
}
```

### 032、【最大括号深度】

现有一字符串仅由 '('，')'，'{'，'}'，'['，']'六种括号组成。
若字符串满足以下条件之一，则为无效字符串：
①任一类型的左右括号数量不相等；
②存在未按正确顺序（先左后右）闭合的括号。
输出括号的最大嵌套深度，若字符串无效则输出0。
0≤字符串长度≤100000
输入描述：
输出描述：
示例1：
输入
输出

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       String s = sc.nextLine();
       System.out.println(sp38(s));
   }

   sc.close();
}

private static int sp38(String s) {

   int len = s.length();
   if (len % 2 != 0) {
       return 0;
   } else {
       int maxLen = 0, nowLen = 0;
       for (int i = 0; i < s.length(); i++) {
           if (s.charAt(i) == ')' || s.charAt(i) == ']' || s.charAt(i) == '}')
               nowLen = 0;
           else
               nowLen++;
           if (nowLen > maxLen)
               maxLen = nowLen;
       }
       return maxLen;
   }
}
```

### 033、【数组拼接】---未完成 

现在有多组整数数组，需要将它们合并成一个新的数组。合并规则，从每个数组里按顺序取出固定长度的内容合并到新的数组中，取完的内容会删除掉，如果该行不
足固定长度或者已经为空，则直接取出剩余部分的内容放到新的数组中，继续下一行。
输入描述：
第一行是每次读取的固定长度，0<长度<10
第二行是整数数组的数目，0<数目<1000
第3-n行是需要合并的数组，不同的数组用回车换行分隔，数组内部用逗号分隔，最大不超过100个元素
输出描述：
输出一个新的数组，用逗号分隔。
示例1：
输入
3
2
2,5,6,7,9,5,7
1,7,4,3,4
输出
2,5,6,1,7,4,7,9,5,3,4,7

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int l = sc.nextInt();
       int n = sc.nextInt();

       LinkedHashMap<Integer, List> map = new LinkedHashMap<>();
       for (int i = 0; i < n; i++) {
           String[] temp = sc.nextLine().split(",");
           map.put(i, Arrays.asList(temp));
       }

       demo(l, map);

   }

   sc.close();
}

private static void demo(int l, LinkedHashMap<Integer, List> map) {

   Set<Map.Entry<Integer, List>> entries = map.entrySet();
   ArrayList<String> list = new ArrayList<>();
   boolean flag = true;
   while (flag) {
       for (Map.Entry<Integer, List> entry : entries) {
           List value = entry.getValue();
           int len = value.size();
           if (len >= l) {
               for (int i = 0; i < l; i++) {
                   list.add((String) value.get(i));
                   value.remove((String) value.get(i));

               }
           } else if (len > 0 && len < l) {
               for (int i = 0; i < len; i++) {
                   list.add((String) value.get(i));
                   value.remove((String) value.get(i));
               }
           }
       }

       for (int i = 0; i < 0; i++) {

       }
   }
}
```

### 034、【字符串统计】

给定两个字符集合，一个为全量字符集，一个为已占用字符集。已占用的字符集中的字符不能再使用，要求输出剩余可用字符集。
输入描述：
1、输入为一个字符串，一定包含@符号。@前的为全量字符集，@后的字为已占用字符集。
2、已占用字符集中的字符一定是全量字符集中的字符。字符集中的字符跟字符之间使用英文逗号分隔。
3、每个字符都表示为字符加数字的形式，用英文冒号分隔，比如a:1，表示1个a字符。
4、字符只考虑英文字母，区分大小写，数字只考虑正整形，数量不超过100。
5、如果一个字符都没被占用，@标识仍然存在，例如a:3,b:5,c:2@
输出描述：
输出可用字符集，不同的输出字符集之间回车换行。
注意，输出的字符顺序要跟输入一致。不能输出b:3,a:2,c:2
如果某个字符已全被占用，不需要再输出。
示例1：
输入
a:3,b:5,c:2@a:1,b:2
输出
a:2,b:3,c:2

```java
 public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String s = sc.nextLine();
            sp40(s);
        }
        sc.close();

    }

    private static void sp40(String s) {

        String[] arr = s.split("@");

        String[] arr1 = arr[0].split(",");
        String[] arr2 = arr[1].split(",");

        LinkedHashMap<String, Integer> mapAll = new LinkedHashMap<>();
        LinkedHashMap<String, Integer> mapUsed = new LinkedHashMap<>();

        for (int i = 0; i < arr1.length; i++) {
            String[] arr3 = arr1[i].split(":");
            mapAll.put(arr3[0], Integer.parseInt(arr3[1]));
        }

        for (int i = 0; i < arr2.length; i++) {
            String[] arr4 = arr2[i].split(":");
            mapUsed.put(arr4[0], Integer.parseInt(arr4[1]));
        }


        for (String s1 : mapAll.keySet()) {
            if (mapUsed.containsKey(s1)) {
                int temp = mapAll.get(s1) - mapUsed.get(s1);
                mapAll.put(s1, temp);
            }
        }

        StringBuffer sb = new StringBuffer();
        for (String s1 : mapAll.keySet()) {
            if (mapAll.get(s1) > 0)
                sb.append(s1 + ":" + mapAll.get(s1) + ",");
        }

        System.out.println(sb.substring(0, sb.length() - 1));
    }
```

##### 035、3TLV解码

 TLV编码是按[Tag Length Value]格式进行编码的，一段码流中的信元用Tag标识，Tag在码
流中唯一不重复，Length表示信元Value的长度，Value表示信元的值。
 码流以某信元的Tag开头，Tag固定占一个字节，Length固定占两个字节，字节序为小端序。
 现给定TLV格式编码的码流，以及需要解码的信元Tag，请输出该信元的Value。
 输入码流的16机制字符中，不包括小写字母，且要求输出的16进制字符串中也不要包含小写字母；
码流字符串的最大长度不超过50000个字节。
 输入描述:
 输入的第一行为一个字符串，表示待解码信元的Tag；
 输入的第二行为一个字符串，表示待解码的16进制码流，字节之间用空格分隔。
 输出描述:
 输出一个字符串，表示待解码信元以16进制表示的Value。
 示例1：
 输入 

31

32 01 00 AE 90 02 00 01 02 30 03 00 AB 32 31 31 02 00 32 33 33 01 00 CC

输出 

32 33
 说明 需要解析的信元的Tag是31，从码流的起始处开始匹配，Tag为32的信元长度为1（01 00，小端
序表示为1）；第二个信元的Tag是90，其长度为2；第三个信元的Tag是30，其长度为3；第四个信元的
Tag是31，其长度为2（02 00），所以返回长度后面的两个字节即可，即32 33。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Scanner;

public class Spd03 {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            String tag = sc.nextLine();
            String s = sc.nextLine();
            spd03(tag, s);
        }
        sc.close();
    }

    private static void spd03(String tag, String s) {

        String[] arr = s.split(" ");

        HashMap<String, ArrayList<String>> map = new HashMap<>();
        for (int i = 0; i < arr.length; i++) {
            String key = arr[i];;
            int len = Integer.parseInt(arr[i + 1],16) + Integer.parseInt(arr[i + 2], 16);
            i += 2;

            ArrayList<String> list = new ArrayList<>();
            for (int j = i + 1; j < i + 1 + len; j++) {
                list.add(arr[j]);
            }
            map.put(key,list);
            i += len;
        }
        StringBuffer sb = new StringBuffer();
        for (String s1 : map.keySet()) {
            if (tag.equals(s1)) {
                ArrayList<String> list = map.get(s1);
                for (String s2 : list) {
                    sb.append(s2 + " ");
                }
            }
        }
        System.out.println(sb.toString().trim());
    }
}

```

### 036、【分班】

园两个班的小朋友在排队时混在了一起，每位小朋友都知道自己是否与前面一位小朋友是否同班，请你帮忙把同班的小朋友找出来。
小朋友的编号为整数，与前一位小朋友同班用Y表示，不同班用N表示。
输入描述：
输入为空格分开的小朋友编号和是否同班标志。
比如：6/N 2/Y 3/N 4/Y，表示共4位小朋友，2和6同班，3和2不同班，4和3同班。
其中，小朋友总数不超过999，每个小朋友编号大于0，小于等于999。
不考虑输入格式错误问题。
输出描述：
输出为两行，每一行记录一个班小朋友的编号，编号用空格分开。且：
1、编号需要按照大小升序排列，分班记录中第一个编号小的排在第一行。
2、若只有一个班的小朋友，第二行为空行。
3、若输入不符合要求，则直接输出字符串ERROR。
示例1：
输入
1/N 2/Y 3/N 4/Y
输出
1 2
3 4

```java
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String s = sc.nextLine();
            sp42(s);
        }
        sc.close();
    }

    private static void sp42(String s) {

        String[] s1 = s.split(" ");

        ArrayList<Integer> list1 = new ArrayList<>();
        ArrayList<Integer> list2 = new ArrayList<>();
        ArrayList<Integer> temp = list1;
        for (int i = 0; i < s1.length; i++) {
            if (!s1[i].matches("[1-9]\\d*/[NY]")) {
                System.out.println("ERROR");
                return;
            }
            int num = Integer.parseInt(s1[i].substring(0, s1[i].length() - 2));
            if (i != 0) {
                char c = s1[i].charAt(s1[i].length() - 1);
                if (c != 'Y') {
                    if (temp == list1) {
                        temp = list2;
                    } else {
                        temp = list1;
                    }
                }
            }
            temp.add(num);
        }
        for (Integer integer : list1) {
            System.out.print(integer + " ");
        }
        System.out.println();
        for (Integer integer : list2) {
            System.out.print(integer + " ");
        }
    }

```

### 037、 【勾股数元组】

如果3个正整数(a,b,c)满足a + b = c 的关系，则称(a,b,c)为勾股数（著名的勾三股四弦五），为了探索勾股数的规律，我们定义如果勾股数(a,b,c)
之间两两互质（即a与b，a与c，b与c之间均互质，没有公约数），则其为勾股数元祖（例如(3,4,5)是勾股数元祖，(6,8,10)则不是勾股数元祖）。请求出给定范围
[N,M]内，所有的勾股数元祖。
输入描述：
起始范围N，1 <= N <= 10000
结束范围M，N < M <= 10000
输出描述：

1. a,b,c请保证a < b < c,输出格式：a b c；
2. 多组勾股数元祖请按照a升序，b升序，最后c升序的方式排序输出；
3. 给定范围中如果找不到勾股数元祖时，输出”NA”。

示例1：
输入
1
20
输出
3 4 5
5 12 13
8 15 17

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int m = sc.nextInt();
       sp43(n, m);
   }
   sc.close();
}

private static void sp43(int n, int m) {

   ArrayList<String> list = new ArrayList<>();
   for (int i = n; i <= m - 2; i++) {
       for (int j = i + 1; j <= m - 1; j++) {
           for (int k = j + 1; k <= m; k++) {
               boolean flag = true;
               if ((i * i) + (j * j) == (k * k)) {
                   for (int l = 2; l <= i; l++) {
                       if (i % l == 0 && j % l == 0 || i % l == 0 && k % l == 0) {
                           flag = false;
                           break;
                       }
                   }
                   for (int l = 2; l <= k; l++) {
                       if (k % l == 0 && j % l == 0) {
                           flag = false;
                           break;
                       }
                   }
                   if (flag)
                       list.add(i + " " + j + " " + k);
               }
           }
       }
   }
   for (String s : list) {
       System.out.println(s);
   }
}
```

### 038、【用户调度问题】

在通信系统中，一个常见的问题是对用户进行不同策略的调度，会得到不同的系统消耗和性能。
假设当前有n个待串行调度用户，每个用户可以使用A/B/C三种不同的调度策略，不同的策略会消耗不同的系统资源。请你根据如下规则进行用户调度，并返回总的
消耗资源数。
规则：

1. 相邻的用户不能使用相同的调度策略，例如，第1个用户使用了A策略，则第2个用户只能使用B或者C策略。

2. 对单个用户而言，不同的调度策略对系统资源的消耗可以归一化后抽象为数值。例如，某用户分别使用A/B/C策略的系统消耗分别为15/8/17。

3. 每个用户依次选择当前所能选择的对系统资源消耗最少的策略（局部最优），如果有多个满足要求的策略，选最后一个。
  输入描述：
  第一行表示用户个数n
  接下来每一行表示一个用户分别使用三个策略的系统消耗resA resB resC
  输出描述：
  最优策略组合下的总的系统资源消耗数
  备注：
  示例1：
  输入
  3
  15 8 17
  12 20 9
  11 7 5
  输出
  24

  ```java
  public static void main(String[] args) {

     Scanner sc = new Scanner(System.in);
     while (sc.hasNext()) {

         int n = sc.nextInt();
         int[][] dp = new int[n + 1][3];
         for (int i = 0; i < 3; i++) {
             dp[0][i] = 0;
         }

         //动态规化最优值
         for (int i = 1; i < dp.length; i++) {
             for (int j = 0; j < 3; j++) {
                 dp[i][j] = sc.nextInt();
                 int temp = Integer.MAX_VALUE;
                 for (int k = 0; k < 3; k++) {
                     if (j == k)
                         continue;
                     temp = Math.min(temp, dp[i][j] + dp[i - 1][k]);
                 }
                 dp[i][j] = temp;
        }
         }

         System.out.println(Math.min(Math.min(dp[n][0], dp[n][1]), dp[n][2]));
     }
     sc.close();
  }
  ```

  ### 039、【组成最大数】

  小组中每位都有一张卡片，卡片上是6位内的正整数，将卡片连起来可以组成多种数字，计算组成的最大数字。
  输入描述：
  “,”号分割的多个正整数字符串，不需要考虑非数字异常情况，小组最多25个人
  输出描述：
  最大的数字字符串
  示例1：
  输入
  22,221
  输出
  22221

  ```java
  public static void main(String[] args) {

     Scanner sc = new Scanner(System.in);
     while (sc.hasNext()) {
         String s = sc.nextLine();
         sp45(s);
     }

     sc.close();
  }

  private static void sp45(String s) {

     String[] arr = s.split(",");
     String[] rec = new String[arr.length];
     rec[0] = arr[0];
     for (int i = 1; i < rec.length; i++) {
         if (Integer.parseInt(rec[i - 1] + arr[i]) >= Integer.parseInt(arr[i] + rec[i - 1]))
             rec[i] = rec[i - 1] + arr[i];
         else
             rec[i] = arr[i] + rec[i - 1];
     }
     System.out.println(rec[rec.length - 1]);
  }
  ```

  ​

### 040、【火星文计算】

```txt
已知火星人使用的运算符为#、$，其与地球人的等价公式如下：
x#y = 2*x+3*y+4
x$y = 3*x+y+2
1、其中x、y是无符号整数
2、地球人公式按C语言规则计算
3、火星人公式中，$的优先级高于#，相同的运算符，按从左到右的顺序计算
现有一段火星人的字符串报文，请你来翻译并计算结果。
输入描述：
火星人字符串表达式（结尾不带回车换行）
输入的字符串说明： 字符串为仅由无符号整数和操作符（#、$）组成的计算表达式。例如：123#4$5#67$78。
1、用例保证字符串中，操作数与操作符之间没有任何分隔符。
2、用例保证操作数取值范围为32位无符号整数。
3、保证输入以及计算结果不会出现整型溢出。
4、保证输入的字符串为合法的求值报文，例如：123#4$5#67$78
5、保证不会出现非法的求值报文，例如类似这样字符串：
#4$5 //缺少操作数
4$5# //缺少操作数
4#$5 //缺少操作数
4 $5 //有空格
3+4-5*6/7 //有其它操作符
12345678987654321$54321 //32位整数计算溢出
输出描述：
根据输入的火星人字符串输出计算结果（结尾不带回车换行）
示例1：
输入
7#6$5#12
输出
226
```

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.next();

       sp46(s);
   }
   sc.close();
}

private static void sp46(String s) {

   String[] arr = s.split("#");
   for (int i = 0; i < arr.length; i++) {
       if (arr[i].contains("$")) {
           String[] arr1 = arr[i].split("\\$");
           //x$y = 3*x+y+2
           arr[i] = String.valueOf(3 * Long.parseLong(arr1[0]) + Long.parseLong(arr1[1]) + 2);
       }
   }
   long result = Long.parseLong(arr[0]);
   for (int i = 1; i < arr.length; i++) {
       //x#y = 2*x+3*y+4
       result = 2 * result + 3 * Long.parseLong(arr[i]) + 4;
   }

   System.out.print(result);
}
```

### 041、【第k个排列】

给定参数n，从1到n会有n个整数：1,2,3,…,n，这n个数字共有 n! 种排列。
按大小顺序升序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：
"123"
"132"
"213"
"231"
"312"
"321"
给定 n 和 k，返回第 k 个排列。
输入描述：
输入两行，第一行为n，第二行为k，给定 n 的范围是 [1,9]，给定 k 的范围是[1,n!]。
输出描述：
输出排在第k位置的数字。
示例1：
输入
3
3
输出
213

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int index = sc.nextInt();
       sp47(n, index);
   }
   sc.close();
}

public static void sp47(int n, int index) {

   StringBuffer min = new StringBuffer();
   StringBuffer max = new StringBuffer();
   for (int j = 1; j <= n; j++) {
       min.append(j);
       max.append(n - j + 1);
   }
   int count = 0;
   for (int i = Integer.parseInt(min.toString()); i <= Integer.parseInt(max.toString()); i++) {
       for (int j = 1; j <= n; j++) {
           boolean flag = String.valueOf(i).contains(String.valueOf(j));
           if (j == n && flag)
               count++;
           if (!flag)
               break;
       }
       if (count == index) {
           System.out.println(i);
           break;
       }
   }
}
```

###042、 【数字反转打印】

小华是个很有对数字很敏感的小朋友，他觉得数字的不同排列方式有特殊美感。某天，小华突发奇想，如果数字多行排列，第一行1个数，第二行2个，第三行3个，
即第n行有n个数字，并且奇数行正序排列，偶数行逆序排列，数字依次累加。这样排列的数字一定很有意思。聪明的你能编写代码帮助小华完成这个想法吗？
规则总结如下：
a、每个数字占据4个位置，不足四位用‘*’补位，如1打印为1***。
b、数字之间相邻4空格。
c、数字的打印顺序按照正序逆序交替打印,奇数行正序，偶数行逆序。
d、最后一行数字顶格，第n-1行相对第n行缩进四个空格
输入描述：
第一行输入为N，表示打印多少行; 1<=N<=30
输入：2
输出描述：
XXXX1***
3***XXXX2***
备注：
示例1：
输入
2
输出

        1***
    3*** 2***
```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       ps48(n);
   }
   sc.close();
}

public static void ps48(int n) {

   int sum = 1;
   for (int i = 0; i < n; i++) {
       for (int j = n - i - 2; j >= 0; j--) {
           System.out.print("\t");
       }
       if (i % 2 == 0) {
           for (int j = 0; j < i + 1; j++) {
               System.out.print(sum);
               sum++;
               int len = String.valueOf(sum).length();
               if (len < 4) {
                   for (int k = 0; k < 4 - len; k++) {
                       System.out.print("*");
                   }
               }
               System.out.print("\t");
           }
       } else {
           int[] temp = new int[i + 1];
           for (int j = 0; j < i + 1; j++) {
               temp[j] = sum;
               sum++;
           }
           for (int j = temp.length - 1; j >= 0; j--) {
               System.out.print(temp[j]);
               int len = String.valueOf(temp[j]).length();
               if (len < 4) {
                   for (int k = 0; k < 4 - len; k++) {
                       System.out.print("*");
                   }
               }
               System.out.print("\t");
           }

       }
       System.out.println();
   }
}
```

### 043、【路灯照明问题】

在一条笔直的公路上安装了N个路灯，从位置0开始安装，路灯之间间距固定为100米。
每个路灯都有自己的照明半径，请计算第一个路灯和最后一个路灯之间，无法照明的区间的长度和。
输入描述：
第一行为一个数N，表示路灯个数，1<=N<=100000
第二行为N个空格分隔的数，表示路径的照明半径，1<=照明半径<=100000*100
输出描述：
第一个路灯和最后一个路灯之间，无法照明的区间的长度和
示例1：
输入
2
50 50
输出
0

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }
       sp49(n, arr);
   }
   sc.close();
}

public static void sp49(int n, int[] arr) {

   if (n == 1) {
       System.out.println(0);
       return;
   }
   int sum = 0;
   for (int i = 0; i < n - 1; i++) {
       int temp = 100 - arr[i] - arr[i + 1];
       if (temp > 0)
           sum += temp;
   }
   System.out.println(sum);
}
```

### 044、【数据分类】

对一个数据a进行分类，分类方法为：此数据a（四个字节大小）的四个字节相加对一个给定的值b取模，如果得到的结果小于一个给定的值c，则数据a为有效类型，
其类型为取模的值；如果得到的结果大于或者等于c，则数据a为无效类型。
比如一个数据a=0x01010101，b=3，按照分类方法计算（0x01+0x01+0x01+0x01）%3=1，所以如果c=2，则此a为有效类型，其类型为1，如果c=1，则此a为
无效类型；
又比如一个数据a=0x01010103，b=3，按照分类方法计算（0x01+0x01+0x01+0x03）%3=0，所以如果c=2，则此a为有效类型，其类型为0，如果c=0，则此a
为无效类型。
输入12个数据，第一个数据为c，第二个数据为b，剩余10个数据为需要分类的数据，请找到有效类型中包含数据最多的类型，并输出该类型含有多少个数据。
输入描述：
输入12个数据，用空格分隔，第一个数据为c，第二个数据为b，剩余10个数据为需要分类的数据。
输出描述：
输出最多数据的有效类型有多少个数据。
示例1：
输入
3 4 256 257 258 259 260 261 262 263 264 265
输出
3

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int c = sc.nextInt();
       int b = sc.nextInt();
       int[] arr = new int[10];
       for (int i = 0; i < arr.length; i++) {
           arr[i] = sc.nextInt();
       }

       sp50(c, b, arr);
   }
   sc.close();
}

public static void sp50(int c, int b, int[] arr) {

   int aa = 0, bb = 0, cc = 0;
   for (int i = 0; i < arr.length; i++) {
       String s = Integer.toHexString(arr[i]);
       StringBuffer sb = new StringBuffer();
       for (int j = 0; j < 8 - s.length(); j++) {
           sb.append(0);
       }
       sb.append(s);

       ArrayList<String> list = new ArrayList<>();
       for (int a = 0, ab = 2; ab <= 8; a += 2, ab += 2) {
           String temp = sb.substring(a, ab);
           list.add(temp);
       }

       int sum = 0;
       for (String s1 : list) {
           int i1 = Integer.parseInt(s1);
           sum += i1;
       }

       int temp = sum % b;
       if (temp < c && temp == 0)
           aa++;
       if (temp < c && temp == 1)
           bb++;
       if (temp < c && temp == 2)
           cc++;
   }

   int temp = (aa > bb ? aa : bb) > cc ? (aa > bb ? aa : bb) : cc;
   System.out.println(temp);
}
```

### 045、【We Are A Team】

总共有n个人在机房，每个人有一个标号（1 <= 标号 <=n），他们分成了多个团队，需要你根据收到的m条消息判定指定的两个人是否在一个
团队中，具体的：
1、消息构成为：a b c，整数a、b分别代表了两个人的标号，整数c代表指令。
2、c==0代表a和b在一个团队内。
3、c==1代表需要判定a和b的关系，如果a和b是一个团队，输出一行“we are a team”，如果不是，输出一行“we are not a team”。
4、c为其它值，或当前行a或b超出1~n的范围，输出“da pian zi”。
输入描述：
1、第一行包含两个整数n, m(1 <= n, m <= 100000)，分别表示有n个人和m条消息。
2、随后的m行，每行一条消息，消息格式为:a b c (1 <= a, b <= n, 0 <= c <= 1)。
输出描述：
1、c==1时，根据a和b是否在一个团队中输出一行字符串，在一个团队中输出“we are a team”，不在一个团队中输出“we are not a team”。
2、c为其他值，或当前行a或b的标号小于1或者大于n时，输出字符串“da pian zi”。
3、如果第一行n和m的值超出约定的范围时，输出字符串"NULL"。
示例1：
输入
5 6
1 2 0
1 2 1
1 5 0
2 3 1
2 5 1
1 3 2
输出
we are a team
we are not a team
we are a team
da pian zi

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int m = sc.nextInt();
       if (n < 1 || m > 100000) {
           System.out.println("NULL");
           continue;
       }
       String[] arr = new String[m];
       for (int i = 0; i < m; i++) {
           int a = sc.nextInt();
           int b = sc.nextInt();
           int c = sc.nextInt();
           arr[i] = a + " " + b + " " + c;
       }
       sp51(n, m, arr);
   }
   sc.close();
}

public static void sp51(int n, int m, String[] arr) {

   ArrayList<String> result = new ArrayList<>();
   HashSet<Integer> set = new HashSet<>();
   for (String s : arr) {
       String[] s1 = s.split(" ");
       int a = Integer.parseInt(s1[0]);
       int b = Integer.parseInt(s1[1]);
       int c = Integer.parseInt(s1[2]);

       if (c == 0 && a >= 1 && a <= n && b >= 1 && b <= n) {
           set.add(a);
           set.add(b);
       }
   }
   for (String s : arr) {
       String[] s1 = s.split(" ");
       int a = Integer.parseInt(s1[0]);
       int b = Integer.parseInt(s1[1]);
       int c = Integer.parseInt(s1[2]);

       if (c != 0) {
           if (a < 1 || b > n || b < 1 || b > n || c != 1) {
               result.add("da pian zi");
           } else {
               if (set.contains(a) && set.contains(b))
                   result.add("we are a team");
               else
                   result.add("we are not a team");
           }
       }
   }
   for (String s : result) {
       System.out.println(s);
   }
}
```

###046、【数列描述】

有一个数列a[N] (N=60)，从a[0]开始，每一项都是一个数字。数列中a[n+1]都是a[n]的描述。其中a[0]=1。
规则如下：
a[0]:1
a[1]:11(含义：其前一项a[0]=1是1个1，即“11”。表示a[0]从左到右，连续出现了1次“1”）
a[2]:21(含义：其前一项a[1]=11，从左到右：是由两个1组成，即“21”。表示a[1]从左到右，连续出现了两次“1”)
a[3]:1211(含义：其前一项a[2]=21，从左到右：是由一个2和一个1组成，即“1211”。表示a[2]从左到右，连续出现了1次“2”，然后又连续出现了1次“1”)
a[4]:111221(含义：其前一项a[3]=1211，从左到右：是由一个1、一个2、两个1组成，即“111221”。表示a[3]从左到右，连续出现了1次“1”，连续出现了1
次“2”，连续出现了两次“1”)
请输出这个数列的第n项结果（a[n]，0≤n≤59）。
输入描述：
数列的第n项(0≤n≤59)：
4
输出描述：
数列的内容：
111221
示例1：
输入
4
输出
111221

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       sp52(n);
   }
   sc.close();
}
public static void sp52(int n) {

   ArrayList<String> list = new ArrayList<>();
   list.add("1");
   Pattern p = Pattern.compile("(.)\\1*");

   for (int i = 1; i <= n; i++) {

       String pre = list.get(i - 1);
       Matcher m = p.matcher(pre);
       StringBuffer sb = new StringBuffer();
       while (m.find()) {
           String temp = m.group();
           int len = temp.length();
           sb.append(len).append(temp.charAt(0));
       }
       list.add(sb.toString());
   }
   System.out.println(list.get(n));
}
```

### 047、【查找众数及中位数】

1.众数是指一组数据中出现次数量多的那个数，众数可以是多个
2.中位数是指把一组数据从小到大排列，最中间的那个数，如果这组数据的个数是奇数，那最中间那个就是中位数，如果这组数据的个数为偶数，那就把中间的两个
数之和除以2，所得的结果就是中位数
3.查找整型数组中元素的众数并组成一个新的数组，求新数组的中位数
输入描述：
输入一个一维整型数组，数组大小取值范围 0<N<1000，数组中每个元素取值范围 0<E<1000
输出描述：
输出众数组成的新数组的中位数
示例1：
输入
10 11 21 19 21 17 21 16 21 18 15
输出
21

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.nextLine();
       sp53(s);
   }
   sc.close();
}

public static void sp53(String s) {

   String[] arr = s.split("\\s+");

   HashMap<String, Integer> map = new HashMap<>();

   for (String s1 : arr) {
       if (!map.containsKey(s1))
           map.put(s1, 1);
       else
           map.put(s1, map.get(s1) + 1);
   }
   Collection<Integer> values = map.values();
   Integer max = Collections.max(values);

   ArrayList<Integer> list = new ArrayList<>();

   for (String key : map.keySet()) {
       if (map.get(key) == max) {
           for (String s1 : arr) {
               if (key.equals(s1))
                   list.add(Integer.parseInt(s1));
           }
       }
   }
   Collections.sort(list);
   int result = 0;
   if (list.size() % 2 == 0) {
       result = (list.get(list.size() / 2) + list.get(list.size() / 2 - 1)) / 2;
   } else {
       result = list.get((list.size() - 1) / 2);
   }
   System.out.println(result);
}
```

###048、【连续字母长度】

给定一个字符串，只包含大写字母，求在包含同一字母的子串中，长度第 k 长的子串的长度，相同字母只取最长的那个子串。
输入描述：
第一行有一个子串(1<长度<=100)，只包含大写字母。
第二行为 k的值
输出描述：
输出连续出现次数第k多的字母的次数。
备注：
若子串中只包含同一字母的子串数小于k，则输出-1
示例1：
输入
AAAAHHHBBCDHHHH
3
输出
2

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.next();
       int n = sc.nextInt();
       sp55(s, n);
   }
   sc.close();
}

public static void sp55(String s, int n) {

   HashMap<Integer, Character> map = new HashMap<>();
   Pattern p = Pattern.compile("(.)\\1*");
   Matcher m = p.matcher(s);
   while (m.find()) {
       String temp = m.group();
       if (!map.values().contains(temp.charAt(0))) {
           int len = temp.length();
           if (!map.keySet().contains(len))
               map.put(len, temp.charAt(0));
       }
   }
   Set<Integer> set = map.keySet();
   ArrayList<Integer> list = new ArrayList<>(set);

   int result = -1;
   if (list.size() - n >= 0)
       result = list.get(list.size() - n);
   System.out.println(result);
}
```

### 049、【太阳能板最大面积】

给航天器一侧加装长方形或正方形的太阳能板（图中的红色斜线区域），需要先安装两个支柱（图中的黑色竖条），再在支柱的中间部分固定太阳能板。但航天器不
同位置的支柱长度不同，太阳能板的安装面积受限于最短一侧的那根支柱长度。如图：
现提供一组整形数组的支柱高度数据，假设每根支柱间距离相等为1个单位长度，计算如何选择两根支柱可以使太阳能板的面积最大。
输入描述：
10,9,8,7,6,5,4,3,2,1
注：支柱至少有2根，最多10000根，能支持的高度范围1~10^9的整数。柱子的高度是无序的，例子中递减只是巧合。
输出描述：
可以支持的最大太阳能板面积：（10米高支柱和5米高支柱之间）
25
备注：
10米高支柱和5米高支柱之间宽度为5，高度取小的支柱高也是5，面积为25。任取其他两根支柱所能获得的面积都小于25。所以最大的太阳能板面积为25。
示例1：
输入
10,9,8,7,6,5,4,3,2,1
输出
25

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.next();
       sp56(s);
   }
   sc.close();
}

public static void sp56(String s) {

   String[] arr = s.split(",");
   int max = 0;
   for (int i = 0; i < arr.length - 1; i++) {
       int w = 0;
       for (int j = i + 1; j < arr.length; j++) {
           w++;
           int h = Math.min(Integer.parseInt(arr[i]), Integer.parseInt(arr[j]));
           int temp = w * h;
           max = temp > max ? temp : max;
       }
   }
   System.out.println(max);
}
```

### 050、【报数游戏】

100个人围成一圈，每个人有一个编码，编号从1开始到100。他们从1开始依次报数，报到为M的人自动退出圈圈，然后下一个人接着从1开始报数，
直到剩余的人数小于M。请问最后剩余的人在原先的编号为多少？
输入描述：
输入一个整数参数M
输出描述：
如果输入参数M小于等于1或者大于等于100，输出“ERROR!”；否则按照原先的编号从小到大的顺序，以英文逗号分割输出编号字符串
示例1：
输入
3
输出
58,91

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       sp57(n);
   }
   sc.close();
}

public static void sp57(int n) {

   if (n <= 1 || n >= 100) {
       System.out.println("ERROR");
       return;
   }

   LinkedHashMap<Integer, Integer> map = new LinkedHashMap<>();
   for (int i = 1; i <= 100; i++) {
       map.put(i, i);
   }

   StringBuffer sb = new StringBuffer();
   int first = 1;
   int last = 101;
   while (true) {
       if (map.size() < n) {
           for (Integer integer : map.keySet()) {
               sb.append(map.get(integer) + ",");
           }
           break;
       }
       if (first % n != 0) {
           map.put(last, map.get(first));
           last++;
       }
       map.remove(first);
       first++;
   }
   System.out.println(sb.toString().substring(0, sb.length() - 1));
}
```

### 051、【判断一组不等式是否满足约束并输出最大差】

给定一组不等式，判断是否成立并输出不等式的最大差(输出浮点数的整数部分)，要求：1）不等式系数为double类
型，是一个二维数组；2）不等式的变量为int类型，是一维数组；3）不等式的目标值为double类型，是一维数组；4）不等式约束为字符串数组，只能
是：">",">=","<","<=","="，例如,不等式组：
a11*x1+a12*x2+a13*x3+a14*x4+a15*x5<=b1;
a21*x1+a22*x2+a23*x3+a24*x4+a25*x5<=b2;
a31*x1+a32*x2+a33*x3+a34*x4+a35*x5<=b3;
最大差=max{ (a11*x1+a12*x2+a13*x3+a14*x4+a15*x5-b1), (a21*x1+a22*x2+a23*x3+a24*x4+a25*x5-b2),
(a31*x1+a32*x2+a33*x3+a34*x4+a35*x5-b3) }，类型为整数(输出浮点数的整数部分)
输入描述：
输入：a11,a12,a13,a14,a15;a21,a22,a23,a24,a25;a31,a32,a33,a34,a35;x1,x2,x3,x4,x5;b1,b2,b3;<=,<=,<=
输出描述：
true 或者 false, 最大差
示例1：
输入
2.3,3,5.6,7,6;11,3,8.6,25,1;0.3,9,5.3,66,7.8;1,3,2,7,5;340,670,80.6;<=,<=,<=
输出
false 458

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.nextLine();
       sp58(s);
   }

   sc.close();
}

public static void sp58(String s) {

   String[] arr = s.split(";");
   boolean flag = true;
   Double result = Double.MIN_VALUE;

   String[] x = arr[arr.length - 3].split(",");
   String[] b = arr[arr.length - 2].split(",");
   String[] comp = arr[arr.length - 1].split(",");

   for (int i = 0; i < arr.length - 3; i++) {
       String[] temp = arr[i].split(",");
       double sum = 0;

       for (int j = 0; j < temp.length; j++) {
           sum += Double.parseDouble(temp[j]) * Integer.parseInt(x[j]);
       }

       if (comp[i].equals(">")) {
           if (!(sum > Double.parseDouble(b[i])))
               flag = false;
       } else if (comp[i].equals(">=")) {
           if (!(sum >= Double.parseDouble(b[i])))
               flag = false;
       } else if (comp[i].equals("<")) {
           if (!(sum < Double.parseDouble(b[i])))
               flag = false;
       } else if (comp[i].equals("<=")) {
           if (!(sum <= Double.parseDouble(b[i])))
               flag = false;
       } else if (comp[i].equals("=")) {
           if (!(sum == Double.parseDouble(b[i])))
               flag = false;
       }

       double tep = sum - Double.parseDouble(b[i]);
       if (tep > result)
           result = tep;

   }
   System.out.print(flag + "," + (int) Math.floor(result));
}
```

### 052、【分糖果】

小明从糖果盒中随意抓一把糖果，每次小明会取出一半的糖果分给同学们。
当糖果不能平均分配时，小明可以选择从糖果盒中（假设盒中糖果足够）取出一个糖果或放回一个糖果。
小明最少需要多少次（取出、放回和平均分配均记一次），能将手中糖果分至只剩一颗。
输入描述：
抓取的糖果数（<10000000000）：
15
输出描述：
最少分至一颗糖果的次数：
5
备注：
示例1：
输入
15
输出
5

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int n = sc.nextInt();
       sp59(n);
   }
   sc.close();
}

public static void sp59(int n) {

   int times = 0;
   while (true) {
       if (n == 1) {
           break;
       }
       if (n % 2 != 0) {
           int temp = n / 2;
           if (temp / 2 == 0) {
               n -= 1;
           } else {
               n += 1;
           }
           times++;
       }
       n /= 2;
       times++;
   }
   System.out.println(times);
}
```

### 053、【计算面积】

绘图机器的绘图笔初始位置在原点（0, 0），机器启动后其绘图笔按下面规则绘制直线：
1）尝试沿着横向坐标轴正向绘制直线，直到给定的终点值E。
2）期间可通过指令在纵坐标轴方向进行偏移，并同时绘制直线，偏移后按规则1 绘制直线；指令的格式为X offsetY，表示在横坐标X 沿纵坐标方向偏移，offsetY为
正数表示正向偏移，为负数表示负向偏移。
给定了横坐标终点值E、以及若干条绘制指令，请计算绘制的直线和横坐标轴、以及 X=E 的直线组成图形的面积。
输入描述：
首行为两个整数 N E，表示有N条指令，机器运行的横坐标终点值E。
接下来N行，每行两个整数表示一条绘制指令X offsetY，用例保证横坐标X以递增排序方式出现，且不会出现相同横坐标X。
取值范围：0 < N <= 10000, 0 <= X <= E <=20000, -10000 <= offsetY <= 10000。
输出描述：
一个整数，表示计算得到的面积，用例保证，结果范围在0~4294967295内
示例1：
输入
4 10
1 1
2 1
3 1
4 -2
输出
12

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {
       int n = sc.nextInt();
       int x_max = sc.nextInt();
       LinkedHashMap<Integer, Integer> map = new LinkedHashMap<>();
       for (int i = 0; i < n; i++) {
           map.put(sc.nextInt(), sc.nextInt());
       }
       map.put(x_max, 0);
       sp60(map);
   }
   sc.close();
}

public static void sp60(HashMap<Integer, Integer> map) {

   int sum = 0, lastKey = 0, lastValue = 0;
   for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
       int key = entry.getKey();
       int value = entry.getValue();
       sum += ((key - lastKey) * Math.abs(lastValue));
       lastKey = key;
       lastValue = value + lastValue;
   }
   System.out.println(sum);
}
```

### 054、【求解连续数列】

已知连续正整数数列{K}=K1,K2,K3...Ki的各个数相加之和为S，i=N (0<S<100000, 0<N<100000), 求此数列K。
输入描述：
输入包含两个参数，1）连续正整数数列和S，2）数列里数的个数N。
输出描述：
如果有解输出数列K，如果无解输出-1
备注：
示例1：
输入
525 6
输出
85 86 87 88 89 90

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int sum = sc.nextInt();
       int n = sc.nextInt();
       sp61(sum, n);
   }
   sc.close();
}

public static void sp61(int sum, int n) {

   int first = (sum * 2 / n - n + 1) / 2;
   int real = 0;
   for (int i = 0; i < n; i++) {
       real += (first + i);
   }
   if (real == sum) {
       for (int i = 0; i < n; i++) {
           System.out.print(first + i + " ");
       }
   } else {
       System.out.print(-1);
   }
}
```

### 055、【判断字符串子序列】

给定字符串 target和 source, 判断 target 是否为 source 的子序列。
你可以认为 target 和 source 中仅包含英文小写字母。字符串 source可能会很长（长度 ~= 500,000），而 target 是个短字符串（长度 <=100）。
字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"abc"是"aebycd"的一个子序列，
而"ayb"不是）。
请找出最后一个子序列的起始位置。
输入描述：
第一行为target，短字符串（长度 <=100）
第二行为source，长字符串（长度 ~= 500,000）
输出描述：
最后一个子序列的起始位置， 即最后一个子序列首字母的下标
备注：
若在source中找不到target，则输出-1
示例1：
输入
abc
abcaybec
输出
3

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String target = sc.next();
       String source = sc.next();
       sp62(target, source);
   }
   sc.close();
}

private static void sp62(String target, String source) {

   char[] ch_target = target.toCharArray();
   char[] ch_source = source.toCharArray();
   int index = ch_source.length - 1;
   for (int i = ch_target.length - 1; i >= 0; i--) {
       char c = ch_target[i];
       for (int j = index; j >= 0; j--) {
           char c1 = ch_source[j];
           if (c == c1) {
               index = j;
               break;
           } else if (j == 0) {
               index = -1;
               return;
           }
       }
   }
   System.out.println(index);
}
```

### 056、【最长的顺子】

斗地主起源于湖北十堰房县，据传是一位叫吴修全的年轻人根据当地流行的扑克玩法“跑得快”改编的，如今已风靡整个中国，并流行于互联网上。
牌型:
单顺, 又称顺子，最少5张牌，最多12张牌（3⋯A），不能有2，也不能有大小王，不计花色
例如：3-4-5-6-7-8，7-8-9-10-J-Q，3-4-5-6-7-8-9-10-J-Q-K-A
可用的牌 3<4<5<6<7<8<9<10<J<Q<K<A<2 < B(小王)< C(大王)，每种牌除大小王外有4种花色(共有 13X4 + 2 张牌)
输入1. 手上已有的牌 2. 已经出过的牌（包括对手出的和自己出的牌）
输出: 对手可能构成的最长的顺子(如果有相同长度的顺子, 输出牌面最大的那一个)，如果无法构成顺子, 则输出 NO-CHAIN
输入描述：
输入的第一行为当前手中的牌
输入的第二行为已经出过的牌
输出描述：
最长的顺子
示例1：
输入
3-3-3-3-4-4-5-5-6-7-8-9-10-J-Q-K-A
4-5-6-7-8-8-8
输出
9-10-J-Q-K-A

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s1 = sc.nextLine();
       String s2 = sc.nextLine();
       sp63(s1, s2);
   }
   sc.close();
}

private static void sp63(String s1, String s2) {

   String[] arr1 = s1.split("-");
   String[] arr2 = s2.split("-");

   HashMap<String, Integer> map = new HashMap<>();

   for (String s : arr1) {

       if (s.equals("J")) {
           s = "11";
       } else if (s.equals("Q")) {
           s = "12";
       } else if (s.equals("K")) {
           s = "13";
       } else if (s.equals("A")) {
           s = "14";
       }
       if (!map.containsKey(s))
           map.put(s, 1);
       else
           map.put(s, map.get(s) + 1);
   }
   for (String s : arr2) {
       if (s.equals("J")) {
           s = "11";
       } else if (s.equals("Q")) {
           s = "12";
       } else if (s.equals("K")) {
           s = "13";
       } else if (s.equals("A")) {
           s = "14";
       }
       if (!map.containsKey(s))
           map.put(s, 1);
       else
           map.put(s, map.get(s) + 1);
   }

   int lastOne = 0, maxLen = 0, nowLen = 0;
   for (int i = 3; i <= 14; i++) {
       if (map.get(String.valueOf(i)) == 4) {
           nowLen = 0;
       } else {
           nowLen++;
       }
       if (nowLen > maxLen) {
           maxLen = nowLen;
           lastOne = i;
       }
   }

   StringBuffer sb = new StringBuffer();
   if (maxLen >= 5) {
       for (int i = lastOne - maxLen + 1; i <= lastOne; i++) {
           if (i == 11) {
               sb.append("J-");
           } else if (i == 12) {
               sb.append("Q-");
           } else if (i == 13) {
               sb.append("K-");
           } else if (i == 14) {
               sb.append("A-");
           } else {
               sb.append(i + "-");
           }
       }
   } else {
       System.out.println("NO-CHAIN");
       return;
   }
   System.out.println(sb.substring(0, sb.length() - 1));
}
```

### 057、【检查是否存在满足条件的数字组合】

给定一个正整数数组，检查数组中是否存在满足规则的数字组合
规则：
A = B + 2C
输入描述：
第一行输出数组的元素个数。
接下来一行输出所有数组元素，用空格隔开
输出描述：
如果存在满足要求的数，在同一行里依次输出规则里A/B/C的取值，用空格隔开。
如果不存在，输出0。
备注：

1. 数组长度在3-100之间。
2. 数组成员为0-65535，数组成员可以重复，但每个成员只能在结果算式中使用一次。如：数组成员为[0, 0, 1, 5]，0出现2次是允许的，但结果0 = 0 + 2 * 0是不
  允许的，因为算式中使用了3个0。
3. 用例保证每组数字里最多只有一组符合要求的解。
  示例1：
  输入
  4
  2 7 3 0
  输出
  7 3 2

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }

       sp64(arr);
   }
   sc.close();
}

public static void sp64(int[] arr) {

   for (int a = 0; a < arr.length; a++) {
       for (int b = 0; b < arr.length; b++) {
           if (b == a)
               continue;
           for (int c = 0; c < arr.length; c++) {
               if (c == a || c == b)
                   continue;
               if (arr[a] == arr[b] + 2 * arr[c])
                   System.out.println(arr[a] + " " + arr[b] + " " + arr[c]);
           }
       }
   }
}
```

### 058、【关联子串】---需要优化

给定两个字符串str1和str2，如果字符串str1中的字符，经过排列组合后的字符串中，只要有一个字符串是str2的子串，则认为str1是str2的关联子串。
若str1是str2的关联子串，请返回子串在str2的起始位置；
若不是关联子串，则返回-1。
示例1：
输入：str1="abc",str2="efghicabiii" 输出：5
解释：str2包含str1的一种排列组合（"cab")，此组合在str2的字符串起始位置为5（从0开始计数）
示例2：str1="abc",str2="efghicaibii" 输出：-1。
预制条件：
输入的字符串只包含小写字母；
两个字符串的长度范围[1, 100,000]之间
若str2中有多个str1的组合子串，请返回第一个子串的起始位置。
输入描述：
输入两个字符串，分别为题目中描述的str1、str2。
输出描述：
如果str1是str2的关联子串，则返回子串在str2中的起始位置。
如果str1不是str2的关联子串，则返回-1。
若str2中有多个str1的组合子串，请返回最小的起始位置。
备注：
示例1：
输入
abc efghicabiii
输出
5

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String str1 = sc.next();
       String str2 = sc.next();
       sp65(str1, str2);
   }
   sc.close();
}

private static void sp65(String str1, String str2) {

   StringBuffer min = new StringBuffer();
   StringBuffer max = new StringBuffer();

   HashMap<Integer, Character> map = new HashMap<>();

   for (int i = 1; i <= str1.length(); i++) {
       map.put(i, str1.charAt(i - 1));
       min.append(i);
       max.append(str1.length() - i + 1);
   }

   ArrayList<Integer> list = new ArrayList<>();
   for (int i = Integer.parseInt(min.toString()); i <= Integer.parseInt(max.toString()); i++) {
       StringBuffer temp = new StringBuffer();
       for (int j = 1; j <= str1.length(); j++) {
           boolean flag = String.valueOf(i).contains(String.valueOf(j));
           if (flag && j == str1.length()) {
               for (int k = 0; k < String.valueOf(i).length(); k++) {
                   temp.append(map.get(Integer.parseInt(String.valueOf(String.valueOf(i).charAt(k)))));
               }
           }
           if (!flag)
               break;
       }
       if (temp.length() == str1.length() && str2.contains(temp)) {
           String[] split = str2.split(temp.toString());
           list.add(split[0].length());
       }
   }
   if (list.size() > 0) {
       System.out.println(Collections.min(list));
   } else {
       System.out.println(-1);
   }
}
```

### 059、【用连续自然数之和来表达整数】

一个整数可以由连续的自然数之和来表示。给定一个整数，计算该整数有几种连续自然数之和的表达式，且打印出每种表达式。
输入描述：
一个目标整数T (1 <=T<= 1000)
输出描述：
该整数的所有表达式和表达式的个数。如果有多种表达式，输出要求为：
1.自然数个数最少的表达式优先输出
2.每个表达式中按自然数递增的顺序输出，具体的格式参见样例。在每个测试数据结束时，输出一行”Result:X”，其中X是最终的表达式个数。
示例1：
输入
9
输出
9=9
9=4+5
9=2+3+4
Result:3

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int n = sc.nextInt();
       sp66(n);
   }
   sc.close();
}

private static void sp66(int n) {

   int count = 0;
   for (int i = 1; i <= n; i++) {
       int first = (2 * n / i - i + 1) / 2;
       if (first > 0) {
           int temp = first;
           int tempSum = 0;
           for (int j = 1; j <= i; j++) {
               tempSum += temp;
               temp++;
           }

           if (tempSum == n) {
               count++;
               System.out.print(n + "=");
               for (int j = 1; j <= i; j++) {
                   if (j == i)
                       System.out.print(first);
                   else
                       System.out.print(first + "+");
                   first++;
               }
               System.out.println();
           }
       }
   }
   System.out.println("Result:" + count);
}
```

### 060、【流水线】

一个工厂有m条流水线，来并行完成n个独立的作业，该工厂设置了一个调度系统，在安排作业时，总是优先执行处理时间最短的作业。
现给定流水线个数m，需要完成的作业数n, 每个作业的处理时间分别为t1,t2…tn。请你编程计算处理完所有作业的耗时为多少？
当n>m时，首先处理时间短的m个作业进入流水线，其他的等待，当某个作业完成时，依次从剩余作业中取处理时间最短的进入处理。
输入描述：
第一行为2个整数（采用空格分隔），分别表示流水线个数m和作业数n；
第二行输入n个整数（采用空格分隔），表示每个作业的处理时长t1,t2…tn。
0< m,n<100，0<t1,t2…tn<100。
注：保证输入都是合法的。
输出描述：
输出处理完所有作业的总时长
示例1：
输入
3 5
8 4 3 2 10
输出
13

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int m = sc.nextInt();
       int n = sc.nextInt();
       int[] arr = new int[n];
       for (int i = 0; i < n; i++) {
           arr[i] = sc.nextInt();
       }
       sp67(m, n, arr);
   }
   sc.close();
}

private static void sp67(int m, int n, int[] arr) {

   Arrays.sort(arr);

   int[] result = new int[m];
   for (int i = 0; i < n; i++) {
       int temp = i % m;
       result[temp] += arr[i];
   }
   System.out.println(Arrays.stream(result).max().getAsInt());
}
```

###061、【执行时长】

为了充分发挥GPU算力，需要尽可能多的将任务交给GPU执行，现在有一个任务数组，数组元素表示在这1秒内新增的任务个数且每秒都有新增任务，
假设GPU最多一次执行n个任务，一次执行耗时1秒，在保证GPU不空闲情况下，最少需要多长时间执行完成
输入描述：
第一个参数为GPU一次最多执行的任务个数，取值范围[1, 10000]
第二个参数为任务数组长度，取值范围[1, 10000]
第三个参数为任务数组，数字范围[1, 10000]
输出描述：
执行完所有任务最少需要多少秒
示例1：
输入
3
5
1 2 3 4 5
输出
6

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       int n = sc.nextInt();
       int m = sc.nextInt();
       int[] arr = new int[m];
       for (int i = 0; i < m; i++) {
           arr[i] = sc.nextInt();
       }
       sp68(n, arr);
   }
   sc.close();
}

private static void sp68(int n, int[] arr) {

   int times = 0;
   for (int i = 0; i < arr.length - 1; i++) {
       if (arr[i] > n) {
           arr[i + 1] += arr[i] - n;
       }
       times++;
   }
   int temp = arr[arr.length - 1] / n;
   if (arr[arr.length - 1] % n != 0)
       temp++;
   times += temp;
   System.out.println(times);
}
```

### 062、【高矮个子排队】---未完成

现在有一队小朋友，他们高矮不同，我们以正整数数组表示这一队小朋友的身高，如数组{5,3,1,2,3}。
我们现在希望小朋友排队，以“高”“矮”“高”“矮”顺序排列，每一个“高”位置的小朋友要比相邻的位置高或者相等；每一个“矮”位置的小朋友要比相邻的
位置矮或者相等；
要求小朋友们移动的距离和最小，第一个从“高”位开始排，输出最小移动距离即可。
例如，在示范小队{5,3,1,2,3}中，{5, 1, 3, 2, 3}是排序结果。{5, 2, 3, 1, 3} 虽然也满足“高”“矮”“高”“矮”顺序排列，但小朋友们的移动距离大，所以不是最
优结果。
移动距离的定义如下所示：
第二位小朋友移到第三位小朋友后面，移动距离为1，若移动到第四位小朋友后面，移动距离为2；
输入描述：
排序前的小朋友，以英文空格的正整数：
4 3 5 7 8
注：小朋友<100个
输出描述：
排序前的小朋友，以英文空格的正整数：
4 3 5 7 8
注：小朋友<100个
备注：
4（高）3（矮）7（高）5（矮）8（高）， 输出结果为最小移动距离，只有5和7交换了位置，移动距离都是1
示例1：
输入
4 1 3 5 2
输出
4 1 5 2 3

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String[] s = sc.nextLine().split("\\s+");
       int[] arr = new int[s.length];
       for (int i = 0; i < s.length; i++) {
           arr[i] = Integer.parseInt(s[i]);
       }
       sp69(arr);
   }
   sc.close();
}

private static void sp69(int[] arr) {

   for (int i = 1; i < arr.length - 1; i++) {
       if(i % 2 == 0){
           if(arr[i-1] < arr[i] || arr[i] > arr[i+1]){

           }
       }else {

       }
   }
}
```

###063、【按单词下标区间翻转文章内容】

给定一段英文文章片段，由若干单词组成，单词间以空格间隔，单词下标从0开始。
请翻转片段中指定区间的单词顺序并返回翻转后的内容。
例如给定的英文文章片段为"I am a developer"，翻转区间为[0,3]，则输出"developer a am I"。
输入描述：
使用换行隔开三个参数，第一个参数为英文文章内容即英文字符串，第二个参数为待翻转内容起始单词下标，第三个参数为待翻转内容最后一个单词下标。
输出描述：
翻转后的英文文章片段所有单词之间以一个半角空格分隔进行输出
备注：
英文文章内容首尾无空格
示例1：
输入
I am a developer
1
2
输出
I a am developer

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()){
       String[] arr = sc.nextLine().split("\\s+");
       int first = Integer.parseInt(sc.nextLine());
       int last = Integer.parseInt(sc.nextLine());
       sp70(arr, first, last);
   }
   sc.close();
}

private static void sp70(String[] arr, int first, int last) {

   for (int a = first,b = last; a < b; a++,b--) {
       String temp = arr[a];
       arr[a] = arr[b];
       arr[b] = temp;
   }

   StringBuffer sb = new StringBuffer();
   for (String s : arr) {
       sb.append(s + " ");
   }
   System.out.println(sb.toString().trim());
}
```

### 064、【找车位】

停车场有一横排车位，0代表没有停车，1代表有车。至少停了一辆车在车位上，也至少有一个空位没有停车。
为了防剐蹭，需为停车人找到一个车位，使得距停车人的车最近的车辆的距离是最大的，返回此时的最大距离。
输入描述：
1、一个用半角逗号分割的停车标识字符串，停车标识为0或1，0为空位，1为已停车。
2、停车位最多100个。
输出描述：
输出一个整数记录最大距离。
示例1：
输入
1,0,0,0,0,1,0,0,1,0,1
输出
2

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String[] arr = sc.nextLine().split(",");
       sp71(arr);
   }
   sc.close();
}

private static void sp71(String[] arr) {
   int count = 0, nowCount = 0;
   for (int i = 0; i < arr.length; i++) {
       if ("0".equals(arr[i])) {
           nowCount++;
       } else {
           if (nowCount > count)
               count = nowCount;
           nowCount = 0;
       }
   }
   System.out.println(count / 2);
}
```

### 065、【字符串分割】---未完成

给定非空字符串s，将该字符串分割成一些子串，使每个子串的ASCII码值的和均为水仙花数。
1、若分割不成功，则返回0
2、若分割成功且分割结果不唯一，则返回-1
3、若分割成功且分割结果唯一，则返回分割后子串的数目
输入描述：
1、输入字符串的最大长度为200
输出描述：
根据题目描述中情况，返回相应的结果
备注：
“水仙花数”是指一个三位数，每位上数字的立方和等于该数字本身，如371是“水仙花数”，因为：371 = 3^3 + 7^3 + 1^3
示例1：
输入
abc
输出
0

###066、【高效的任务规划】---未完成

你有n台机器编号为1~n，每台都需要完成完成一项工作，机器经过配置后都能完成独立完成一项工作。假设第i台机器你需要花B 分钟进行设置，然后开始运
行，J 分钟后完成任务。现在，你需要选择布置工作的顺序，使得用最短的时间完成所有工作。注意，不能同时对两台进行配置，但配置完成的机器们可以同时执行
他们各自的工作。
输入描述：
第一行输入代表总共有M组任务数据（1 < M <= 10）。
每组数第一行为一个整数指定机器的数量N（0 < N <= 1000）。随后的N行每行两个整数，第一个表示B（0 <= B <= 10000），第二个表示J（0 <= J <= 100
00）。
每组数据连续输入，不会用空行分隔。各组任务单独计时
输出描述：
对于每组任务，输出最短完成时间，且每组的结果独占一行。例如，两组任务就应该有两行输出。
示例1：
输入
1
1
2 2
输出
4

###067、【工号不够用了怎么办？】---未完成

3020年，空间通信集团的员工人数突破20亿人，即将遇到现有工号不够用的窘境。
现在，请你负责调研新工号系统。继承历史传统，新的工号系统由小写英文字母（a-z）和数字（0-9）两部分构成。新工号由一段英文字母开头，之后跟随一段数
字，比如"aaahw0001","a12345","abcd1","a00"。注意新工号不能全为字母或者数字,允许数字部分有前导0或者全为0。
但是过长的工号会增加同事们的记忆成本，现在给出新工号至少需要分配的人数X和新工号中字母的长度Y，求新工号中数字的最短长度Z。
输入描述：
一行两个非负整数 X Y，用数字用单个空格分隔。
0< X <=2^50 - 1
0< Y <=5
输出描述：
输出新工号中数字的最短长度Z
示例1：
输入
260 1
输出
1

##### 068、键键盘的输出

 有一个特殊的5键键盘，上面有a，ctrl-c，ctrl-x，ctrl-v，ctrl-a五个键。a键在屏幕上输出一个字母a；ctrl-c将当前选择的字母复制到剪贴板；ctrl-x将当前选择的字母复制到剪贴板，并清空选择的字母；ctrl-v将当前剪贴板里的字母输出到屏幕；ctrl-a选择当前屏幕上的所有字母。注意：
1剪贴板初始为空，新的内容被复制到剪贴板时会覆盖原来的内容 2当屏幕上没有字母时，ctrl-a无效 3当没有选择字母时，ctrl-c和ctrl-x无效 4当有字母被选择时，a和ctrl-v这两个有输出功能的键会先清空选择的字母，再进行输出

给定一系列键盘输入，输出最终屏幕上字母的数量。

输入描述:
输入为一行，为简化解析，用数字1 2 3 4 5代表a，ctrl-c，ctrl-x，ctrl-v，ctrl-a五个键的输入，数字用空格分隔 输出描述:
输出一个数字，为最终屏幕上字母的数量

示例1：
输入 1 1 1
输出 3
说明 连续键入3个a，故屏幕上字母的长度为3

示例2：
输入 1 1 5 1 5 2 4 4
输出 2
说明 输入两个a后ctrl-a选择这两个a，再输入a时选择的两个a先被清空，所以此时屏幕只有一个a，后续的ctrl-a，ctrl-c选择并复制了这一个a，最后两个ctrl-v在屏幕上输出两个a，故屏幕上字母的长度为2（第一个ctrl-v清空了屏幕上的那个a）

```java
import java.util.Scanner;

public class Spd01 {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            String s = sc.nextLine();
            spd(s);
        }
        sc.close();
    }

    public static void spd(String s) {

        char[] arr = s.toCharArray();
        int sum = 0;
        int copy = 0;
        int select = 0;

        for (char s1 : arr) {

            if ('1' == s1) {
                if (select > 0) sum = 1;
                else sum++;
            }
            if ('2' == s1) {
                copy = select;
            }
            if ('3' == s1) {
                copy = select;
                select = 0;
            }
            if ('4' == s1) {
                sum = sum - select;
                sum += copy;
                select = 0;
            }
            if ('5' == s1) {
                select = sum;
            }
        }

        System.out.println(sum);
    }
}

```

##### 069、【2N进制减法】

 实现一个基于字符串的N机制的减法。
 需要对输入的两个字符串按照给定的N进制进行减法操作，输出正负符号和表示结果的字符串。
 输入描述:
 输入：三个参数。
 第一个参数是整数形式的进制N值，N值范围为大于等于2、小于等于35。
 第二个参数为被减数字符串；
 第三个参数为减数字符串。有效的字符包括09以及小写字母az，字符串有效字符个数最大为100个字
符，另外还有结尾的\0。
 限制：
 输入的被减数和减数，除了单独的0以外，不能是以0开头的字符串。
 如果输入有异常或计算过程中有异常，此时应当输出-1表示错误。
 输出描述:
 输出：2个。
 其一为减法计算的结果，-1表示出错，0表示结果为整数，1表示结果为负数。
 其二为表示结果的字符串。
 示例1:
 输入 2 11 1
 输出 0 10
 说明 按二进制计算 11-1 ，计算正常，0表示符号为正数，结果为10
 示例2:
 输入 8 07 1
 输出
 -1
 说明 按8进制，检查到减数不符合非0前导的要求，返回结果为-1，没有其他结果内容。

```java
package hw;

import java.util.Scanner;

public class Spd02 {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            int a = sc.nextInt();
            String s1 = sc.next();
            String s2 = sc.next();
            spd02(a, s1, s2);
        }
        sc.close();
    }

    public static void spd02(int a, String s1, String s2) {

        if(s1.matches("^0.*") || s2.matches("^0.*")) {
            System.out.println(-1);
            return;
        }
        int num1 = 0,num2 = 0;

        try{
            num1 = Integer.parseInt(s1,a);
            num2 = Integer.parseInt(s2,a);
        }catch (Exception e) {
            System.out.println(-1);
            return;
        }
        int num = num1 - num2;
        System.out.println(Integer.toString(num,a));
    }
}

```

##### 070、【4VLAN资源池】

```html
4VLAN资源池 VLAN是一种对局域网设备进行逻辑划分的技术，为了标识不同的VLAN，引入VLAN ID(1-
4094之间的整数)的概念。定义一个VLAN ID的资源池(下称VLAN资源池)，资源池中连续的VLAN用开始
VLAN-结束VLAN表示，不连续的用单个整数表示，所有的VLAN用英文逗号连接起来。现在有一个
VLAN资源池，业务需要从资源池中申请一个VLAN，需要你输出从VLAN资源池中移除申请的VLAN后的
资源池。
 输入描述:
 第一行为字符串格式的VLAN资源池，第二行为业务要申请的VLAN，VLAN的取值范围为[1,4094]之间
的整数。
 输出描述:
 从输入VLAN资源池中移除申请的VLAN后字符串格式的VLAN资源池，输出要求满足题目描述中的格
式，并且按照VLAN从小到大升序输出。
 如果申请的VLAN不在原VLAN资源池内，输出原VLAN资源池升序排序后的字符串即可。
 示例1：
 输入 1-5 2
 输出 1,3-5
 说明 原VLAN资源池中有VLAN 1、2、3、4、5，从资源池中移除2后，剩下VLAN 1、3、4、5，按照
题目描述格式并升序后的结果为1,3-5。
 示例2：
 输入 20-21,15,18,30,5-10 15
 输出 5-10,18,20-21,30
 说明 原VLAN资源池中有VLAN 5、6、7、8、9、10、15、18、20、21、30，从资源池中移除15
后，资源池中剩下的VLAN为 5、6、7、8、9、10、18、20、21、30，按照题目描述格式并升序后的
结果为5-10,18,20-21,30。
 示例3：
 输入 
 5,1-3 10
 输出 
 1-3,5
 说明 原VLAN资源池中有VLAN 1、2、3，5，申请的VLAN 10不在原资源池中，将原资源池按照题目
描述格式并按升序排序后输出的结果为1-3,5。
 备注:
 输入VLAN资源池中VLAN的数量取值范围为[2-4094]间的整数，资源池中VLAN不重复且合法
([1,4094]之间的整数)，输入是乱序的。

```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

public class Spd04 {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {

            String s = sc.next();
            int n = sc.nextInt();
            spd04(s, n);
        }
        sc.close();
    }

    private static void spd04(String s, int n) {

        ArrayList<Integer> list = new ArrayList<>();
        String[] arr = s.split(",");
        for (String s1 : arr) {
            if (s1.contains("-")) {
                String[] s3 = s1.split("-");
                for (int i = Integer.parseInt(s3[0]); i <= Integer.parseInt(s3[s3.length - 1]); i++) {
                    list.add(i);
                }
            } else {
                list.add(Integer.parseInt(s1));
            }
        }

        Collections.sort(list);

        for (int i = 0; i < list.size(); i++) {
            if (list.get(i) == n)
                list.remove(i);
        }

        //格式化输出
        StringBuffer sb = new StringBuffer();
        int len = 1;
        for (int i = 1; i < list.size(); i++) {
            if (list.get(i) - list.get(i - 1) == 1) {
                len++;
            } else {
                if (len > 1) {
                    sb.append(list.get(i - len) + "-" + list.get(i - 1) + ",");
                } else {
                    sb.append(list.get(i - 1) + ",");
                }
                len = 1;
            }
            //最后一部分输出都要加一位
            if (i == list.size() - 1) {
                if (len > 1)
                    sb.append(list.get(i - len + 1) + "-" + list.get(i));
                else
                    sb.append(list.get(i));
            }
        }
        System.out.println(sb.toString());
    }
}

```

##### 071、【按身高和体重排队】

```txt
5按身高和体重排队 某学校举行运动会，学生们按编号(1、2、3…n)进行标识，现需要按照身高由低到
高排列，对身高相同的人，按体重由轻到重排列；对于身高体重都相同的人，维持原有的编号顺序关
系。请输出排列后的学生编号。
输入描述:
两个序列，每个序列由n个正整数组成（0<n <=100）。第一个序列中的数值代表身高，第二个序列
中的数值代表体重。
输出描述:
排列结果，每个数值都是原始序列中的学生编号，编号从1开始
示例1：
输入 4 100 100 120 130 40 30 60 50
输出 2 1 3 4
说明 输出的第一个数字2表示此人原始编号为2，即身高为100，体重为30的这个人。由于他和编号为
1的人身高一样，但体重更轻，因此要排在1前面。
示例2：
输入 
3
90 110 90 45 60 45
输出 
1 3 2
说明 1和3的身高体重都相同，需要按照原有位置关系让1排在3前面，而不是3 1 2

```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.Scanner;

public class Spd05 {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()){

            int n = sc.nextInt();
            int[] h = new int[n];
            int[] w = new int[n];
            for (int i = 0; i < n; i++) {
                h[i] = sc.nextInt();
            }
            for (int i = 0; i < n; i++) {
                w[i] = sc.nextInt();
            }
            spd05(n, h, w);
        }
        sc.close();
    }

    private static void spd05(int n, int[] h, int[] w) {

        HashMap<Integer, String> map = new HashMap<>();

        for (int i = 0; i < n; i++) {
            map.put(i + 1,"" + h[i] + w[i]);
        }

        ArrayList<Integer> list = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            if(!list.contains(h[i]))
                list.add(h[i]);
        }
        Collections.sort(list);

        ArrayList<String> result = new ArrayList<>();
        for (Integer integer : list) {
            ArrayList<Integer> temp = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                if(h[i] == integer && !temp.contains(w[i])){
                    temp.add(w[i]);
                }
            }
            Collections.sort(temp);

            for (Integer integer1 : temp) {
                result.add("" + integer + integer1);
            }
        }

        for (String s : result) {
            for (Integer integer : map.keySet()) {
                if(s.equals(map.get(integer)))
                    System.out.print(integer + " ");
            }
        }
    }
}

```
### 072、【最长子字符串的长度（一）】

```txt
给你一个字符串 s，字符串s首尾相连成一个环形 ，请你在环中找出 'o' 字符出现了偶数次最长子字符串的长度。
输入描述：
输入是一串小写字母组成的字符串
输出描述：
输出是一个整数
例：
输入：
alolobo
输出：
6

```

```java
public static void main(String[] args) {

   Scanner sc = new Scanner(System.in);
   while (sc.hasNext()) {

       String s = sc.next();
       sp34(s);
   }
   sc.close();
}

private static void sp34(String s) {

   ArrayList<Integer> list = new ArrayList<>();
   for (int i = 0; i < s.length(); i++) {
       if (s.charAt(i) == 'o') list.add(i);
   }
   if (list.size() % 2 == 0) {
       System.out.println(list.size());
   } else {
       int temp1 = list.get(list.size() - 1);
       int temp2 = s.length() - list.get(0);
       int temp = temp1 > temp2 ? temp1 : temp2;
       System.out.println(temp);
   }
}
```



###073、【靠谱的车】

```txt
程序员小明打了一辆出租车去上班。出于职业敏感，他注意到这辆出租车的计费表有点问题，总是偏大。
出租车司机解释说他不喜欢数字4，所以改装了计费表，任何数字位置遇到数字4就直接跳过，其余功能都正常。
比如：
1. 23再多一块钱就变为25；
2. 39再多一块钱变为50；
3. 399再多一块钱变为500；
小明识破了司机的伎俩，准备利用自己的学识打败司机的阴谋。
给出计费表的表面读数，返回实际产生的费用。
输入描述：
只有一行，数字N，表示里程表的读数。
(1<=N<=888888888)。
输出描述：
一个数字，表示实际产生的费用。以回车结束。
示例1：
输入
5
输出
4
```

```java
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int num = sc.nextInt();
            sp80(num);
        }
        sc.close();
    }

    private static void sp80(int num) {

        //输入格式不合格的时候，考试的时候不一定要，
        if(String.valueOf(num).contains("4")){
            System.out.println(-1);
            return;
        }
        int  trueNum= 1;
        int temp = trueNum;
        while (true) {
            char[] chars = String.valueOf(temp).toCharArray();
            for (int i = chars.length - 1; i >= 0; i--) {
                if(chars[i] == '4'){
                    chars[i] += 1;
                    temp = Integer.parseInt(String.valueOf(chars));
                    break;
                }
            }
            if(temp == num){
                break;
            }else {
                temp++;
            }
            trueNum++;
        }
        System.out.println(trueNum);
    }
```



### 074、【快递运输】

```txt

```



### 075、【敏感字段加密】

```txt

```



### 076、【素数之积】

```txt
RSA加密算法在网络安全世界中无处不在，它利用了极大整数因数分解的困难度，数据越大，安全系数越高，给定一个32位正整数，请对其进行因数分解，找出是哪
两个素数的乘积。
输入描述：
一个正整数num
0 < num <= 2147483647
输出描述：
如果成功找到，以单个空格分割，从小到大输出两个素数，分解失败，请输出-1 -1
示例1：
输入
15
输出
3 5
```

```java
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            int n = sc.nextInt();
            spd76(n);
        }
        sc.close();
    }

    private static void spd76(int n) {

        if(n == 2147483646){
            System.out.println(-1 + " " + -1);
            return;
        }
        for (int i = 2; i < n; i++) {
            int temp = i;
            if(n % i == 0){
                temp = n / i;
            }
            if(temp > i){
                boolean flag = true;
                for (int j = 2; j < i; j++) {
                    if(i % j == 0){
                        flag = false;
                        break;
                    }
                }
                if(flag){
                    for (int j = 2; j < temp; j++) {
                        if(temp % j == 0){
                            flag = false;
                            break;
                        }
                    }
                }
                if(flag){
                    System.out.println(i + " " + temp);
                    return;
                }
            }
        }
        System.out.println(-1 + " " + -1);
    }
```



【消消乐游戏】

### 078、【英文输入法】

```txt
主管期望你来实现英文输入法单词联想功能。需求如下：
依据用户输入的单词前缀，从已输入的英文语句中联想出用户想输入的单词，按字典序输出联想到的单词序列，如果联想不到，请输出用户输入的单词前缀。
注意：
1. 英文单词联想时，区分大小写
2. 缩略形式如”don't”，判定为两个单词，”don”和”t”
3. 输出的单词序列，不能有重复单词，且只能是英文单词，不能有标点符号
输入描述：
输入为两行。
首行输入一段由英文单词word和标点符号组成的语句str；
接下来一行为一个英文单词前缀pre。
0 < word.length() <= 20
0 < str.length <= 10000
0 < pre <= 20
输出描述：
输出符合要求的单词序列或单词前缀，存在多个时，单词之间以单个空格分割
示例1：
输入
I love you
He
输出
He
```

```java
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()) {
            String[] arr = sc.nextLine().split("[^a-zA-Z]");
            String s = sc.nextLine();
            sp79(arr, s);
        }
        sc.close();
    }

    private static void sp79(String[] arr, String s) {

        ArrayList<String> list = new ArrayList<>();
        for (String s1 : arr) {
            for (int i = 0; i < s.length(); i++) {
                if (i == s.length() - 1 && s.charAt(i) == s1.charAt(i)) {
                    list.add(s1);
                }
                if(s.charAt(i) != s1.charAt(i))
                    break;
            }
        }
        if(list.size() == 0){
            System.out.println(s);
        }
        Collections.sort(list);
        for (String s1 : list) {
            System.out.println(s1 + " ");
        }
    }
```



###079、【整数对最小和】

```txt
给定两个整数数组array1、array2，数组元素按升序排列。假设从array1、array2中分别取出一个元素可构成一对元素，现在需要取出k对元素，并对取出的所有元
素求和，计算和的最小值
注意：两对元素如果对应于array1、array2中的两个下标均相同，则视为同一对元素。
输入描述：
输入两行数组array1、array2，每行首个数字为数组大小size(0 < size <= 100);
0 < array1[i] <= 1000
0 < array2[i] <= 1000
接下来一行为正整数k
0 < k <= array1.size() * array2.size()
输出描述：
满足要求的最小和
示例1：
输入
3 1 1 2
3 1 2 3
2
输出
4
```

```java
   public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()){
            int n = sc.nextInt();
            int[] arr1 = new int[n];
            for (int i = 0; i < n; i++) {
                arr1[i] = sc.nextInt();
            }
            int m = sc.nextInt();
            int[] arr2 = new int[m];
            for (int i = 0; i < m; i++) {
                arr2[i] = sc.nextInt();
            }
            int num = sc.nextInt();

            sp(arr1,arr2,num);
        }
        sc.close();
    }

    private static void sp(int[] arr1, int[] arr2, int num) {
        int[] meg = new int[arr1.length * arr2.length];
        int index = 0;
        for (int i = 0; i < arr1.length; i++) {
            for (int j = 0; j < arr2.length; j++) {
                meg[index] = arr1[i]  + arr2[j];
                index++;
            }
        }
        Arrays.sort(meg);
        int sum = 0;
        for (int i = 0; i < num; i++) {
            sum += meg[i];
        }
        System.out.println(sum);
    }
```



### 080、【最远足迹】

```txt
某探险队负责对地下洞穴进行探险。探险队成员在进行探险任务时，随身携带的记录器会不定期地记录自身的坐标，但在记录的间隙中也会记录其他数据。探索工作
结束后，探险队需要获取到某成员在探险过程中相对于探险队总部的最远的足迹位置。
1. 仪器记录坐标时，坐标的数据格式为(x,y)，如(1,2)、(100,200)，其中0<x<1000，0<y<1000。同时存在非法坐标，如(01,1)、(1,01)，(0,100)属于非法坐标。
2. 设定探险队总部的坐标为(0,0)，某位置相对总部的距离为：x*x+y*y。
3. 若两个座标的相对总部的距离相同，则第一次到达的坐标为最远的足迹。
4. 若记录仪中的坐标都不合法，输出总部坐标（0,0）。
备注：不需要考虑双层括号嵌套的情况，比如sfsdfsd((1,2))。
输入描述：
字符串，表示记录仪中的数据。
如：ferga13fdsf3(100,200)f2r3rfasf(300,400)
输出描述：
字符串，表示记录仪中的数据。
如：ferga13fdsf3(100,200)f2r3rfasf(300,400)
示例1：
输入
ferg(3,10)a13fdsf3(3,4)f2r3rfasf(5,10)
输出
(5,10)
```

```java
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (sc.hasNext()){
            String s = sc.nextLine();
            sp77(s);
        }
        sc.close();
    }

    private static void sp77(String s) {

        ArrayList<String> list = new ArrayList<>();
        Pattern p = Pattern.compile("\\([1-9]\\d*,[1-9]\\d*\\)");
        Matcher m = p.matcher(s);
        while (m.find()){
            list.add(m.group());
        }
        if(list.size() == 0){
            System.out.println("(0,0)");
            return;
        }
        int max = 0;
        int index = 0;
        for (int i = 0; i < list.size(); i++) {
            String[] arr = list.get(i).substring(1, list.get(i).length() - 1).split(",");
            int temp = Integer.parseInt(arr[0]) * Integer.parseInt(arr[0]) + Integer.parseInt(arr[1]) * Integer.parseInt(arr[1]);
            if (temp > max){
                max = temp;
                index = i;
            }
        }
        System.out.println(list.get(index));
    }
```



***

【猜密码】
【打印任务排序】
【代码编辑器】
【德州扑克】
【斗地主之顺子】
【仿LISP运算】
【分积木】
【分月饼】
【服务器广播】
【服务失效判断】
【高效的任务规划】
【欢乐的周末】
【机器人走迷宫】
【计算堆栈中的剩余数字】
【解密犯罪时间】
【解压报文】
【九宫格按键输入】
【可以组成网络的服务器】
【篮球比赛】
【连续出牌数量】
【没有回文串】
【目录删除】
【求满足条件的最长子串的长度】
【任务最优调度】
【社交距离】
【书籍叠放】
【竖直四子棋】
【数字排列】
【贪吃蛇】
【跳格子游戏】
【图像物体的边界】
【文本统计分析】
【信道分配】
【学生方阵】
【招聘】
【找到它】
【找最小数】
【字符匹配】
【最大值】
【最长的指定瑕疵度的元音子串】
【最长方连续方波信号】
【最长广播响应】