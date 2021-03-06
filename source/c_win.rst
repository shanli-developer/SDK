1 概述
------

    本文档主要介绍Win-SDK
    接口函数（API），主要针对FAE，测试人员和客户使用 。

2 功能性接口
------------

2.1 初始化接口
^^^^^^^^^^^^^^

-  程序启动

   .. code:: c

       接口：int echat_win_on_load(void *arg)
       参数：arg 配置文件路径
       返回：成功 0；失败 -1
       注意：程序启动时调用此接口，如果调用了echat_win_on_unload， 想再次启动程序，必须调用echat_win_on_load

-  程序退出

   .. code:: c

       接口：void echat_win_on_unload()

2 .2 登录配置接口
^^^^^^^^^^^^^^^^^

-  登录

   .. code:: c

       接口：int echat_win_login(const char* account, const char* password, int role)
       参数：account  账号
         password 密码
         role  用户角色 普通用户 0 ； 调度员 2
       返回：成功 0；失败 -1

-  使用配置文件中的账号信息登录

   .. code:: c

       接口：int echat_win_loginWithSaved()
       返回：成功 0；失败 -1

-  注销

   .. code:: c

       接口：int echat_win_logout()
       返回：0

-  配置文件是否存有账号配置

   .. code:: c

       接口：boolean echat_win_hasAccount()
       返回：有 True；无 False

-  从配置文件获取账号配置信息

   .. code:: c

       接口：int echat_win_getSavedAccount(char*account,const char* password,int*role)
       参数：account  用于保存账号（必须分配内存）
         password  用于保存密码（必须分配内存）
         role  用于保存角色（必须分配内存）
       返回：成功 0；失败 -1

-  保存账号配置

   .. code:: c

       接口：int echat_win_saveAccount(const char* account, const char* password, int role)
       参数：account  账号
         password 密码
         role  用户角色 普通用户 0 ； 调度员 2
       返回：成功 0；失败 -1

-  清空账号配置

   .. code:: c

       接口：int echat_win_clearAccount()
       返回：0

-  获取登录状态

   .. code:: c

       接口：ECHAT_ONLINE_STATUS echat_win_getOnlineStatus()
       返回：未知 0；不在线 1；登录中  2；在线  3

2.3 对讲接口
^^^^^^^^^^^^

-  开始讲话

   .. code:: c

       接口：int echat_win_startSpeak()
       返回：成功 0；失败 -1

-  停止讲话

   .. code:: c

       接口：int echat_win_stopSpeak()
       返回：成功 0；失败 -1

-  获取当前用户的讲话状态

   .. code:: c

       接口：boolean echat_win_isSpeaking()
       返回：正在讲话 True；其它 False

-  是否有人在讲话

   .. code:: c

       接口：boolean echat_win_isListening()
       返回：有人讲话 True；其它 False

-  获取正在播放声音的人的user信息

   .. code:: c

       接口：int echat_win_getPlayingSoundUser(win_user_t* user)
       参数：user  用于存放用户信息（必须分配内存）
       返回：成功 0；失败 -1

-  获取正在讲话的人的个数

   .. code:: c

       接口：size_t echat_win_getSpeakingUsersSize()
       返回：正在讲话人的个数
       注意：监听时使用

-  获取正在讲话的人的user信息

   .. code:: c

       接口：int echat_win_getSpeakingUsers(win_user_t* user, size_t size)
       参数：user  用于存放用户信息（必须分配内存）
         size  用户个数
       返回：成功  0；失败 -1；参数错误 -3
       注意：监听时使用

2.4 群组操作接口
^^^^^^^^^^^^^^^^

-  加入群组

   .. code:: c

       接口：int echat_win_joinGroup( gid_t gid)
       参数：gid  群组ID
       返回：成功 0；失败 -1

-  离开当前群组

   .. code:: c

       接口：int echat_win_leaveGroup()
       返回：成功 0；失败 -1

-  获取当前群组ID

   .. code:: c

       接口：gid_t echat_win_getCurrentGroup()
       返回：成功 群组ID；失败 0

-  获取群组的个数

   .. code:: c

       接口：size_t echat_win_getGroupSize()
       返回：群组个数

-  获取群组列表

   .. code:: c

       接口：int  echat_win_getGroupList(win_group_t* group,size_t size)
       参数：group  用于存放群组数据（必须分配内存）
         size  群组的个数
       返回：成功  获取群组的个数；错误  -1；内存不足  -2；参数错误  -3

-  通过群组ID获取群组信息

   .. code:: c

       接口：int echat_win_getGroupByGid(win_group_t* group, gid_t gid)
       参数：group  用于存放群组信息（必须分配内存）
         gid  群组ID
       返回：成功 0；失败 -1

-  建立临时群组

   .. code:: c

       接口：int echat_win_Call(const uid_t* uids,size_t size)
       参数：uids  用户ID列表
         size  用户ID个数
       返回：成功 0；失败 -1

2.5 用户操作接口
^^^^^^^^^^^^^^^^

-  修改用户名

   .. code:: c

       接口：int echat_win_changeName(const char* name)
       参数：name  新用户名
       返回：成功 0；失败 -1

-  修改密码

   .. code:: c

       接口：int echat_win_changePassword(const char* oldpassword,const char* newpassword)
       参数：oldpassword  旧密码
         newpassword  新密码
       返回：成功 0；失败 -1

-  获取当前用户ID

   .. code:: c

       接口：uid_t echat_win_getUid()
       返回：成功 用户ID；失败 0

-  获取当前用户名

   .. code:: c

       接口：int echat_win_getName(char* name)
       参数：name  用于存放用户名（必须分配内存）
       返回：0

-  查询用户是否被遥闭

   .. code:: c

       接口：boolean echat_win_hasAudioEnabled()
       返回：遥开 True；遥闭 False

-  通过群组ID获取群组成员个数

   .. code:: c

       接口：size_t echat_win_getMemberSize(gid_t gid)
       参数：gid  群组ID
       返回：成员个数； 参数错误 -2

-  获取群组成员列表

   .. code:: c

       接口：int echat_win_getMemberList(win_member_t* member, size_t size, gid_t gid)
       参数：member  用于存放成员数据（必须分配内存）
         size  成员的个数
         gid  群组ID
       返回：成功  获取成员的个数；错误  -1；内存不足  -2；参数错误  -3

-  通过用户ID获取用户信息

   .. code:: c

       接口：int echat_win_getUser(win_user_t* user,uid_t uid)
       参数：user  用于存放用户信息（必须分配内存）
         uid  用户ID
       返回：成功 获取的用户信息个数；失败 -1；参数错误  -2

-  通过用户ID列表获取多个用户信息

   .. code:: c

       接口：int echat_win_getUsers(win_user_t* user, uid_t* uids, size_t size)
       参数：user  用于存放用户信息（必须分配内存）
         uids  用户ID列表
         size  用户个数
       返回：成功 获取的用户信息个数；失败 -1；参数错误  -2

-  获取所有用户个数

   .. code:: c

       接口：int echat_win_getAllUserSize()
       返回：用户个数

-  获取所有用户ID

   .. code:: c

       接口：int echat_win_getAllUsersId(uid_t* uids, size_t size)
       参数：uids  用于存放用户ID列表（必须分配内存）
         size  用户个数
       返回：成功  获取的用户信息个数；失败 -1；参数错误 -2

-  获取当前用户信息

   .. code:: c

       接口：int echat_win_getCurrentUser(win_user_t* user)
       参数：user  用于存放用户列表信息（必须分配内存）
       返回：成功 0；失败 -1

2.6 监听接口
^^^^^^^^^^^^

-  开始监听某个群组

   .. code:: c

       接口：int echat_win_startWatchGroup(gid_t gid)
       参数：gid  群组ID
       返回：成功 0；失败 -1

-  停止监听某个群组

   .. code:: c

       接口：int echat_win_stopWatchGroup(gid_t gid)
       参数：gid  群组ID
       返回：成功 0；失败 -1

-  获取已监听群组信息

   .. code:: c

       接口：int echat_win_getWatchGroups(win_group_t*watch_groups,size_t size)
       参数：watch_groups  存放监听群组的信息（必须分配内存）
         size  群组个数
       返回：成功  0；失败 -1；参数错误 -3

2.7 广播操作接口
^^^^^^^^^^^^^^^^

-  发送文字广播

   .. code:: c

       接口：int echat_win_broadcast_send_text(char* text,uid_t* uids,size_t size)
       参数：text  文字字符串
         uids  发给用户的ID列表
         size  用户个数
       返回：成功 0；失败 -1

-  录制待广播的语音数据

   .. code:: c

       接口：int echat_win_broadcast_start_record()
       返回：成功 0；失败 -1

-  停止录制待广播的语音数据

   .. code:: c

       接口：int echat_win_broadcast_stop_record()
       返回：成功 0；失败 -1

-  播放已录制的待播放语音数据

   .. code:: c

       接口：int echat_win_broadcast_play_recordaudio(win_pfn_broadcast_play_async_stop pfn)
       参数：pfn  发送已录制的语音广播数据
       返回：成功 0；失败 -1

-  发送已录制的语音广播数据

   .. code:: c

       接口：int echat_win_broadcast_send_recordaudio(uid_t* uids,size_t size)
       参数：uids  发给用户的ID列表
         size  用户个数
       返回：成功 0；失败 -1

2.8 调度操作接口
^^^^^^^^^^^^^^^^

-  临时加入组

   .. code:: c

       接口：int echat_win_schedule_temp_join_group(gid_t gid, uid_t* uids,size_t size)
       参数：gid  要加入的群组ID
         uids  用户ID列表
         size  用户个数
       返回：成功 0；失败 -1

-  临时移除组

   .. code:: c

       接口：int echat_win_schedule_temp_leave_group(gid_t gid, uid_t* uids,size_t size)
       参数：gid  要加入群组ID（为0表示移出组）
         uids  用户ID列表
         size  用户个数
       返回：成功 0；失败 -1

-  遥开摇闭

   .. code:: c

       接口：int echat_win_schedule_audioenable(uid_t* uids,size_t size, boolean audioenabled)
       参数：uids  用户ID列表
         size  用户个数
         audioenabled  遥开 True  摇闭  False
       返回：成功 0；失败 -1

-  强拉,拉到调度员所在的群组

   .. code:: c

       接口：int echat_win_schedule_force_dispatch(uid_t* uids,size_t size)
       参数：uids  用户ID列表
         size  用户个数
       返回：成功 0；失败 -1

-  强拆

   .. code:: c

       接口：int echat_win_schedule_takemic(uid_t* uids,size_t size)
       参数：uids  用户ID列表
         size  用户个数
       返回：成功 0；失败 -1

-  开启/关闭定位

   .. code:: c

       接口：int echat_win_schedule_switch_location(uid_t* uids, size_t size, boolean enable, uint32_t period)
       参数：auids  用户ID列表
         size  用户个数
         enable  打开 True  关闭  False
         period  上报速度 （30~300s）
       返回：成功 0；失败 -1

2.9 本地录音操作接口
^^^^^^^^^^^^^^^^^^^^

-  本地录音开关

   .. code:: c

       接口：int echat_win_record_set_enabled(boolean record_enabled,char* pathDir)
       参数：record_enabled  打开 True   关闭 False
         pathDir  保存路径，例如：“D:/record_file”
       返回：成功 0；失败 -1
       注意：录音时自动保存在该路径，生成文件名是：时间+GID+UID.evrc

-  本地录音播放

   .. code:: c

       接口：int echat_win_record_play_audio(char* path)
       参数：path  路径及文件名，例如：“D:/record_file/时间+GID+UID.evrc”
       返回：成功 0；失败 -1
       注意：播放已录音的文件，需要路径及文件名

3 消息通知回调接口
------------------

-  设置在线状态回调

   .. code:: c

       接口：int echat_win_set_online_status_cb(win_pfn_notify_online_status cb)
       参数：cb 在线状态变化时通知函数
       返回：成功 0；失败 -1
       注意：当前用户上线或下线时触发通知在线状态

-  设置消息通知回调

   .. code:: c

       接口：int echat_win_set_message_cb(win_pfn_notify_message cb)
       参数：cb 对外通知消息函数
       返回：成功 0；失败 -1
       注意：暂不可用

-  设置错误消息回调

   .. code:: c

       接口：int echat_win_set_error_cb(win_pfn_notify_error cb)
       参数：cb错误消息通知函数
       返回：成功 0；失败 -1
       注意：有操作错误或异常错误触发通知错误信息（见 3 错误信息标识）

-  设置遥开摇闭通知回调

   .. code:: c

       接口：int echat_win_set_audio_enabled_cb(win_pfn_notify_audio_enable cb)
       参数：cb被遥开、摇闭时回调函数
       返回：成功 0；失败 -1
       注意：当被遥开或摇闭时触发，通知当前状态

-  设置修改用户名回调

   .. code:: c

       接口：int echat_win_set_change_name_cb(win_pfn_notify_change_name cb)
       参数：cb  用户名修改通知函数
       返回：成功 0；失败 -1
       注意：主动修改用户名时触发，通知结果

-  设置修改密码回调

   .. code:: c

       接口：int echat_win_set_change_password_cb(win_pfn_notify_change_password cb)
       参数：cb密码修改通知函数
       返回：成功 0；失败 -1
       注意：主动修改用户密码时触发，通知结果

-  设置用户列表回调

   .. code:: c

       接口：int echat_win_set_memberlist_cb(win_pfn_notify_memberlist cb)
       参数：cb 通知用户列表函数
       返回：成功 0；失败 -1
       注意：请求用户信息时，用户信息从服务器返回后触发，通知用户信息已更新

-  设置成员改变回调

   .. code:: c

       接口：int echat_win_set_member_changed_cb(win_pfn_notify_members_changed cb)
       参数：cb 成员改变时通知函数
       返回：成功 0；失败 -1
       注意：当有成员信息改变时，通知改变结果（进组、离组等）

-  设置用户角色改变回调

   .. code:: c

       接口：int echat_win_set_roles_changed_cb(win_pfn_notify_members_changed cb)
       参数：cb  用户角色改变时通知函数
       返回：成功 0；失败 -1
       注意：暂不可用

-  设置群组列表回调

   .. code:: c

       接口：echat_win_set_grouplist_cb(win_pfn_notify_grouplist cb)
       参数：cb 通知群组列表时函数
       返回：成功 0；失败 -1
       注意：主动请求群组或群组改变时触发，通知群组列表信息（群组改变：进入群组、调度当前群组、群组列表改变时）

-  设置用户开始讲话回调

   .. code:: c

       接口：int echat_win_set_user_start_speak_cb(win_pfn_notify_user_start_speak cb)
       参数：cb 用户开始讲话时通知函数
       返回：成功 0；失败 -1
       注意：其它成员开始讲话时触发，通知群组ID和讲话人ID

-  设置用户停止讲话回调

   .. code:: c

       接口：int echat_win_set_user_stop_speak_cb(win_pfn_notify_user_stop_speak cb)
       参数：cb 用户停止讲话时通知函数
       返回：成功 0；失败 -1
       注意：其它成员结束讲话时触发，通知群组ID和讲话人ID

-  设置开始讲话回调

   .. code:: c

       接口：int echat_win_set_start_speak_cb(win_pfn_notify_start_speak cb)
       参数：cb 开始讲话时通知函数
       返回：成功 0；失败 -1
       注意：自己开始讲话时触发，通知（0,0）

-  设置停止讲话回调

   .. code:: c

       接口：int echat_win_set_stop_speak_cb(win_pfn_notify_stop_speak cb)
       参数：cb 停止讲话时通知函数
       返回：成功 0；失败 -1
       注意：自己结束讲话时触发，结束讲话包含：主动结束（通知0，0）、被摘MIC和获取MIC失败（通知群组ID和当前用户ID）等

-  设置当前群组回调

   .. code:: c

       接口：int echat_win_set_current_group_cb(win_pfn_notify_current_group cb)
       参数：cb 当前群组通知函数
       返回：成功 0；失败 -1
       注意：当前群组变化时触发，当前群组变化包含：进组失败（通知0，0）、主动进组和调度进组（通知当前群组ID和0）

-  设置用户改变回调

   .. code:: c

       接口：int echat_win_set_users_changed_cb(win_pfn_notify_users_changed cb)
       参数：cb 用户信息改变时通知函数
       返回：成功 0；失败 -1
       注意：当用户信息改变时触发，通知用户的信息，包含：用户角色、上下线、遥开摇闭、定位信息等

-  设置监听群组回调

   .. code:: c

       接口：int echat_win_set_watchgrouplist_cb(win_pfn_notify_watchgrouplist cb)
       参数：cb 监听群组时通知函数
       返回：成功 0；失败 -1
       注意：当设置了监听群组时触发，登录时有默认监听群组或配置监听群组

-  设置开始播放回调

   .. code:: c

       接口：int echat_win_set_start_playing_cb(win_pfn_notify_start_playing_sound cb)
       参数：cb 开始播放时通知函数
       返回：成功 0；失败 -1
       注意：其它成员开始讲话，自己开始播放音频时触发，通知群组ID和讲话人ID

-  设置停止播放回调

   .. code:: c

       接口：int echat_win_set_stop_playing_cb(win_pfn_notify_stop_playing_sound cb)
       参数：cb 停止播放时通知函数
       返回：成功 0；失败 -1
       注意：当其它成员结束讲话，自己停止播放音频时触发，通知群组ID和讲话人ID

-  设置调度回调

   .. code:: c

       接口：int echat_win_set_dispatcher_cb(win_pfn_notify_dispatcher cb)
       参数：cb 调度时通知函数
       返回：成功 0；失败 -1
       注意：当有调度（强拉/临时加入组/移除组）时触发，通知被调度的群组ID、用户ID列表

-  设置强拆回调

   .. code:: c

       接口：int echat_win_set_take_mic_cb(win_pfn_notify_takemic cb)
       参数：cb 调度员拆掉用户MIC通知函数
       返回：成功 0；失败 -1
       注意：暂不可用

-  设置开始录音回调

   .. code:: c

       接口：int echat_win_set_start_record_cb(win_pfn_notify_start_recordfile cb)
       参数：cb 获取MCI通知函数
       返回：成功 0；失败 -1
       注意：开始录音时触发，通知群组ID、讲话人ID和录音参数（文件名、当前时间、状态（成功/失败））

-  设置录音结束回调

   .. code:: c

       接口：int echat_win_set_finish_record_cb(win_pfn_notify_end_recordfile cb)
       参数：cb 录音结束通知函数
       返回：成功 0；失败 -1
       注意：有停止录音时触发，通知群组ID、讲话人ID和录音参数（文件名、当前时间、状态（成功/失败））

-  设置发送录音回调

   .. code:: c

       接口：int echat_win_set_send_cb(win_pfn_notify_send_callback cb)
       参数：cb 发送通知函数
       返回：成功 0；失败 -1
       注意：发送录音，服务器返回时触发，通知发送结果（成功  True；失败  False）

-  设置播放录音文件结束回调

   .. code:: c

       接口：int echat_win_set_play_recordfile_end_cb(win_pfn_notify_play_recordfile_end cb)
       参数：cb 播放录音文件结束通知函数
       返回：成功 0；失败 -1
       注意：本地录音结束时触发，通知

-  设置遥开摇闭回调

   .. code:: c

       接口：int echat_win_set_audio_enabledcall_cb(win_pfn_audio_enabled_callback cb)
       参数：cb 管理员设置用户的遥开摇闭时的通知函数
       返回：成功 0；失败 -1
       注意：管理员设置用户的遥开摇闭服务器返回时触发，通知结果（成功  True；失败  False）、状态（遥开 1；摇闭 0）、被摇闭的用户ID列表

-  设置定位开启关闭回调

   .. code:: c

       接口：int echat_win_set_switch_location_cb(win_pfn_switch_location cb)
       参数：cb 管理员设置用户定位时的通知函数
       返回：成功 0；失败 -1
       注意：管理员设置用户定位服务器返回时触发，通知结果（成功  True；失败  False）、状态（开启 1；关闭 0）、被打开的用户ID列表

-  设置多媒体消息回调

   .. code:: c

       接口：int echat_win_setmultimedia_message_cb(win_pfn_multimedia_message cb)
       参数：cb 收到多媒体消息的通知函数
       返回：成功 0；失败 -1
       注意：当服务器推送多媒体消息时触发，通知收到的消息

4 错误信息标识
--------------

.. code:: c

    ECHAT_ERR_ACCOUNT_ERROR             -1      //帐号密码错误
    ECHAT_ERR_ACCOUNT_OUTSERVICE        -2      //帐号已欠费或已超出服务期
    ECHAT_ERR_ACCOUNT_NOEXIST           -3      //帐号不存在
    ECHAT_ERR_INVALID_LOGIN_PRIVILEGE   -4      //无效的帐号登录权限
    ECHAT_ERR_ACCOUNT_NOCONFIG          -5      //没有配置帐号信息

    ECHAT_ERR_KICKOUT                   -10     //帐号已在其他位置登录
    ECHAT_ERR_LOGIN_TIMEOUT             -11     //帐号登录超时
    ECHAT_ERR_LOGOUT                    -12     //帐号已注销

    ECHAT_ERR_NETWORK_DISCONNECT        -20     //网络连接失败
    ECHAT_ERR_NETWORK_RECONECTING       -21     //网络正在重连
    ECHAT_ERR_CANNOT_CONNECT            -22     //无法连接服务器
    ECHAT_ERR_NETWORK_CONECTING         -23     //正在连接服务器

    ECHAT_ERR_JOIN_GROUP_FAILED         -30     //加入群组失败
    ECHAT_ERR_JOIN_GROUP_TIMEOUT        -31     //加入群组请求超时

    ECHAT_ERR_REQMIC_REFUSED            -40     //抢麦被拒绝
    ECHAT_ERR_REQMIC_REPEATEDLY         -41     //短暂时间内多次抢麦,不允许
    ECHAT_ERR_REQMIC_IN_AUDIOENABLE     -42     //遥闭状态抢麦,不允许
    ECHAT_ERR_REQMIC_IN_ERR_SSTATE      -43     //内部会话状态错误
    ECHAT_ERR_REQMIC_NO_AUDIOFOCUS      -44     //抢麦时申请音频焦点失败
    ECHAT_ERR_REQMIC_IN_LOW_ROLE        -45     //以较低的角色值抢麦
    ECHAT_ERR_REQMIC_TIMEOUT            -46     //接收抢麦回应超时

    ECHAT_ERR_RECORD_DEVICE             -50     //打开录音设备失败

5 常用枚举定义
--------------

6 结构类型定义
--------------
