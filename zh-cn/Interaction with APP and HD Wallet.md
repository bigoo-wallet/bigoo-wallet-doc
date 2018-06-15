# APP与HD Wallet交互用例

<table>
  <tr><th>用例</th></tr>
  <tr><td><a href="#device_request_for_connection">设备每次插入手机</a></td></tr>
  <tr><td><a href="#device_init">硬件钱包初始化</a></td></tr>
  <tr><td><a href="#device_restore">硬件钱包设备恢复</a></td></tr>
  <tr><td><a href="#transfer">转账</a></td></tr>
</table>

## APP与HD Wallet交互的重点注意事项

每个设备在出厂时，均会内置<code>device_id</code>和<code>api_secret</code>。

所有请求的数据中均会带有如下5个属性，分别是：
<pre><code>api_version: 协议版本
method: 接口名称
timestamp: 当前发起请求的时间戳
device_id: 设备的ID
sign: 用于校验数据来源的签名</code></pre>

## 用例

### <a id="device_request_for_connection"></a>设备每次插入手机

#### 请求发起方
  
  硬件设备

#### 使用的接口
<pre><code>device_request_for_connection</code></pre>

#### 目的
  
  外设插入手机后，需要把外设的基本信息传给APP。只有连接成功后，才能进行后续的操作。

### 流程

<img src="https://raw.githubusercontent.com/bigoo-wallet/bigoo-wallet-doc/master/zh-cn/interaction-with-app-and-hd-wallet-img/device_request_for_connection.png">

### <a id="device_init"></a>硬件钱包初始化

#### 请求发起方

  APP

#### 使用的接口
<pre><code>app_request_for_init
app_request_for_set_pwd
app_request_for_init_check
app_request_for_mnemonic_page
app_request_for_show_pwd_keyboard
app_request_for_input_pwd_finish
app_request_for_tap_pwd
app_request_for_backspace_number</code></pre>

### 流程

<img src="https://raw.githubusercontent.com/bigoo-wallet/bigoo-wallet-doc/master/zh-cn/interaction-with-app-and-hd-wallet-img/device_init.png">


### <a id="device_restore"></a>硬件钱包设备恢复

#### 请求发起方

  APP

#### 使用的接口
<pre><code>app_request_for_signature_start
app_request_for_word_input
app_request_for_word_input_finish
app_request_for_delete_word
app_request_for_backspace_word
app_request_for_tap_word
app_request_for_show_pwd_keyboard
app_request_for_input_pwd_finish
app_request_for_tap_pwd
app_request_for_backspace_number
app_request_for_restore_check</code></pre>

### 流程

<img src="https://raw.githubusercontent.com/bigoo-wallet/bigoo-wallet-doc/master/zh-cn/interaction-with-app-and-hd-wallet-img/device_restore.png">

### <a id="transfer"></a>转账

#### 请求发起方

  APP

#### 使用的接口
<pre><code>app_request_for_signature_start
app_request_for_signature
app_request_for_signature_finish
app_request_for_signature_check
app_request_for_show_pwd_keyboard
app_request_for_input_pwd_finish
app_request_for_tap_pwd
app_request_for_backspace_number</code></pre>

### 流程

<img src="https://raw.githubusercontent.com/bigoo-wallet/bigoo-wallet-doc/master/zh-cn/interaction-with-app-and-hd-wallet-img/transfer.png">
