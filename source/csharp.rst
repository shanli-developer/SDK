概述
----

此 C# SDK 适用于.net framework>2.0版本，基于 善理公版引擎 C++ API 构建。

安装
~~~~

-  引用\ ``ShanliTech.EChat.SDK.DLL`` 到项目中。

**注意：SDK分为32位和64位，请根据引入的SDK设置为 ``平台目标`` **

.. figure:: D:\微信图片_20180124184017.png
   :alt: 微信图片\_20180124184017

   微信图片\_20180124184017

文件目录
^^^^^^^^

.. code:: ini

    |-eChat-CSharp-SDK_V1.0
     |---libs
     |-----x86
     |-------ShanliTech.EChat.SDK.dll
     |-------amrcc.dll
     |-------echat.dll
     |-------echat.ini
     |-------echat_log.dll
     |-------evrcc.dll
     |-------libscl.dll
     |-------msvcp120d.dll
     |-------msvcr120d.dll
     |-------pthreadVSE2.dll
     |-------vccorlib120d.dll
     |-----x64

依赖库文件
^^^^^^^^^^

-  从文件夹lib拷贝关联的dll到bin目录下。

NAudio.dll、amrcc.dll、echat.dll、echat.ini、echat\_log.dll、evrcc.dll、libscl.dll、msvcp120d.dll、msvcr120d.dll、pthreadVSE2.dll、vccorlib120d.dll

其他
^^^^

C# SDK引用了第三方的开源项目
NAudio,因此，您需要在项目中引用它(可以使用NuGet管理dll)

初始化
~~~~~~

初始化配置
^^^^^^^^^^

-  传参返回值采用UTF-8编码
-  ``Context`` 和 ``Dns`` 配置，修改echat.ini文件：

   .. code:: ini

       [account]
       context = dev
       [network]
       dns = <语音服务IP:端口>

初始化DLL加载
^^^^^^^^^^^^^

.. code:: c#

    // 启动必须先完成初始化的工作，否则开发包无法正常运行
    EChat.EChatApi.Init();

    // 在退出前也需要对库进行释放
    Chat.EChatApi.UnInit();

    // 向外层发送错误通知，包括错误信息及错误码
    EChat.Net.EChatApi.Instance.onError += Instance_onError;
    private static void Instance_onError(string message, int error)
            {
                string result = string.Empty;
                switch (error)
                {
                    case -1:
                        result = "账号密码错误";
                        break;
                    case -2:
                        result = "账号已欠费或超出服务期";
                        break;
                    case -3:
                        result = "账号不存在";
                        break;
                    case -4:
                        result = "无效的账号登录权限";
                        break;
                    case -10:
                        result = "账号已在其它位置登录";
                        break;
                    case -11:
                        result = "账号登录超时";
                        break;
                    case -20:
                        result = "网络连接失败";
                        break;
                    case -21:
                        result = "网络正在重连";
                        break;
                    case -30:
                        result = "加入群组失败";
                        break;
                    case -31:
                        result = "加入群组请求超时";
                        break;
                    case -40:
                        result = "当前有人正在讲话<服务器回馈>";
                        break;
                    case -41:
                        result = "短时间内不允许多次抢麦";
                        break;
                    case -42:
                        result = "摇闭状态不允许抢麦";
                        break;
                    case -43:
                        result = "内部会话状态错误<主动放麦后立马去抢麦>";
                        break;
                    case -44:
                        result = "抢麦时申请音频焦点失败";
                        break;
                    case -45:
                        result = "已较低的优先级抢麦<出现在当时正在有人讲话，自身比讲话人优先级低时>";
                        break;
                       case -46:
                        result = "网络不稳定";
                        break;
                    case -50:
                        result = "打开录音设备失败";
                        break;
                }
            }

接口定义
--------

账号管理
~~~~~~~~

登录
^^^^

-  方法签名

.. code:: c#

      public int Login(string account, string password, int role)

-  参数描述

+------------+-----------------------------------+------------------+
| 参数       | 描述                              | 备注             |
+============+===================================+==================+
| account    | 用户名                            |                  |
+------------+-----------------------------------+------------------+
| password   | 明文密码                          |                  |
+------------+-----------------------------------+------------------+
| role       | 用户角色，0:对讲用户；3：调度员   | 调度台请使用 3   |
+------------+-----------------------------------+------------------+

-  返回值

    执行成功标示 0：成功 ，-1：失败

-  通知事件

-  ``onOnlineStatusChanged`` ，当用户登录成功后触发，\ ``OnlineStatus``
   状态改为 ``ONLINE_STATUS_ONLINE`` 参考 `通知事件 -
   登录状态改变通知 <#loginStatus>`__
-  ``OnError``\ ，当用户登录失败时触发。参考 `配置及加载 -
   初始化 <#init>`__

-  示例

.. code:: c#

    // 创建用户管理对象
    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    accountManager.onOnlineStatusChanged += accountManager_onOnlineStatusChanged;
    accountManager.Login("13800000009", "1", 3);

    /// <summary>
    ///  用户登录成功的通知  
    /// </summary>
    /// <param name="status">
    /// 用户在线枚举 []()
    /// </param>
    private void accountManager_onOnlineStatusChanged(OnlineStatus status){
      if(status == Net.OnlineStatus.ONLINE_STATUS_ONLINE)
      {
          //登录成功
      }
    }

注销
^^^^

-  方法签名

.. code:: c#

       public void Logout();

-  参数

无

-  返回值

无

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    accountManager.Logout();

账号信息查询
^^^^^^^^^^^^

获取用户信息
''''''''''''

-  方法签名

.. code:: c#

     public User GetCurrentUser()

-  参数

无

-  返回值

用户实体类 User 参考 `实体类定义 -> 用户实体类 <#user>`__

-  示例

.. code:: c#

      var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
      var user = accountManager.GetCurrentUser();

​

获取在线状态
''''''''''''

-  方法签名

.. code:: c#

       public OnlineStatus GetOnlineStatus();

-  参数

无

-  返回值

返回在线状态的枚举，参考： `常用枚举 -> 用户在线 <#onlineStatus>`__

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    var status = accountManager.GetOnlineStatus();

获取当前账号UID
'''''''''''''''

-  方法签名

.. code:: c#

      public int GetUid();

-  参数

无

-  返回值

返回当前登录用户的UID

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    var uid = accountManager.GetUid();

获取用户名
''''''''''

-  方法签名

.. code:: c#

      public string GetName();

-  参数

无

-  返回值

返回当前登录用户的中文名

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    var name = accountManager.GetName();

是否被禁言状
''''''''''''

-  方法签名

.. code:: c#

      public bool HasAudioEnabled();

-  参数

无

-  返回值

布尔类型， true：正常发言 ，false：被禁言。

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    var hasAudioEnabled = accountManager.HasAudioEnabled();

更改账号信息
^^^^^^^^^^^^

修改用户名
''''''''''

-  方法签名

.. code:: c#

      public int ChangeName(string name);

-  参数

+--------+--------------+--------+
| 参数   | 描述         | 备注   |
+========+==============+========+
| name   | 新用户名称   |        |
+--------+--------------+--------+

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

``onChangeNameResult`` 修改用户名触发此事件，事件处理是否修改成功。

-  示例

.. code:: c#

      var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
      var ret = accountManager.ChangeName("张三");

      _accountManager.onChangeNameResult += _accountManager_onChangeNameResult;
      private void _accountManager_onChangeNameResult(bool success)
      {
        if(success){
          // 修改成功
        }else{
          // 修改失败
        }
      }

​

修改密码
''''''''

-  方法签名

.. code:: c#

      public int ChangePassword(string oldpassword, string newpassword);

-  参数

+---------------+----------+--------+
| 参数          | 描述     | 备注   |
+===============+==========+========+
| oldpassword   | 旧密码   |        |
+---------------+----------+--------+
| newpassword   | 新密码   |        |
+---------------+----------+--------+

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

``onChangePasswordResult`` , 修改密码触发此事件，事件处理是否修改成功

-  示例

.. code:: c#

      var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
      var ret = accountManager.ChangePassword("123456","qwe");

      _accountManager.onChangePasswordResult += _accountManager_onChangePasswordResult;
      private void _accountManager_onChangePasswordResult(bool success)
      {
        if(success){
          // 修改成功
        }else{
          // 修改失败
        }
      }

本地账号存储
^^^^^^^^^^^^

保存账号
''''''''

-  方法签名

.. code:: c#

       public int SaveAccount(Account account);

-  参数

传入账号实体类， 请参考 `实体类定义 -> 账号实体类 <#account>`__ 定义。

-  返回值

成功标示 0：成功 ，-1：失败

-  示例

.. code:: c#

      var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
      var account = new Account(Config.Account, Config.Password, 0, 0, 0);
      var ret = accountManager.SaveAccount(account);
      if(ret == 0){
        // 成功
      }else if(ret == -1){
        // 失败
      }

​

清空已保存账号
''''''''''''''

-  方法签名

.. code:: c#

      public int ClearAccount();

-  参数

无

-  返回值

成功标示 0：成功 ，-1：失败

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    var ret = accountManager.ClearAccount();
    if(ret == 0){
      // 成功
    }else if(ret == -1){
      // 失败
    }

是否已保存账号
''''''''''''''

-  方法签名

.. code:: c#

      public bool HasAccount();

-  参数

无

-  返回值

成功标示 true：有 ，false：无

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    var ret = accountManager.HasAccount();
    if(ret){
      // 存在
    }else if(ret == -1){
      // 不存在
    }

获取已保存的账号
''''''''''''''''

-  方法签名

.. code:: c#

      public Account GetSavedAccount();

-  参数

无

-  返回值

返回账号实体类，请参考 `实体类定义 -> 账号实体类 <#account>`__ 定义。

-  示例

.. code:: c#

      var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
      var account = accountManager.GetSavedAccount();
      if(account !=null){
        // todo
      }

​

使用已存储的账号登录
''''''''''''''''''''

-  方法签名

.. code:: c#

      public  int  LoginWithSaved();

-  参数

无

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

-  ``onOnlineStatusChanged`` ，当用户登录成功后触发，\ ``OnlineStatus``
   状态改为 ``ONLINE_STATUS_ONLINE`` 参考 `通知事件 -
   登录状态改变通知 <#loginStatus>`__
-  ``OnError``\ ，当用户登录失败时触发。参考 `配置及加载 -
   初始化 <#init>`__

-  示例

.. code:: c#

    var accountManager = EChat.Net.EChatApi.Instance.AccountManager;
    accountManager.LoginWithSaved();

    /// <summary>
    ///  用户登录成功的通知  
    /// </summary>
    /// <param name="status">
    /// 用户在线枚举
    /// </param>
    private void accountManager_onOnlineStatusChanged(OnlineStatus status){
      if(status == Net.OnlineStatus.ONLINE_STATUS_ONLINE)
      {
          //登录成功
      }
    }

通知事件
^^^^^^^^

登录状态改变通知
''''''''''''''''

-  事件定义

.. code:: c#

      // 事件
      public event win_pfn_notify_online_status onOnlineStatusChanged;

​

-  示例

.. code:: c#

      // 注册事件
      _accountManager.onOnlineStatusChanged += _accountManager_onOnlineStatusChanged;

      /// <summary>
      ///  用户登录成功的通知  
      /// </summary>
      /// <param name="status">
      /// 用户在线枚举 []()
      /// </param>
      private void _accountManager_onOnlineStatusChanged(OnlineStatus status){
        if(status == Net.OnlineStatus.ONLINE_STATUS_ONLINE)
        {
            //登录成功
        }
      }

参考： `常用枚举 -> 在线状态 <#onlineStatus>`__

修改用户通知
''''''''''''

-  事件定义

.. code:: c#

      public event win_pfn_notify_change_name onChangeNameResult;

-  示例

.. code:: c#

      accountManager.onChangeNameResult += accountManager_onChangeNameResult;
      private void accountManager_onChangeNameResult(bool success)
      {
          if(success){
            // 成功
          } else {
            // 失败
          }
      }

​

修改密码通知
''''''''''''

-  事件定义

.. code:: c#

      public event win_pfn_notify_change_password onChangePasswordResult;

​

-  示例

.. code:: c#

    accountManager.onChangePasswordResult += _accountManager_onChangePasswordResult;
    private void accountManager_onChangePasswordResult(bool success)
    {
        if(success){
          // 成功
        } else {
          // 失败
        }
    }

当前用户遥开遥闭状态通知
''''''''''''''''''''''''

-  事件定义

.. code:: c#

      public event win_pfn_notify_audio_enable onAudioEnableChanged;

​

-  示例

.. code:: c#

     accountManager.onAudioEnableChanged += _accountManager_onAudioEnableChanged; 
    private void _accountManager_onAudioEnableChanged(bool audioEnable)
    {
      if(audioEnable){
        // 遥开
      }else{
        // 遥闭
      }
    }

注册监听用户状态变化
''''''''''''''''''''

-  被动触发条件 echat 服务内部 User
   缓存变更时触发，例如用户状态改变或用户名改变。

-  示例

.. code:: c#

    var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
    groupManager.onUserChanged += _groupManager_onUserChanged;

    void groupManager_onUserChanged(int count, Net.User[] users)
    {
       Console.WriteLine("有{0}个用户状态更新了，我是否需要？",count)
    }

群组
~~~~

加入群组
^^^^^^^^

-  方法签名

.. code:: c#

      public int JoinGroup(int gid);

-  参数

+--------+----------+--------+
| 参数   | 描述     | 备注   |
+========+==========+========+
| gid    | 群组Id   |        |
+--------+----------+--------+

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

见 `通知事件 <#groupEvent>`__

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      int result = groupManager.JoinGroup(gid);
      if( result == 0){
        // 执行成功，当前群组切换通知
      }

​

离开群组
^^^^^^^^

-  方法签名

.. code:: c#

      public int LeaveGroup();

-  参数

无

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

见 `通知事件 <#groupEvent>`__

-  示例

.. code:: c#

    var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
    int result = groupManager.LeaveGroup();
    if( result == 0){
      // 执行成功，当前群组切换通知
    }

创建临时群组
^^^^^^^^^^^^

-  方法签名

.. code:: c#

      public int Call(long[] uids)；

-  参数

+--------+----------------+--------+
| 参数   | 描述           | 备注   |
+========+================+========+
| uids   | 用户uid 数组   |        |
+--------+----------------+--------+

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

见 `通知事件 <#groupEvent>`__

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      int[] uids = new[] { 1,2 };
      groupManager.call(uids);

销毁临时群组
^^^^^^^^^^^^

临时群组创建者退出临时群组即为销毁临时群组。

群组信息查询
^^^^^^^^^^^^

获取群组列表
''''''''''''

-  方法签名

.. code:: c#

      public Group[] GetGroupList();

-  参数

无

-  返回值

`Group 实体数组 <#group>`__

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      var groups = groupManager.GetGroupList();

​

根据GID 查询群组信息
''''''''''''''''''''

-  方法签名

.. code:: c#

      public Group GetGroupByGid(int gid);

-  参数

+--------+----------+--------+
| 参数   | 描述     | 备注   |
+========+==========+========+
| gid    | 群组Id   |        |
+--------+----------+--------+

-  返回值

群组实体

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      var group = groupManager.GetGroupByGid(1);

获取当前群组所在Id
''''''''''''''''''

-  方法签名

.. code:: c#

       public int GetCurrentGroup();

-  参数

无

-  返回值

当前群组 Id

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      var gid = groupManager.GetCurrentGroup();

​

获取成员列表
''''''''''''

-  方法签名

.. code:: c#

      public Member[] GetMemberList(int gid)

-  参数

+--------+----------+--------+
| 参数   | 描述     | 备注   |
+========+==========+========+
| gid    | 群组Id   |        |
+--------+----------+--------+

-  返回值

见 `成员实体 <#member>`__

群组设置
^^^^^^^^

监听群组
''''''''

-  方法签名

.. code:: c#

      public int StartWatchGroup(int gid);

-  参数

+--------+----------+--------+
| 参数   | 描述     | 备注   |
+========+==========+========+
| gid    | 群组Id   |        |
+--------+----------+--------+

-  返回值

成功标示 0：成功 ，-1：失败

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      var return  = groupManager.StopWatchGroup(1);
      if(return == 0){
          // TODO 成功监听
      }

​

​

取消监听群组
''''''''''''

-  方法签名

.. code:: c#

      public Group StopWatchGroup(int gid);

-  参数

+--------+----------+--------+
| 参数   | 描述     | 备注   |
+========+==========+========+
| gid    | 群组Id   |        |
+--------+----------+--------+

-  返回值

见 `群组实体 <#group>`__

-  示例

.. code:: c#

      var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
      var group = groupManager.StopWatchGroup(1);
      if(group !=null){
          Console.WriteLine(" gid ：{0}",group.gid);  
      }

​

####

.. raw:: html

   <div id="groupEvent">

.. raw:: html

   </div>

通知事件

群组变化事件
''''''''''''

**被动触发**

-  其他成员进入所在的组时
-  其他成员在自身所在的组下线时
-  切换群组时

**主动触发**

-  进组

-  创建临时组

.. code:: c#

    //当前所在群组变化通知
    //public event win_pfn_notify_current_group onCurrentGroup;

    //群组列表发生变化通知
    //public event win_pfn_notify_grouplist onGroupList;

    //群组成员发生变化通知
    //public event win_pfn_notify_members_changed onMemberChanged;

    //成员列表获取完毕
    //public event win_pfn_notify_memberlist onMemberList;

    var groupManager = EChat.Net.EChatApi.Instance.GroupManager;
    groupManager.onGroupList += groupManager_onGroupList;
    groupManager.onCurrentGroup += groupManager_onCurrentGroup;
    groupManager.onMemberChanged += groupManager_onMemberChanged;
    groupManager.onMemberList += groupManager_onMemberList;

    void groupManager_onCurrentGroup(int gid, string group_name)
    {
        // 更新当前成员列表
    }

    void groupManager_onGroupList()
    {
        // 刷新群组列表
    }
    void groupManager_onMemberChanged(int gid, int[] us, int uc, int[] rs, int rc, int[] js, int jc, int[] ls, int lc)
    {
      //增量更新成员列表
      // us 新加入当前群组人员id， 调度台强拆动作
      // rs 退出当前群组人员id， 调度台强拉动作 
      // js 进入当前群组人员 id
      // ls 离开当前群组人员id
    }

    void groupManager_onMemberList(int gid)
    {
     //更新成员列表
    }

对讲
~~~~

抢麦 - 获取话权
^^^^^^^^^^^^^^^

-  方法签名

.. code:: c#

      public int StartSpeak();

-  参数

无

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

见 `抢麦通知 <#startSpeak>`__

-  示例

.. code:: c#

      var talkManager = EChatApi.GetInstance().TalkManager;
      talkManager.onStartSpeak += _talkManager_onStartSpeak;
      talkManger.StartSpeak();

      private void _talkManager_onStartSpeak(int gid, string group_name)
      {
          //TODO 抢到麦权  
      }

​

放麦 - 释放话权
^^^^^^^^^^^^^^^

-  方法签名

.. code:: c#

      public int StopSpeak()；

-  参数

无

-  返回值

执行成功标示 0：成功 ，-1：失败

-  通知事件

`放麦通知 <#stopSpaek>`__

-  示例

.. code:: c#

      var talkManager = EChatApi.GetInstance().TalkManager;
      talkManager.onStopSpeak += _talkManager_onStopSpeak;
      talkManger.StopSpeak();

      private void _talkManager_onStopSpeak(int gid, string group_name)
      {
          //TODO 释放麦权  
      }

对讲信息查询
^^^^^^^^^^^^

查询当前播放语音的用户
''''''''''''''''''''''

-  方法签名

.. code:: c#

      public User GetPlayingSoundUser()；

-  参数

无

-  返回值

见 `用户实体 <#user>`__

​

查询当前所有再讲话的用户
^^^^^^^^^^^^^^^^^^^^^^^^

-  方法签名

.. code:: c#

      public User[] GetSpeakingUsers();

-  参数

无

-  返回值

见 `用户实体数组 <#user>`__

-  示例

自己当前是否正在讲话
^^^^^^^^^^^^^^^^^^^^

-  方法签名

.. code:: c#

      public bool IsSpeaking();

-  参数

无

-  返回值

成功标示 true：在说话 ，false：没有说话

​

自己当前是否正收听语音
^^^^^^^^^^^^^^^^^^^^^^

-  方法签名

.. code:: c#

      public bool IsListening();

-  参数

无

-  返回值

成功标示 true：在说话 ，false：没有说话

通知事件
^^^^^^^^

#####

.. raw:: html

   <div id="startSpeak">

.. raw:: html

   </div>

抢麦通知

-  事件定义

.. code:: c#

       public event win_pfn_notify_start_speak onStartSpeak;

​

#####

.. raw:: html

   <div id="stopSpeak">

.. raw:: html

   </div>

放麦通知

-  事件定义

.. code:: c#

      public event win_pfn_notify_start_speak onStartSpeak;

​

-  示例

有人讲话通知
''''''''''''

-  事件定义

.. code:: c#

      public event win_pfn_notify_start_playing_sound onStartPlaying;

-  示例

.. code:: c#

      var talkManager = EChatApi.GetInstance().TalkManager;
      talkManager.onStartPlaying += talkManager_onStartPlaying;

      private void _talkManager_onStartPlaying(int uid, string name, int gid, string group_name)
      {
          // TODO 其他人讲话          
      }

​

讲话停止通知
''''''''''''

-  事件定义

.. code:: c#

       public event win_pfn_notify_user_stop_speak onUserStopSpeak;

-  示例

.. code:: c#

    var talkManager = EChatApi.GetInstance().TalkManager;
    talkManager.onStopPlaying += talkManager_onStopPlaying;

    private void talkManager_onStopPlaying(int uid, string name, int gid, string group_name)
    {
       // TODO 其他人停止讲话      
    }

广播
~~~~

文字广播
^^^^^^^^

发送文字广播
''''''''''''

-  方法签名

.. code:: c#

    public int SendTextBroadcast(string text, int[] uids)

-  参数描述

+------------+------------------------+-------------------------------+
| 参数定义   | 描述                   | 约束                          |
+============+========================+===============================+
| text       | 文字信息               | 文字长度1-40；编码格式uft-8   |
+------------+------------------------+-------------------------------+
| uids       | 收听广播的用户ID集合   | - -                           |
+------------+------------------------+-------------------------------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
    var uids = new[] { 50711 };
    _broadcastManager.SendTextBroadcast("测试文字广播",  uids)

语音广播
^^^^^^^^

开始录制音频文件
''''''''''''''''

-  方法签名

.. code:: c#

    public int StartRecordBroadcastAudio()

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

     var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
    _broadcastManager.StartRecordBroadcastAudio()

停止录制音频文件
''''''''''''''''

-  方法签名

.. code:: c#

    public int StopRecordBroadcastAudio()

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

     var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
    _broadcastManager.StopRecordBroadcastAudio()

发送语音广播
''''''''''''

-  方法签名

.. code:: c#

     public int SendAudioBroadcast(int[] uids)

-  参数描述

+------------+------------------------+--------+
| 参数定义   | 描述                   | 约束   |
+============+========================+========+
| uids       | 收听广播的用户ID集合   | - -    |
+------------+------------------------+--------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

     var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
     var uids = new[] { 50711 };
    _broadcastManager.SendAudioBroadcast(uids)；

播放已录制的待播放音频文件
''''''''''''''''''''''''''

-  方法签名

.. code:: c#

     public int PlayRecordBroadcastAudio()

-  返回值 执行标示：0【成功】 -1【失败】
-  示例

.. code:: c#

    var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
    _broadcastManager.PlayRecordBroadcastAudio()

事件定义
^^^^^^^^

录音结束回调
''''''''''''

-  主动触发条件 执行 PlayRecordBroadcastAudio 方法

-  事件定义

.. code:: c#

    public event win_pfn_broadcast_play_async_stop OnPlayStop；

-  示例

.. code:: c#

    var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
    _broadcastManager.OnPlayStop += _broadcastManager_OnPlayStop;
    private void _broadcastManager_OnPlayStop()
    {
        
    }

发送广播结果回调
''''''''''''''''

-  主动触发条件 执行 SendAudioBroadcast 方法

执行SendTextBroadcast方法

-  事件定义

.. code:: c#

    public event win_pfn_notify_send_callback onSended;

-  示例

.. code:: c#

    var _broadcastManager = EChat.Net.EChatApi.Instance.BroadcastManager;
    _broadcastManager.onSended += _broadcastManager_onSended;

    private void _broadcastManager_onSended(bool success)
    {
       //发送成功
    }

调度
~~~~

临时加入组
^^^^^^^^^^

-  方法签名

.. code:: c#

    public int TempJoinGroup(int gid, int[] uids)

-  参数描述

+------------+--------------------+--------+
| 参数定义   | 描述               | 约束   |
+============+====================+========+
| gid        | 临时加入群组编号   | --     |
+------------+--------------------+--------+
| uids       | 用户ID集合         | --     |
+------------+--------------------+--------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    var uids = new[] { 50711 };
    _dispatchManager.TempJoinGroup(600612, uids);

临时移出组
^^^^^^^^^^

-  方法签名

.. code:: c#

    public int TempLeaveGroup(int gid, int[] uids)

-  参数描述

+------------+--------------+-----------+
| 参数定义   | 描述         | 约束      |
+============+==============+===========+
| gid        | 群组Id       | 赋值为0   |
+------------+--------------+-----------+
| uids       | 用户ID集合   | --        |
+------------+--------------+-----------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    var uids = new[] { 50711 };
    _dispatchManager.TempLeaveGroup(0, uids);

​

遥开遥闭
^^^^^^^^

-  方法签名

.. code:: c#

    public int Audioenable(int[] uids, bool audioenabled)

-  参数描述

+----------------+------------------------+--------+
| 参数定义       | 描述                   | 约束   |
+================+========================+========+
| uids           | 用户ID集合             | --     |
+----------------+------------------------+--------+
| audioenabled   | true:开启 false:关闭   | --     |
+----------------+------------------------+--------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    var uids = new[] { 50711 };
    _dispatchManager.Audioenable(uids, true)

强拉
^^^^

-  方法签名

在群组列表中把在线不在本群组内的人员强拉回调度员所在群组

.. code:: c#

    public int ForceDispatch(int[] uids)

-  参数描述

+------------+--------------+--------+
| 参数定义   | 描述         | 约束   |
+============+==============+========+
| uids       | 用户ID集合   | --     |
+------------+--------------+--------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    var uids = new[] { 50711 };
    _dispatchManager.ForceDispatch(uids)；

​

强拆
^^^^

-  方法签名

在群组列表中把正在讲话的用户强拆，强制中断其语音

.. code:: c#

    public int Takemic(int[] uids)

-  参数描述

+------------+--------------+--------+
| 参数定义   | 描述         | 约束   |
+============+==============+========+
| uids       | 用户ID集合   | --     |
+------------+--------------+--------+

-  返回值 执行成功标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    var uids = new[] { 50711 }; 
    _dispatchManager.Takemic(uids)

​

事件定义
^^^^^^^^

强拉结果通知事件
''''''''''''''''

-  主动触发条件 执行ForceDispatch 方法

-  事件定义

.. code:: c#

    public event win_pfn_notify_dispatcher onDispatched;

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    _dispatchManager.onDispatched += _dispatchManager_onDispatched;

    private void _dispatchManager_onDispatched(bool success, bool has_gid, int gid, int[] uids, int uid_size)
    {
      //success:是否成功
      //has_gid:是否有群组变化
      //gid:群组编号
      //uids:用户Id集合
      //uid_size:用户Id的数量
    }

强拆结果通知事件
''''''''''''''''

-  主动触发条件 执行 Takemic 方法

-  事件定义

.. code:: c#

    public event win_pfn_notify_takemic onTakeMic;

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    _dispatchManager.onTakeMic += _dispatchManager_onTakeMic;

    public delegate void win_pfn_notify_takemic(bool success, int[] uids, int uids_size)
    {
      // success  是否成功
      // uids     用户Id集合
      //user_size 用户Id数量
    }

遥开遥闭结果通知事件
''''''''''''''''''''

-  主动触发条件 执行 Audioenable方法
-  事件定义

.. code:: c#

    public event win_pfn_audio_enabled_callback onAudioEnabled;

-  示例

.. code:: c#

    var _dispatchManager = EChat.Net.EChatApi.Instance.DispatchManager;
    _dispatchManager.onAudioEnabled += _dispatchManager_onAudioEnabled;
    public delegate void win_pfn_audio_enabled_callback(bool success, bool audio_enabled, int[] uids, int size)
    {
      //success       是否成功
      //audio_enabled 遥开遥闭状态
      //uids          用户Id集合
      //size          用户Id数量
    }

本地录音
~~~~~~~~

设置本地录音
^^^^^^^^^^^^

-  方法签名

.. code:: c#

    public int EnableLocalRecord(bool enable, string pathDir)

-  参数描述

+------------+---------------------------+--------+
| 参数定义   | 描述                      | 约束   |
+============+===========================+========+
| enable     | true:开启； false：关闭   | --     |
+------------+---------------------------+--------+
| pathDir    | 本地录音文件保存的路径    | --     |
+------------+---------------------------+--------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _recordManager = EChat.Net.EChatApi.Instance.RecordManager;
    _recordManager.EnableLocalRecord(true,"d:\");

播放本地录音
^^^^^^^^^^^^

-  方法签名

.. code:: c#

    public int PlayLocalRecord(string path)

-  参数描述

+------------+------------------+------------------------+
| 参数定义   | 描述             | 约束                   |
+============+==================+========================+
| path       | 文件的绝对路径   | 播放的文件格式为evrc   |
+------------+------------------+------------------------+

-  返回值 执行标示：0【成功】 -1【失败】

-  示例

.. code:: c#

    var _recordManager = EChat.Net.EChatApi.Instance.RecordManager;
    _recordManager.PlayLocalRecord("D:\2018_1_18_13_45_4_996_600901_50711.evrc")

​

事件定义
^^^^^^^^

开始录音的通知事件
''''''''''''''''''

-  事件定义

.. code:: c#

    public event win_pfn_notify_start_recordfile onStartRecord;

-  示例

.. code:: c#

    var _recordManager = EChat.Net.EChatApi.Instance.RecordManager;
    _recordManager.onStartRecord += _recordManager_onStartRecord;
    public delegate void win_pfn_notify_start_recordfile(int gid, int uid, string starttime, string filename)
    {
      //gid       群组Id
      //uid       用户Id
      //starttime 开始时间（绝对时间）
      //filename  文件名称
      
      //存储当前讲话者的基本信息
      Dictionary<string, object> ops = new Dictionary<string, object>();
      ops.Add("speakername", dispatchManager.GetUser(uid).name);
      ops.Add("groupname", groupManager.GetGroupByGid(gid).name);
      ops.Add("gid", gid);
      ops.Add("uid", uid);
      ops.Add("starttime", starttime);
      ops.Add("filename", filename);
      MimiCache.LOCAL_RECORDS.Add(filename, ops);
    }

结束录音的通知事件
''''''''''''''''''

-  事件定义

.. code:: c#

    public event win_pfn_notify_end_recordfile onStopRecord;

-  示例

.. code:: c#

    var _recordManager = EChat.Net.EChatApi.Instance.RecordManager;
    _recordManager.onStopRecord += _recordManager_onStopRecord;
    private void _recordManager_onStopRecord(bool success, int gid, int uid, string finishtime, string filename)
    {
      //success 是否成功
      //git     群组ID
      //uid     用户ID
      //finishtime 结束时间（绝对时间）
      //文件名称
      
      //将录音成功的基本信息存储到队列中
    }

数据结构定义
------------

账号信息
~~~~~~~~

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

群组信息
~~~~~~~~

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

用户信息
~~~~~~~~

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

成员信息
~~~~~~~~

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

数据定义
--------

登录错误码
~~~~~~~~~~

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

用户在线状态
~~~~~~~~~~~~

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
