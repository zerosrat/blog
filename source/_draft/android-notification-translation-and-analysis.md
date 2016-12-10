title: Android Notification 官方文档翻译及解析
date: 2015-12-05 15:41:38
description: 学习并使用Notification
categories: Android
tag: Android
---
## 前言

Notification（以下会用 **通知** 替代）是一种消息，它会显示在你的应用程序的UI之外。当你告诉系统发出一个通知，它首先会以一个图标的形式显示在 **通知区域** (notification area)。要查看通知的详细内容，用户会打开 **通知抽屉** （notification drawer）。通知区域和通知抽屉都是用户能够随时查看的系统控制区域。

![通知区域中的通知](http://7xoxnz.com1.z0.glb.clouddn.com/notification_area.png)

![通知抽屉中的通知](http://7xoxnz.com1.z0.glb.clouddn.com/notification_drawer.png)

> 官方文档地址 - [Notification | Android](http://developer.android.com/guide/topics/ui/notifiers/notifications.html)

> 注：本指南使用的是 `Support Library version 4` 的 `NotificationCompat.Builder` 类，除非另有提及。
<!-- more -->
## 设计时需要考虑

通知，是Android UI的重要组成部分，拥有它自己的设计准则。Android 5.0引入了Material Design是尤为重要的一个变化，建议阅读 [Material Design](http://developer.android.com/training/material/index.html) 的更多信息。你可以阅读 [Notification](http://developer.android.com/design/patterns/notifications.html) 的设计指南，来了解如何设计通知和他们之间的交互。

## 创建一个通知

你需要在一个 `Notification.Builder`  对象中明确通知UI的信息和动作。要创建通知本身，你需要调用 `NotificationCompat.Builder.build()` ， 这个调用会返回一个包含你的声明内容的通知对象。要发出通知，调用 `NotificationManager.notify()` 即可。

### 需要明确的通知内容

一个 `Notification` 对象必须包含如下内容：

- 设置小图标 `setSmallIcon()`
- 设置标题 `setContentTitle()`
- 设置具体文本内容 `setContentText()`

### 通知的动作

虽然他们是可选的，但你至少需要为通知添加一个动作。这个动作允许用户直接从通知跳转到你应用中的一个活动，在那里用户能够看到更多内容或是更多操作。

通知提供了多种动作。你需要定义当用户点击通知被触发的动作；通常这个动作会打开你的应用中的活动。你也可以在通知上加上Button来添加其他动作，像是关闭闹钟或是对文本信息进行回复；这个特征在Android 4.1中加入。如果你使用了动作Button，你必须在你的活动中将其逻辑实现；更多请参考处理兼容性章节。

在一个通知中，动作是由 `PendingIntent` 定义的。调用 `setContentIntent()` 来添加PendingIntent后，当用户点击通知抽屉的通知时，就会跳转到一个活动中去了。

### 创建一个简单的通知

下面的代码定义了一个用户点击后可以返回活动的通知。

> 在官方版本的基础上，加入了点击通知后通知消失
> 具体的代码是 `notification.flags |= Notification.FLAG_AUTO_CANCEL;`
> 当然也可以调用 `NotificationManager` 的 `cancel()` 方法来让指定通知消失

``` java
NotificationCompat.Builder mbuilder = new NotificationCompat.Builder(MainActivity.this)
        .setSmallIcon(R.mipmap.ic_launcher)
        .setContentTitle("Title")
        .setContentText("Hello Notification");
//开启一个活动并保证活动的返回栈顺序，详情见之后章节        
TaskStackBuilder taskStackBuilder = TaskStackBuilder.create(MainActivity.this);
taskStackBuilder.addParentStack(SecondAty.class);
taskStackBuilder.addNextIntent(new Intent(MainActivity.this, SecondAty.class));
PendingIntent pendingIntent = taskStackBuilder.getPendingIntent(0, PendingIntent.FLAG_CANCEL_CURRENT);
mbuilder.setContentIntent(pendingIntent);

NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
Notification notification= mbuilder.build();
notification.flags |= Notification.FLAG_AUTO_CANCEL;
manager.notify(1, notification);
```
### 处理兼容性

并非所有通知特性在一个安卓版本是可用的，即使是这些方法在支持类 `NotificationCompat.Builder` 中有设置。例如，动作Button，它依赖于可展开通知，只会在Android 4.1 和更高版本出现（即使设置了动作Button，低版本中也不会出现）。

为了保证最好的兼容性，使用 `NotificationCompat` 和它的子类创建通知，特别是 `NotificationCompat.Builder`. 另外，照如下步骤来实现一个通知：

1. 为用户提供通知的功能，无论他们使用的是那个版本。为了做到这一点，保证所有功能是对用户是有用的。你或许要添加一个新的活动来做到这一点。
例如，你想要使用 `addAction()` 来提供控制多媒体播放的开始和停止功能，首先需要在活动中实现这个控制。
2. 保证所有用户可以在活动中使用到这个功能，即是点击通知之后进入活动。为了做到这一点，为活动创建 `PendingIntent` ，调用 `setContentIntent()` 来将 `PendingIntent` 添加到通知中。
3. 现在添加可展开通知特性到你的通知中。记住任何你加入的功能在活动中是有意义的。

## 开启一个活动并保证活动的返回栈顺序

当你从通知开启一个活动时，你必须保证用户期望的活动返回栈顺序。点击返回键从应用的正常返回一直到主屏幕。为了保证返回逻辑，你应该在一个新的栈中开启活动。你如何设置 `PendingIntent` 来获得一个新的栈取决于你开启活动的特性。这里通常有两种情况：

### 常规活动

    你开启的活动是应用工作流程的一部分时。在这种情况下，设置 `PendingIntent` 来开启一个新的栈，并且提供 `PendingIntent` 一个返回栈来重新产生应用的标准返回顺序。

    Gmail app的通知是个不错的例子。当你点击一个电邮信息的通知，你看见了通知。点击返回键使你从Gmail返回到主界面，就好像你之前已经从主界面进入了Gmail而非从一个通知中进入。

    不管是什么应用，你在什么时候点击通知，这都会发生。例如，如果你正在Gmail中写信息，这时你点击了一个Gmail信息的通知，你会立即进入那个email中。点击返回键会到收件箱，然后才是主屏幕，而非把你带到你正在编辑的信息。
    
### 特殊活动

    用户只会看见这个活动如果它是从通知跳转进入的。在某种意义上，通知不能完全展示出所需信息。在这种情况下，设置 `PendingIntent` 来开启一个新的栈。这时不需要创造一个返回栈，因为开启的活动不属于应用工作流程的一部分。点击返回键会返回主屏幕。
    
### 设置一个常规活动的 PendingIntent

为了设置 `PendingIntent` 进入活动，按如下步骤：

1. 在mainifest中定义你的application的活动层次
    a. 支持 Android 4.0.3 以及更早版本。为了做到这一点，通过添加一个 `<meta-data>` 标签作为 `<activity>` 的子标签来明确活动的之前活动。
    为这个元素设置 `android:name="android.support.PARENT_ACTIVITY"`.设置 `android:value="<parent_activity_name>`.详情见下面的示例XML。

    b. 支持 Android 4.1 以及更高版本。为了做到这一点，加入 `android:parentActivityName` 到你要开启的活动的 `<activity>` 元素中去。

示例XML：

```html
<activity
    android:name=".MainActivity"
    android:label="@string/app_name" >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
<activity
    android:name=".SecondAty"
    android:parentActivityName=".MainActivity">
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity"/>
</activity>
```

2. 创建基于开启活动的 `Intent` 的返回栈。
    a. 创建开启活动的 `Intent`
    b. 通过 `TaskStackBuilder.create()` 创建一个 `TaskStackBuilder` 对象。
    c. 通过 `addParentStack()` 来添加返回栈。
    d. 通过 `addNextIntent()` 来添加开启活动的 `Intent` 
    e. 如果你需要，通过调用 `TaskStackBuilder.editIntentAt()` 为 `intent` 对象添加参数。有时保证目标活动显示有意义的数据当用户点击返回键时是必要的。
    f. 调用 `getPendingIntent` 来得到 `PendingIntent` 对象。之后你可以将 `PendingIntent` 作为调用 `setContentIntent` 的参数。

如下是个示例代码段：

```java
TaskStackBuilder taskStackBuilder = TaskStackBuilder.create(MainActivity.this);
taskStackBuilder.addParentStack(SecondAty.class);
taskStackBuilder.addNextIntent(new Intent(MainActivity.this, SecondAty.class));
PendingIntent pendingIntent = taskStackBuilder.getPendingIntent(0, PendingIntent.FLAG_CANCEL_CURRENT);
mbuilder.setContentIntent(pendingIntent);
```

### 设置一个特殊活动的 PendingIntent

使用较少，直接放代码，以后用到再更新

```html
<activity
    android:name=".ResultActivity"
...
    android:launchMode="singleTask"
    android:taskAffinity=""
    android:excludeFromRecents="true">
</activity>
```

```java
// Instantiate a Builder object.
NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
// Creates an Intent for the Activity
Intent notifyIntent =
        new Intent(this, ResultActivity.class);
// Sets the Activity to start in a new, empty task
notifyIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK
                        | Intent.FLAG_ACTIVITY_CLEAR_TASK);
// Creates the PendingIntent
PendingIntent notifyPendingIntent =
        PendingIntent.getActivity(
        this,
        0,
        notifyIntent,
        PendingIntent.FLAG_UPDATE_CURRENT
);

// Puts the PendingIntent into the notification builder
builder.setContentIntent(notifyPendingIntent);
// Notifications are issued by sending them to the
// NotificationManager system service.
NotificationManager mNotificationManager =
    (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
// Builds an anonymous Notification object from the builder, and
// passes it to the NotificationManager
mNotificationManager.notify(id, builder.build());
```
