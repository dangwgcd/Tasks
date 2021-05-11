2016-09-05
----------

####1. 核心任务：
完成PetSocket2.0的命令测试工作

#### 2. 任务周期
1.5天

####3.  相关解释：
csf框架现已经升级为csf v0.11，csf v0.11与之前的v0.1框架在使用上是不兼容的，v0.11修正了一些需要修正的bug，并让框架的使用更接近于CodeIgniter，避免了多进程过程中global变量的同步问题，现在YCN-PetSocket的重构工作已经完成，并开辟了新的git项目PetSocket2.0，目前需要对PetSocket2.0进行相关命令的测试工作

####4. 任务目标
完成所有上传的硬件的指令的测试工作，并且确保现有所有的业务逻辑仍然是正确的

####5. 任务步骤
1. 代码部署

2. 在所有业务相关的判断分支（if、switch等）打印日志，例如：


```php
	// 不是所有if和switch都需要打日志，只需要在业务判断部分
	// 比如登录、下线、解析轨迹数据等这些关键节点才需要打印对应日志
	if(isLogin){
		CoreHelper::logMessage('debug','start in login');
	}else if(needParseData){
		CoreHelper::logMessage('debug','start in parse data');
	}
```
>对应分支的逻辑命名需要清晰易懂
>CoreHelper::logMessage('debug',111)  is bad
>CoreHelper::logMessage('debug', 'start in login') is good

3.在所有外部调用前后打印日志
```php
// sth more..

CoreHelper::logMessage('debug', 'relate model start: '+json_encode($data));
$this->relateModel->findByToken($data);
CoreHelper::logMessage('debug', 'relate model end');

// sth more..

CoreHelper::logMessage('debug','ssdb start: '+json_encode($data));
$this->ssdb->set($data);
CoreHelper::logMessage('debug', 'ssdb end');

// sth more..
```
>远程调用包括：
> 数据库调用（MySQL/MongoDB等），对应于Model
> 缓存调用（SSDB/Memcached/Redis等）
>HTTP服务（Curl等）
>消息队列（NSQ等）

4. 测试在现有csf v0.11框架下，接受到的每个指令业务逻辑是否仍然正确

5. 填写测试EXCEL表，示例如下：
<table>
	<tr>
		<th>功能</th>
		<th>指令号</th>
		<th>初始数据</th>
		<th>对应日志截图</th>
		<th>是否正确</th>
  </tr>
  <tr>
		<td>硬件登录操作#1</td>
		<td>AB</td>
		<td>AB1239723947234</td>
		<td>
			截图1.png<br/>
		</td>
		<td>YES</td>
</tr>
<tr>
		<td>硬件登录操作#2</td>
		<td>AB</td>
		<td>AB1239723947234</td>
		<td>
			截图2.png<br/>
		</td>
		<td>YES</td>
</tr>
<tr>
		<td>硬件登录操作#3</td>
		<td>AB</td>
		<td>AB1239723947234</td>
		<td>
			截图3.png<br/>
		</td>
		<td>YES</td>
</tr>
</table>
>每个指令的业务分支需要测试3次以上

#### 6.任务验收
1. 所有业务相关的判断分支均含有日志调用，且逻辑命名清晰易懂
2. 在所有远程调用前后均含有日志调用，且符合要求
3. 对所有指令完成测试，且填写完对应的测试表



