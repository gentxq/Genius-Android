## Version 1.0.0 Guide

[`中文`](CapriceGuide_Kit_1.0.0_ZHH) [`English`](CapriceGuide_Kit_1.0.0) [`Guides`](GuideCatalog) 

## 功能模块

* `command`
  > *  独立服务进程执行命令行工作
  > *  与`ProcessBuilder`操作类似
  > *  智能修正运行错误，解决运行故障
  > *  一键化的启动与取消操作，自由控制
  > *  可同步与异步方式执行，可回调事件

* `net `
  > *  一键`Ping` `DNS` `TelNet` `TraceRoute`
  > *  可控制，可取消；不必关心细节问题
  > *  并发的路由任务，可在40s左右测试完成

* `util`
  > *  `HashKit`  字符串与文件`MD5`获取
  > *  `Tools` `ID` `SN` 确定设备唯一标识
  > *  `Log` 如系统Log一样使用简单，一键开关
  > *  `Log` 可存储日志到文件，方便分析差错
  > *  `Log` 可添加事件监听，方便界面显示日志信息
  > *  `FixedList` 定长队列，自动弹出，保持队列数量
  > *  `UiKit` 支持子线程同步、异步切换到主线程操作
 > *  `GeniusException` 自定义异常类，可截获异常，做自定义处理



## 获取库

* `Star` 和 `Fork` 项目。
* `MavenCentral` 远程导入 :

```gradle
// 在项目 "build.gradle" 中添加
dependencies {
  compile 'com.github.qiujuer:genius-kit:3.0.0-SNAPSHOT'
}

```
## 更新日志

* 版本：`1.0.0`
* 日期：`2015-03-07`
* 日志：[`更新日志`](CapriceNotes_Kit_1.X)

## 使用方法

##### 初始化与销毁

```java
GeniusKit.initialize(Application application);
GeniusKit.dispose();

```
##### `command` 模块


```java
// 执行命令，后台服务自动控制
// 调用方式与ProcessBuilder传参方式一样
// timeout：任务超时值,可选参数
// params：执行参数，如："/system/bin/ping","-c", "4", "-s", "100","www.baidu.com"
Command command = new Command(int timeout, String... params);

// 同步方式
// 完成后结果直接返回
String result = Command.command(new Command(Command.TIMEOUT, "..."));

// 异步方式
// 结果以事件回调方式返回
Command command = new Command("...");
Command.command(command, new Command.CommandListener() {
    @Override
    public void onCompleted(String str) {
    }
    @Override
    public void onCancel() {
    }
    @Override
    public void onError(Exception e) {
    }
});

// 取消一个命令任务
Command.cancel(Command command);

// 重启 Command 服务
Command.restart();

// 销毁
// 调用 ‘Genius.dispose()’ 方法时默认调用
Command.dispose();

```

##### `net` 模块

```java
// Ping
// 传入域名或者IP
// 结果：是否执行成功、延时、丢包
Ping ping = new Ping("www.baidu.com");
// 开始
ping.start();
// 返回
if (ping.getError() == NetModel.SUCCEED) {}
else {}
...
其他操作与Ping类似
...

```

##### `util` 模块

```java
// ===================FixedList===================
// 固定长度队列
// 可指定长度，使用方法与普通队列类似
// 当加入元素数量达到指定数量时将弹出元素
// 头部插入尾部弹出，尾部插入头部弹出

// 初始化最大长度为5
FixedList<Integer> list = new FixedList<Integer>(5);

// 获取最大容量
list.getMaxSize();
// 调整最大长度；缩小长度时将自动删除头部多余元素
list.setMaxSize(3);

// 可使用List操作
List<Integer> list = new FixedList<Integer>(2);


// ====================HashKit==================
// 哈希计算（Md5）
// 可计算字符串与文件Md5值

// 获取字符串MD5
String hash = HashKit.getMD5String(String str);
// 获取文件MD5
String hash = HashKit.getMD5String(File file);


// ======================Log======================
// 日志类
// 调用方法与使用Android Log方法一样
// 可设置其是否存储日志信息
// 可拷贝日志信息到SD卡
// 可在主界面添加事件回调，界面实时显示日志

// 添加回调
// 回调类
Log.LogCallbackListener listener = new LogCallbackListener() {
    @Override
    public void onLogArrived(Log data) {
        ...
    }
};
// 添加
Log.addCallbackListener(listener);

// 是否调用系统Android Log，可控制是否显示
Log.setCallLog(true);
// 是否开启写入文件，文件数量，单个文件大小（Mb）
// 默认存储在程序目录/Genius/Logs
Log.setSaveLog(true, 10, 1);
// 设置是否监听外部存储插入操作
// 开启：插入外部设备（SD）时，将拷贝日志文件到外部存储
// 此操作依赖于是否开启写入文件功能，未开启则此方法无效
Log.setCopyExternalStorage(true, "Test/Logs");

// 拷贝内部存储的日志文件到外部存储（SD）
// 此操作依赖于是否开启写入文件功能，未开启则此方法无效
Log.copyToExternalStorage("Test/Logs");

// 设置日志等级
// ALL(全部显示)，VERBOSE到ERROR依次递减
Log.setLevel(Log.ALL);

// 添加日志
Log.d(TAG, "DEBUG ");


// ====================Tools====================
// 常用工具包
// 全部为静态方法，以后会持续添加完善

// 休眠
Tools.sleepIgnoreInterrupt(long time);
// 拷贝文件
Tools.copyFile(File source, File target);
// AndroidId
Tools.getAndroidId(Context context);
// SN编号
Tools.getSerialNumber();

```
