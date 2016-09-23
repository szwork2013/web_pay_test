
[TOC]:#
# Table of Contents
- [项目总体描述](#项目总体描述)
    - [备注](#备注)
- [客户端](#客户端)
    - [商户端环境](#商户端环境)
    - [商户端的功能描述](#商户端的功能描述)
    - [用户端环境](#用户端环境)
    - [用户端的功能描述](#用户端的功能描述)
- [界面要素](#界面要素)
    - [登陆页面](#登陆页面)
    - [创建订单页面](#创建订单页面)
    - [扫描订单页面](#扫描订单页面)
    - [订单界面](#订单界面)
    - [支付页面(用户、商户共用)](#支付页面用户商户共用)
    - [对话框通知界面](#对话框通知界面)
- [界面事件](#界面事件)
    - [登陆界面](#登陆界面)
    - [创建订单界面](#创建订单界面)
    - [订单界面](#订单界面)
    - [扫描订单界面](#扫描订单界面)
    - [支付界面](#支付界面)
    - [对话框通知界面](#对话框通知界面)
- [Action(向服务器发送和请求的数据)](#action向服务器发送和请求的数据)
    - [登陆页面](#登陆页面)
    - [创建订单页面](#创建订单页面)
    - [扫描页面](#扫描页面)
    - [订单列表页面](#订单列表页面)
    - [支付页面](#支付页面)
- [state数据结构](#state数据结构)

***  

# web_pay_test项目说明
- 待整理

# web_pay_test项目需求描述

## 项目总体描述
- 用户环境：不确定，可能是PC端的Windows系统、嵌入式系统也可能是手机
- 因为用户环境的不确定，因此用webApp开发，以达到跨平台的目的
- 系统分块
  - 客户端：用WebApp，达到跨平台的目的
  - 服务端：实现订单的管理  

### 备注
- 测试采用BDD方式进行开发
- BDD:Behaviour Driven Development（行为驱动开发）
- BDD：作为理论方法学，强调以需求为导向，并鼓励开发人员、QA与需求方之间的协作。
- BDD位于更宏观的位置，让开发者“做正确的事”；  

## 客户端
### 商户端环境
- 不确定，可能是PC端的Windows系统、嵌入式系统也可能是手机 
- 因为商户环境的不确定，因此用webApp开发，以达到跨平台的目的

### 商户端的功能描述
#### 商户登陆
- 商户登陆功能
  - 商户登陆成功：用户名文本框内输入“商户的用户名称”，选择终端类型为商户端，页面会跳转进入“创建订单”页面
  - 商户登陆失败：用户名文本框不输入数据，选择终端类型为商户端，登陆页面的“登陆按钮”不可以点击。

#### 商户创建订单
- 对商品信息的操作
   - 点击“添加”按钮：成功添加商品信息
   - 编辑商品名称文本框：验证规则为任何字符串，长度为100
   - 编辑商品单价文本框：验证规则为0-9的阿拉伯数字，长度为9
   - 编辑商品数量文本框：验证规则为0-9的阿拉伯数字，长度为10
   - 点击“删除”按钮：删除该行商品信息
- 点击“提交”按钮：创建订单
   - 当录入商品信息全部为空时，不能成功提交订单
   - 当录入商品信息不完整时，不能成功提交订单
   - 当录入完整的商品信息，点击“提交”按钮时，会成功提交订单，结果页面跳转进入“订单页面”
- 点击“订单待支付”按钮：
   - 当商户端存在未支付完成的订单时，存在“订单待支付”按钮，
     - 当点击“订单待支付”按钮时，页面跳转到待支付的“订单页面”
   - 当商户端不存在未支付完成的订单时，不存在“订单待支付”按钮。

#### 商户查看订单
- 前提：商户创建订单成功，会跳转到当前订单信息页面
- “切换订单”操作
  - 单击不同的订单tab标签,可以查看该商户不同的订单信息
- 点击“支付”按钮：
   - 当用户未匹配该订单时，商户无法看到优惠信息和结算信息，无法点击“支付”按钮
   - 当用户已匹配该订单时，商户可以点击“支付”按钮，页面跳转进入“订单支付”页面
- 点击“取消”按钮：
   - 情况1：当商户有多个未完成支付的订单时
     - 商户取消订单，订单切换为上一个订单信息
     - 商户取消订单后，商户收到取消订单的通知
   - 情况2：当商户只有一个订单时
     - 商户取消订单，页面跳转到“创建订单”页面
     - 商户取消订单后，结果商户不会收到取消订单的通知（因为不存在同一商户多处登陆的情况，商户可以多次点击“取消”按钮进行取消订单，订单取消后商户就看不到当前取消的订单，切换到下一订单）

#### 商户支付订单
- 前提：用户登陆，且已匹配该订单
- 选择支付结果下拉菜单-选择成功，点击“确定”按钮，
  - 结果：订单支付完成，页面跳转进"创建订单页面"，商户收到支付结果通知：支付完成，支付成功
- 选择支付结果下拉菜单-选择失败，点击“确定”按钮
  - 结果：订单支付完成，页面跳转进"创建订单页面"，商户收到支付结果通知：支付完成，支付失败
- 点击“取消支付”按钮
  - 情况1
    - 场景：只有商户操作该订单。
    - 操作：商户点击“取消支付”按钮
    - 结果：页面跳转到“创建订单”页面
  - 情况2
    - 场景：商户和用户操作同一订单，用户先进入“支付页面”，商户在“订单页面”再点击“支付”按钮变为灰色，用户在支付页面“取消支付”的情况
    - 操作：用户点击“取消支付”按钮
    - 结果：商户端获得支付授权，从“订单页面”跳转进入“支付页面”
    
####  商户端得到通知
- 描述：商户端和用户端已登陆的情况下，用户支付完成，商户会收到通知
  - 无论商户端在哪个页面，用户支付成功订单，商户会收到支付成功通知。
  - 无论商户端在哪个页面，用户支付失败订单，商户会收到支付失败通知。
    
***

### 用户端环境
- 不确定，可能是PC端的Windows系统、嵌入式系统也可能是手机 
- 因为用户环境的不确定，因此用webApp开发，以达到跨平台的目的
  
### 用户端的功能描述  

#### 用户登陆
- 用户登陆功能
  - 用户登陆成功：用户名文本框内输入“用户的用户名称”，选择终端类型为用户端，点击“登陆按钮”，页面会跳转进入“扫描订单”页面
  - 用户登陆失败：用户名文本框内不输入数据，选择终端类型为用户端，登陆页面的“登陆按钮”不可以点击。

#### 用户扫描订单
- 注：已完成的订单状态包括：订单支付成功、订单支付失败、取消订单
- 单个用户登陆的情况
    - “待支付”按钮：未支付完成的订单
      - 当用户没有未完成支付的订单时，结果不会显示“待支付”按钮
      - 当用户存在至少一个未完成支付的订单时，结果会显示带有待支付订单的个数“待支付”按钮
        - 用户点击“待支付”按钮，页面跳转进入“订单页面”
    - “加入”按钮：匹配商户端创建的订单操作
      - 在文本框内录入订单号操作
      - 用户点击“加入”按钮
        - 当加入的订单号是商户端创建但未完成的订单时，页面会跳转到当前订单的“订单页面”
        - 当加入的订单号是商户已完成的订单或者不存在的订单时，页面会跳到已经匹配完成的“订单页面”（待讨论）
- 同一用户多处登陆的情况
   - 前提：商户生成多个订单，用户1和用户2是同一用户，且同时登陆的情况
   - 用户1在扫描订单页面，加入成功1个订单。用户2在扫描订单页面，结果会出现“1个待支付”按钮
   - 用户1在扫描订单页面，加入成功1个订单。用户2在订单页面，结果出现用户1加入成功的订单

- 不同用户登陆，匹配同一订单的情况
  - 前提：商户生成1个订单，用户1和用户2时不同的两个用户
  - 情况1
    - 用户1在“扫描订单”页面点击“加入”一个订单，页面跳转进入该订单的“支付页面”，用户2也去“加入”该订单，用户2跳转进入该订单的“支付页面”，用户1收到通知：用户 u2 参与支付
  - 情况2
    - 用户1进入支付页面，用户2在“扫描订单页面”，用户2会收到通知：订单正在支付, 当前用户不能参与支付

#### 用户查看订单
- “支付”按钮
  - 情况1
    - 商户在订单页面未操作，用户在订单页面，点击“支付”按钮，用户进入“支付页面”
  - 情况2
    - 商户在订单页面点击“支付”按钮，进入订单的支付页面。用户在订单页面，点击“支付”按钮，按钮变为灰色的情况
  - 情况3
    - 前提：用户1和用户2是同一用户
    - 用户1在订单页面点击“支付”按钮，进入订单的支付页面。用户2在订单页面，点击“支付”按钮，按钮变为灰色的情况

#### 用户支付订单
- 单个用户登陆的情况
    - 选择支付结果下拉菜单-选择成功，点击“确定”按钮，
      - 结果：订单支付完成，页面跳转进"扫描订单页面"，用户收到支付结果通知：支付完成，支付成功
    - 选择支付结果下拉菜单-选择失败，点击“确定”按钮
      - 结果：订单支付完成，页面跳转进"扫描订单页面"，用户收到支付结果通知：支付完成，支付失败
    - 点击“取消支付”按钮
      - 情况1
        - 场景：商户和用户操作同一订单，商户先进入“支付页面”，用户在“订单页面”再点击“支付”按钮变为灰色，商户在支付页面“取消支付”的情况
        - 操作：商户点击“取消支付”按钮
        - 结果：用户端获得支付授权，从“订单页面”跳转进入“支付页面”

- 多个同一用户同时登陆的情况
  - 前提：商户端生成两个订单，用户1和用户2是同一用户
  - 用户1在“支付”页面支付成功两个订单，用户2在“订单页面”连续收到两个通知：订单支付成功。

#### 用户通知
- 前提：商户和用户都登陆的情况，商户在“订单页面”点击“取消”按钮，用户无论在哪个页面都会收到“订单通知”  
  - 用户收到多条通知的情况
    - 用户在扫描在订单页面，已加入三个订单，商户在“订单页面”连续取消该2个订单，即点击2次“取消”按钮。结果用户端会在“订单页面”切换到第三个订单的页面，并且会收到两个订单取消的通知
    - 用户在扫描在订单页面，已加入三个订单，商户在“订单页面”连续取消该3个订单，即点击3次“取消”按钮。结果用户端会跳转到“扫描订单页面”，并且会收到三个订单取消的通知
***

## 界面要素  
### 登陆页面
- 界面要素
  - 一个输入框
    - 作用:是输入用户或商户输入的id
  - 一组选择按钮:商户和用户
    - 作用:是切换登录类型（默认值是商户）
  - 登录按钮
    - 商户登陆：登录到添加商品页面（登录后处理登录类型和id数据）
    - 用户登陆：跳转到扫描订单

### 创建订单页面  
- 添加商品组件
   - 输入数据：表单ID、删除事件、提交事件
   - 输出数据：商品信息
   - 组件要素：
     - 商品名称输入框
       - 要素有输入框、输入成功提示、输入失败提示  
     - 单价输入框
       - 要素有输入框（只允许输入数字精确到小数点后两位）、输入成功提示、输入失败提示  
     - 数量输入框
       - 要素有输入框（只允许输入数字）、输入成功提示、输入失败提示    
     - 删除按钮
       - 要素有按钮（有点击事件，点击事件触发后该添加商品组件消失 ） 
    
- 提交商品组件
   - 输入数据：商户拥有待支付订单的数量
   - 输出数据：添加的商品列表信息
   - 组件要素：
     - 添加商品按钮
       - 要素有按钮（有点击事件，点击事件触发后添加一个添加商品组件）
     - 提交商品按钮
       - 要素有按钮（有点击事件，点击事件触发后添加一个添加商品组件）
     - 待支付跳转按钮
       - 要素有按钮（有点击事件，点击事件触发后跳转到外部给的连接）

### 扫描订单页面  
- 界面要素
  - 一个输入框
    - 作用:输入订单号（由商户产生）
  - 一个匹配按钮
    - 作用:如果用户输入的订单号是正确可用的，就匹配该订单并跳转到订单列表页面，
    - 如果不是正确或不可用订单号，提示信息：该订单不存在（红色字体提示）
  - 一个链接
    - 如果该用户有已经匹配且未完成的订单，显示已经匹配且未完成的订单数量，点击后跳转到订单列表页面
    - 如果没有已经匹配订单，不显示
  
### 订单界面
- 界面要素
   - 订单信息(多行)
     - 商品名称
     - 单价
     - 数量
   - 优惠信息
     - 优惠金额
     - 优惠信息
   - 结算
     - 总价
     - 优惠金额
     - 结算金额
   - 页面按钮：
     - 支付
     - 取消
       - 商户登录可见
       - 用户登录不可见
- 组件划分
   - 商品列表
     - 名称（传入数据）
     - 单价（传入数据）
     - 数量（传入数据）
   - 优惠信息
      - 优惠金额
      - 优惠信息（传入数据）
   - 结算
      - 总价（传入数据）
      - 优惠金额（传入数据）
      - 结算金额

###  支付页面(用户、商户共用)
- 界面要素
  - 订单信息
    - 订单号
    - 流水号
    - 订单信息(多行)
      - 商品名称
      - 单价
      - 数量
    - 优惠信息
      - 优惠金额
      - 优惠信息
    - 结算
      - 总价
      - 优惠金额
      - 结算金额
  - 按钮
    - 支付结果下拉菜单：模拟支付成功、失败
    - 确定按钮：确定支付成功或者支付失败
      - 商户点确定，页面跳转到添加商品信息页面
      - 用户点确定，页面跳转到扫描订单页面
    - 取消支付按钮：
      - 商户取消支付该订单，页面跳转到添加商品信息页面
      - 用户取消支付该订单，页面跳转到扫描订单页面

### 对话框通知界面
- 类型(消息、错误、警告)
- 标题
- 消息内容
- 按钮
  - 确定

## 界面事件  
分析对应界面的事件或与对应界面相关的事件
### 登陆界面  
- 登录事件  
- 初始化订单编号事件  
- 初始化订单事件  
- 初始化优惠事件  

### 创建订单界面  
- 添加订单事件

### 订单界面  
- 插入优惠信息事件  
- 取消订单事件  
- 删除订单事件  
- 支付请求事件  

### 扫描订单界面   
- 匹配订单事件  
- 插入订单信息事件

### 支付界面  
- 支付结果事件
- 支付取消事件

### 对话框通知界面

## Action(向服务器发送和请求的数据)
### 登陆页面
- 向服务器传入参数：userId,userType
- 服务器返回数据：商品信息
  - eg:`send:{"eventType":"ORDER_ITEMS","orderId":"11","items":[{"name":"ONLY修身撞色拼接女针织裙","price":34950,"quantity":2},{"name":"ONLY圆点荷叶边女修身裙","price":19950,"quantity":1},{"name":"ONLY棉宽松字母牛仔女外套","price":27450,"quantity":1}]}  
        send:{"eventType":"MARKETING","orderId":"11","amt":58650,"msg":"测试优惠, 一律5折"}`
- 将商品信息存入state中

### 创建订单页面
- 向服务器传入参数：商品信息(Order[orderId].items)
- 服务器返回数据：订单信息
  - eg:`send:{"eventType":"ORDER_ITEMS","orderId":"12","items":[{"name":"ONLY修身撞色拼接女针织裙","price":34950,"quantity":2},{"name":"ONLY圆点荷叶边女修身裙","price":19950,"quantity":1},{"name":"ONLY棉宽松字母牛仔女外套","price":27450,"quantity":1}]}`
- 将订单信息存入state中

### 扫描页面
- 向服务器传入数据：orderId,userId,userType
- 服务器返回数据：订单信息
  - eg:`send:{"eventType":"ORDER_ITEMS","orderId":"14","items":[{"name":"ONLY修身撞色拼接女针织裙","price":34950,"quantity":2},{"name":"ONLY圆点荷叶边女修身裙","price":19950,"quantity":1},{"name":"ONLY棉宽松字母牛仔女外套","price":27450,"quantity":1}]}  
  send:{"eventType":"MARKETING","orderId":"14","amt":58650,"msg":"测试优惠, 一律5折"}  
  send:{"eventType":"MARKETING","orderId":"14","amt":58650,"msg":"测试优惠, 一律5折"}`
- 将订单信息存入state中

### 订单列表页面
- 支付按钮
  - 向服务器传入参数：orderId
  - 服务器返回数据：
    - eg:`send:{"eventType":"PAY_AUTH","orderId":"12"}`
    - 当有用户已进入支付页面时，其他任何用户点击支付按钮，服务器都不会返回数据
  - 将服务器返回信息存入state中（待定）
- 取消按钮
  - 向服务器传入参数：orderId
  - 服务器返回数据：
    - eg:`send:{"eventType":"PAY_COMPLETED","orderId":"13","result":false,"channel":"Client","msg":"取消"}
       send:{"eventType":"PAY_COMPLETED","orderId":"13","result":false,"channel":"Client","msg":"取消"}`
  - 更新state:删除当前订单

### 支付页面
- 确定按钮
  - 向服务器传入数据：OrderId
  - 服务器返回数据:
    - eg:`send:{"eventType":"PAY_COMPLETED","orderId":"12","result":true,"channel":"测试渠道","msg":"成功"}
       send:{"eventType":"PAY_COMPLETED","orderId":"12","result":true,"channel":"测试渠道","msg":"成功"}`
  - 更新state:删除当前订单
- 取消支付按钮
  - 场景1描述：商户和用户操作同一订单，商户进入支付页面，用户点击“支付”按钮变为灰色，商户“取消支付”的情况
  - 前提：商户在订单页面，用户也在订单页面
          商户点击“支付”按钮进入支付页面，然后用户点击“支付”按钮，按钮变为灰色
  - 操作：商户在支付页面点击“取消支付”按钮，用户端获得支付授权，从订单页面跳转进入支付页面，服务器返回信息
  - 向服务器传入数据：OrderId
  - 服务器返回数据:
    - eg:`send:{"eventType":"PAY_AUTH","orderId":"12"}`
  - 场景2:前提是只有商户登陆，且在支付页面
    - 商户点击取消订单，服务器未返回信息

***

## state数据结构  
store中state的数据结构 
```javascript
var state = {
    user: {
        userId: "1",
        userType: "MERCHANT"
    },
    orderIds: [1, 2],
    order: {
        1: {
            orderId: "1",
            items: [
                {"name": "ONLY修身撞色拼接女针织裙", "price": 34950, "quantity": 2},
                {"name": "ONLY圆点荷叶边女修身裙", "price": 19950, "quantity": 1},
                {"name": "ONLY棉宽松字母牛仔女外套", "price": 27450, "quantity": 1}
            ],
            isPayAuth:false
        },
        2: {
            orderId: "2",
            items: [
                {"name": "ONLY修身撞色拼接女针织裙", "price": 34950, "quantity": 2},
                {"name": "ONLY圆点荷叶边女修身裙", "price": 19950, "quantity": 1},
                {"name": "ONLY棉宽松字母牛仔女外套", "price": 27450, "quantity": 1}
            ],
            isPayAuth:false
        }
    },
    marketing: {
        1: {orderId: "1", amt: 58650, msg: "测试优惠, 一律5折"},
        2: {orderId: "2", amt: 58650, msg: "测试优惠, 一律5折"}
    }
}
```

# 目前存在问题-20160922
- 审核代码
- 补充文件描述
- 删除无用文件
- 测试采用BDD方式进行开发
- 返回多个通知，dialog框多次弹出的情况未考虑
- Bug：商户取消订单，用户在支付界面，用户的订单没被取消，还停留在原支付页面。
