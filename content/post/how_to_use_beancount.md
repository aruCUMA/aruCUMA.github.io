---
title: "如何使用 Beancount"
date: 2023-03-29T21:44:29+08:00
tags: 
- funTools
- personalFinance
draft: false
hidden: false
---

## （我）为什么要用 Beancount
我知道 Beancount 是因为 geekplux 的[一篇文章](https://geekplux.com/posts/accounting-earnings-report-graph-theory)，从中我了解到 Beancount 是一种复式记账工具，这一下就把我带回到大学的时候学会计学原理时为会计科目和配平苦恼，但意外的成绩考得不错的回忆里（这门课的老师非常喜欢课堂上点人回答问题，还好我很幸运地一次都没被点到，因为在讲完后面的某章内容前，提问的问题我真的一个都不会，直到学完所有的基础知识才逐渐掌握了）。

除了能给我带来一些让我快乐的优越感和成就感以外，复式记账毕竟是一种独特的认识和解释世界的视角和工具，我想 Beancount 还能帮助我将这一思考方式内化为我的想法的一部分。为思想打开新的一扇门、拓展一个新的空间对我来说总是个非常有趣的事情。

另外，在整理账本的过程中，我发现了一些我没有意识到也是成本的一些支出项目，例如记账的时候需要将 record 归类，在想它到底应该归类到哪个账本里的时候，我就会知道这个成本是必须要付的，还是只是维持某个状态而必须要付的。这一发现对于评估需要每种生活状态的最低成本来说真的非常重要，能够帮助我更有依据地做出一些选择（比如上一个让我压力特别大，并且虽然也挣了一些钱但也花了很大一部分来消减不开心，的班到底是不是值得的）。

最后，记账时需要用到的摊销这一方式能够还原支出的确切使用时间，进而更科学地做预算和评估自己的消费情况（主要是能缓解当月有大额支出的负罪感，摊 1 年不行就摊 2 年 hhhh）。

所以，作为一个极其讨厌手动量化，极其讨厌给自己每天设很多任务导致一睁眼就是代办任务，极其讨厌以不断运转的机器的标准要求自己的人，使用 Beancount 来记账也能获得很多个人乐趣和实际收益。

BTW，在用 Beancount 之前，从各个记账软件辗转之后我用了很长一段时间 numbers 自带的记账模版也非常地好用，其中和上月对比、和预算对比的方法非常实用。
## 如何安装 Beancount（以 macOS 为例）
谁能想到我之前误以为它是一个 App，并且还真找到了一个来安装，直到好几年之后才发现并不是这样。
### 为减少麻烦，确保以下内容为最新版本
- python
- pip
- Command Line Tools for Xcode
### 安装 Beancount 本体和可视化工具 Fava
打开 terminal/终端，通过 pip 安装
```markdown
pip3 install beancount, fava
```
当当，这就完成了！
### 给编辑器安装 Beancount 配色
可以使用 emacs, vim, sublime text 和 vscode 作为编辑器，安装模版后可以让它有配色，然后记账的时候容易看一些
#### sublime text（我在用）
- 首先需要安装 [package control](https://packagecontrol.io/installation)

	``` markdown
	//press cmd+shift+p
	install package control
	```
- 其次通过 package control 安装 [beancount](https://packagecontrol.io/packages/Beancount) 配色
	``` markdown
	//sublime text —— settings —— package control
	 install package beancount
	```
#### vscode（我没在用但是发现有）
拓展中搜索 beancount 即可安装

## 开始使用 Beancount
### 账本和账户设置
在开始记账前，首先要新建一些基础设置的文档，我的设置文档主要有两个，index.beancount 和 account.beancount。
index.beancount 主要承担两个作用，一个是账本 title 和货币的基本信息，另一个是 include 账户设置文件（即 account.beancount）和分月的记账文件，后续在用 fava 进行可视化的时候，可以直接把这一个文件作为路径目标。我的（部分）设置如下：
``` markdown
; Setting
option "title" "MyAccount"
option "operating_currency" "CNY" 
option "operating_currency" "USD"
option "operating_currency" "EUR"
option "render_commas" "true"
include "account.beancount" ;资产账户设置及初始化

; 逐月账本
; 2024
include "2024-Subscription.beancount"
; 2023
include "2023-Subscription.beancount"
include "2023-01.beancount"
```
account.beancount 中需要设置更为具体的账本名称，即分类标准。预设的、大的分类主要有 5 个：```assets, equity, income, expenses, liabilities```。需要在大的分类标准下根据自己的情况设置更细致的分类，我的分类方法如下：
``` markdown
//Assets
2019-01-01 open Assets:Cash
2019-01-01 open Assets:Bank:A
2019-01-01 open Assets:Bank:B
2019-01-01 open Assets:Alipay
2019-01-01 open Assets:Wepay
2019-01-01 open Assets:AccountPrepaid //记录预付款，摊销或实际付款后销掉
2019-01-01 open Assets:AccountReceivable //记录帮别人代付款的支出，收回后销掉

//Equity
2019-01-01 open Equity:Balance

//Income
2019-01-01 open Income:Salary:MajorJob
2019-01-01 open Income:Salary:Freelance
2019-01-01 open Income:Salary:Other
2019-01-01 open Income:Invest
2019-01-01 open Income:Parents
2019-01-01 open Income:Other

//Expenses
2019-01-01 open Expenses:DailyLife:Meal //日常生活，有多个分类，仅展示 2 个
2019-01-01 open Expenses:DailyLife:Transport
2019-01-01 open Expenses:SustainabilityLife:Art //让我觉得开心的支出，仅展示 2 个
2019-01-01 open Expenses:SustainabilityLife:Exercise 
2019-01-01 open Expenses:SustainabilityLife:Wandering:FlightTrain //出去玩
2019-01-01 open Expenses:SustainabilityLife:Wandering:Hotel
2019-01-01 open Expenses:LearningFurther:WorkingSkill //学习
2019-01-01 open Expenses:IncomeRelated:Tax //社保和税
2019-01-01 open Expenses:IncomeRelated:MandatoryInsurance:Pension
2019-01-01 open Expenses:IncomeRelated:MandatoryInsurance:Unemployment
2019-01-01 open Expenses:IncomeRelated:MandatoryInsurance:Medical
2019-01-01 open Expenses:Gift

//Liabilities
2019-01-01 open Liabilities:Credit-Card:A //信用卡
2019-01-01 open Liabilities:Huabei
```
### 开始记账
有借必有贷，借贷必相等，欢迎来到会计学复式记账的世界。
根据我自己的使用习惯，我的记账文件的创建逻辑是每个月单独一个账本，同时每年有一个记录相同支出的账本，包括每月同一天扣费的视频网站会员费、工资、社保、公积金、房租等等。
具体的语法为：
``` markdown
YYYY-mm-dd * ["payee"] "description" #tags
  posting 1
  posting 2

//如
2022-10-07 * "Tiimo" #payinadvance
	Expenses:SustainabilityLife:Subscription 14 CNY
	Assets:AccountPrepaid -14 CNY

//货币换算
2022-12-06 * "还信用卡" #payinadvance
	Liabilities:Credit-Card:A 200 USD @@ 1500 CNY
	Assets:Bank:BOC-6077 -1500 CNY
```

### 我的一些记账习惯
- 工资的处理
	税后工资和公积金分别计入 Income 的细分账户中，税和社保分别计入 Expenses 的细分账户中。
- 全年一次性付费订阅的处理
	在当月的账本中，该次付费计入 Assets:AccountPrepaid，并在 12 个月中分摊，再计入具体的 Expenses 项
- 信用卡和花呗
	在当月的消费中，计入实际的 Expenses 和 Liabilities，后续在还款时用 Assests 的减少销掉这笔 Liabilities
- 采用 tag payinadvance 来查看总和是否为 0，来确认是不是都正确摊销掉了。
- 每一次旅行支出都有自己的 tag，来查看非实用型大型消费的预算。


## 可视化
打开 terminal/终端，输入
```markdown
fava 'path' //此处的 path 是到之前设置的 index.beancount 的路径
```
就会得到一个可视化的地址，在浏览器打开即可。
开着终端，开着可视化网址，在编辑器里修改的时候，刷新网站就会及时更新，能给到及时反馈就很棒。在 fava 里改的话本地文件也会同步。fava 也会提示哪里没配平的报错。
另外，fava 中还可以使用 query，我常用的语句如下
```markdown
//明细
select date, account, tags, narration, position

//Expenses 每月汇总
SELECT sum(cost(position)) as total, year, month 
WHERE account ~ "Expenses:*" 
	and year(date) = 2022 
GROUP BY year, month

//Income 每月汇总
SELECT sum(cost(position)) as total, year, month 
WHERE account ~ "Income:*" 
	and year(date) = 2022 
GROUP BY year, month
```


## 可参考链接
1. https://byvoid.com/zht/blog/beancount-bookkeeping-2/  
2. https://zh.m.wikipedia.org/wiki/%E6%9C%83%E8%A8%88%E7%A7%91%E7%9B%AE
3. https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.yshh8f17jbdb
4. https://docs.google.com/document/d/1P5At-z1sP8rgwYLHso5sEy3u4rMnIUDDgob9Y_BYuWE/edit#