title: 快速入门
---


## 1 支持版本
<div class="env">
    <p class="env-title">环境准备</p>
    <ul>
        <li>支持 Android Studio 1.4 以上或者 Eclipse ADT 15.0.0 以上版本</li>
        <li>支持 JDK 7.0 以上版本</li>
        <li>支持 Android 手机系统 4.0.3以上版本，即 Android SDK 15 以上版本</li>
    </ul>
</div>

## 2 创建 Wilddog 应用
 
首先，你需要在控制面板中创建应用。请参考  [控制面板-创建应用](/console/creat.html)。

## 3 安装 SDK
1. 安装 Wilddog IM SDK：
Android Studio 使用 Gradle 添加 Wilddog IM 的依赖。在你的 build.gradle 添加：

<figure class="highlight java"><table><tbody><tr><td class="code"><pre><div class="line">compile <span class="string">&apos;com.wilddog.client:wilddog-IM-android:<span class="android-auth-version"></span>&apos;</span></div></pre></td></tr></tbody></table></figure>

2. 工程配置
Wilddog IM 解决方案在 Android 上需要 android.permission.INTERNET 权限。你需要在 AndroidMainfest.xml 文件添加：
```xml
  <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.GET_TASKS" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    <uses-permission android:name="android.permission.READ_LOGS" />
    <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.VIBRATE" />
    <uses-permission android:name="android.permission.INTERACT_ACROSS_USERS_FULL" />
    <uses-permission android:name="android.permission.BROADCAST_STICKY" />
    <uses-permission android:name="android.permission.RECEIVE_USER_PRESENT" />
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
     <!--小米push权限-->
   <permission android:name="YOUR_PACKAGE_NAME.permission.MIPUSH_RECEIVE" android:protectionLevel="signature" />
    <uses-permission android:name="YOUR_PACKAGE_NAME.permission.MIPUSH_RECEIVE" />

   <application
        <!-- push 图标--> 
        android:icon="@mipmap/wildicon" >

   <provider
            android:authorities="com.wilddog.provider"
            android:name=".wildim.core.db.WildDBProvider"
            android:exported="true"
            />

       <!--wilddogsync 中的广播接受者-->
   <receiver android:name="com.wilddog.client.receiver.WilddogAuthCastReceiver">
            <intent-filter>
                <action android:name="com.wilddog.wilddogauth.signinsuccess"/>
                <action android:name="com.wilddog.wilddogauth.signoutsuccess"/>
            </intent-filter>
        </receiver>

   <!--华为：-->
    <!-- 第三方相关 :接收Push消息（注册、Push消息、Push连接状态、标签，LBS上报结果）广播 -->
        <receiver android:name="com.wilddog.wilddogimpush.receiver.WilddogHuaweiPushReceiver" >
        <intent-filter>
            <!-- 必须,用于接收token-->
            <action android:name="com.huawei.android.push.intent.REGISTRATION" />
            <!-- 必须，用于接收消息-->
            <action android:name="com.huawei.android.push.intent.RECEIVE" />
            <!-- 可选，用于点击通知栏或通知栏上的按钮后触发onEvent回调-->
            <action android:name="com.huawei.android.push.intent.CLICK" />
            <!-- 可选，查看push通道是否连接，不查看则不需要-->
            <action android:name="com.huawei.intent.action.PUSH_STATE" />
            <!-- 可选，标签、地理位置上报回应，不上报则不需要 -->
            <action android:name="com.huawei.android.push.plugin.RESPONSE" />
        </intent-filter>
        <meta-data android:name="CS_cloud_ablitity" android:value="successRateAnalytics"/>
    </receiver>

 <receiver
            android:name="com.huawei.android.pushagent.PushEventReceiver"
            android:process=":pushservice" >
            <intent-filter>
                <action android:name="com.huawei.android.push.intent.REFRESH_PUSH_CHANNEL" />
                <action android:name="com.huawei.intent.action.PUSH" />
                <action android:name="com.huawei.intent.action.PUSH_ON" />
                <action android:name="com.huawei.android.push.PLUGIN" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_ADDED" />
                <action android:name="android.intent.action.PACKAGE_REMOVED" />

                <data android:scheme="package" />
            </intent-filter>
        </receiver>
        <receiver
            android:name="com.huawei.android.pushagent.PushBootReceiver"
            android:process=":pushservice" >
            <intent-filter>
                <action android:name="com.huawei.android.push.intent.REGISTER" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
            </intent-filter>
            <meta-data
                android:name="CS_cloud_version"
                android:value="\u0032\u0037\u0030\u0035" />
        </receiver>

        <!-- PushSDK:Push服务 -->
        <service
            android:name="com.huawei.android.pushagent.PushService"
            android:process=":pushservice" >
        </service>
    <!--小米 push 服务-->

    <service
            android:enabled="true"
            android:process=":pushservice"
            android:name="com.xiaomi.push.service.XMPushService"/>
        <service
            android:name="com.xiaomi.push.service.XMJobService"
            android:enabled="true"
            android:exported="false"
            android:permission="android.permission.BIND_JOB_SERVICE"
            android:process=":pushservice" />
        <!--注：此service必须在3.0.1版本以后（包括3.0.1版本）加入-->
        <service
            android:enabled="true"
            android:exported="true"
            android:name="com.xiaomi.mipush.sdk.PushMessageHandler" />
        <service android:enabled="true"
            android:name="com.xiaomi.mipush.sdk.MessageHandleService" />
        <!--注：此service必须在2.2.5版本以后（包括2.2.5版本）加入-->
        <receiver
            android:exported="true"
            android:name="com.xiaomi.push.service.receivers.NetworkStatusReceiver" >
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </receiver>
        <receiver
            android:exported="false"
            android:process=":pushservice"
            android:name="com.xiaomi.push.service.receivers.PingReceiver" >
            <intent-filter>
                <action android:name="com.xiaomi.push.PING_TIMER" />
            </intent-filter>
        </receiver>
        <receiver android:name=".wilddogimpush.receiver.WilddogMiPushReceiver"
            android:exported="true"
            >
            <intent-filter>
                <action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED" />
            </intent-filter>
            <intent-filter>
                <action android:name="com.xiaomi.mipush.ERROR" />
            </intent-filter>

        </receiver>


        <-- 小米的push服务的APPID 和APPKEY 注意：以M开头加APPID或者AppKey-->  
        <meta-data
            android:name="Xiaomi_APPID" android:value="M2882303761517522993" />
        <meta-data
            android:name="Xiaomi_APPKEY"  android:value="M5161752266993" />
       <-- 华为的push服务的APPID  注意：以H开头加APPID-->  
        <meta-data
            android:name="Huawei_APPID"  android:value="H10710217" />
       <-- Wilddog的APPID  注意：以W开头加APPID-->  
        <meta-data
            android:name="Wilddog_APPID"  android:value="Wimdemo" />
   </application>

```

## 4 初始化

在一切操作之前，必须先进行一次初始化，设置 Wilddog AppID 和 AndroidContext。可以在 onCreate 方法中设置。

```java
WilddogIMClient wilddogIMClient = WilddogIMClient.newInstance(context, "APP ID",null);

```
## 5 建立连接

Wilddog IM 解决方案会和野狗服务器建立一个长连接，以达到能实时接收消息的目的。你可以通过 addConnectionListener 方法来监听连接状态。调用 connect() 方法来建立连接。
```java
// 监听连接状态
wilddogIMClient.addConnectionListener(this)
// 和野狗服务器建立连接
wilddogIMClient.connect();

```
## 6 用户登录
一般 APP 都会有自己的用户系统，野狗通过 JWT Token 的方式来集成 APP 已有用户。更多信息请参考 [用户集成章节]()

```java
client.signIn(token, new WildValueCallBack<WilddogUser>() {
     @Override
     public void onSuccess(WilddogUser wilddogUser) {
          // 登陆成功后的操作
          }

          @Override
          public void onFailed(int code, String des) {
              Log.e("result",des);
          }
      });
 ```
## 5 发起聊天

聊天分为单聊和讨论组，野狗 IM 解决方案不严格区分它们，当集合为单个userId的时候为单聊，当集合大于1的时候则生成群聊，不需要传入当前用户。
```java
Conversation conversation = WilddogIMClient.newConversation(Array.asList("user id"));
String messageText = "Hi! How are you";
TextMessage message = Message.newMessage(messageText);
conversation.send(message);
```
##6 接收消息

```java
// 注册接收监听
client.addMessageListener(listener);

//实现监听方法

private WilddogIMClient.WilddogIMMessageListener listener=new WilddogIMClient.WilddogIMMessageListener() {
    @Override
    public void onNewMessage(List<com.wilddog.wildim.message.Message> messages) {
        for(com.wilddog.wildim.message.Message wildMessage:messages){
            switch (message.getMessageType()) {

            case TEXT:
            //文本消息
            TextMessage textMessage = (TextMessage)message;

            case IMAGE:
            //图片消息
            ImageMessage imageMessage = (ImageMessage)message;

            case VOICE:
            //语音消息
            VoiceMessage voiceMessage = (VoiceMessage)message;

           }
        }
    }
};
```