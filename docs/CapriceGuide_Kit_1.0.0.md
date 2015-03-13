## Version 1.0.0 Guide

[`ÖÐÎÄ`](CapriceGuide_Kit_1.0.0_ZHH) [`English`](CapriceGuide_Kit_1.0.0) [`Guides`](GuideCatalog) 


## Function modules

* `command`
  > *  Independent service command-line work process execution
  > *  Similar to the `ProcessBuilder` operation
  > *  Intelligent correct operation, solve the operation problem
  > *  One key start and cancel the operation, control of freedom
  > *  Can be synchronous and asynchronous execution, the callback event


* `net tool`
  > *  One key `Ping` `DNS` `TelNet` `TraceRoute`
  > *  Can be controlled, can be cancelled;Don't need to care about the details
  > *  Concurrent routing tasks, can be in around 40 s testing is completed

* `util`
  > *  `HashKit` String with the file `MD5`
  > *  `Tools` `ID` `SN` Determine the device unique identifier
  > *  `Log` Such as system Log as simple to use, one key switch
  > *  `Log` Can store the log to a file, convenient analysis errors
  > *  `Log` You can add event listeners, convenient interface display log information
  > *  `FixedList` Fixed-length queue, automatic pop-up, keep the queue number
  > *  `UIKit` support the child thread `synchronization` `asynchronous` switching to the main thread
  > *  `GeniusException ` a customized exception£¬used intercept and do some thing when the exception is occur

## Get library

* `Star` and `Fork` this project.
* `MavenCentral` remote import:

```gradle
// Adding to your project "build.gradle" file
dependencies {
  compile 'com.github.qiujuer:genius-kit:3.0.0-SNAPSHOT'
}

```
## Update Log 

* Version: `1.0.0`
* Date: `2015-03-07`
* Log: [`Notes`](CapriceNotes_Kit_1.X)


## Method of application

##### Initialization and destruction

```java
GeniusKit.initialize(Application application);
GeniusKit.dispose();

```

##### `command` module

```java
// Execute the command, the background service automatic control
// The same way call way and the ProcessBuilder mass participation
// Timeout: Task timeout, optional parameters
// Params: executing params,such as: "/system/bin/ping","-c", "4", "-s", "100","www.baidu.com"
Command command = new Command(int timeout, String... params);

// Synchronous
// After the completion of the results returned directly
String result = Command.command(new Command(Command.TIMEOUT, "..."));

// Asynchronous
// Results to event callback method returns
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

// To cancel a task command
Command.cancel(Command command);

// Restart the Command service
Command.restart();

// Destroy
// Using 'Genius.dispose()' method is run this destroy
Command.dispose();

```


##### `net tool` module

```java
// Ping
// Introduced to the domain name or IP
// Result: delay, packet loss
Ping ping = new Ping("www.baidu.com");
// Start
ping.start();
// Return
if (ping.getError() == NetModel.SUCCEED) {}
else {}
...
Others similarly
...

```

##### `util` module

```java
// ===================FixedList===================
// Fixed length queue
// Can specify length, using methods similar to ordinary queue
// When join the element number to a specified number elements will pop up
// Insert the tail pop-up head, tail insertion head pops up

// Initialize the maximum length of 5
FixedList<Integer> list = new FixedList<Integer>(5);

// To obtain the maximum capacity
list.getMaxSize();
// Adjust the maximum length; Narrow length will be automatically deleted when the head redundant elements
list.setMaxSize(3);

// Using List to operation
List<Integer> list1 = new FixedList<Integer>(2);


// ====================HashKit==================
// Hash to calculate(Md5)
// String with the file can be calculated Md5 value

// Get the MD5
String hash = HashKit.getMD5String(String str);
// Access to the file MD5
String hash = HashKit.getMD5String(File file);


// ======================Log======================
// Log class
// Calls the method with using Android as the default method
// Can be set if the store log information
// Can copy the log information to SD card
// Can add the event callback in the main interface, the interface, real-time display the log

// Add a callback
// The callback class
Log.LogCallbackListener listener = new LogCallbackListener() {
    @Override
    public void onLogArrived(Log data) {
        ...
    }
};
// Adding
Log.addCallbackListener(listener);

// Whether Android call system Log, can control whether to display
Log.setCallLog(true);
// Is open to a file, the file number, a single file size (Mb)
// The default is stored in the application directory is /Genius/Logs
Log.setSaveLog(true, 10, 1);
// Set whether to monitor external storage inserts
// Open: insert an external device (SD), will copy the log files to external storage
// This operation depends on whether written to the file open function, not open, this method is invalid
Log.setCopyExternalStorage(true, "Test/Logs");

// Copies of internal storage log files to external storage(SD)
// This operation depends on whether written to the file open function, not open, this method is invalid
Log.copyToExternalStorage("Test/Logs");

// Set the log level
// ALL(show all)£¬VERBOSE to ERROR decreasing
Log.setLevel(Log.ALL);

// Add log
Log.d(TAG, "DEBUG ");


// ====================Tools====================
// Commonly used toolkit
// Are all static methods, later will continue to add

// Thread sleep
Tools.sleepIgnoreInterrupt(long time);
// Copy the files
Tools.copyFile(File source, File target);
// AndroidId
Tools.getAndroidId(Context context);
// SN Id
Tools.getSerialNumber();

```



