#### 隐式intent #####

**1、创建隐式的intent之前，需要对原例子进行如下操作：**

在crimeFragment 的布局上添加CHOOSE SUSPECT 和 SEND CRIME REPORT 按钮；

在Crime 类中添加保存的嫌疑人姓名的mSuspect变量;

使用格式化的字符串资源创建消息模板； 

**2、使用格式化的字符串(格式化的说明符)**
    `%[argument_index$][flags][width][.precision]conversion
//格式化修饰符的抽象表示形式`

**3、隐式intent的组成**

1、要执行的操作
通常以Intent 类中的常量进行表示。

2、要访问的数据位置， 
可能是设备以外的资源。
例如：
    final Intent pickContact = new Intent(Intent.ACTION_PICK , ContactsContract.Contacts.CONTENT_URI);
    
要执行的操作是：Intent.ACTION_PICK
要访问的数据位置：ContactsContract.Contacts.CONTENT_URI

3、发送隐式intent的时候，需要获取返回的结果的话。需要使用
    startActivityForResult(pickContact , REQUEST_CONTACT);
pickContact:是隐式intent.
REQUEST_CONTACT是请求代码，用来区别返回数据的来源于哪里。

4、操作数据库的查询语句各个参数代表的意思：
    Cursor c = getActivity().getContentResolver().query(contactUri , queryFields ,null ,null , null);

其中，`contavtUri`参数代表的是指定位置的Uri。
`queryFields` 参数代表的是需要返回那一列的数据，
例如联系人姓名的话:
    String[] queryFields = new String[] {ContactsContract.Contacts.DISPLAY_NAME}

![](https://developer.android.com/guide/topics/providers/images/content-provider-interaction.png)

5、利用intent发送消息
首先创建隐式的intent：
     Intent i = new Intent(Intent.ACTION_SEND);
                i.setType("text/plain");
                i.putExtra(Intent.EXTRA_TEXT , getCrimeReport());
                i.putExtra(Intent.EXTRA_SUBJECT , getString(R.string.crime_report_subject));
                /*
                 *  可以取消用户的默认选项设置。
                 **/
                i = Intent.createChooser(i , getString(R.string.send_report));
                startActivity(i);

同时可以创建一个action选择器：用于每次都选择特定的应用。

    i = Intent.createChooser(i , getString(R.string.send_report));

