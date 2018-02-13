安装
----

-  安装SetupEChatCom.msi

-  将ActiveX.cab文件放在Web的根目录下

​

初始化
------

初始化配置
~~~~~~~~~~

-  传参返回值采用UTF-8编码
-  ``Context`` 和 ``Dns``
   配置，修改echat.ini文件（文件路径在桌面上面）：

   .. code:: ini

       [account]
       context = dev
       [network]
       dns = <语音服务IP:端口>

示例
----

登录示例
~~~~~~~~

加载ActiveX控件
^^^^^^^^^^^^^^^

注意： clsid必须为117343BA-BEAF-485D-B80C-7B5BE7395697不能更改

.. code:: html

        <object id="myAcX" classid="clsid:117343BA-BEAF-485D-B80C-7B5BE7395697" 
            codebase="ActiveX.cab#version=1,0,0,0">
        </object>

初始化EchatApi
^^^^^^^^^^^^^^

注意：actix.Init()只能初始化一次。

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript">
        window.actx = null
        window.myself;
        function start() {
            actx = document.getElementById("myAcX");
            if (actx != null) {
                //初始化EchatApi
                actx.Init();
                //登录方法
                actx.Login("13800000009", "1", 3);
            }
        }
        window.onload = start;
    </script>

登录成功事件
^^^^^^^^^^^^

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript"  event="onOnlineStatusChangedX(message)">
      if(message == 3){
         window.location.href = "WebForm2.aspx"; 
      }
    </script>

登录失败事件
^^^^^^^^^^^^

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onErrorX(messag,error)">
      //错误码可以查询  数据定义--登录错误码
      alert("onErrorX" + error);
    </script>

主页面示例
~~~~~~~~~~

加载ActiveX控件
^^^^^^^^^^^^^^^

注意： clsid必须为117343BA-BEAF-485D-B80C-7B5BE7395697不能更改

.. code:: html

     <object id="myAcX" classid="clsid:117343BA-BEAF-485D-B80C-7B5BE7395697"  
            codebase="ActiveX.cab#version=1,0,0,0">
     </object>

初始化EchatApi中的事件
^^^^^^^^^^^^^^^^^^^^^^

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript">
        window.actx = null
        window.myself;
        function start() {
            actx = document.getElementById("myAcX");
            if (actx != null) {
                actx.Init2();
            }
        }
        window.onload = start;
    </script>

接口定义
--------

账号管理
~~~~~~~~

Login方法（String,String,Int)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

调度员登录方法

语法
''''

.. code:: c#

      public int Login(string account, string password, int role)

参数

account

​ Type:System.String

​ 登录账号

password

​ Type:System.String

​ 明文密码

role

​ Type:System.Int32

​ 用户角色，0:对讲用户；3：调度员

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

备注
''''

参数role传固定值3

通知事件
''''''''

-  onOnlineStatusChangedX，当用户登录成功后触发，\ ``OnlineStatus`` 为
   ``ONLINE_STATUS_ONLINE``
-  ``onErrorX``\ ，当用户登录失败时触发。错误码参考： 错误码

示例
''''

.. code:: html

    <input id="Submit13" type="submit" value='登录' runat="server"  onclick="Login()" />

    <script language="javascript" type="text/javascript">
    //登录方法
    function Login(){
      actx.Login("13800000009", "1", 3);
    }
    </script>

    <script language="javascript" for="myAcX" type="text/javascript" event="onOnlineStatusChangedX(message)">
        alert("onOnlineStatusChangedX" + message);
        //message为3表示登录成功
        if (message == 3) {
            window.location.href = "WebForm2.aspx"; 
        }
    </script>

    <script language="javascript" for="myAcX" type="text/javascript" event="onErrorX(message,error)">
        alert("onErrorX" + message + error);
    </script>

Logout方法()
^^^^^^^^^^^^

退出登录界面

语法
''''

.. code:: c#

       public int Logout();

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

    <input id="Submit13" type="submit" value='退出' runat="server"  onclick="Logout()" />

    <script language="javascript" type="text/javascript">
    function Logout(){
      actx.Logout();
    }
    </script>

GetCurrentUser方法()
^^^^^^^^^^^^^^^^^^^^

获取用户信息

语法
''''

.. code:: c#

     public string GetCurrentUser()

返回值

Type: System.String

序列化的 JSON 字符串。

对象：用户实体类 User 参考 ：用户信息

示例
''''

代码示例

.. code:: html

    <input id="Submit37" type="submit" value='获取当前登录的账号信息' runat="server"  onclick="GetCurrentUser()" />

    <script language="javascript" type="text/javascript">
    function GetCurrentUser() {
            actx.GetCurrentUser();
      }
    </script>

返回数据示例

.. code:: json

    {"gid":0,"uid":1,"role":3,"online":0,"has_gid":0,"has_role":1,"audio_enabled":1,"name":"账号09"}

GetOnlineStatus方法()
^^^^^^^^^^^^^^^^^^^^^

获取当前用户在线状态

语法
''''

.. code:: c#

       public OnlineStatus GetOnlineStatus();

返回值

Type：OnlineStatus

返回在线状态的枚举，参考： 用户在线状态

示例
''''

.. code:: html

     <input id="Submit34" type="submit" value='获取在线状态' runat="server"  onclick="GetOnlineStatus()" />

    <script language="javascript" type="text/javascript">
    function GetOnlineStatus() {
         actx.GetOnlineStatus();
      }
    </script> 

GetUid方法()
^^^^^^^^^^^^

获取当前账号UID

语法
''''

.. code:: c#

      public int GetUid();

返回值

Type:System.Int32

返回当前登录用户的UID

​

示例
''''

.. code:: html

    <input id="Submit35" type="submit" value='获取账号ID' runat="server"  onclick="GetUid()" />

    <script language="javascript" type="text/javascript">
    function GetUid() {
       actx.GetUid();
     }
    </script>

GetName方法()
^^^^^^^^^^^^^

获取用户名

语法
''''

.. code:: c#

      public string GetName();

返回值

Type:System.Sting

返回当前登录用户的中文名

示例
''''

.. code:: html

    <input id="Submit36" type="submit" value='获取中文名' runat="server"  onclick="GetName()" />

    <script language="javascript" type="text/javascript">
    function GetName() {
           actx.GetName();
     }
    </script>

HasAudioEnabled方法()
^^^^^^^^^^^^^^^^^^^^^

是否被禁言状态

语法
''''

.. code:: c#

      public bool HasAudioEnabled();

返回值

Type：System.Boolean

true：正常发言 ，false：被禁言。

​

示例
''''

.. code:: html

    <input id="Submit36" type="submit" value='是否被禁言' runat="server"  onclick="HasAudioEnabled()" />

    <script language="javascript" type="text/javascript">
    function HasAudioEnabled() {
            actx.HasAudioEnabled();
        }
    </script>

ChangeName方法 (string)
^^^^^^^^^^^^^^^^^^^^^^^

修改用户名

语法
''''

.. code:: c#

      public int ChangeName(string name);

参数

name

​ Type:System.String

​ 登录账号

​

+--------+--------------+--------+
| 参数   | 描述         | 备注   |
+========+==============+========+
| name   | 新用户名称   |        |
+--------+--------------+--------+

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

​

通知事件
''''''''

-  ``onChangeNameResultX`` 修改用户名触发此事件，事件处理是否修改成功。

示例
''''

.. code:: html

    <input id="Submit18" type="submit" value='修改用户名' runat="server"  onclick="ChangeName()" />

    <script language="javascript" type="text/javascript">
    function ChangeName() {
            actx.ChangeName("账号09");
        }
    </script>

    <script language="javascript" for="myAcX" type="text/javascript" event="onChangeNameResultX(success)">
    if(success){
      //账号名称修改成功
      }else{
      //账号名称修改失败
      }
    </script>

ChangePassword方法(String,String)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

修改密码

语法
''''

.. code:: c#

      public int ChangePassword(string oldpassword, string newpassword);

参数

oldpassword

​ Type:System.String

​ 旧密码（明文）

newpassword

​ Type:System.String

​ 新密码（明文）

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

-  ``onChangePasswordResultX`` ,
   修改密码触发此事件，事件处理是否修改成功

示例
''''

.. code:: html

    <input id="Submit19" type="submit" value='ChangePassword' runat="server"  onclick="ChangePassword()" />

    <script language="javascript" type="text/javascript">
    function ChangePassword() {
            actx.ChangePassword("2","1");
      }
    </script>

    <script language="javascript" for="myAcX" type="text/javascript" event="onChangePasswordResultX(success)">
       if(success){
      //账号密码修改成功
      }else{
      //账号密码修改失败
      }
    </script>

SaveAccount方法(String)
^^^^^^^^^^^^^^^^^^^^^^^

保存账号

语法
''''

.. code:: c#

       public int SaveAccount(string account);

参数

account

​ Type: System.String

​ 序列化的 JSON 字符串。

​ 对象：用户实体类 User 参考 用户信息

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

    <input id="Submit14" type="submit" value='保存账号' runat="server"  onclick="SaveAccount()" />

    <script language="javascript" type="text/javascript">
    function SaveAccount() {
            var obj = new Object();
            obj.Name = "13800000009";
            obj.Password = "1";
            obj.Role = 3;
            obj.IsRemembered = 0;
            obj.IsAutoLogin = 0;
      
            var jsonStr = JSON.stringify(obj);
            actx.SaveAccount(jsonStr);
        }
    </script>

ClearAccount方法()
^^^^^^^^^^^^^^^^^^

清空已保存账号

语法
''''

.. code:: c#

      public int ClearAccount();

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

    <input id="Submit44" type="submit" value='ClearAccount' runat="server"  onclick="ClearAccount()" />

    <script language="javascript" type="text/javascript"> 
    function ClearAccount() {
            actx.ClearAccount();
        }
    </script>

HasAccount方法()
^^^^^^^^^^^^^^^^

是否已保存账号

语法
''''

.. code:: c#

      public bool HasAccount();

返回值

Type：System.Boolean

true：有 ，false：无

示例
''''

.. code:: html

    <input id="Submit15" type="submit" value='HasAccount' runat="server"  onclick="HasAccount()" />

    <script language="javascript" type="text/javascript">
    function HasAccount() {
      var result = actx.HasAccount();
      if(result){
        //有保存的账号
      }else{
        //没有保存的账号
      }
    }
    </script>

GetSavedAccount方法()
^^^^^^^^^^^^^^^^^^^^^

获取已保存的账号

语法
''''

.. code:: c#

      public string GetSavedAccount();

返回值

Type: System.String

序列化的 JSON 字符串。

对象：账号实体类 Account参考 账号信息

示例
''''

代码示例

.. code:: html

    <input id="Submit17" type="submit" value='获取已保存的账号' runat="server"  onclick="GetSavedAccount()" />

    <script language="javascript" type="text/javascript">
    function GetSavedAccount() {
            actx.GetSavedAccount();
        }
    </script>

返回值示例

.. code:: json

    {"Name":"13800000009","Password":"1","Role":3,"IsRemembered":0,"IsAutoLogin"：0}

LoginWithSaved方法()
^^^^^^^^^^^^^^^^^^^^

使用已存储的账号登录

语法
''''

.. code:: c#

      public int LoginWithSaved();

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

-  ``onOnlineStatusChangedX`` ，当用户登录成功后触发，\ ``OnlineStatus``
   状态改为 ``ONLINE_STATUS_ONLINE``
-  ``OnErrorX``\ ，当用户登录失败时触发。

示例
''''

.. code:: html

    <input id="Submit16" type="submit" value='LoginWithSaved' runat="server"  onclick="LoginWithSaved()" />

    <script language="javascript" type="text/javascript">
    function LoginWithSaved() {
           actx.LoginWithSaved();
        }
    </script>

onOnlineStatusChangedX事件
^^^^^^^^^^^^^^^^^^^^^^^^^^

登录状态改变通知

语法
''''

.. code:: c#

     void onOnlineStatusChangedX(object OnlineStatus);

参数

status

​ Type: System.Object 用户在线状态

​ OnlineStatus 参考： 用户在线状态

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onOnlineStatusChangedX(message)">
        if (message == OnlineStatus.ONLINE_STATUS_ONLINE) {
            //此为用户在线状态
        }
    </script>

onChangeNameResultX事件
^^^^^^^^^^^^^^^^^^^^^^^

修改用户名称通知

语法
''''

.. code:: c#

      void onChangeNameResultX(object success);

参数

success

​ Type: System.Object 修改名称结果

​ true：修改成功 false：修改失败

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onChangeNameResultX(success)">
    if(success){
      //账号名称修改成功
      }else{
      //账号名称修改失败
      }
    </script>

onChangePasswordResultX事件
^^^^^^^^^^^^^^^^^^^^^^^^^^^

修改密码通知

语法
''''

.. code:: c#

      void onChangePasswordResultX(object success);

​ 参数

success

​ Type: System.Object 修改密码结果

​ true：修改成功 false：修改失败

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onChangePasswordResultX(success)">
       if(success){
      //账号密码修改成功
      }else{
      //账号密码修改失败
      }
    </script>

onAudioEnableChangedX事件
^^^^^^^^^^^^^^^^^^^^^^^^^

当前用户遥开遥闭状态通知

语法
''''

.. code:: c#

       void onAudioEnableChangedX(object audio_enable);

参数

audio\_enable

​ Type: System.Object

​ true：遥开 false：遥闭

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onAudioEnableChangedX(audio_enable)">
      if(audio_enable){
        //遥开
      }else{
        //遥闭
      }
    </script>

onUserChangedX事件
^^^^^^^^^^^^^^^^^^

注册监听用户状态变化

-  被动触发条件

echat 服务内部 User 缓存变更时触发，例如用户状态改变或用户名改变。

当前用户所在群组里的其他用户正在讲话时

语法
''''

.. code:: c#

     void onUserChangedX(object count, object usersListjson);

参数

count

​ Type: System.Object 变化用户数

​ usersListjson：

​ Type：System.String

​ 序列化的 JSON 字符串。

​ 对象：用户实体类 User 参考 ：用户信息

​

示例
''''

代码示例

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onUserChangedX(count,usersListjson)">
        
    </script>

返回数据示例

.. code:: json

    usersListjson数据：
    [{"gid":600901,"uid":50711,"role":0,"online":1,"has_gid":1,"has_role":1,"audio_enable":1,"name":"账号01"}]

群组
~~~~

JoinGroup方法(Int)
^^^^^^^^^^^^^^^^^^

根据群组ID，进入到相应的群组里

语法
''''

.. code:: c#

      public int JoinGroup(int gid);

参数

gid

​ Type:System.Int32

​ 群组ID

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 通知事件

示例
''''

.. code:: html

    <input id="Submit22" type="submit" value='JoinGroup' runat="server"  onclick="JoinGroup()" />

    <script language="javascript" type="text/javascript">
        function JoinGroup()
        {
            actx.JoinGroup(600901);
        }
    </script>

LeaveGroup方法()
^^^^^^^^^^^^^^^^

用户离开当前所在群组

语法
''''

.. code:: c#

      public int LeaveGroup();

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 通知事件

示例
''''

.. code:: html

    <input id="Submit23" type="submit" value='LeaveGroup' runat="server"  onclick="LeaveGroup()" />

    <script language="javascript" type="text/javascript">
        function LeaveGroup() {
            actx.LeaveGroup();
        }
    </script>

Call方法(Array)
^^^^^^^^^^^^^^^

创建临时群组

语法
''''

.. code:: c#

      public int Call(Array uids)；

参数

uids

​ Type:System.Array

​ 一维Array，用户ID

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 通知事件

示例
''''

.. code:: html

    <input id="Submit7" type="submit" value='Call' runat="server"  onclick="Call()" />
    <script language="javascript" type="text/javascript">
      function Call() {
        var uids = new Array()
        uids[0] = 50720;
        uids[1] = 50720;
        
        actx.Call(uids);
        }
    </script>

销毁临时群组
^^^^^^^^^^^^

临时群组创建者退出临时群组即为销毁临时群组。

GetGroupList方法()
^^^^^^^^^^^^^^^^^^

获取当前登录的调度员所在集团的所有群组列表

语法
''''

.. code:: c#

      public string GetGroupList();

返回值

Type: System.String

序列化的 JSON 字符串。

对象：群组实体类 Group参考：群组信息

示例
''''

代码示例

.. code:: html

    <input id="Submit20" type="submit" value='GetGroupList' runat="server"  onclick="GetGroupList()" />

    <script language="javascript" type="text/javascript">
       function GetGroupList() {
            actx.GetGroupList();
        }
    </script>

返回值示例

GetGroupByGid方法(int )
^^^^^^^^^^^^^^^^^^^^^^^

根据GID 查询群组信息

语法
''''

.. code:: c#

      public string GetGroupByGid(int gid);

参数

gid

​ Type:System.Int32

​ 群组ID

返回值

Type: System.String

序列化的 JSON 字符串。

对象：群组实体类 Group参考：群组信息

示例
''''

代码示例

.. code:: html

     <input id="Submit20" type="submit" value='GetGroupByGid' runat="server"  onclick="GetGroupByGid()" />

    <script language="javascript" type="text/javascript">
       function GetGroupByGid() {
           actx.GetGroupByGid(600901);
        }
    </script>

返回值示例：

GetCurrentGroup()
^^^^^^^^^^^^^^^^^

获取用户当前所在的群组ID

语法
''''

.. code:: c#

       public int GetCurrentGroup();

返回值

Type:System.Int32

群组ID

示例
''''

.. code:: html

    <input id="Submit20" type="submit" value='GetCurrentGroup' runat="server"  onclick="GetCurrentGroup()" />

    <script language="javascript" type="text/javascript">
       function GetCurrentGroup() {
            actx.GetCurrentGroup();
        }
    </script>

GetMemberList(Int)
^^^^^^^^^^^^^^^^^^

获取成员列表

语法
''''

.. code:: c#

      public string GetMemberList(int gid)

参数

gid

​ Type:System.Int32

​ 群组ID

返回值

Type: System.String

序列化的 JSON 字符串。

对象：成员实体类Member参考：成员信息

示例
''''

代码示例

.. code:: html

     <input id="Submit20" type="submit" value='GetMemberList' runat="server"  onclick="GetMemberList()" />

    <script language="javascript" type="text/javascript">
         function GetMemberList() {
            actx.GetMemberList(600901);
        }
    </script>

返回值示例：

StartWatchGroup(Int)
^^^^^^^^^^^^^^^^^^^^

监听群组

语法
''''

.. code:: c#

      public int StartWatchGroup(int gid);

参数

gid

​ Type:System.Int32

​ 群组ID

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='StartWatchGroup' runat="server"  onclick="StartWatchGroup()" />

    <script language="javascript" type="text/javascript">
        function StartWatchGroup() {
           actx.StartWatchGroup(600901);
        }
    </script>

​

StopWatchGroup(Int)
^^^^^^^^^^^^^^^^^^^

取消监听群组

语法
''''

.. code:: c#

      public String StopWatchGroup(int gid);

参数

gid

​ Type:System.Int32

​ 群组ID

返回值

Type: System.String

序列化的 JSON 字符串。

对象：群组实体类Group参考：群组信息

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='StopWatchGroup' runat="server"  onclick="StopWatchGroup()" />

    <script language="javascript" type="text/javascript">
         function StopWatchGroup() {
           actx.StopWatchGroup(600901);
        
    </script>

####

.. raw:: html

   <div id="onCurrentGroup">

onCurrentGroupX事件

.. raw:: html

   </div>

当前所在群组变化通知

语法
''''

.. code:: c#

     void onCurrentGroupX(object gid, object group_name);

参数

​ gid

​ Type: System.Object

​ 群组ID

group\_name

​ Type: System.Object

​ 群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onCurrentGroupX(gid,group_name)">
        alert("onCurrentGroupX:gid " + gid + "  group_name:" + group_name);
        //更新成员列表
    </script>

####

.. raw:: html

   <div id="onMemberChanged">

onMemberChangedX事件

.. raw:: html

   </div>

群组成员发生变化通知

-  被动触发条件

​ 其他成员进入所在的组时

​ 其他成员在自身所在的组下线时

​ 切换群组时

-  主动触发

​ 进组

​ 离开群组

​ 创建临时组

语法
''''

.. code:: c#

     void onMemberChangedX(object value);

参数

value

​ Type：System.String

​ 序列化的 JSON 字符串。

​ 对象：群组成员变化实体类 MemberChanged 参考 ： 群组成员变化

示例
''''

.. code:: html


    <script language="javascript" for="myAcX" type="text/javascript" event="onMemberChangedX(val)">
        alert("onMemberChangedX" + val);
      
    </script>

####

.. raw:: html

   <div id="onMemberList">

onMemberListX事件

.. raw:: html

   </div>

成员列表获取完毕

语法
''''

.. code:: c#

     void onMemberListX(object gid);

参数

​ gid

​ Type: System.Object

​ 群组ID

示例
''''

.. code:: html


    <script language="javascript" for="myAcX" type="text/javascript" event="onMemberListX(gid)">
        alert("onMemberListX" + gid );
        //更新成员列表
    </script>

####

.. raw:: html

   <div id="onGroupList">

onGroupListX事件

.. raw:: html

   </div>

群组列表发生变化通知

语法
''''

.. code:: c#

    void onGroupListX();

示例
''''

.. code:: html


    <script language="javascript" for="myAcX" type="text/javascript" event="onGroupListX()">
        alert("onGroupListX");
         // 刷新群组列表
    </script>

对讲
~~~~

StartSpeak方法()
^^^^^^^^^^^^^^^^

抢麦 - 获取话权

语法
''''

.. code:: c#

      public int StartSpeak();

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 onStartSpeakX事件

示例
''''

.. code:: html

    <input id="Submit20" type="submit" value='StartSpeak' runat="server"  onclick="StartSpeak()" />

    <script language="javascript" type="text/javascript">
       function StartSpeak() {
           actx.StartSpeak();
        }
    </script>

StopSpeak方法()
^^^^^^^^^^^^^^^

放麦 - 释放话权

语法
''''

.. code:: c#

      public int StopSpeak()；

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 onStopSpeakX事件

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='StopSpeak' runat="server"  onclick="StopSpeak()" />

    <script language="javascript" type="text/javascript">
       function StopSpeak() {
           actx.StopSpeak();
        }
    </script>

GetPlayingSoundUser方法()
^^^^^^^^^^^^^^^^^^^^^^^^^

获取正在播放声音的人的user信息

语法
''''

.. code:: c#

      public string GetPlayingSoundUser()；

返回值

Type: System.String

序列化的 JSON 字符串。

对象：用户实体类User参考：用户信息

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='GetPlayingSoundUser' runat="server"  onclick="GetPlayingSoundUser()" />

    <script language="javascript" type="text/javascript">
       function GetPlayingSoundUser() {
           actx.GetPlayingSoundUser();
        }
    </script>

返回数据示例：

.. code:: json

    {"gid":600901,"uid":50711,"role":0,"online":1,"has_gid":1,"has_role":1,"audio_enable":1,"name":"账号01"}

GetSpeakingUsers方法()
^^^^^^^^^^^^^^^^^^^^^^

获取正在讲话的人的user信息（不一定是正在播放的，对于多群组而言)

语法
''''

.. code:: c#

      public string GetSpeakingUsers();

返回值

Type: System.String

序列化的 JSON 字符串。

对象：用户实体类User集合参考：用户信息

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='GetSpeakingUsers' runat="server"  onclick="GetSpeakingUsers()" />

    <script language="javascript" type="text/javascript">
        function GetSpeakingUsers() {
           actx.GetSpeakingUsers();
        }
    </script>

返回数据示例

.. code:: json

    [{"gid":600901,"uid":50711,"role":0,"online":1,"has_gid":1,"has_role":1,"audio_enable":1,"name":"账号01"},{"gid":600901,"uid":50712,"role":0,"online":1,"has_gid":1,"has_role":1,"audio_enable":1,"name":"账号02"}]

IsSpeaking方法()
^^^^^^^^^^^^^^^^

自己当前是否正在讲话

语法
''''

.. code:: c#

      public bool IsSpeaking();

返回值

Type:System.Boolean

成功标示 true：在说话 ，false：没有说话

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='IsSpeaking' runat="server"  onclick="IsSpeaking()" />

    <script language="javascript" type="text/javascript">
       function IsSpeaking() {
          var result = actx.IsSpeaking();
         if(result){
           //正在讲话
         }else{
           //没有讲话
         }
        }
    </script>

IsListening()
^^^^^^^^^^^^^

自己当前是否正收听语音

语法
''''

.. code:: c#

     public bool IsListening();

返回值

Type:System.Boolean

成功标示 true：在说话 ，false：没有说话

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='IsListening' runat="server"  onclick="IsListening()" />

    <script language="javascript" type="text/javascript">
        function IsListening() {
          var result = actx.IsListening();
         if(result){
           //正在讲话
         }else{
           //没有讲话
         }
        }
    </script>

####

.. raw:: html

   <div id="startSpeak">

onStartSpeakX事件

.. raw:: html

   </div>

抢麦通知

-  主动触发条件

​ 调用StartSpeak()方法

语法
''''

.. code:: c#

    void onStartSpeakX(object gid, object group_name);

参数

gid

​ Type: System.Object

​ 当前讲话者所在的群组ID

group\_name

​ Type: System.Object

​ 当前讲话者所在的群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onStartSpeakX(gid,group_name)">
        alert("onStartSpeakX  gid:" + gid + " group_name:" + group_name);
    </script>

####

.. raw:: html

   <div id="stopSpeak">

onStopSpeakX事件

.. raw:: html

   </div>

放麦通知

-  主动触发条件

​ 调用StopSpeak()方法

-  被动触发事件

​ 讲话超时被摘麦

语法
''''

.. code:: c#

     void onStopSpeakX(object gid, object group_name);

参数

gid

​ Type: System.Object

​ 当前讲话者所在的群组ID

group\_name

​ Type: System.Object

​ 当前讲话者所在的群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onStopSpeakX(gid,group_name)">
        alert("onStopSpeakX gid:" + gid + " group_name:" + group_name);
    </script>

####

.. raw:: html

   <div id="onStartPlay">

onStartPlayingX事件

.. raw:: html

   </div>

开始播放讲话者事件通知

语法
''''

.. code:: c#

      void onStartPlayingX(object uid, object name, object gid, object group_name);

参数

uid

​ Type: System.Object

​ 讲话者ID

name

​ Type: System.Object

​ 讲话者名称

gid

​ Type: System.Object

​ 群组ID

group\_name

​ Type: System.Object

​ 群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onStartPlayingX(uid,name,gid,group_name)">
        alert("onStartPlayingX  uid:" + uid + " name:" + name + "  gid:" + gid + "  group_name:" + group_name);
        
    </script>

####

.. raw:: html

   <div id="onStopPlay">

onStopPlayingX事件

.. raw:: html

   </div>

停止播放讲话者事件通知

语法
''''

.. code:: c#

     void onStopPlayingX(object uid, object name, object gid, object group_name);

参数

uid

​ Type: System.Object

​ 讲话者ID

name

​ Type: System.Object

​ 讲话者名称

gid

​ Type: System.Object

​ 群组ID

group\_name

​ Type: System.Object

​ 群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onStopPlayingX(uid,name,gid,group_name)">
        alert("onStopPlayingX uid:" + uid + " name:" + name + "  gid:" + gid + "  group_name:" + group_name);
    </script>

####

.. raw:: html

   <div id="onUserStartSpeak">

onUserStartSpeakX事件

.. raw:: html

   </div>

调度员所在群组里，其他成员抢麦成功事件通知

语法
''''

.. code:: c#

    void onUserStartSpeakX(object uid, object name, object gid, object group_name);

参数

uid

​ Type: System.Object

​ 讲话者ID

name

​ Type: System.Object

​ 讲话者名称

gid

​ Type: System.Object

​ 群组ID

group\_name

​ Type: System.Object

​ 群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onUserStartSpeakX(uid,name,gid,group_name)">
        alert("onUserStartSpeakX uid:" + uid + " name:" + name + "  gid:" + gid + "  group_name:" + group_name);
    </script>

####

.. raw:: html

   <div id="onUserStopSpeak">

onUserStopSpeakX事件

.. raw:: html

   </div>

调度员所在群组里，其他成员放麦事件通知

语法
''''

.. code:: c#

    void onUserStopSpeakX(object uid, object name, object gid, object group_name);

参数

uid

​ Type: System.Object

​ 讲话者ID

name

​ Type: System.Object

​ 讲话者名称

gid

​ Type: System.Object

​ 群组ID

group\_name

​ Type: System.Object

​ 群组名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onUserStopSpeakX(uid,name,gid,group_name)">
        alert("onUserStopSpeakX uid:" + uid + " name:" + name + "  gid:" + gid + "  group_name:" + group_name);
    </script>

广播
~~~~

SendTextBroadcast方法(string , Array)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

发送文字广播

语法
''''

.. code:: c#

    public int SendTextBroadcast(string text, Array uids)

参数

text

​ Type:System.String

​ 文字信息（文字长度1-40个字符）

uids

​ Type:System.Array

​ 一维Array，收听广播的用户ID集合

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 onSendedX事件

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='SendTextBroadcast' runat="server"  onclick="SendTextBroadcast()" />

    <script language="javascript" type="text/javascript">
        function SendTextBroadcast() {
            var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            actx.SendTextBroadcast("文字广播测试", uids);
        }
    </script>

StartRecordBroadcastAudio方法()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

开始录制音频文件

语法
''''

.. code:: c#

    public int StartRecordBroadcastAudio()

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='StartRecordBroadcastAudio' runat="server"  onclick="StartRecordBroadcastAudio()" />

    <script language="javascript" type="text/javascript">
        function StartRecordBroadcastAudio() {
            actx.StartRecordBroadcastAudio();
        }
    </script> 

StopRecordBroadcastAudio方法()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

停止录制音频文件

语法
''''

.. code:: c#

    public int StopRecordBroadcastAudio()

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='StopRecordBroadcastAudio' runat="server"  onclick="StopRecordBroadcastAudio()" />

    <script language="javascript" type="text/javascript">
        function StopRecordBroadcastAudio() {
            actx.StopRecordBroadcastAudio();
        }
    </script>

SendAudioBroadcast(Array)
^^^^^^^^^^^^^^^^^^^^^^^^^

发送语音广播

语法
''''

.. code:: c#

     public int SendAudioBroadcast(Array uids)

参数

uids

​ Type:System.Array

​ 一维Array，收听广播的用户ID集合

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 onSendedX事件

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='SendAudioBroadcast' runat="server"  onclick="SendAudioBroadcast()" />

    <script language="javascript" type="text/javascript">
        function SendAudioBroadcast() {
            var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
           actx.SendAudioBroadcast(uids);
        }
    </script>

PlayRecordBroadcastAudio方法()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

播放已录制的待播放音频文件

语法
''''

.. code:: c#

     public int PlayRecordBroadcastAudio()

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

通知事件
''''''''

见 OnPlayStopX事件

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='PlayRecordBroadcastAudio' runat="server"  onclick="PlayRecordBroadcastAudio()" />

    <script language="javascript" type="text/javascript">
        function PlayRecordBroadcastAudio() {
            actx.PlayRecordBroadcastAudio();
        }
    </script>

####

.. raw:: html

   <div id="onSend">

onSendedX事件

.. raw:: html

   </div>

发送广播结果响应事件

-  主动触发条件 执行 SendAudioBroadcast 方法

执行SendTextBroadcast方法

语法
''''

.. code:: c#

    void onSendedX(object success);

参数

success：

​ Type:System.Object

​ ture:发送成功 false:发送失败

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onSendedX(success)">
        alert("onSendedX  " + success);
        if(success) {
          //发送成功
        }else{
          //发送失败
        }
    </script>

####

.. raw:: html

   <div id="OnPlayStop">

OnPlayStopX事件

.. raw:: html

   </div>

录音结束回调

-  主动触发条件

​ 执行 PlayRecordBroadcastAudio 方法

语法
''''

.. code:: c#

    void OnPlayStopX();

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="OnPlayStopX()">
        //调用发送广播方法
       
    </script>

调度
~~~~

TempJoinGroup方法(Int,Array)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

将在线的用户临时加入到指定的群组中

语法
''''

.. code:: c#

    public int TempJoinGroup(int gid, Array uids)

参数

gid

​ Type:System.Int32

​ 群组ID

uids

​ Type:System.Array

​ 一维Array，用户ID集合

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='TempJoinGroup' runat="server"  onclick="TempJoinGroup()" />

    <script language="javascript" type="text/javascript">
        function TempJoinGroup() {
           var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            actx.TempJoinGroup(600901, uids);
        }
    </script>

TempLeaveGroup(Int, Array)
^^^^^^^^^^^^^^^^^^^^^^^^^^

临时移出组

语法
''''

.. code:: c#

    public int TempLeaveGroup(int gid, Array uids)

参数

gid

​ Type:System.Int32

​ 群组ID

uids

​ Type:System.Array

​ 一维Array，用户ID集合

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='TempLeaveGroup' runat="server"  onclick="TempLeaveGroup()" />

    <script language="javascript" type="text/javascript">
       function TempLeaveGroup() {
            var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            actx.TempLeaveGroup(0, uids);
        }
    </script>

Audioenable方法(Array, bool)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

遥开遥闭

语法
''''

.. code:: c#

    public int Audioenable(Array uids, bool audioenabled)

参数

uids

​ Type:System.Array

​ 一维Array，用户ID集合

audioenabled

​ Type:System.Boolean

​ true:遥开 false:遥闭

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

    <input id="Submit20" type="submit" value='遥开' runat="server"  onclick="Audioenable()" />

    <script language="javascript" type="text/javascript">
        function Audioenable() {
            var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            actx.Audioenable(uids,true);
        }
    </script>

ForceDispatch方法(Array)
^^^^^^^^^^^^^^^^^^^^^^^^

强拉，在群组列表中把在线不在本群组内的人员强拉回调度员所在群组

语法
''''

.. code:: c#

    public int ForceDispatch(Array uids)

参数

uids

​ Type:System.Array

​ 一维Array，用户ID集合

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='ForceDispatch' runat="server"  onclick="ForceDispatch()" />

    <script language="javascript" type="text/javascript">
        function ForceDispatch() {
           var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            actx.ForceDispatch(uids);
        }
    </script>

Takemic方法(Array)
^^^^^^^^^^^^^^^^^^

强拆，在群组列表中把正在讲话的用户强拆，强制中断其语音

语法
''''

.. code:: c#

    public int Takemic(Array uids)

参数

uids

​ Type:System.Array

​ 一维Array，用户ID集合

返回值

Type:System.Int32

执行成功标示：0【成功】 -1【失败】

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='Takemic' runat="server"  onclick="Takemic()" />

    <script language="javascript" type="text/javascript">
       function Takemic() {
          var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            alert(actx.Takemic(mycars));
        }
    </script>

GetUser方法(int)
^^^^^^^^^^^^^^^^

获取用户信息

语法
''''

.. code:: c#

    public string GetUser(int uid)

参数

uid

​ Type:System.Int32

​ 用户ID

返回值

Type: System.String

序列化的 JSON 字符串。

对象：用户实体类User参考：用户信息

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='GetUser' runat="server"  onclick="GetUser()" />

    <script language="javascript" type="text/javascript">
       function GetUser() {
            actx.GetUser(50711);
        }
    </script>

返回数据示例

.. code:: json

    {"gid":600901,"uid":50711,"role":0,"online":1,"has_gid":1,"has_role":1,"audio_enable":1,"name":"账号01"}

GetUsers方法(Array)
^^^^^^^^^^^^^^^^^^^

获取用户信息集合

语法
''''

.. code:: c#

    public string GetUser(Array uids)

参数

uids

​ Type:System.Array

​ 一维Array，用户ID集合

返回值

Type: System.String

序列化的 JSON 字符串。

对象：用户实体类User集合参考：用户信息

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='GetUsers' runat="server"  onclick="GetUsers()" />

    <script language="javascript" type="text/javascript">
       function GetUsers() {
            var uids = new Array()
            uids[0] = 50720;
            uids[1] = 50720;
            actx.GetUsers(uids);
        }
    </script>

返回数据示例

.. code:: json

    [{"gid":600901,"uid":50711,"role":0,"online":1,"has_gid":1,"has_role":1,"audio_enable":1,"name":"账号01"}]

####

.. raw:: html

   <div id="onDispatched">

onDispatchedX事件

.. raw:: html

   </div>

强拉结果通知事件

-  主动触发条件

​ 执行ForceDispatch 方法

语法
''''

.. code:: c#

    void onDispatchedX(object usersListjson);

参数

usersListjson

Type: System.Object

序列化的 JSON 字符串。

对象：Dispatched 参考 ： 强拉信息

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onDispatchedX(usersListjson)">
        alert("onDispatchedX" + usersListjson);
    </script>

####

.. raw:: html

   <div id="onTakeMic">

onTakeMicX事件

.. raw:: html

   </div>

强拆结果通知事件

-  主动触发条件

​ 执行 Takemic 方法

语法
''''

.. code:: c#

     void onTakeMicX(object success);

参数

success

​ Type: System.Object

​ true：成功 false：失败

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onTakeMicX(success)">
        alert("onTakeMicX" + success);
    </script>

####

.. raw:: html

   <div id="onAudioEnabled">

onAudioEnabledX事件

.. raw:: html

   </div>

遥开遥闭结果通知事件

-  主动触发条件

​ 执行 Audioenable方法

语法
''''

.. code:: c#

     void onAudioEnabledX(object usersListjson);

参数

usersListjson

Type: System.Object

序列化的 JSON 字符串。

对象： Andioenable 参考 ： 遥开遥闭

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onAudioEnabledX(usersListjson)">
        alert("onAudioEnabledX" + usersListjson);
    </script>

本地录音
~~~~~~~~

EnableLocalRecord方法(bool, string)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

设置本地录音

语法
''''

.. code:: c#

    public int EnableLocalRecord(bool enable, string pathDir)

参数

enable

​ Type:System.Boolean

​ true:开启本地录音 false:关闭本地录音

pathDir

​ Type:System.String

​ 本地录音文件保存的路径

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

     <input id="Submit20" type="submit" value='EnableLocalRecord' runat="server"  onclick="EnableLocalRecord()" />

    <script language="javascript" type="text/javascript">
        function EnableLocalRecord() {
            actx.EnableLocalRecord(true，"d:\");
        }
    </script>

PlayLocalRecord方法(string)
^^^^^^^^^^^^^^^^^^^^^^^^^^^

播放本地录音

语法
''''

.. code:: c#

    public int PlayLocalRecord(string path)

参数

path

​ Type:System.String

​ 文件的绝对路径（播放的文件格式为evrc）

返回值

Type:System.Int32

执行成功标示 0：成功 ，-1：失败

示例
''''

.. code:: html

    <input id="Submit20" type="submit" value='PlayLocalRecord' runat="server"  onclick="PlayLocalRecord()" />

    <script language="javascript" type="text/javascript">
       function PlayLocalRecord() {
            actx.PlayLocalRecord(true，"D:\2018_1_18_13_45_4_996_600901_50711.evrc");
        }
    </script>

onStartRecordX事件
^^^^^^^^^^^^^^^^^^

开始录音的通知事件

语法
''''

.. code:: c#

    void onStartRecordX(object gid, object uid, object starttime, object filename);

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onStartRecordX(gid,uid,starttime,filename)">
        
    </script>

参数

gid

​ Type: System.Object

​ 群组ID

uid

​ Type: System.Object

​ 讲话者ID

starttime

​ Type: System.Object

​ 开始录制时间

filename

​ Type: System.Object

​ 文件名称

onStopRecordX事件
^^^^^^^^^^^^^^^^^

结束录音的通知事件

语法
''''

.. code:: c#

    void onStopRecordX(object success, object gid, object uid, object finishtime, object filename);

参数

success

​ Type: System.Object

​ 执行结果true:成功 false:失败

gid

​ Type: System.Object

​ 群组ID

uid

​ Type: System.Object

​ 讲话者ID

finishtime

​ Type: System.Object

​ 结束录制时间

filename

​ Type: System.Object

​ 文件名称

示例
''''

.. code:: html

    <script language="javascript" for="myAcX" type="text/javascript" event="onStopRecordX(success,gid,uid,finishtime,filename)">
        
    </script>

数据结构定义
------------

###

.. raw:: html

   <div id="Account">

账号信息

.. raw:: html

   </div>

.. code:: c#

     public struct Account
     {
            /// <summary>
            /// 登录名
            /// </summary>
            public string Name

            /// <summary>
            /// 密码
            /// </summary>
            public string Password

            /// <summary>
            /// 用户角色
            /// 0：对讲用户 3：调度员
            /// </summary>
            public int Role

            /// <summary>
            /// 是否自动登录
            /// </summary>
            public int IsAutoLogin

            /// <summary>
            /// 是否记住密码
            /// </summary>
            public int IsRemembered
     }

###

.. raw:: html

   <div id="Group">

群组信息

.. raw:: html

   </div>

.. code:: c#

     public struct Group
       {
        /// <summary>
        /// 群组类型：1为普通组，2为临时组，3为紧急组
        /// </summary>
        public int type;

        /// <summary>
        /// 群组编号
        /// </summary>
        public UInt32 gid;

        /// <summary>
        /// 群组名称
        /// </summary>
        public string name;

        /// <summary>
        /// 群组优先级
        /// </summary>
        public int prior;

        /// <summary>
        /// 服务IP
        /// </summary>
        public UInt32 ip;

        /// <summary>
        /// 服务端口
        /// </summary>
        public UInt16 port;
    }

###

.. raw:: html

   <div id="User">

用户信息

.. raw:: html

   </div>

.. code:: c#

      public struct User
      {
        /// <summary>
        /// 所在群组的编号
        /// </summary>
        public UInt32 gid;

        /// <summary>
        /// 用户编号
        /// </summary>
        public UInt32 uid;

        /// <summary>
        /// 用户角色 0：对讲用户 3：调度员
        /// </summary>

        public UInt32 role;
        /// <summary>
        /// 在线状态
        /// </summary>
        public int online;

        /// <summary>
        /// 是否有群组编号
        /// </summary>
        public int has_gid;

        /// <summary>
        /// 是否有角色
        /// </summary>
        public int has_role;

        /// <summary>
        /// 语音状态是否打开
        /// </summary>
        public int audio_enabled;

        /// <summary>
        /// 成员名称
        /// </summary>
        public string name;
      }

###

.. raw:: html

   <div id="Member">

成员信息

.. raw:: html

   </div>

.. code:: c#

    public struct Member
    {
        /// <summary>
        /// 所在群组的编号
        /// </summary>
        public UInt32 gid;

        /// <summary>
        /// 用户编号
        /// </summary>
        public UInt32 uid;

        /// <summary>
        /// 用户角色
        /// </summary>
        public UInt32 role;

        /// <summary>
        /// 在线状态
        /// </summary>
        public int online;

        /// <summary>
        /// 是否再组
        /// </summary>
        public int ingroup;

        /// <summary>
        /// 是否有群组编号
        /// </summary>
        public int has_gid;

        /// <summary>
        /// 是否有角色
        /// </summary>
        public int has_role;

        /// <summary>
        /// 语音状态是否打开
        /// </summary>
        public int audio_enabled;

        /// <summary>
        /// 成员名称
        /// </summary>
        public string name;
    }

###

.. raw:: html

   <div id="MemberChanged">

群组成员变化

.. raw:: html

   </div>

.. code:: c#

     public class MemberChanged
        {
            /// <summary>
            /// 群组ID
            /// </summary>
            public int Gid;
       
            /// <summary>
            /// 新加入当前群组人员id
            /// </summary>
            public List<int> Us;
       
            /// <summary>
            /// 新加入成员个数
            /// </summary>
            public int Uc;
       
            /// <summary>
            ///退出当前群组人员id
            /// </summary>
            public List<int> Rs;
       
            /// <summary>
            /// 退出成员个数
            /// </summary>
            public int Rc;
       
            /// <summary>
            /// 进入当前群组人员 id
            /// </summary>
            public List<int> Js;
       
            /// <summary>
            /// 进入成员个数
            /// </summary>
            public int Jc;
       
            /// <summary>
            /// 离开当前群组人员i
            /// </summary>
            public List<int> Ls;
       
            /// <summary>
            /// 离开成员个数
            /// </summary>
            public int Lc;
        }

###

.. raw:: html

   <div id="Dispatched">

强拉信息

.. raw:: html

   </div>

.. code:: c#

     public  class Dispatched
        {
            /// <summary>
            /// 执行结果
            /// </summary>
            public bool Success;
       
            /// <summary>
            /// 是否有群组
            /// </summary>
            public bool HasGid;
       
            /// <summary>
            /// 群组ID
            /// </summary>
            public int Gid;
       
            /// <summary>
            /// 用户ID集合
            /// </summary>
            public List<int> Uids;
       
            /// <summary>
            /// 用户个数
            /// </summary>
            public int UidSize;
        }

###

.. raw:: html

   <div id="AudioEnabled">

遥开遥闭

.. raw:: html

   </div>

.. code:: c#

    public class AudioEnabled
        {
            /// <summary>
            /// 执行结果
            /// </summary>
            public bool Success;
       
            /// <summary>
            /// 遥开遥闭状态
            /// </summary>
            public bool AudioEnable;
      
            /// <summary>
            /// 用户ID集合
            /// </summary>
            public List<int> Uids;
      
            /// <summary>
            /// 用户个数
            /// </summary>
            public int Size;
        }

数据定义
--------

###

.. raw:: html

   <div id="LoginError">

错误码

.. raw:: html

   </div>

::

        -1：帐号密码错误
        -2：帐号已欠费或已超出服务期
        -3：帐号不存在
        -4：无效的帐号登录权限
        -10：帐号已在其他位置登录
        -11：帐号登录超时
        -20：网络连接失败
        -21：网络正在重连
        -30：加入群组失败
        -31：加入群组请求超时
        -40:当前有人正在讲话<服务器回馈>
        -41：短时间内不允许多次抢麦
        -42：摇闭状态不允许抢麦
        -43：内部会话状态错误<主动放麦后立马去抢麦>
        -44：抢麦时申请音频焦点失败
        -45：已较低的优先级抢麦<出现在当时正在有人讲话，自身比讲话人优先级低时>
        -46：网络不稳定
        -50：打开录音设备失败

接口调用返回值
~~~~~~~~~~~~~~

::

     -1：操作失败
      0：操作成功

###

.. raw:: html

   <div id="OnlineStatus">

用户在线状态

.. raw:: html

   </div>

.. code:: c#

    public enum OnlineStatus
    {
        /// <summary>
        /// 未知状态
        /// </summary>
        ONLINE_STATUS_UNKNOWN = 0,
        /// <summary>
        /// 离线状态
        /// </summary>
        ONLINE_STATUS_OFFLINE = 1,
        /// <summary>
        /// 登录中
        /// </summary>
        ONLINE_STATUS_LOGINING = 2,
        /// <summary>
        /// 在线状态
        /// </summary>
        ONLINE_STATUS_ONLINE = 3
    }
