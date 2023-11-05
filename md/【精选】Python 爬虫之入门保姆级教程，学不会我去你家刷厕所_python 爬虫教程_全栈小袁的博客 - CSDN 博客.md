> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_47971206/article/details/117267037?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169918426216800192225092%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169918426216800192225092&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-117267037-null-null.142^v96^pc_search_result_base2&utm_term=python%E7%88%AC%E8%99%AB&spm=1018.2226.3001.4187)

*   注重版权，转载请注明原作者和原文链接
*   作者：Bald programmer

今天这个教程采用最简单的爬虫方法，适合小白新手入门，代码不复杂
-------------------------------

#### 文章目录

*   [今天这个教程采用最简单的爬虫方法，适合小白新手入门，代码不复杂](#_4)
*   *   [首先打开咋们的网站](#_10)
    *   *   [一、导入相关库（requests 库）](#requests_19)
        *   [二、相关的参数（url，headers）](#urlheaders_25)
        *   [三、向网站发出请求](#_45)
        *   [四、匹配（re 库，正则表达式）](#re_63)
        *   [五、获取图片，保存到文件夹中（os 库）](#os_92)
        *   [完整代码](#_126)

爬虫的介绍以及原理等等七七八八的东西我就不多 bb 了，咋们直接上教程

本案例我就以 彼岸图网 这个网站做教程，原网址下方链接

[https://pic.netbian.com/](https://pic.netbian.com/)  

### 首先打开咋们的网站

可以看到有很多好看的图片，一页总共 21 张图片  
![](https://img-blog.csdnimg.cn/20210525233901886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
我们右键选择`检查`或者直接按`F12`来到控制台  
点击左上角的`箭头`或者快捷键`ctrl+shift+c`，然后随便点在一张图片上面  
![](https://img-blog.csdnimg.cn/20210525234505208.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
![](https://img-blog.csdnimg.cn/20210525234701843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
这时候我们就能看到这张图片的详细信息，`src`后面的链接就是图片的链接，将鼠标放到链接上就能看到图片，这就是我们这次要爬的  
![](https://img-blog.csdnimg.cn/20210525234816804.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)

#### 一、导入相关库（requests 库）

```
import requests

```

requests 翻译过来就是请求的意思，用来向某一网站发送请求  

#### 二、相关的参数（url，headers）

  
我们回到刚刚的控制台，点击上方的`Network`，按下`ctrl+r`刷新，随便点开一张图片  
![](https://img-blog.csdnimg.cn/2021052523565590.png)  
![](https://img-blog.csdnimg.cn/20210525235816491.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
这里我们只需要到两个简单的参数，本次案例只是做一个简单的爬虫教程，其他参数暂时不考虑

<table><thead><tr><th>参数</th><th>作用</th></tr></thead><tbody><tr><td>Request URL</td><td>发送请求的网站地址，也就是图片所在的网址</td></tr><tr><td>user-agent</td><td>用来模拟浏览器对网站进行访问，避免被网站监测出非法访问</td></tr></tbody></table>

![](https://img-blog.csdnimg.cn/20210526000505777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
![](https://img-blog.csdnimg.cn/20210526000543614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
参数代码的准备

```
url = "https://pic.netbian.com/uploads/allimg/210317/001935-16159115757f04.jpg"
headers = {
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36"
}

```

#### 三、向网站发出请求

```
response = requests.get(url=url,headers=headers)
print(response.text) # 打印请求成功的网页源码，和在网页右键查看源代码的内容一样的

```

![](https://img-blog.csdnimg.cn/20210526111229667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
这时候我们会发现乱码？！！！！这其实也是很多初学者头疼的事情，乱码解决不难

```
# 通过发送请求成功response，通过(apparent_encoding)获取该网页的编码格式，并对response解码
response.encoding = response.apparent_encoding
print(response.text)

```

看着这些密密麻麻的一大片是不是感觉脑子要炸了，其实我们只需要找到我们所需要的就可以了  
![](https://img-blog.csdnimg.cn/20210526003901134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
  

#### 四、匹配（re 库，正则表达式）

  
什么是正则表达式？简单点说就是由用户制定一个规则，然后代码根据我们指定的所规则去指定内容里匹配出正确的内容  
我们在前面的时候有看到图片信息是什么样子的，根据信息我们可以快速找到我们要的  
![](https://img-blog.csdnimg.cn/20210526004156390.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
![](https://img-blog.csdnimg.cn/20210526004343288.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
接下来就是通过正则表达式把一个个图片的链接和名字给匹配出来，存放到一个列表中

```
import re
"""
. 表示除空格外任意字符（除\n外）
* 表示匹配字符零次或多次
? 表示匹配字符零次或一次
.*? 非贪婪匹配
"""
# src后面存放的是链接，alt后面是图片的名字
# 直接(.*?)也是可以可以直接获取到链接，但是会匹配到其他不是我们想要的图片
# 我们可以在前面图片信息看到链接都是/u····开头的，所以我们就设定限定条件(/u.*?)这样就能匹配到我们想要的
parr = re.compile('src="(/u.*?)".alt="(.*?)"')
image = re.findall(parr,response.text)
for content in image:
    print(content)

```

![](https://img-blog.csdnimg.cn/20210526005515141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
这样我们的链接和名字就存放到了 image 列表中了，通过打印我们可以看到以下内容  
`image[0]`：列表第一个元素，也就是链接和图片  
`image[0][0]`：列表第一个元素中的第一个值，也就是链接  
`image[0][1]`：列表第一个元素中的第二个值，也就是名字  
![](https://img-blog.csdnimg.cn/20210526112104734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)

#### 五、获取图片，保存到文件夹中（os 库）

首先通过`os库`创建一个文件夹（当前你也可以手动在脚本目录创建一个文件夹）

```
import os
path = "彼岸图网图片获取"
if not os.path.isdir(path):
    ok.mkdir(path)

```

然后对列表进行遍历，获取图片

```
# 对列表进行遍历
for i in image:
    link = i[0] # 获取链接
    name = i[1] # 获取名字
    """
    在文件夹下创建一个空jpg文件，打开方式以 'wb' 二进制读写方式
    @param res：图片请求的结果
    """
    with open(path+"/{}.jpg".format(name),"wb") as img:
        res = requests.get(link)
        img.write(res.content) # 将图片请求的结果内容写到jpg文件中
        img.close() # 关闭操作
    print(name+".jpg 获取成功······")

```

运行我们就会发现报错了，这是因为我们的图片链接不完整所导致的  
![](https://img-blog.csdnimg.cn/20210526114057233.png)  
我们回到图片首页网站，点开一张图片，我们可以在地址栏看到我们的图片链接缺少前面部分，我们复制下来 `https://pic.netbian.com`  
![](https://img-blog.csdnimg.cn/20210526114609622.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
在获取图片的发送请求地址前加上刚刚复制的`https://pic.netbian.com`![](https://img-blog.csdnimg.cn/20210526114810990.png)  
运行，OK，获取完毕  
![](https://img-blog.csdnimg.cn/20210526114906284.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)  
![](https://img-blog.csdnimg.cn/20210526115301983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Nzk3MTIwNg==,size_16,color_FFFFFF,t_70)

#### 完整代码

```
import requests
import re
import os

url = "https://pic.netbian.com/"
headers = {
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36"
}

response = requests.get(url=url,headers=headers)
response.encoding = response.apparent_encoding

"""
. 表示除空格外任意字符（除\n外）
* 表示匹配字符零次或多次
? 表示匹配字符零次或一次
.*? 非贪婪匹配
"""
parr = re.compile('src="(/u.*?)".alt="(.*?)"') # 匹配图片链接和图片名字
image = re.findall(parr,response.text)

path = "彼岸图网图片获取"
if not os.path.isdir(path): # 判断是否存在该文件夹，若不存在则创建
    os.mkdir(path) # 创建
    
# 对列表进行遍历
for i in image:
    link = i[0] # 获取链接
    name = i[1] # 获取名字
    """
    在文件夹下创建一个空jpg文件，打开方式以 'wb' 二进制读写方式
    @param res：图片请求的结果
    """
    with open(path+"/{}.jpg".format(name),"wb") as img:
        res = requests.get("https://pic.netbian.com"+link)
        img.write(res.content) # 将图片请求的结果内容写到jpg文件中
        img.close() # 关闭操作
    print(name+".jpg 获取成功······")

```

本次教程到这里就结束了，是不是只爬了一页这么一点图片觉得不过瘾？

别急，下期我教大家如何获取十几页或者几十页甚至几百页的图片

*   本次文章分享就到这，有什么疑问或有更好的建议可在评论区留言，也可以私信我
*   感谢阅读~