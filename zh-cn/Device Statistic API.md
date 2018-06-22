# 设备统计 API

统计设备的主要指标。采用<code>HTTP协议</code>。目前需要统计的功能包括：<code>设备连接</code>，<code>设备转账</code>，<code>设备恢复</code>。

## 请求方式
<code>HTTP/HTTPS</code>

## Request 方法
<code>POST</code>

## HTTP 消息体
所有接口内容通过<code>JSON</code>字符串的形式传递。

## 接口格式

接口内容为JSON字符串，示例：
<pre><code>{
  "device_id": "xxxxx",           // (String) 必选. 设备的ID
  "timestamp": 1234567890,        // (String) 必选. 请求发起时的UTC时间戳

  "data": {                       // (Object) 必选. 不同的method对应不同的data. 没有内容时，传递空结构体 {}
    "country": "China",
    "province": "Beijing",
    "city": "Beijing"
  }
}</code></pre>

接口返回的数据格式：
<pre><code>{
  "state": 1,                     // (Int) 必选. 1表示请求成功, 0表示请求失败 

  "data": {                       // (Object) 必选.没有内容时，传递空结构体 {}
    "err_no": "DEVICE_NOT_FOUND"  // (Int) 如果state为0时，返回的错误代码. 如果state为0时，此为必选项
  }
}</code></pre>

对于接口的必选项，后续API描述中不再重复展示。

## 接口定义

### 设备连接

#### 请求地址
<code>/device/connect</code>

#### 请求的数据内容
<pre><code>{
  "data": {
    "software_version": "1.0.1",           // (String) 必选. 设备软件版本
    "firmware_version": "1.0.1",           // (String) 必选. 设备固件版本
    "country": "China",                    // (String) 可选. 所在国家, 没有时传递空字符串
    "province": "Beijing",                 // (String) 可选. 所在省份, 没有时传递空字符串
    "city": "Beijing"                      // (String) 可选. 所在城市, 没有时传递空字符串
  }
}</code></pre>


### 设备转账

#### 请求地址
<code>/device/transfer</code>

#### 请求的数据内容
<pre><code>{
  "data": {
  }
}</code></pre>

### 设备恢复

#### 请求地址
<code>/device/restore</code>

#### 请求的数据内容
<pre><code>{
  "data": {
  }
}</code></pre>


