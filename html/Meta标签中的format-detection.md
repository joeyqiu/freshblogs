# Meta标签中的format-detection

format-detection —— 格式检测，用来检测html里的一些格式，主要有以下几个设置： 

* name=”format-detection” content=”telephone=no”
* name=”format-detection” content=”email=no”
* name=”format-detection” content=”adress=no”



或者直接写成

* name=”format-detection” content=”telephone=no,email=no,adress=no”



### telephone 
主要作用是是否设置自动将你的数字转化为拨号连接 
telephone=no 禁止把数字转化为拨号链接 
telephone=yes 开启把数字转化为拨号链接，默认开启

### email 
告诉设备不识别邮箱，点击之后不自动发送 
email=no 禁止作为邮箱地址 
email=yes 开启把文字默认为邮箱地址，默认情况开启

### adress 
adress=no 禁止跳转至地图 
