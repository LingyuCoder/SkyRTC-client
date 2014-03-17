SkyRTC-client
=============
##简介
一个Node.js编写的WebRTC浏览器端库，为客户端库，需要配合服务器端库[SkyRTC](https://github.com/LingyuCoder/SkyRTC)共同使用，用于搭建基于WebRTC和WebSocket技术的在线音频、视频聊天室

##SkyRTC客户端库
[SkyRTC](https://github.com/LingyuCoder/SkyRTC)

可使用npm进行安装：
```
$ npm install skyrtc
```

##SkyRTC实例
[SkyRTC-demo](https://github.com/LingyuCoder/SkyRTC-demo)是一个基于SkyRTC搭建的多房间的在线音频、视频、文字聊天室，并能够共享文件

##SkyRTC-client的使用
通过在HTML中引入JavaScript文件的方式引入：
```html
<script type="text/javascript" src="/SkyRTC-client.js"></script>
```

可通过如下方式监听SkyRTC-client的事件：
```javascript
SkyRTC.on('someEvent', function(params) {
    //...
});
```

连接后台WebSocket服务器：
```javascript
SkyRTC.connect("ws:" + window.location.href.substring(window.location.protocol.length).split('#')[0], window.location.hash.slice(1));
```

##方法
* connect
* on
* sendFile
* shareFile
* sendMessage
* broadcast
* sendFileAccept
* sendFileRefuse
* createStream
* attachStream

###connect
连接WebSocket后台服务器，建立信令交互的信道，并加入到一个房间之中

参数：
* server——服务器地址
* room——房间名称

返回值：
无

###on
向SkyRTC-client相关事件绑定回调函数

参数：
* eventName——事件名称
* callback——事件触发时的回调函数

返回值：
无

###sendFile
向所在房间中的某个特定用户请求发送文件

参数：
* dom——包含需要被发送的文件的input[type='file']的id或者dom对象
* socketId——接收文件的用户的id

###shareFile
向所在房间中的所有其他用户请求发送文件

参数：
* dom——包含需要被发送的文件的input[type='file']的id或者dom对象

返回值：
无

###sendMessage
向所在房间内的某个特定用户发送消息

参数：
* message——需要被发送的消息内容
* socketId——接收消息用户的id

返回值：
无

###broadcast
向所在房间中的所有其他用户发送文字消息

参数：
* msg——需要被发送的消息字符串

返回值：
无

###sendFileAccept
接收到文件发送请求后，同意接收文件

参数：
* sendId——发送文件的id

返回值：
无

###sendFileRefuse
接收到文件发送请求后，拒绝接收文件

参数：
* sendId——发送文件的id

返回值：
无

###createStream
创建本地视频流

参数：
* constraints——创建的视频流的约束对象

返回值：
无

###attachStream
接收到远程视频流后，将视频流绑定到video标签上

参数：
* stream——远程视频流对象
* domId——需要被绑定的video标签的id

返回值：
无

##原生事件
###连接建立
* connected
* socket_opened
* socket_error
* socket_closed
* socket_receive_message

###信令交换
* get_peers
* get_ice_candidate
* get_offer
* get_answer
* new_peer
* remove_peer

###建立流
* stream_created
* stream_created_error

###PeerConnection相关事件
* pc_get_ice_candidate
* pc_opened
* pc_add_stream
* pc_add_data_channel

###DataChannel相关事件
* data_channel_create_error
* data_channel_opened
* data_channel_closed
* data_channel_message
* data_channel_error

###文件发送相关
* send_file_error
* send_file
* send_file_refused
* send_file_accepted
* send_file_chunk
* sended_file
* receive_file_chunk
* receive_file
* receive_file_ask
* receive_file_error

##连接建立事件详解
###connected
在于后台服务器成功创立WebSocket连接后触发

参数：
* socket——与后台连接的WebSocket实例

###socket_opened
WebSocket连接开启后触发

参数：
* socket——与后台连接的WebSocket实例

###socket_error
WebSocket连接发生错误后触发

参数：
* error——错误对象
* socket——与后台连接的WebSocket实例

###socket_closed
WebSocket连接关闭后触发

参数：
* socket——与后台连接的WebSocket实例

###socket_receive_message
WebSocket连接接收到非自定义事件格式的信息时触发

参数：
* socket——与后台连接的WebSocket实例
* jsonMsg——JSON格式的message

##信令交换事件详解
###get_peers
在获取当前房间所有用户的id后调用

参数：
* socketIds——房间内其他用户的id字符串列表

###get_ice_candidate
在获得了ICE Candidate信令后调用

参数：
* candidate——ICE Candidate信令数据

###get_offer
在获得offer信令后调用

参数：
* offer——offer信令数据
###get_answer
在获得answer信令后调用

参数：
* answer——answer信令数据

###new_peer
在有新用户加入后调用

参数：
* socketId：新用户的id

###remove_peer
有用户断开连接后调用

参数：
* socketId：断开连接的用户的socket id

##建立流
###stream_created
成功建立本地视频流时触发

参数：
* stream——本地视频流对象

###stream_created_error
建立本地视频流失败时触发

参数：
* error——错误对象

##PeerConnection相关事件
###pc_opened
PeerConnection成功开启后触发

参数：
* socketId——PeerConnection所连接用户的id
* pc——成功开启的PeerConnection实例

###pc_get_ice_candidate
获得从ICE Candidate消息时触发

参数：
* candidate——ICE Candidate信令内容
* socketId——PeerConnection所连接用户的id
* pc——获得信令PeerConnection实例

###pc_add_stream
通过PeerConnection上接收到视频流后触发

参数：
* stream——添加的流对象
* socketId——PeerConnection所连接用户的id
* pc——增加流的PeerConnection实例

###pc_add_data_channel
通过PeerConnection上接收到data channel后触发

参数：
* channel——添加的data channel
* socketId——PeerConnection所连接用户的id
* pc——增加data channel的PeerConnection实例

##DataChannel相关事件
###data_channel_create_error
data channel创建失败时触发

参数：
* socketId——data channel所连接用户的id
* error——错误对象

###data_channel_opened
data channel成功开启后触发

参数：
* channel——开启的data channel实例
* socketId——data channel所连接用户的id

###data_channel_closed
data channel关闭后触发

参数：
* channel——被关闭的data channel实例
* socketId——data channel所连接用户的id

###data_channel_message
data channel上接收到数据且非文件信令类型时触发

参数：
* channel——接收到数据的data channel实例
* socketId——data channel所属用户的id
* message——json格式的接收到的具体数据信息

###data_channel_error
data channel发生错误时触发

参数：
* channel——发生错误的data channel实例
* socketId——data channel所连接用户的id
* error——错误对象

##文件发送相关
###send_file
读取需要发送的文件完毕，并请求对方接收时触发

参数：
* sendId：发送的文件id
* socketId：接收者的id
* file：被发送的文件对象

###send_file_error
发送方发送文件失败时触发

参数：
* error——错误对象
* socketId——接收者的id
* sendId——发送失败的文件的id，为空则表示获取文件失败
* file——发送失败的文件对象，为空则表示获取文件失败

###send_file_refused
对方拒绝接收文件时触发

参数：
* sendId——被拒绝接收的文件id
* socketId——接收者的id
* file——被拒绝接收的文件

###send_file_accepted
对方同意接收文件时触发

参数：
* sendId——被同意接收的文件id
* socketId——接收者的id
* file——被同意接收的文件

###send_file_chunk
文件碎片发送后触发

参数：
* sendId——文件碎片所属文件的id
* socketId——接收者的id
* percent——已发送的百分比
* file——被发送的文件对象

###sended_file
文件发送完成后触发

参数：
* sendId——被发送的文件Id
* socketId——接收者的id
* file——被发送的文件对象
###receive_file_chunk
接收到文件碎片时触发

参数：
* sendId——接收的文件的Id
* socketId——发送者的id
* fileName——文件的名称
* percent——文件接收到的百分比

###receive_file
接收完整个文件后触发

参数：
* sendId——接收的文件的Id
* socketId——发送者的id
* name——接收到的文件的名称

###receive_file_ask
接收到文件发送请求后触发

参数：
* sendId——被请求接收的文件的id
* socketId——发送者的id
* fileName——被请求接收的文件的名称
* fileSize——被请求接收的文件的大小

###receive_file_error
接收文件错误时触发

参数：
* error——错误对象
* sendId——接收文件的id
* socketId——发送者的id

