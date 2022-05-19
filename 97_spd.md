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

##### 一、键键盘的输出

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

##### 2N进制减法

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

##### 三、3TLV解码

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
        for (String s1 : map.keySet()) {
            if(tag.equals(s1)) {
                ArrayList<String> list = map.get(s1);
                for (String s2 : list) {
                    //如果后面对不上，就用String.trim()
                    System.out.print(s2 + " ");
                }
            }
        }
    }
}

```

##### 四、4VLAN资源池

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

##### 五、按身高和体重排队

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

