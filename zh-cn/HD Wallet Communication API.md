# 手机APP与硬件钱包外设的接口协议

<table>
  <tr><th>版本</th><th>日期</th><th>更新描述</th></tr>
  <tr><td>v1.1</td><td>2018-05-12</td><td>添加了助记词恢复相关接口</td></tr>
  <tr><td>v1.2</td><td>2018-05-14</td><td>将接口协议改成纯文本形式</td></tr>
  <tr><td>v1.3</td><td>2018-05-14</td><td>
    1. 删除设备重命名接口；
    <br>
    2. 删除重置设备密码接口。以上两个接口提供的功能可以通过其他形式解决。
  </td></tr>
  <tr><td>v1.4</td><td>2018-05-15</td><td>增加sign生成方式的说明</td></tr>
  <tr><td>v1.5</td><td>2018-05-30</td><td>
    1. 放行密码改成6位数字；
    <br>
    2. device_request_for_connection接口，新增一个root_address字段。
  </td></tr>
  <tr><td>v1.6</td><td>2018-05-30</td><td>
    新增app_request_for_backspace_number接口。
  </td></tr>

  <tr><td>v1.7</td><td>2018-06-13</td><td>
    修改了device_request_for_connection接口的描述文字。
  </td></tr>

  <tr><td>v1.8</td><td>2018-06-15</td><td>
    增加了app_request_for_signature_start、app_request_for_signature_finish、app_request_for_signature_check、app_request_for_restore_start、app_request_for_restore_check接口。
    <br>
    删除app_request_for_show_keyboard接口。
  </td></tr>

  <tr><td>v1.8</td><td>2018-06-15</td><td>
    修改了 app_request_for_restore_check 接口中的描述文字。
  </td></tr>

  
</table>

## API目录

<table>
  <tr><th>API分类</th><th>API</th><th>描述</th></tr>
  <tr><th rowspan="4">放行密码相关操作</th>
    <td><a href="#app_request_for_show_pwd_keyboard">app_request_for_show_pwd_keyboard</a></td>
    <td>显示密码输入界面</td></tr>
  <tr><td><a href="#app_request_for_input_pwd_finish">app_request_for_input_pwd_finish</a></td>
    <td>密码输入完成</td></tr>
  <tr><td><a href="#app_request_for_tap_pwd">app_request_for_tap_pwd</a></td>
    <td>当前输入的数字</td></tr>
  <tr><td><a href="#app_request_for_backspace_number">app_request_for_backspace_number</a></td>
    <td>删除输入的数字</td></tr>

  <tr><th rowspan="">外设插入，请求连接APP</th>
    <td><a href="#device_request_for_connection">device_request_for_connection</a></td>
    <td>每次插入设备需要给APP传递的消息</td></tr>


  <tr><th rowspan="4">设备初始化</th>
    <td><a href="#app_request_for_init">app_request_for_init</a></td>
    <td>在设备屏幕上显示助记词</td></tr>
  <tr><td><a href="#app_request_for_set_pwd">app_request_for_set_pwd</a></td>
    <td>设置密码</td></tr>
  <tr><td><a href="#app_request_for_mnemonic_page">app_request_for_mnemonic_page</a></td>
    <td>助记词显示页面的翻页</td></tr>
  <tr><td><a href="#app_request_for_init_check">app_request_for_init_check</a></td>
    <td>APP发起设置初始化完成确认</td></tr>

  <tr><th rowspan="7">设备恢复</th>
    <td><a href="#app_request_for_restore_start">app_request_for_restore_start</a></td>
    <td>开始恢复设备</td></tr>
  <tr><td><a href="#app_request_for_word_input">app_request_for_word_input</a></td>
    <td>单词输入确认</td></tr>
  <tr><td><a href="#app_request_for_word_input_finish">app_request_for_word_input_finish</a></td>
    <td>单词输入完成</td></tr>
  <tr><td><a href="#app_request_for_delete_word">app_request_for_delete_word</a></td>
    <td>删除整个单词</td></tr>
  <tr><td><a href="#app_request_for_backspace_word">app_request_for_backspace_word</a></td>
    <td>删除上一个输入的字符</td></tr>
  <tr><td><a href="#app_request_for_tap_word">app_request_for_tap_word</a></td>
    <td>显示刚刚输入的字母</td></tr>
  <tr><td><a href="#app_request_for_restore_check">app_request_for_restore_check</a></td>
    <td>设备恢复确认</td></tr>


  <tr><th rowspan="3">设备信息</th>
    <td><a href="#app_request_for_add_token">app_request_for_add_token</a></td>
    <td>记录用户已经持有的币种</td></tr>
  <tr><td><a href="#app_request_for_get_token_list">app_request_for_get_token_list</a></td>
    <td>获取当前外设的币种支持列表</td></tr>
  <tr><td><a href="#app_request_for_remove_token">app_request_for_remove_token</a></td>
    <td>请求设备移除用户已经持有的某个币种</td></tr>

  <tr><th rowspan="4">转账</th>
    <td><a href="#app_request_for_signature_start">app_request_for_signature_start</a></td>
    <td>开始签名</td></tr>
  <tr><td><a href="#app_request_for_signature">app_request_for_signature</a></td>
    <td>发送签名参数</td></tr>
  <tr><td><a href="#app_request_for_signature_finish">app_request_for_signature_finish</a></td>
    <td>签名结束</td></tr>
  <tr><td><a href="#app_request_for_signature_check">app_request_for_signature_check</a></td>
    <td>签名检查</td></tr>

</table>

## 手机APP与硬件钱包外设通信时的互信解决办法
  
设备出厂时，会内置device_id（设备ID）和api_secret（API密钥）两个字符串。连接手机后，手机会检查此设备是否合法。
device_id和api_secret任何情况下都不可以被修改。

### sign的生成方式
sign是由<code>device_id</code>, <code>api_secret</code>, <code>timestamp</code>, <code>method</code>这4个变量拼接成一个字符串，然后通过md5生成一个32位的校验码。此顺序不能改变！形式如下：
<pre><code>String sign = md5(device_id+api_secret+method+timestamp);</code></pre>

收到数据的一方会用同样的规则在本地生成一个<code>local_sign</code>，并进行对比。如果<code>local_sign!=sign</code>，说明请求不合法。

## 接口格式说明

数据传输的协议内容为文本格式，每一行代表协议的一个属性，以[\n]结束。格式为：属性名+冒号+[空格]+内容，例：
<pre><code>api_version: 1.0.1</code></pre>
不要以接口属性的顺序来解析，因为属性的位置非固定！！

所有请求的数据中均会带有如下5个属性，分别是：
<pre><code>api_version: 协议版本
method: 接口名称
timestamp: 当前发起请求的时间戳
device_id: 设备的ID
sign: 用于校验数据来源的签名
</code></pre>
  
所有应答的数据中均会带有如下六个字段，分别是：
<pre><code>api_version: 协议版本
method: 接口名称
timestamp: 当前发起请求的时间戳
device_id: 设备的ID
sign: 用于校验数据来源的签名
state: 表示请求是否成功的状态, success表示成功, fail表示失败
</code></pre>

### 请求成功后，对方给出的应答
  
  如果一个API请求的返回数据中包含state字段，并且state字段的值为“success”，表示这个请求成功。

<pre><code>api_version: 1.0.1         // (String) 协议版本号
method: device_request_for_connection // (String) 设备连接申请
timestamp: 12345678901                // (Int) 请求返回时的UTC时间戳
device_id: xxxxxxx                    // (String) 外设的ID
sign: 32位MD5字符串                    // (String) 用于信息校验的字符串
state: success                        // (String) 请求成功
</code></pre>

### 请求失败后，对方给出的应答
  
如果请求异常，那么会以错误码（err_no）的形式告知请求方，具体的错误码附在本文档的最后。

请求失败的示例：
<pre><code>api_version: 1.0.1         // (String) 协议版本号
method: device_request_for_connection // (String) 设备连接申请
timestamp: 12345678901                // (Int) 请求返回时的UTC时间戳
device_id: xxxxxxx                    // (String) 外设的ID
sign: 32位MD5字符串                    // (String) 用于信息校验的字符串
state: fail                           // (String) 请求失败
err_no: ERR_NO_PERMISSION             // (String) 错误码
</code></pre>

## 放行密码相关操作

### <a id="app_request_for_show_pwd_keyboard"></a>放行密码操作第一步，唤起放行密码界面&输入密码
<table><tr><th>接口名称</th><td>app_request_for_show_pwd_keyboard</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

APP发起，请求在设备屏幕上显示密码输入界面。放行密码是由6个数字组成。
密码输入界面上显示0~9这10个数字，每次显示，数字的位置都是随机的，如下:
<table><tr><td>1</td><td>3</td><td>5</td></tr>
<tr><td>0</td><td>2</td><td>9</td></tr>
<tr><td>4</td><td>8</td><td>7</td></tr>
<tr><td>6</td><td></td><td></td></tr></table>

#### APP请求的内容

<pre><code>method:app_request_for_show_pwd_keyboard
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_show_pwd_keyboard
...</code></pre>

##### 失败

<pre><code>method: app_request_for_show_pwd_keyboard
...</code></pre>

### <a id="app_request_for_input_pwd_finish"></a>放行密码操作第二步，输入已经键入的6位密码位置
<table><tr><th>接口名称</th><td>app_request_for_input_pwd_finish</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

当设备中出现了密码输入界面后，手机端APP可以给外设发送想要设置的密码的下标。可以通过手机端传入要设置的密码的下标。假设外设屏幕中显示的密码输入界面如下: 
<table><tr><td>1</td><td>3</td><td>5</td></tr>
<tr><td>0</td><td>2</td><td>9</td></tr>
<tr><td>4</td><td>8</td><td>7</td></tr>
<tr><td>6</td><td></td><td></td></tr></table>

当手机传来的num_index为0,2,4,8时，对应设备上的密码数字为：1,5,2,7

#### APP请求的内容

<pre><code>method: app_request_for_input_pwd_finish
num_index: 1,2,3,4    // (String) 设备屏幕上显示的密码数字的下标,以“,”隔开
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_input_pwd_finish
pwd_ticket: 123aew    // (String) 设备随机生成一个字符串，用于后续流程确认密码输入是否正确
...</code></pre>

##### 失败

<pre><code>method: app_request_for_input_pwd_finish
...</code></pre>

### <a id="app_request_for_tap_pwd"></a>放行密码操作，当前键入的密码位置(辅助接口)
<table><tr><th>接口名称</th><td>app_request_for_tap_pwd</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述
假设一个场景：在APP端点击一个密码的位置后，可以同步在设备的屏幕上显示选择好的数字。可以提升用户体验。
当设备中出现了密码输入界面后，手机端APP可以给外设发送想要设置的密码的下标。可以通过手机端传入要设置的密码的下标。假设外设屏幕中显示的密码输入界面如下: 
<table><tr><td>1</td><td>3</td><td>5</td></tr>
<tr><td>0</td><td>2</td><td>9</td></tr>
<tr><td>4</td><td>8</td><td>7</td></tr>
<tr><td>6</td><td></td><td></td></tr></table>

当手机传来的num_index为2，对应设备上的密码数字为：5

#### APP请求的内容

<pre><code>method: app_request_for_tap_pwd
num_index: 2    // (String) 设备屏幕上显示的密码数字的下标
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_tap_pwd
pwd_ticket: 123aew    // (String) 设备随机生成一个字符串，用于后续流程确认密码输入是否正确
...</code></pre>

##### 失败

<pre><code>method: app_request_for_tap_pwd
...</code></pre>

### <a id="app_request_for_backspace_number"></a>放行密码操作——助记词恢复——删除上一个输入的字符(辅助接口) 
<table><tr><th>接口名称</th><td>app_request_for_backspace_number</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  可以删除所录入的助最后一个数字，直到所有录入的数字删除为止。

#### APP请求的内容

<pre><code>method: app_request_for_backspace_number
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_backspace_number
  ...</code></pre>

##### 失败

<pre><code>method: app_request_for_backspace_number"
...</code></pre>


## 外设插入，请求连接APP

### <a id="device_request_for_connection"></a>插入设备后，外设向手机发送连接请求
<table><tr><th>接口名称</th><td>device_request_for_connection</td></tr>
<tr><th>数据发起方</th><td>外设</td></tr>
<tr><th>数据接收方</th><td>APP</td></tr></table>

#### 描述

  外设插入到手机后，必须给APP发送一条连接申请，这是所有其他通信的基础。

#### 外设请求的内容

<pre><code>method: device_request_for_connection
device_name: 设备名                // (String) 设备名字
device_software_version: 1.0.1    // (String) 设备软件版本
device_firmware_version: 1.0.1    // (String) 设备固件版本
initialized: 1                    // (String) 设备是否已经完成过初始化 1: 已经完成过初始化, 0: 未完成初始化（这是个新设备）
root_address: 1                    // (String) 比特币格式的账户ROOT地址
...</code></pre>

#### APP返回的内容

##### 成功

<pre><code>method: device_request_for_connection
user_unique_code: user_unique_code    // (String) 唯一标识用户的一个字符串
...</code></pre>

##### 失败
<pre><code>method: device_request_for_connection
...</code></pre>


## 设备初始化

  需要注意，如果设备之前已经被初始化，那么设备将不在接受设备初始化相关的请求

### <a id="app_request_for_init"></a>设备初始化 第一步，请求设备生成密钥，并在设备屏幕上显示助记词。
<table><tr><th>接口名称</th><td>app_request_for_init</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

 设备第一次初始化，需要外设使用BIP44协议生成比特币和以太币的私钥。生成好比特币和以太坊的私钥后，需要在设备的屏幕上显示助记词。
 助记词一共有12个，需要用户抄录下来。通过设备上的按键进行翻页。


#### APP请求的内容

<pre><code>method: app_request_for_init
...</code></pre>

#### 外设返回的内容
  
  当外设完成了生成比特币和以太币的私钥后，需要显示助记词。
  设备左侧按键为“上一步”，右侧按键为“下一步”。
  等用户点击“下一步”按键后，再次回放助记词，让用户核对。当用户再次“下一步”后，表示助记词填记录完成。这时给APP返回初始化完成请求。

##### 成功

当用户确认抄录完毕后，通过点击设备上的确认按键，返回此成功数据

<pre><code>method: app_request_for_init
...</code></pre>

##### 失败

<pre><code>method: app_request_for_init
...</code></pre>

### <a id="app_request_for_set_pwd"></a>设备初始化第二步，设置密码
<table><tr><th>接口名称</th><td>app_request_for_set_pwd</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

设备初始化时，到达设置密码的步骤，需要先调用"app_request_for_show_pwd_keyboard"接口，让设备调出放行密码输入窗口。然后执行正常的输入密码操作，最终换取pwd_ticket.

app_request_for_set_pwd接口调用成功后，设备需要将刚刚设置的放行密码保存在本地。

#### APP请求的内容

<pre><code>method: app_request_for_set_pwd
pwd_ticket: aasd1233    // (String) 放行密码确认后生成的凭证
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_set_pwd
...</code></pre>

##### 失败

<pre><code>method: app_request_for_set_pwd
...</code></pre>

### <a id="app_request_for_init_check"></a>设备初始化: 第三步，APP发起设置初始化完成确认
<table><tr><th>接口名称</th><td>app_request_for_init_check</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

当所有对设备的初始化设置全部完成后，手机端会向设备发送一个请求，询问是否初始化完成。设备端需要检查以下内容: 
<pre><code>1、比特币和以太币的秘钥是否生成。
2、放行密码是否设置完成。
3、设备名字是否设置完成。
</code></pre>
确认所有设置均成功后，设备端会保存一个状态，标识设备已经初始化完成。此处呼应device_request_for_connection接口的initialized状态。如果一个设备已经被初始化完成，那么就不能够再次被初始化。除非恢复出厂设置后。

#### APP请求的内容

<pre><code>method: app_request_for_init_check
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_init_check
...</code></pre>

##### 失败

<pre><code>method: app_request_for_init_check
...</code></pre>

### <a id="app_request_for_mnemonic_page"></a>设备初始化 助记词显示页面的翻页
<table><tr><th>接口名称</th><td>app_request_for_mnemonic_page</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

 助记词一共有12个，设备屏幕尺寸较小，可能需要多个页面方可全部显示出来。


#### APP请求的内容

<pre><code>method: app_request_for_mnemonic_page
page: 1    // (Int) 显示的页码
...</code></pre>

#### 外设返回的内容
  
##### 成功
<pre><code>method: app_request_for_mnemonic_page
...</code></pre>

##### 失败
<pre><code>method: app_request_for_mnemonic_page
...</code></pre>

## 设备恢复

### <a id="app_request_for_restore_start"></a>开始恢复设备。显示虚拟助记词录入的键盘
<table><tr><th>接口名称</th><td>app_request_for_restore_start</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  显示助记词的虚拟键盘界面。每次显示，字母顺序都是随机的。如下：
<table><tr><td>a</td><td>k</td><td>c</td><td>e</td><td>r</td><td>g</td><td>x</td></tr>
<tr><td>o</td><td>p</td><td>q</td><td>d</td><td>s</td><td>t</td><td>u</td></tr>
<tr><td>n</td><td>m</td><td>l</td><td>b</td><td>j</td><td>i</td><td>h</td></tr>
<tr><td>v</td><td>w</td><td>f</td><td>y</td><td>z</td><td></td><td></td></tr></table>

#### APP请求的内容

<pre><code>method: app_request_for_restore_start
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_restore_start
...</code></pre>

##### 失败

<pre><code>method: app_request_for_restore_start
...</code></pre>

### <a id="app_request_for_word_input"></a>单词输入
<table><tr><th>接口名称</th><td>app_request_for_word_input</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  已经选择的字母顺序。

#### APP请求的内容

<pre><code>method: app_request_for_word_input
letter_index: 1,2,3,4,5    // (String) 输入的字母顺序
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_word_input
...</code></pre>

##### 失败

<pre><code>method: app_request_for_word_input
...</code></pre>

### <a id="app_request_for_word_input_finish"></a>单词输入完成
<table><tr><th>接口名称</th><td>app_request_for_word_input_finish</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  助记词一共有12个。输入成功后，需要外设使用BIP44协议生成比特币和以太币的私钥。生成好比特币和以太坊的私钥后，需要在设备的屏幕上显示助记词。设置完成后，进入密码设置环节。至此与设备初始化的流程无异。

##### APP请求的内容

<pre><code>method: app_request_for_word_input_finish
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_word_input_finish
...</code></pre>

##### 失败

<pre><code>method: app_request_for_word_input_finish",
...</code></pre>

### <a id="app_request_for_delete_word"></a>删除已经输入的助记词(删除整个单词)
<table><tr><th>接口名称</th><td>app_request_for_delete_word</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  如果输入的助记词有错误，可以通过此接口删除已经输入的助记词。可以一直删除，直到之前输入的单词都删除完为止。

#### APP请求的内容

<pre><code>method: app_request_for_delete_word
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_delete_word
  ...</code></pre>

##### 失败

<pre><code>method: app_request_for_delete_word"
...</code></pre>

### <a id="app_request_for_backspace_word"></a>删除上一个输入的字符
<table><tr><th>接口名称</th><td>app_request_for_backspace_word</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  可以删除所录入的助记词最后一个字母，直到所有录入的助记词删除为止。

#### APP请求的内容

<pre><code>method: app_request_for_backspace_word
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_backspace_word
  ...</code></pre>

##### 失败

<pre><code>method: app_request_for_backspace_word"
...</code></pre>

### <a id="app_request_for_tap_key"></a>同步输入的字母（辅助）
<table><tr><th>接口名称</th><td>app_request_for_tap_key</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  当前录入的字母顺序。可用于APP与设备端屏幕显示的同步

#### APP请求的内容

<pre><code>method: app_request_for_tap_key
letter_index: 1    // (String) 输入的字母顺序
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_tap_key
...</code></pre>

##### 失败

<pre><code>method: app_request_for_tap_key
...</code></pre>


### <a id="app_request_for_backspace_word"></a>删除上一个输入的字符
<table><tr><th>接口名称</th><td>app_request_for_backspace_word</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  可以删除所录入的助记词最后一个字母，直到所有录入的助记词删除为止。

#### APP请求的内容

<pre><code>method: app_request_for_backspace_word
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_backspace_word
  ...</code></pre>

##### 失败

<pre><code>method: app_request_for_backspace_word"
...</code></pre>

### <a id="app_request_for_restore_check"></a>设备恢复完成后的检查
<table><tr><th>接口名称</th><td>app_request_for_restore_check</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

  设备恢复后，发起的检查确认。检查助记词和密码是否存已经正确保存到硬件中。

#### APP请求的内容

<pre><code>method: app_request_for_restore_check
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_restore_check
...</code></pre>

##### 失败

<pre><code>method: app_request_for_restore_check
...</code></pre>


## 设备信息

### <a id="app_request_for_add_token"></a>记录用户已经持有的币种
<table><tr><th>接口名称</th><td>app_request_for_add_token</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

#### APP请求的内容

<pre><code>method: app_request_for_add_token
token_type: eth_erc-20    // (String) 币种类型，分为coin和其他类型。目前支持的有coin和eth_erc-20两种
token_index": 60          // (Int) 币种的索引，参考: https://github.com/satoshilabs/slips/blob/master/slip-0044.md
token_name: IDHUB         // (String) 币种的名字。
token_address: xxxxx      // (String) 币种地址，可能是空字符串。目前只有在token_type是eth_erc-20时，才会用到。
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_add_token
...</code></pre>

##### 失败

<pre><code>method: app_request_for_add_token
...</code></pre>

### <a id="app_request_for_get_token_list"></a>获取当前外设的币种支持列表
<table><tr><th>接口名称</th><td>app_request_for_get_token_list</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述

#### APP请求的内容

<pre><code>method: app_request_for_get_token_list
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>// token_list表示这个设备下，用户拥有的代币列表，
// 其中：
// token_type: 代币类型
// token_index表示: 代币的索引
// token_name: 代币的名字
// token_address: 代币合约地址（可能为空，目前只有当token_type为eth_erc-20时，才会生效），如果没有token_address也要放置一个空的字符串。
// 由于每个用户都可能拥有多个代币，可以用逗号“,”把不同的代币区分开
// token_type、token_index、token_name、token_address的顺序是不变的。
token_list: [token_type]|[token_index]|[token_name]|[token_address],... 

// 例：
method: app_request_for_get_token_list
eth_erc-20|60|IDHUB|xxxxx,coin|60,ETH|,...
...</code></pre>

##### 失败

<pre><code>method: app_request_for_get_token_list
...</code></pre>

### <a id="app_request_for_remove_token"></a>请求设备移除用户已经持有的某个币种
<table><tr><th>接口名称</th><td>app_request_for_remove_token</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述
  外设需要根据token_type、token_index和token_address三个字段查找并移除持有的币种

#### APP请求的内容

<pre><code>method: app_request_for_remove_token
token_list: [token_type]|[token_index]|[token_address]
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_remove_token
...</code></pre>

##### 失败

<pre><code>method: app_request_for_remove_token
...</code></pre>


## 转账

### <a id="app_request_for_signature_start"></a>请求签名
<table><tr><th>接口名称</th><td>app_request_for_signature_start</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述
  请求外设开始签名。调用此接口，表示开始一次新的签名任务

#### APP请求的内容

<pre><code>method: app_request_for_signature_start
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_signature_start
...</code></pre>

##### 失败

<pre><code>method: app_request_for_signature_start
...</code></pre>

### <a id="app_request_for_signature"></a>请求签名
<table><tr><th>接口名称</th><td>app_request_for_signature</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述
  此方法有可能会循环调用。比如有多个input时，那么就会调用多次。

#### APP请求的内容

<pre><code>method: app_request_for_signature
token_type: coin           // (String) 数字货币类型
token_index: 60            // (Int) 数字货币SLIP44注册币种的索引
token_address: xxxx        // (String) 如果是erc-20代币，需要给出智能合约的地址
content: 00001002030xxx    // (String) 需要签名的内容
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_signature
...</code></pre>

##### 失败

<pre><code>method: app_request_for_signature
...</code></pre>

### <a id="app_request_for_signature_finish"></a>签名参数传输结束
<table><tr><th>接口名称</th><td>app_request_for_signature_finish</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述
  签名的所有参数传递结束

#### APP请求的内容

<pre><code>method: app_request_for_signature_finish
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_signature_finish
...</code></pre>

##### 失败

<pre><code>method: app_request_for_signature_finish
...</code></pre>

### <a id="app_request_for_signature_check"></a>签名结果检查
<table><tr><th>接口名称</th><td>app_request_for_signature_check</td></tr>
<tr><th>数据发起方</th><td>APP</td></tr>
<tr><th>数据接收方</th><td>外设</td></tr></table>

#### 描述
  签名结束后，验证放行密码是否正确。如果正确，返回签名后的交易字符串。

#### APP请求的内容

<pre><code>method: app_request_for_signature_check
pwd_ticket: abcdefg   // (String) 放行密码确认后，得出的凭证
...</code></pre>

#### 外设返回的内容

##### 成功

<pre><code>method: app_request_for_signature_check
content: 000000xxxxxxxxx0000    //（String）签名后的交易字符串
...</code></pre>

##### 失败

<pre><code>method: app_request_for_signature_check
...</code></pre>