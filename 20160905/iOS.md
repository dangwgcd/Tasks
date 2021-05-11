2016-09-05
----------

####1. 核心任务：
 完成调试日志模块

#### 2. 任务周期
1.5天

####3.  相关解释：
我们如果将iOS和Android这样的GUI编程做一个通用模型，那么你通过各种数学演算后最终会得到这样一个公式：
```javascript
fn(states,props) = Component // 多简洁而美好的公式啊^_^
```
其中states代表的是提供给组件的外部数据，props代表的是组件内部自有的状态变量，通过某种fn的模式转换我们就可以得到一个组件对象Component，当然尽管理想情况下states和props是不可变（immutable）的，而现实中我们由于各种原因（你们懂的）没有100%实现这种模式，但我们可以得到一个结论，在fn固定的情况下（相当于你的业务逻辑实现），states和props才是决定一个组件的关键！
因此我们在DEBUG的时候（排除掉你自身的业务错误逻辑），我们判断一个组件的实现是否正确，那么依靠输出请求前后的调试日志就可以得到比较靠谱的判断。
>如果不太理解，可以直接跳过以上描述，只需要知道调试日志能够帮助我们更快的联调和解决问题就可以了

####4. 任务目标
完成调试日志logger类，并提供logMessage的类方法，并在debug=true的状态能够进行调试日志的采集工作且能够有对应界面操作，点击后依靠七牛的SDK上传对应的调试日志

####5. 任务步骤
1. 确定一个logger类，取名为CLogger
```php
class CLogger{
	// sth need to do...
}
```
2.在配置debug变量，当debug=true时进行调试日志的输出
```php
class CLogger{
	private $debugger = false;
}
```
3.编写logMessage类方法，当debug=false时跳过，当debug=true时利用对应的库进行日志打印（打印级别为debug）
```php
class CLogger{
	private $debugger = false;
	public static function logMessage($str){
		if(!$this->debugger){
			return;
		}

		// you should use lib's log method to write
	}
}
```
>要求：
> 1.每条打印的日志需要包含日志级别（debug）/ 打印时间 / 内容 / 调用位置 4个信息
> 2.存储的日志文件名称以小时进行分割，例如：debug20160904230000.log
> 3.日志文件需要可读可写，方便之后的上传

4.在应用相关设置列表处新增一个“上传调试日志”的button，当debug=true时才能够显示（所以现在你可以把上面的$debugger抽到配置文件中去了，配置文件可以是一个静态类（推荐），可以是一个纯文本text，也可以是一个yaml，也可以是一个json文件，怎麼方便怎么来）
```php
// 1.修改你的调试日志类
class CLogger{
	private $debugger = false;
	public function CLogger(){
		// 1.读取配置文件（若是文件）
		// 2.获取到debug的变量值
		$debug = Config::DEBUG;
		// 3.设置自己的$debugger
		$this->debugger = $debug;
	}
	public static function logMessage($str){
		if(!$this->debugger){
			return;
		}

		// you should use lib's log method to write
	}
}

// 2.增加一个上传日志的button在应用相关设置列表处
// sth to do...
```
5.在日志类中新增一个上传的方法，调用后将会上传存储目录下的所有调试日志到七牛云服务上，全部成功后删除本地的日志文件
```php
class CLogger{
	// sth you write already...

	public static function uploadLogs(){
		// 使用七牛的SDK进行日志的上传
		// 成功后删除本地的日志文件
	}
}
```
>上传所需：
>iOS SDK：http://developer.qiniu.com/code/v7/sdk/objc.html#io-put
>Android SDK：http://developer.qiniu.com/code/v7/sdk/android.html#io-put
>Bucket：ycn-debug-log
>AK：bd8LY9ZjlVrIlRKT0BGCIZdv0L_vY9on-adaq7z3
>SK：UShxqdbs-REiii2LFXhkf9XOzQpZsSgu1YrfBEUR

6.绑定上传日志操作，当点击后调用Clogger::uploadLogs方法进行上传，全部上传完成后给出弹框提示

#### 6.任务验收
1. 调试日志格式正确，符合要求
2. 日志文件名称正确，符合要求
3. 仅在debug=true时下进行日志打印
4. 仅在debug=true时在应用相关设置列表出出现上传调试日志按钮
5. 点击上传按钮后上传所有日志，成功后删除本地的日志