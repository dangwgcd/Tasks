2016-09-07
----------

####1. 核心任务：
完成Android调试日志打点

#### 2. 任务周期
1天

####3.  相关解释：
从之前的解释我们可以知道，在fn固定的情况下，判断一个组件的状态和编写是否正确我们可以通过stats（外部属性）和props（内部状态）来得到结果


####4. 任务目标
对HTTP请求、Socket读写、View页面切换、View点击进行调试日志打点

####5. 任务步骤
1. 对HTTP请求进行调试日志打点，打点位置为<请求前>和<请求后>，请求前的打点需要打印对应的传递参数，请求后的打点需要包括成功和失败两种情况，例如：
```php
CLogger::logMessage('debug', 'request to login start');
CLogger::logMessage('debug', 'login data:'.json_encode($data));
Request::Get($data,function($error,$response,$data){
    if($err || $response['status_code'] != 200){
        CLogger::logMessage('debug', 'request login faild, error:'.$error['msg'].' response:'.json_encode($response));
    }else{
        CLogger::logMessage('debug', 'request login success, result:'.json_encode($data));    
    }
    CLogger:logMessage('debug', request to login end');
});
```
> 要求:
> 1. 请求前的打点若存在参数则需要打印传入的参数
> 2. 请求后的打点需要考虑成功和失败两种情况
> 3. 请求后成功需要打印对应的返回结果
> 4. 请求后失败需要打印对应的错误

2. 对Socket读写进行打点，socket read的数据需要包括原始数据和解析后的数据两个部分，socket write需要包括传入的数据
```php
    // socket read
    $data = socket.recv();
    CLogger::logMessage('debug','socket recevie original data:'.$data);
    $data = parse($data);
    CLogger::logMessage('debug','socket receive parse data:'.$data);
    
    // socket write
    $data = 'sth you write';
    CLogger::logMessage('debug','socket write start');
    socket.write($data);
    CLogger::logMessage('debug','socket write data:'.$data);
    CLogger::logMessage('debug','socket write success');
```

3. 对页面跳转进行打点，进入一个新页面则进行一次打点，打点需要包括所在ViewController的类名（也可以是其他的唯一名称）
```php
   class HomeViewController{
        public function HomeViewController(){
            CLogger::logMessage('debug','start in HomeViewController');
        }
   }
```

4. 对点击进行打点，点击调试日志包括界面上所有可被点击的区域
```php
    class HomeViewController{
        public function SosBtnClick(){
            CLogger:logMessage('debug','sos btn click');
        }
    }
```


#### 6.任务验收
1. HTTP打点符合要求
2. Socket打点符合要求
3. 页面跳转打点符合要求
4. 所有可点击区域打点符合要求