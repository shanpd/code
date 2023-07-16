### 一、性能指标

```
1、硬件要求
2、软件要求
3、测试组网
4、性能指标
	1、系统性能指标
		响应时间
		系统处理能力
		吞吐量
		并发用户数
		错误率
	2、资源性能指标
		CPU
		内存
		磁盘吞吐量
		网络吞吐量
```

### 二、测试方法

```
1、覆盖所有改造参数(正向)
2、组合可行参数(正向——覆盖率的问题，考虑人力和时间成本)
3、参数边界(比如年龄边界)
4、如果参数的取范围是枚举，需要覆盖所有的枚举值(测试所有可能数据)
5、空数据(逆向_不录入数据)
6、包含特殊字符(逆向_+-*/=&*_)
7、越界数据(逆向_比如长度太大或太短)
8、错误数据(逆向_比如错误的手机号，身份证，重复的ID)
异常场景校验
1、越权验证：比如不存在
2、状态验证：比如siteid状态为非上线状态
3、限制验证：比如siteid个数限制
4、重复验证相同数据：
5、交叉认证：
一、兼容性
内容兼容：删除版本是否把这个版本添加的表数据删除
页面兼容：升级时新的页面兼容老的后台数据
浏览器兼容：
二、国际化
1、场景法
2、流程法
3、等价类
4、边界值
5、因果图和判定表
6、正交排列组合
7、补充和错误推导
```

### 三、linux三剑客

```
一、正则
二、grep（A,B,C,c）
	1、过滤
三、sed(-i保存在文件)---spd/cai(替换，查找，删除，添加)：找认证做啥
1、查找
-n '/1,2p/p' a.txt
-n '/101/,/105/p' a.txt
2、删除
-n '/1,2p/p' a.txt
-n '/101/,/105/p' a.txt
3、新增
-n '/$a abc/' a.txt
4、替换
-n 's/a/b/g' a.txt
四、awk（取列，统计，计算）修改变量 --v OFS == :
-F 'BEGIN{}条件{}END{}' a.txt
五、其它(制表符，命令说明，统计数量)
column -t
info
wc -l
```

### 四、抓包、

```
tcpdump -s O -l -i any -w -port 8080 | string
```

### 五、常用命令

```
#!/bin/bash
# while
IFS='='
while read site sitedomain
do
	weget ${sitedomain --no-check-certficate -O /dev/null -o /home/spd/hosting/log/${site}}.log
done < /home/spd/config

# for
for name in `cat config`
do
	weget ${sitedomain --no-check-certficate -O /dev/null -o /home/spd/hosting/log/${site}}.log
done

curl "http://test007.hosting-test-aws/agcconnect.link" -kvo /dev/null
curl "http://wisecontent-hostingtest.hwcloudtest.cn/index.html" -H "x-hw-dl-host:test007.hosting-test-aws.agcconnect.link" -kvo /dev/null

# 文件切割
split -b 400M 408.zip 1_

```

