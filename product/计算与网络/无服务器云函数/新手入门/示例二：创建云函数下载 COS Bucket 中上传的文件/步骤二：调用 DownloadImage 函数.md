请用户按照以下步骤来测试刚刚创建的`DownloadImage`函数。

## 使用 COS 上传文件测试
1. 登录腾讯云控制台，导航至【对象存储服务】，在 Bucket 列表中选择第一步中创建的`TestBucket`Bucket，点击【上传文件】按钮，上传一张[示例图片:testimage.jpeg](https://mc.qcloudimg.com/static/img/e683e8627510ec92344329c5219c1939/testimage.jpeg)（您可将此示例图片下载到本地并用作测试）。

2. 导航至【无服务器云函数】，选择刚刚创建的`DownloadImage`。

3. 点击【日志】选项卡，观察是否有刚刚上传图片后的函数运行日志。日志中应有本地下载下来的图片的日志，类似：

```
Loading function
Uri is http://TestBucket-1251111111.cosgz.myqcloud.com/testimage.jpeg?sign=QlEyq+WH8g5RpD+L6sPk05XhVQthPTEyNTE3NjIyMjcmaz1BS0lEWURoMDg1eFFwNDgxNjF1T24yQ0tLVmJlZWJ2RHU2ak8mZT0xNDk2ODM5NDQ3JnQ9MTQ5NjgzOTE0NyZyPTk1NDI3NjgyNCZmPS8xNDcyNjQwNzgwXzg0X3cxNjE0X2g0NDAucG5nJmI9ZG9uZ3l1YW50ZXEE
Starting new HTTP connection (1): TestBucket-1251111111.cosgz.myqcloud.com
http://TestBucket-1251111111.cosgz.myqcloud.com:80 "GET /testimage.jpeg?sign=QlEyq+WH8g5RpD+L6sPk05XhVQthPTEyNTE3NjIyMjcmaz1BS0lEWURoMDg1eFFwNDgxNjF1T24yQ0tLVmJlZWJ2RHU2ak8mZT0xNDk2ODM5NDQ3JnQ9MTQ5NjgzOTE0NyZyPTk1NDI3NjgyNCZmPS8xNDcyNjQwNzgwXzg0X3cxNjE0X2g0NDAucG5nJmI9ZG9uZ3l1YW50ZXEE HTTP/1.1" 200 62296
Download file [/testimage.jpeg] Success
testimage.jpeg
```

## 手动模拟测试
您还可以通过手动输入类似COS触发的测试数据来观察函数运行状态。

1) 在测试函数弹出框中，从测试模版中选择`COS 上传/删除文件测试代码`，默认测试数据将出现在窗口中。需要做以下修改：

 - 把`cosBucket`中`name`更换成刚刚创建的`TestBucket`;
 - 把`cosObject`中`key`的值更换成刚刚上传的文件`\testimage.jpeg`；

如下：
```
{  
   "Records":[  
      {
        "event": {
          "eventVersion":"1.0",
          "eventSource":"qcs::cos",
          "eventName":"cos:ObjectCreated:*,
          "eventTime":"1970-01-01T00:00:00.000Z",
          "eventQueue":"qcs:0:cos:gz:1251111111:cos",
          "requestParameters":{
            "requestSourceIP": "111.111.111.111",
            "requestHeaders":{
              "Authorization": "Example"
            }
          }
         },
         "cos":{  
            "cosSchemaVersion":"1.0",
            "cosNotificationId":"设置的或返回的 ID",
            "cosBucket":{  
               "name":"TestBucket", # Notice Here
               "appid":"1251111111",
               "region":"gz",
            },
            "cosObject":{  
               "key":"/testimage.jpg", # Notice Here
               "size":"1024",
               "meta":{
                 "Content-Type": "text/plain",
                 "x-cos-meta-test": "自定义的 meta",
                 "x-image-test": "自定义的 meta"
               },
               "url": "访问文件的源站url"
            }
         }
      }
   ]
}  
```
2) 点击【运行】按钮，代码开始运行并将显示测试结果。其中：
 - 函数返回值部分将显示运行结果，还将显示代码中 `return` 语句返回的函数执行结果。
 - 运行信息部分将显示函数运行的时间、内存等信息。
 - 日志部分将显示函数运行时生成的日志，包括用户代码中的打印语句、函数运行失败trace stack等，将会写入至日志模块。

3) 您可以多运行几次，并点击【日志】选项卡来查看每一次运行的日志信息。