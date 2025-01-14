<img src="https://github.com/mailhu/email/blob/master/image/title.png"  height="200" width="600">

# Email for Android
[![](https://jitpack.io/v/mailhu/email.svg)](https://jitpack.io/#mailhu/email)

&emsp;&emsp;Email for Android是基于JavaMail封装的电子邮件库，简化在Android客户端中编写发送和接收电子邮件的的代码。把它集成到你的Android项目中，只需简单配置邮件服务器，即可使用，所见即所得哦！



# Install
步骤一、将JitPack存储库添加到根目录的build.gradle中：
```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```
步骤二、在项目的app模块下的build.gradle里加：
```gradle
dependencies {
    implementation 'com.github.mailhu:email:2.4.1'
}
```


# Instructions
#### 步骤一、在Android项目中的AndroidManifest.xml文件中添加联网权限。
```xml
<uses-permission android:name="android.permission.INTERNET"/>
```
#### 步骤二、在Java代码中使用教程。

##### 1.配置邮件服务器的信息
```java
//配置邮件服务器
EmailConfig emailConfig = new EmailConfig()
        .setSmtpHost("smtp.qq.com")             //设置发件服务器地址，网易邮箱为smtp.163.com
        .setSmtpPort(465)                       //设置发件服务器端口，网易邮箱为25
        .setPopHost("pop.qq.com")               //设置收件服务器地址，网易邮箱为pop.163.com
        .setPopPort(995)                        //设置收件服务器端口，网易邮箱为110
        .setImapHost("imap.qq.com")             //设置收件服务器地址，网易邮箱为imap.163.com
        .setImapPort(993)                       //设置收件服务器端口，网易邮箱为993
        .setAccount("from@qq.com")              //你的邮箱地址
        .setPassword("abcdefg");                //你的邮箱密码或授权码
```

##### 2.发送邮件的代码
```java
//邮件发送，确保配置emailConfig的信息正确
EmailSendClient emailSendClient = new EmailSendClient(emailConfig);
emailSendClient        
        .setTo("to@qq.com")                         //收件人的邮箱地址
        .setCc("cc@qq.com")                         //抄送人的邮箱地址
        .setBcc("bcc@qq.com")                       //密送人的邮箱地址
        .setNickname("百年小糊涂")                   //设置发信人的昵称
        .setSubject("这是一封测试邮件")               //邮件主题
        .setText("Hello World !")                   //邮件正文，若是发送HTML类型的正文用setContent()
        .sendAsyn(this, new GetSendCallback() {     //this是调用该代码的Activity
            @Override
            public void sendSuccess() {
                //发送成功（这里可更新UI）
            }

            @Override
            public void sendFailure(String errorMsg) {
                //发送失败，errorMsg是错误信息（这里可更新UI）
            }
        });
```

##### 3.接收邮件的代码
使用POP3协议接收邮件
```java
//获取邮件，确保配置emailConfig的信息正确
EmailReceiveClient emailReceiveClient = new EmailReceiveClient(emailConfig);
emailReceiveClient
        .popReceiveAsyn(this, new GetReceiveCallback() {   //this是调用该代码的Activity
            @Override
            public void gainSuccess(List<EmailMessage> messageList, int count) {
                //获取邮件成功（这里可更新UI）
            }

            @Override
            public void gainFailure(String errorMsg) {
                //获取邮件失败，errorMsg是错误信息（这里可更新UI）
            }
        });
```
使用IMAP协议接收邮件
```java
//获取邮件，确保配置emailConfig的信息正确
EmailReceiveClient emailReceiveClient = new EmailReceiveClient(emailConfig);
emailReceiveClient
        .imapReceiveAsyn(this, new GetReceiveCallback() {   //this是调用该代码的Activity
            @Override
            public void gainSuccess(List<EmailMessage> messageList, int count) {
                //获取邮件成功（这里可更新UI）
            }

            @Override
            public void gainFailure(String errorMsg) {
                //获取邮件失败，errorMsg是错误信息（这里可更新UI）
            }
        });
```

##### 4.检查邮箱和配置的服务器的信息是否正确的代码
```java
//验证邮箱和检查邮件服务器
EmailExamine emailExamine = new EmailExamine(emailConfig);
emailExamine
        .connectServer(this, new GetConnectCallback() {     //this是调用该代码的Activity
            @Override
            public void loginSuccess() {
                //邮箱登录成功（这里可更新UI）
            }

            @Override
            public void loginFailure(String errorMsg) {
                //邮箱登录失败，errorMsg是错误信息（这里可更新UI）
            }
        });
```

##### 5.检查对方是否为垃圾邮件发送者的代码
```java
//检查对方是否为垃圾邮件发送者的代码，使用Foxmail邮箱域名为示例
EmailExamine examine = new EmailExamine();
examine.spamCheck(this, "www.foxmail.com", new GetSpamCheckCallback() {     //this是调用该代码的Activity
    @Override
    public void gotResult(boolean isSpammer) {
        String s = (isSpammer)? "是垃圾邮件发送者" : "不是垃圾邮件发送者";
        Log.d("oversee", s);
    }
});
```

注：sendAsyn()、popReceiveAsyn()、imapReceiveAsyn()，connectServer()，spamCheck()等方法传入参数Activity时，代表执行完子线程后会切回主线程，若不传入参数Activity时，子线程执行完毕不会切回主线程

#### 步骤三、若使用QQ邮箱发送邮件，登录QQ邮箱，进入【设置】-【帐户】，把下列服务开启，然后获取授权码。如下图：

<img src="https://github.com/mailhu/email/blob/master/image/image_1.PNG"  height="200" width="600">

<img src="https://github.com/mailhu/email/blob/master/image/image_2.PNG"  height="250" width="600">



# ProGuard
```
-dontwarn org.apache.**
-dontwarn com.sun.**
-dontwarn javax.activation.**
-keep class org.apache.** { *;}
-keep class com.sun.** { *;}
-keep class javax.activation.** { *;}
-keep class com.smailnet.email.** { *;}
```



# Update log
### &ensp;Email for Android 2.4.1
1. 修复解析邮件内容出现的bug

### &ensp;Email for Android 2.4.0
1. 增加检测垃圾邮件发送者的接口

### &ensp;Email for Android 2.3.2
1. 再次修复ContentUtil类异常

### &ensp;Email for Android 2.3.1
1. 修复ContentUtil类异常

### &ensp;Email for Android 2.3
1. 增加设置发信人昵称的接口
2. 增加设置抄送人，密送人的接口
3. 增加设置文本型邮件正文的setText( )接口
4. 设置收件人的接口使用setTo( )代替setReceiver( )，2.3版本后建议使用setTo( )

### &ensp;Email for Android 2.2
1. 对发送邮件和接收邮件等接口增加一些新特性
2. 优化部分代码

### &ensp;Email for Android 2.1
1. 增加使用IMAP协议接收邮件的接口
2. 增加检查Host和Port的工具类

### &ensp;Email for Android 2.0.1
1. 修改个别类和方法的命名

### &ensp;Email for Android 2.0
1. 增加使用POP3协议接收邮件接口
2. 增加检查邮件服务器是否可连接的接口
3. 重构EmailConfig类
4. 重构使用SMTP协议发送邮件的类
5. 2.0版本是全新版本（不向下兼容）

### &ensp;Email for Android 1.1
1. 优化和重构代码
2. 增删和修改个别接口

### &ensp;Email for Android 1.0
1. 只需简单配置，即可发送一封电子邮件 


# About the author
##### 邮箱：zguanhu@foxmail.com

# License
```
Copyright 2018 张观湖

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
