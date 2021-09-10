# VLAN 配置

## 1.1 Vlan 配置命令

### 1.1.1 `debug gvrp event`

- 命令：`debug gvrp event interface (ethernet | port-channel | ) IFNAME no debug gvrp event interface (ethernet | port-channel | ) IFNAME`
- 功能：开启/关闭 GVRP 事件调试信息开关，包括状态机的转移以及定时器到期等信息。
- 参数：
  + ethernet：物理端口
  + port-channel：汇聚端口
  + IFNAME：端口名
- 命令模式：特权配置模式缺省情况：默认事件调试信息开关关闭。
- 使用指南：把 GVRP 事件调试信息开关打开。
- 举例：显示 GVRP 事件调试信息。

  ```text
  Switch(config)#debug gvrp event interface ethernet 1/0/1
  %Jan 16 02:25:14 2006 GVRP EVENT: LO -> VO ，interface ethernet 1/0/1，vlan 100
  %Jan 16 02:35:15 2006 GVRP EVENT: join timer expire，interface ethernet 1/0/1
  ```

### 1.1.2 `debug gvrp packet`

- 命令：`debug gvrp packet (receive | send) interface (ethernet | port-channel | ) IFNAMEno debug gvrp packet (receive | send) interface (ethernet | port-channel | ) IFNAME`
- 功能：开启/关闭 GVRP 报文调试信息开关。
- 参数：
  + receive:打开收到 GVRP 报文调试开关
  + send:打开发送 GVRP 报文调试开关
  + ethernet:物理端口
  + port-channel:汇聚端口
  + IFNAME:端口名
- 命令模式：特权配置模式
- 缺省情况：默认报文调试信息开关关闭。
- 使用指南：把 GVRP 报文调试信息开关打开。
- 举例：显示 GVRP 报文收发信息。

  ```text
  Switch(config)#debug gvrp packet receive interface ethernet 1/0/1
  Receive packet, smac 00-21-27-aa-0f-46, dmac 01-80-C2-00-00-21,
  
  length 90, protocol ID:1, attribute type:0x01,
  Attribute Index Length Event Value
  ----  -----   -------     -------
  1     10      joinIn      100
  2     10      joinEmpty   140
  3     10      leaveIn     150
  4     10      leaveEmpty  180
  ```

### 1.1.3 `dot1q-tunnel enable`

- 命令：`dot1q-tunnel enable no dot1q-tunnel enable `
- 功能： 使交换机的access端口进入dot1q-tunnel
- 模式；本命令的no命令为恢复缺省值。
- 参数： 无。 
- 命令模式：端口配置模式。
- 缺省情况：端口缺省为没有使能 dot1q-tunnel 功能。
- 使用指南：
  端口使能 dot1q-tunnel 后，对于从该端口进入的无 VLAN tag（以下简称tag）的数据包打上一层 tag；
  对于已经有 tag 的数据包打上外层 tag，tag 中的 TPID 为 8100，VLAN ID 为该端口所属的 VLAN ID。
  带有双层 tag 的数据包由 MAC 地址与外层 tag 决策转发，直到从 access 端出去时去掉外层 tag。
  由于打上外层 tag 时可能会使数据包长度超过正常大小，故建议该命令与Jumbo功能一同使用。本命令在 access 端口上使用。
  该命令和 dot1q-tunnel tpid 互斥，并且与 vlan-translation enable 互斥。
- 举例： 将端口1加入 VLAN3 并启用 dot1q-tunnel 功能。

  ```text
  Switch(config)#vlan 3
  Switch(Config-Vlan3)#switchport interface ethernet 1/0/1
  Switch(Config-Vlan3)#exit
  Switch(config)#interface ethernet 1/0/1
  Switch(Config-If-Ethernet1/0/1)# dot1q-tunnel enable
  Switch(Config-If-Ethernet1/0/1)# exit
  Switch(config)#
  ```

### 1.1.4 `dot1q-tunnel untag add c-tag`

- S5750E 交换机不支持此命令

### 1.1.5 `dot1q-tunnel selective enable`

- S5750E 交换机不支持此命令。

### 1.1.6 `dot1q-tunnel selective s-vlan`

- S5750E 交换机不支持此命令。

### 1.1.7 `dot1q-tunnel tpid`

- 命令：`dot1q-tunnel tpid {0x8100 | 0x9100 | 0x9200 | <1-65535> } `
- 功能：设置交换机 trunk 端口的协议类型（TPID）。
- 参数：无。
- 命令模式：端口配置模式。
- 缺省情况：端口缺省 TPID 为 0x8100。
- 使用指南：
  该功能是为了方便与其它厂商的设备进行互连。
  与交换机 trunk 端口连接的设备如果发送 TPID 为 0x9100 的数据包，则将该端口的 TPID 设置为 0x9100，这样交换机就可以正常接收并处理收到的数据包。
  该命令和 dot1q-tunnel enable 互斥。
- 举例：将交换机的端口 10 设置为 trunk 端口，并设置其 TPID 为 0x9100。
  ```text
  Switch(config)#interface ethernet 1/0/10
  Switch(Config-If-Ethernet1/0/10)#switchport mode trunk
  Switch(Config-If-Ethernet1/0/10)#dot1q-tunnel tpid 0x9100
  Switch(Config-If-Ethernet1/0/10)#exit
  Switch(config)#
  ```

### 1.1.8 `garp timer join`

- 命令：`garp timer join <200-500>`
- 功能：设置 garp join 定时器的时间，注意 join timer 必须小于 leave timer 的 1/2。
- 参数：`<200-500>`:定时器时间（ms） 
- 命令模式：全局配置模式缺省情况：join 定时器的默认值为 200（ms）。
- 使用指南：检查设置值是否满足范围要求，满足则把 garp 定时器时间修改为设置值，否则返回配置错误。
- 举例：将 garp join 定时器的时间设置为 200ms。

  ```text
  Switch(config)#garp timer join 200
  ```

### 1.1.9 `garp timer leave` 

- 命令：`garp timer leave <500-1200> `
- 功能：设置 garp leave 定时器的时间，注意 leave timer 必须大于 join timer 的2倍，并且小于 leaveAll timer。
- 参数：`<500-1200>`:定时器时间（ms） 
- 命令模式：全局配置模式缺省情况：leave 定时器的默认值为 600（ms）。
- 使用指南：检查设置值是否满足范围要求，满足则把 garp 定时器时间修改为设置值，否则返回配置错误。
- 举例：将 garp leave 定时器的时间设置为 600ms。

  ```text
  Switch (config)#garp timer leave 600
  ```

### 1.1.10 `garp timer leaveAll`

- 命令：`garp timer leaveall <5000-60000> `
- 功能：设置 garp leaveAll 定时器的时间，注意 leaveAll timer 必须大于 leave timer。
- 参数：`<5000-60000>`:定时器时间（ms） 
- 命令模式：全局配置模式缺省情况：leaveAll 定时器默认值为 10000（ms）。
- 使用指南：检查设置值是否满足范围要求，满足则把 GARP 的 leaveAll 定时器修改为设置值，否则返回配置错误。
- 举例：将 garp leaveAll 定时器的时间设置为 20000ms

  ```text
  Switch(config)# garp timer leaveall 20000
  ```

### 1.1.11 `gvrp`（全局)

- 命令：
  + `gvrp `
  + `no gvrp` 
- 功能：全局开启/关闭 GVRP 功能。
- 参数：无。
- 命令模式：全局配置模式
- 缺省情况：默认关闭 GVRP 全局功能。
- 使用指南：开启全局 GVRP 功能，只有开启全局 GVRP 功能 GVRP 模块才能正常工作。
- 举例：显示开启全局 GVRP 功能。

  ```text
  Switch(config)#gvrp
  ```

### 1.1.12 `gvrp`（端口）

- 命令：
  + `gvrp`
  + `no gvrp` 
- 功能：开启/关闭端口 GVRP 功能。
- 注意：可以在没有开启全局 GVRP 功能前先开启端口 GVRP 功能，但是不生效，只有当开启全局 GVRP 功能后才生效。
- 参数：无。
- 命令模式：端口配置模式
- 缺省情况：默认关闭 GVRP 端口功能。
- 使用指南：只有在 trunk 端口和 hybrid 端口上才能开启 GVRP 功能，在 access 端口开启 GVRP 功能会返回一个不能在 access 端口 开启 GVRP 的错误，在端口上开启 GVRP 功能后会把该端口添加到 GVRP 中（即给该端口在 GVRP 中添加相应状态机等结构）。
- 举例：显示开启端口 GVRP 功能。

  ```text
  Switch(config-if-ethernet1/0/1)#gvrp
  ```

### 1.1.13 `no garp timer`

- 命令：`no garp timer (join | leave | leaveall)`
- 功能：把 `garp join|leave|leaveAll` 定时器恢复成默认值
- 参数：
 +  join: join定时器
 +  leave: leave定时器
 +  leaveAll: leaveAll定时器
- 命令模式：全局配置模式
- 缺省情况：`join|leave|leaveAll` 定时器的默认值为 `200|600|10000`（ms）
- 使用指南：检查相应 timer 设置成默认值后是否满足范围要求，满足则把 GARP 的 `join|leave|leaveAll` 定时器时间还原为默认值，否则返回配置错误。
- 举例：GARP 定时器时间还原到默认值

  ```text
  Switch(config)#no garp timer leaveal
  ```

### 1.1.14 `name`

- 命令：`name <vlan-name> no name`
- 功能：为 VLAN 指定名称，VLAN 的名称是对该 VLAN 一个描述性字符串；本命令的 no 操作为删除 VLAN 的名称
- 参数：`<vlan-name>` 为指定的 vlan 名称字符串
- 命令模式：VLAN 配置模式。缺省情况：VLAN 缺省名称为 vlanXXX，其中 XXX 为 VID
- 使用指南：交换机提供为不同的 VLAN 指定名称的功能，有助于用户记忆 VLAN，方便管理
- 举例：为 VLAN100 指定名称为 TestVlan

  ```text
  Switch(Config-Vlan100)#name TestVlan
  ```

### 1.1.15 `private-vlan`

- 命令：`private-vlan {primary | isolated | community} no private-vlan`
- 功能： 将当前 VLAN 设置为 Private VLAN，该命令的 no 操作为取消 Private VLAN 设置
- 参数： 
  + `primary` 将当前 VLAN 设置为 Primary VLAN
  + `isolated` 将当前 VLAN 设置为 Isolated VLAN
  + `community` 将当前 VLAN 设置 为 Community VLAN
- 命令模式：VLAN 配置模式
- 缺省情况：缺省没有 Private VLAN 配置。
- 使用指南：Private VLAN 分为三种：
  + Primary VLAN，Isolated VLAN 和 Community VLAN。
  + Primary VLAN 内的端口可以和关联到该 Primary VLAN 的 Isolated VLAN 和 Community VLAN 中的端口进行通信；
  + Isolated VLAN 内的端口之间是隔绝的，它们只可以和其相关联的 Primary VLAN 内的端口通信；
  + Community VLAN 内的端口相互之间可以通信，也可以和其相关联的 Primary VLAN 内的端口通信；
  + 在Isolated VLAN内的端口和在Community VLAN内的端口之间不能通信。
  + 只有不包含任何以太网端口的 VLAN 才能被设置为 Private VLAN；
  + 只有设置了关联关系的 Private VLAN 才能将 Access 类型的以太网端口设置为成员端口，Isolate VLAN 的成员端口应该关闭 ingress 功能，否则无法通讯；
  + 普通 VLAN 若被设置成 Private VLAN 后，会自动将所属以太网端口清空。
  + 另外注意 GVRP 不传播 Private VLAN 的信息。
- 举例：将 VLAN100、200、300 设置为 private vlan，类型分别为 primary、Isolated、Community

  ```text
  Switch(config)#vlan 100
  Switch(Config-Vlan100)#private-vlan primary
  Note:This will remove all the ports from vlan 100
  Switch(Config-Vlan100)#exit
  Switch(config)#vlan 200
  Switch(Config-Vlan200)#private-vlan isolated
  Note:This will remove all the ports from vlan 200
  Switch(Config-Vlan200)#exit
  Switch(config)#vlan 300
  Switch(Config-Vlan300)#private-vlan community
  Note:This will remove all the ports from vlan 300
  Switch(Config-Vlan300)#exit
  ```

### 1.1.16 `private-vlan association`

- 命令：`private-vlan association <secondary-vlan-list> no private-vlan association`
- 功能： 设置 Private VLAN 的绑定操作，该命令的 no 操作为取消 Private VLAN 绑定
- 参数： `<secondary-vlan-list>` 为与指定 Primary VLAN 相关联的 Secondary VLAN 列表，Secondary VLAN 包括 Isolated VLAN 和 Community VLAN 两种，支持 `;` 连接多个 Secondary VLAN
- 命令模式：VLAN 配置模式。
- 缺省情况：缺省没有 Private VLAN 绑定。
- 使用指南：
  只有 Primary 类型的 VLAN 才能设置 Private VLAN 关联关系；
  被关联到 Primary VLAN 上的 Secondary VLANs 内的各个端口可以和关联的 Primary VLAN 内的各个端口进行通信。
  在设置 Private VLAN 关联前，三种类型的 Private VLAN 都没有以太网端口的成员端口；
  存在 Private VLAN 关联关系的 Primary VLAN 不能被删除；
  被解除关联关系的 Private VLANs 会自动将所属成员端口清空。
- 举例：将 Isolated VLAN200、Community VLAN300 关联到 Primary VLAN100 上。
  ```text
  Switch(Config-Vlan100)#private-vlan association 200;300
  ```

### 1.1.17 `show dot1q-tunnel`

- 命令：`show dot1q-tunnel`
- 功能：显示所有处于 dot1q-tunnel 状态的端口信息。
- 参数：无。 
- 命令模式：特权和配置模式。
- 使用指南：可使用该命令显示处于 dot1q-tunnel 状态的端口信息。
- 举例：显示当前 dot1q-tunnel 的状态信息。
  ```text
  Switch#show dot1q-tunnel 
  Interface Ethernet1/0/1: dot1q-tunnel is enable
  Interface Ethernet1/0/3: dot1q-tunnel is enable
  ```

### 1.1.18 `show garp timer`

- 命令：`show garp timer (join | leave | leaveall | )` 
- 功能：显示当前各个定时器的时间，注意这个时间不是定时器运行的剩余时间，而是每次开启定时器赋予的定时器初始值。
- 参数：
  + join: join 定时器
  + leave: leave 定时器
  + leaveAll: leaveAll 定时器
- 命令模式：特权配置模式
- 缺省情况：`join|leave|leaveAll` 定时器的默认值为 `200|600|10000`（ms）。
- 使用指南：根据命令中选择的定时器，输出相应定时器时间。
- 举例：显示出当前各个定时器的时间。
  ```text
  Switch#show garp timer join Garp join timer’s value is 200(ms)
  ```

### 1.1.19 `show gvrp fsm information`

- 命令：`show gvrp fsm information interface (ethernet | port-channel ) IFNAME`
- 功能：显示指定端口或所有端口当前所有属性注册状态机和请求状态机的状态。
- 参数：
  + ethernet: 物理端口
  + port-channel: 汇聚端口
  + IFNAME: 端口名
- 命令模式：特权配置模式
- 缺省情况：注册状态机默认状态是 MT，请求状态机默认状态是 VO。
- 使用指南：遍历端口的所有注册状态机和请求状态机，并显示出相应状态。
- 举例：显示所有状态机状态。
  ```text
  Switch#show gvrp fsm information interface ethernet 1/0/1 
  VA：Very anxious Active member，AA：Anxious Active member，QA：Quiet Active member 
  VP：Very anxious Passive member，AP：Anxious Passive member，QP：Quiet Passive member 
  VO：Very anxious Observer，AO：Anxious Observer，QO：Quiet Observer
  LA：Leaving Acitve member，LO：leaving Observer 
  Interface ethernet 1/0/1 gvrp fsm information:  
   Index VLANID     Applicant   Registrar
  ----   --------   ----------  --------- 
  1      100        VO          LV 
  2      300        VP          IN
  ```

### 1.1.20 `show gvrp leaveAll fsm information`

- 命令：`show gvrp leaveall fsm information interface (ethernet | port-channel ) IFNAME`
- 功能：显示指定端口或所有端口 leaveAll 状态机状态。
- 参数：
  + ethernet: 物理端口
  + port-channel: 汇聚端口
- IFNAME: 端口名命令模式：
- 特权配置模式缺省情况：leaveAll 注册状态机默认状态是 passive。
- 使用指南：查看端口 leaveAll 状态机状态。
- 举例：显示端口 leaveAll 状态机状态。

  ```text
  Switch#show gvrp leaveall fsm information interface ethernet 1/0/1 
  Interface         leaveAll fsm 
  ----------        ------------ 
  Ethernet1/0/1     passive
  ```

### 1.1.21 `show gvrp leavetimer running information`

- 命令：`show gvrp leavetimer running information (vlan <1-4094> |) interface (Ethernet | port-channel |) IFNAME`
- 功能：显示当前端口所有 leavetimer 运行信息。
- 参数：
  + `<1-4094>`：VLAN 标识
  + Ethernet：物理端口
  + port-channel：汇聚端口
  + IFNAME：端口名
  + 命令模式：特权配置模式
- 缺省情况：leavetimer 默认是关闭状态。
- 使用指南：遍历端口的所有注册状态机，显示出每个 leave 定时器的运行状态及到期时间。
- 举例：显示当前端口所有 leave 定时器的运行状态及到期时间。

  ```text
  Switch#show gvrp leavetimer running information interface ethernet 1/0/1 
  VLANID   running state    expired time 
  -------  ---------------  --------- 
  100      UP               0.2s 
  300      DOWN             non
  ```

### 1.1.22 `show gvrp port-member`

- 命令：`show gvrp (active |) port-member `
- 功能：显示当前所有开启 GVRP 的端口，active 表示开启 GVRP 并且处于活动状态的端口。
- 参数：Active：表示端口处于活动状态
- 命令模式：特权配置模式
- 缺省情况：默认端口关闭 GVRP 协议。
- 使用指南：遍历 GVRP 中保存的开启了 GVRP 协议或者开启协议并处于活动状态的端口，显示出来。
- 举例：显示所有开启 GVRP 协议（或者并处于活动状态）的端口。
  
  ```text
  Switch#show gvrp port member 
  Ports which were enabled gvrp included：
  Ethernet1/0/3（T） Ethernet1/0/4（T）
  Ethernet1/0/5（T） Ethernet1/0/6（T）
  Ethernet1/0/7（T） Ethernet1/0/8（T）
  Ethernet1/0/9（T） Ethernet1/0/10（T）
  ```

### 1.1.23 `show gvrp port registerd vlan`

- 命令：`show gvrp port (dynamic | static | ) registerd vlan interface (Ethernet | port-channel |) IFNAME` 
- 功能：显示当前端口所有动态注册或者静态注册的 VLAN。
- 参数：
    + dynamic：动态注册
    + static：静态注册
    + Ethernet:物理端口
    + ort-channel:汇聚端口
    + IFNAME：端口名
- 命令模式：特权配置模式
- 缺省情况：默认情况下端口动态或者静态注册的 VLAN 个数为零。
- 使用指南：遍历端口的注册状态机，把包含动态或者静态注册的注册状态机对应的 VLAN 显示出来。
- 举例：显示当前端口所有的动态注册或者静态注册的 VLAN。

  ```text
  Switch#show gvrp port registerd vlan interface ethernet 1/0/1 
  Current port dynamic registerd vlan included： 
  Vlan10 vlan20 
  Vlan40 vlan60 
  Current port static registerd vlan included：
  Vlan10 vlan30 
  Vlan40 vlan200
  ```
  
### 1.1.24 show gvrp timer running information

- 命令：`show gvrp timer (join| leaveall) running information interface (ethernet | port-channel |) IFNAME`
- 功能：显示当前端口所有 join|leaveAll 定时器运行信息。
- 参数：
    + join: join 定时器
    + leaveAll: leaveAll 定时器
    + ethernet:物理端口
    + port-channel:汇聚端口
    + IFNAME:端口名
- 命令模式：特权配置模式
- 缺省情况：默认状态下 join 定时器是关闭的，leaveAll 定时器是开启的。
- 使用指南：查看端口当前的 join|leaveAll 定时器运行状态。
- 举例：显示各个定时器的运行状态及到期时间。

  ```text
  Switch(config)#show gvrp timer join running information interface ethernet 1/0/1 
  Current port’s jointimer running state is: UP 
  Current port’s jointimer expired time is: 0.2 s
  ```
  
###   1.1.25 show gvrp vlan registerd port

- 命令：`show gvrp vlan <1-4094> registerd port `
- 功能：显示出注册了指定 VLAN 的端口集合。
- 参数：<1-4094>：VLAN 标识
- 命令模式：特权配置模式
- 缺省情况：默认情况下注册了指定 VLAN 的端口集合为空。
- 使用指南：无。
- 举例：显示注册了当前 VLAN 的所有端口。

  ```text
  Switch#show gvrp vlan 100 registerd port 
  Ethernet1/0/3（T） Ethernet1/0/4（T） 
  Ethernet1/0/5（T） Ethernet1/0/6（T） 
  Ethernet1/0/7（T） Ethernet1/0/8（T） 
  Ethernet1/0/9（T） Ethernet1/0/10（T）
  ```
  
### 1.1.26 show vlan

- 命令：`show vlan [brief| summary] [id <vlan-id>] [name <vlan-name>] [internal usage [id <vlan-id>| name <vlan-name>]] [private-vlan [id <vlan-id> | name <vlan-name> ]]`

- 功能：显示所有 VLAN 或者指定 VLAN 的详细状态信息。

- 参数：
  + brief 简要信息；
  + `<summary>` 显示 VLAN 统计信息；`<vlan-id>` 为指定要显示状态信息的 VLAN 的 VLAN ID，取值范围 1~4094；
  + `<vlan-name>` 为指定要显示状态
  + 信息的 VLAN 的 VLAN 名，长度为1~11。
  + private-vlan 显示 private-vlan 的 id、name、关联 vlan 和端口等相关信息。
  
- 命令模式：特权和配置模式。

- 使用指南：如果不指定 `<vlan-id>` 或 `<vlan-name>` ，则显示交换机所有 VLAN 的状态信息。

- 举例：显示当前 VLAN 状态信息；显示当前 VLAN 统计信息。
  
  ```text
  Switch#show vlan 
  VLAN  Name         Type       Media          Ports
  ----  --------     -------    ---------      ---------------------------------------
  1     default      Static     ENET           Ethernet1/0/1 Ethernet1/0/2 Ethernet1/0/3
                                               Ethernet1/0/4 Ethernet1/0/9 Ethernet1/0/10
                                               Ethernet1/0/11 Ethernet1/0/12
  2     VLAN0002     Static     ENET           Ethernet1/0/5 Ethernet1/0/6 Ethernet1/0/7
                                               Ethernet1/0/8
  Switch#sh vlan summary
  The max. vlan entrys: 4094
  Existing Vlans:
  Universal Vlan:
  1    12    13    15    16    22
  Total Existing Vlans is:6
  ```
  
  | 显示内容 | 解释                                       |
  | -------- | ------------------------------------------ |
  | VLAN     | VLAN号。                                   |
  | Name     | VLAN名。                                   |
  | Type     | VLAN的属性，是静态配置的还是动态学习到的。 |
  | Media    | VLAN的二层属性。                           |
  | Ports    | VLAN内Access端口。                         |
  
  ```text
  Switch(config)#show vlan private-vlan
  VLAN    Name         Type         Asso      VLAN         Ports
  ----    --------     --------     -----     -----        ------------------------------------------
  100     VLAN0100     Primary      101       102          Ethernet1/0/9 Ethernet1/0/10 Ethernet1/0/11
                                                           Ethernet1/0/12 Ethernet1/0/13 
  101     VLAN0101     Community    100                    Ethernet1/0/9 Ethernet1/0/10 Ethernet1/0/11
                                                           Ethernet1/0/12 Ethernet1/0/13
  102     VLAN0102     Isolate      100                    Ethernet1/0/9
  ```
  
### 1.1.27 show vlan-translation

- 命令：`show vlan-translation`
- 功能：显示 vlan-translation 相关配置。
- 参数：无。 
- 命令模式：特权用户配置模式。
- 使用指南：显示交换机 vlan-translation 的相关配置。 交换机的 access 口不支持此功能。
- 举例：显示 vlan-translation 相关配置。

  ```text
  Switch# show vlan-translation 
  Interface Ethernet1/0/1: 
  vlan-translation is enable, miss drop is not set 
  vlan-translation 5 to 10 in
  ```
  
### 1.1.28 switchport access vlan

- 命令：`switchport access vlan <vlan-id> no switchport access vlan`
- 功能：将当前 Access 端口加入到指定 VLAN；本命令 no 操作为将当前端口从 VLAN 里删除。
- 参数：`<vlan-id>` 为当前端口要加入的 vlan VID，取值范围为1~4094。
- 命令模式：端口配置模式。
- 缺省情况：所有端口默认属于 VLAN1。
- 使用指南：只有属于 Access mode 的端口才能加入到指定的 VLAN 中，并且 Access 端口同时只能加入到一个 VLAN 里去。
- 举例：设置某 Access 端口加入 VLAN 100。

  ```text
  Switch(config)#interface ethernet 1/0/8 
  Switch(Config-If-Ethernet1/0/8)#switchport mode access 
  Switch(Config-If-Ethernet1/0/8)#switchport access vlan 100 
  Switch(Config-If-Ethernet1/0/8)#exit
  ```
  
### 1.1.29 switchport dot1q-tunnel

- S5750E交换机不支持此命令。

### 1.1.30 switchport forbidden vlan

- 命令：`switchport forbidden vlan {WORD | all | add WORD | except WORD | remove WORD} no switchport forbidden vlan`
- 功能：设置端口的 forbidden vlan，注意此命令只能在 trunk 端口或者 hybrid 端口上配置，但是可以在没有开启 GVRP 功能的端口上配置。no 命令为取消端口的 forbidden vlanlist。
- 参数：
   + WORD: 将 vlanList 加为 forbidden vlan，同时覆盖之前的配置 
   + all: 将所有 vlan 都设置为 forbidden 
   + vlan add WORD: 在现有的 forbidden vlanList 中增加 vlanList 
   + except WORD: 将除了 vlanList 外所有的 vlan 都设置为 forbidden vlan 
   + remove WORD: 从现有的 forbidden vlanList 中删除 vlanList 指定的 vlan
- 命令模式：端口配置模式
- 缺省情况：默认端口的 forbidden vlanList 为空。
- 使用指南：把端口的 forbidden vlanList 相应标识位置位，并清除端口的 allow vlanList 标志位。如果端口静态加入了这些 VLAN，则静态离开这些 VLAN，并且向 GVRP 模块发出消息，使该端口相应 VLAN 的注册状态机进入 forbidden 工作模式。
- 举例：端口离开相应 VLAN，GVRP 相应注册状态机进入 forbidden 模式。

  ```text
  Switch (config-if-ethernet1/0/1)# switchport forbidden vlan all
  ```
  
###  1.1.31 switchport hybrid allowed vlan

- 命令：`switchport hybrid allowed vlan {WORD | all | add WORD | except WORD | remove WORD} {tag | untag}
no switchport hybrid allowed vlan`
- 功能：设置 Hybrid 端口允许以 tag 或 untag 方式通过的 VLAN；本命令的 no 操作为恢复缺省情况
- 参数：
  + WORD：将 vlan List 加为 allowed vlan ,同时覆盖之前的配置。 
  + all：将所有VLAN都设置为 allowed vlan。
  + add WORD：在现有的 allowed vlanList 中增加 vlanList 。
  + except WORD：将除了 vlan List 外所有的VLAN都设置为 allowed vlan 。
  + remove WORD：从现有的 allow vlanList 中删除 vlanList 指定的 VLAN 。
  + tag：以 tag 方式加入指定的 VLAN 。
  + untag：以 untag 方式加入指定的 VLAN 。
- 命令模式：端口配置模式。
- 缺省情况：不允许任何 VLAN 的流量通过。
- 使用指南：用户可以通过本命令设置哪些 VLAN 的流量通过 Hybrid 端口，没有包含的 VLAN 流量则被禁止。
          设置 allowed vlan 为 tag 和 untag 方式的区别为：设置VLAN为 untag 方式时，通过 Hybrid 口
          去往该VLAN的流量不带tag；设置 VLAN 为 tag 方式时，通过 Hybrid 口去往该 VLAN 的流量带着相应的 tag 。
          Hybrid 不可以同时以 tag 和 untag 方式 allowed 同一个 VLAN ，如果先后配置了 allowed 某个 VLAN 为 tag 和 untag 方式，
          则后配置的会覆盖前面的配置。
- 举例：设置 Hybrid 端口以 untag 方式 allowed vlan 1;3;5-20，以 tag 方式 allowed vlan 100;300;500-2000。 

  ```text
  Switch(config)#interface ethernet 1/0/5 
  Switch(Config-If-Ethernet1/0/5)#switchport mode hybrid 
  Switch(Config-If-Ethernet1/0/5)#switchport hybrid allowed vlan 1;3;5-20 untag
  Switch(Config-If-Ethernet1/0/5)#switchport hybrid allowed vlan 100;300;500-2000 tag
  Switch(Config-If-Ethernet1/0/5)#exit
  ```
  
### 1.1.32 switchport hybrid native vlan

- 命令：`switchport hybrid native vlan <vlan-id> no switchport hybrid native vlan`
- 功能：设置 Hybrid 端口的 PVID ；本命令的 no 操作为恢复缺省值
- 参数：`<vlan-id>`为 Hybrid 端口的 PVID 。
- 命令模式：端口配置模式。
- 缺省情况：Hybrid 端口默认的 PVID 为1。
- 使用指南：Hybrid 端口的PVID的作用是当一个 untagged 的帧进入 Hybrid 端口，端口会对这个 untagged 帧打上带有本命令设置的 native PVID 的 tag 标记，用于 VLAN 的转发
- 举例：设置某 Hybrid 端口的 native vlan 为100。

  ```text
  Switch(config)#interface ethernet1/0/5 
  Switch(Config-If-Ethernet1/0/5)#switchport mode hybrid 
  Switch(Config-If-Ethernet1/0/5)#switchport hybrid native vlan 100 
  Switch(Config-If-Ethernet1/0/5)#exit
  ```
  
### 1.1.33 switchport interface

- 命令：`switchport interface [ethernet | portchannel] [<interface-name | interface-list>] no switchport interface [ethernet | portchannel] [<interface-name | interface-list>]` 
- 功能：给 VLAN 分配以太网端口的命令；本命令的 no 操作为删除指定 VLAN 内的一个或一组端口。
- 参数：ethernet 要添加的为以太网端口；portchannel 要添加的为一个链路聚合端口；
       `<interface-name>` 端口名称，如e1/0/1，若选择端口名称则不用选 etherneth 或 
       portchannel；`<interface-list>` 要添加或者删除的以太网端口的列表，支持”;” ”-” ，
       如：ethernet 1/0/1;3;4-7;8，或者是`<interface-list>`要添加或删除的端口链路聚合，
       如 port-channel 1 。
- 命令模式：VLAN 配置模式。
- 缺省情况：新建立的 VLAN 缺省不包含任何端口。
- 使用指南：Access 端口为普通端口，可以加入 VLAN ，但同时只允许加入一个 VLAN 。
- 举例：为 VLAN 100分配百兆以太网端口1，3，4-7，8。

  ```text
  Switch(Config-Vlan100)#switchport interface ethernet 1/0/1;3;4-7;8
  ```
  
### 1.1.34 switchport mode

- 命令：`switchport mode {trunk | access | hybrid} `
- 功能：设置交换机的端口为 access 模式，trunk 模式或 hybrid 模式。
- 参数：trunk 表示端口允许通过多个VLAN的流量；access 为端口只能属于一个 VLAN；
       hybrid 表示端口可以以 tag 或 untag 方式允许通过多个 VLAN 的流量。
- 命令模式：端口配置模式。
- 缺省情况：端口缺省为 Access 模式。
- 使用指南：工作在 trunk mode 下的端口称为 Trunk 端口，Trunk 端口可以通过多个 VLAN
          的流量，通过 Trunk 端口之间的互联，可以实现不同交换机上的相同 VLAN 的互通；工作在 access mode 下
          的端口称为 Access 端口，Access 端口可以分配给一个 VLAN ，并且同时只能分配给一个 VLAN 。Hybrid 端口
          同样可以通过多个 VLAN 的流量，可以接收和发送多个 VLAN 的报文，可以用于交换机之间连接，也可以用于连
          接用户的计算机。Hybrid 端口和 Trunk 端口在接收数据时，处理方法是一样的，唯一不同之处在于发送数据
          时：Hybrid 端口可以允许多个 VLAN 的报文发送时不打标签，而 Trunk 端口只允许缺省 VLAN 的报文发送时不
          打标签。端口的属性不允许在 Hybrid 和 Trunk 之间直接转换，必须先配置为 Access ，再配置为 Hybrid 或
          Trunk。取消端口的Trunk或Hybrid属性时，端口属性恢复到默认的Access属性，属于vlan 1。
举例：将端口5设置为 trunk 模式，端口8设置为 access 模式，端口10为 Hybrid 模式

  ```text
  Switch(config)#interface ethernet 1/0/5 
  Switch(Config-If-Ethernet1/0/5)#switchport mode trunk 
  Switch(Config-If-Ethernet1/0/5)#exit 
  Switch(config)#interface ethernet 1/0/8 
  Switch(Config-If-Ethernet1/0/8)#switchport mode access 
  Switch(Config-If-Ethernet1/0/8)#exit 
  Switch(config)#interface ethernet 1/0/10 
  Switch(Config-If-Ethernet1/0/10)#switchport mode hybrid 
  Switch(Config-If-Ethernet1/0/10)#exit        
  ```
  
### 1.1.35 switchport mode trunk allow-null

- 命令：`switchport mode trunk allow-null`
- 功能：新增加一个配置端口为 trunk 端口的方式，由于原来把端口配置成 trunk 端口默认加
       入所有 VLAN ，对于这种方式无论如何处理在开启 GVRP 功能时都不合适。所以这里增加了一个配
       置端口为 trunk 端口的方式，它默认不会加入任何 VLAN ，所以在这样的 trunk 端口上开启 
       GVRP 功能比较合适。因此，推荐用户在开启端口 GVRP 功能时，先用此命令把端口配置成 trunk 端口。
       该命令在端口已经是 trunk 端口时也可以使用，相当于把 allow-list 置空，离开所有 VLAN
- 参数：无
- 命令模式：端口配置模式
- 缺省情况：默认端口为 access 模式。
- 使用指南：把端口模式变为 trunk 模式，并使其离开所有 vlan ，allow-list 置空
- 举例： 

  ```text
  Switch(config-if-ethernet1/0/1)#switchport mode trunk allow-null
  ```

### 1.1.36 switchport trunk allowed vlan

- 命令：`switchport trunk allowed vlan {WORD | all | add WORD | except WORD | remove WORD} 
        no switchport trunk allowed vlan`
- 功能：设置 Trunk 端口允许通过 VLAN ；本命令的 no 操作为恢复缺省情况
- 参数：
  + WORD： 指定的 VIDs ；
  + all： 所有的 VIDs ，即1－4094；
  + add： 在现有的 allow vlan 后面加入指定的 VIDs ；
  + except： 除了指定的 VIDs 外所有的VID都加为 allow vlan ；
  + remove： 从现有的 allow vlan 列表中删除指定的 allow vlan
- 命令模式：端口配置模式。
- 缺省情况：Trunk 端口缺省允许通过所有 VLAN。
- 使用指南：用户可以通过本命令设置哪些 VLAN 的流量通过 Trunk 端口，没有包含的 VLAN 流量则被禁止
- 举例：设置 Trunk 端口允许通过VLAN1，3，5-20的流量。

  ```text
  Switch(config)#interface ethernet 1/0/5 
  Switch(Config-If-Ethernet1/0/5)#switchport mode trunk 
  Switch(Config-If-Ethernet1/0/5)#switchport trunk allowed vlan 1;3;5-20 
  Switch(Config-If-Ethernet1/0/5)#exit
  ```
  
### 1.1.37 switchport trunk native vlan

- 命令：`switchport trunk native vlan <vlan-id> no switchport trunk native vlan` 
- 功能：设置 Trunk 端口的 PVID ；本命令的 no 操作为恢复缺省值。
- 参数：`<vlan-id>` 为 Trunk 端口的 PVID 。 
- 命令模式：端口配置模式。 
- 缺省情况：Trunk 端口默认的 PVID 为1。
- 使用指南：在802.1Q中定义了 PVID 这个概念。Trunk 端口的 PVID 的作用是当一个 untagged 的帧进入 Trunk 端口，端口会对这个 untagged 帧打上带有本命令设置的 native PVID 的 tag 标记，用于 VLAN 的转发。
- 举例：设置某 Trunk 端口的 native vlan 为100。
 
  ```text
  Switch(config)#interface ethernet1/0/5 
  Switch(Config-If-Ethernet1/0/5)#switchport mode trunk 
  Switch(Config-If-Ethernet1/0/5)#switchport trunk native vlan 100 
  Switch(Config-If-Ethernet1/0/5)#exit
   ```
  
### 1.1.38 vlan

- 命令：`vlan WORD 
       no vlan WORD`
- 功能： 创建 VLAN 或者创建并进入 VLAN 配置模式。如果参数表示的为用‘;’和‘-’连接的多
      个 vlan ，则只是创建这些vlan 。如果参数只是一个 vlan ，若这个 vlan 存在，则进
      入 vlan 配置模式；若此 vlan 不存在，则创建此 vlan 并进入 vlan 配置模式。在 VLAN
      模式中，用户可以配置 VLAN 名称和为该 VLAN 分配交换机端口。本命令的 no 操作为删除
      指定的 VLAN ，如果参数指定了多个 vlan ，则同时删除这些 vlan 。
- 参数： WORD 为要创建/删除的 VLAN 的VID，取值范围为1~4094，用‘;’和‘-’连接。
- 命令模式：全局配置模式。
- 缺省情况：交换机缺省只有 VLAN1 。
- 使用指南：VLAN1 为交换机的缺省 VLAN ，用户不能配置和删除 VLAN1 。允许配置 VLAN 的总共数量为4094个。另需要提醒的是不能使用本命令删除通过 GVRP 学习到的动态 VLAN 。
- 举例：创建 VLAN 100，并且进入 VLAN 100的配置模式。

  ```text
  Switch(config)#vlan 100 
  Switch(Config-Vlan100)#
  ```
  
### 1.1.39 vlan internal

- 命令：`vlan <2-4094> internal `
- 功能：指定内部 VLAN 的 ID 号。当某个 ID 号被指定为内部 VLAN 的ID后，就被保留不允许其它 VLAN 使用，即不能再用来创建VLAN。内部VLAN仅用于LOOPBACK接口，不能添加物理端口。新设置的内部VLAN ID必须保存配置并重启交换机后才能生效。
- 参数：`<2-4094>`:指定为内部 VLAN 的ID号，取值范围为2~4094。
- 命令模式：全局配置模式。
- 缺省情况：缺省的内部 VLAN 的 ID 为1006。
- 使用指南：交换机缺省采用1006为内部 VLAN 的 ID ，
一般不需要修改；只有在组网需要1006作为 VLAN 的 ID 时，才有必要把内部 VLAN ID 设置为其它值。内部 VLAN ID 一定要选择一个不使用的ID号，否则会影响其它 VLAN 的使用。该命令设置后，必须保存配置并重启交换机后才能生效。
- 举例：设置100为内部 VLAN 的 ID 。

  ```text
  Switch(config)#vlan 100 internal
  ```

### 1.1.40 vlan ingress enable

- 命令：`vlan ingress enable no vlan ingress enable`
- 功能：打开端口的 VLAN 入口过滤功能；本命令的 no 操作为关闭入口过滤功能
- 命令模式：端口配置模式缺省情况：系统缺省打开端口的 VLAN 入口过滤功能
- 使用指南：当打开端口的 VLAN 入口过滤功能，系统在接收数据时会检查源端口是否是该 VLAN 的成员端口，如果是则接受数据并转发到目的端口，否则丢弃该数据
- 举例：关闭端口的 VLAN 入口规则

  ```text
  Switch(Config-If-Ethernet1/0/1)# no vlan ingress enable
  ```
  
### 1.1.41 vlan-translation

- 命令：`vlan-translation <old-vlan-id> to <new-vlan-id> {in | out} no vlan-translation <old-vlan-id> {in | out}`
- 功能： 添加 VLAN translation 转换规则，使原 VLAN ID 与现 VLAN ID 产生一条映射；本命令的 no 命令为删除对应映射
- 参数： old-vlan-id 为原 VLAN ID；new-vlan-id 为翻译后的 VLAN ID ；in 为入口翻译, out 为出口翻译
- 命令模式： 端口配置模式。缺省情况： 缺省没有 VLAN translation 翻译关系
- 使用指南： 本命令为设置 VLAN translation 的翻译关系。数据包将根据设置的翻译关系进行匹配，如果匹配成功，则将 VLAN ID 改变为设置条目中的 VLAN ID ，如果匹配不成功，则由 vlan-translation miss drop 命令决定下一步的转发。交换机的 access 口不支持此功能
- 举例： 在端口1将进入的 VLAN 100的数据经入口翻译后划入 VLAN2

  ```text
  Switch#config Switch(config)#interface ethernet 1/0/1
  Switch(Config-If-Ethernet1/0/1)#vlan-translation enable
  Switch(Config-If-Ethernet1/0/1)#vlan-translation 100 to 2 in
  Switch(Config-If-Ethernet1/0/1)#exit Switch(config)#
  ```
  
### 1.1.42 vlan-translation enable

- 命令：`vlan-translation enable no vlan-translation enable`
- 功能：开启端口的 vlan-translation 功能；本命令的 no 命令为恢复缺省值
- 参数：无。
- 命令模式：端口配置模式
- 缺省情况：端口缺省为没有使能 VLAN translation 功能
- 使用指南：此命令与 dot1q-tunnel enable 互斥。 交换机的 access 口不支持此功能
- 举例：端口1上开启 VLAN translation 功能
 
  ```text
  Switch#config Switch(config)#interface ethernet 1/0/1
  Switch(Config-If-Ethernet1/0/1)#vlan-translation enable 
  ```
  
### 1.1.43 vlan-translation miss drop

- 命令：`vlan-translation miss drop in
        no vlan-translation miss drop in`
- 功能：定义翻译失败时丢包；本命令的 no 命令为恢复缺省值
- 参数：in 为入口
- 命令模式：端口配置模式
- 缺省情况：翻译失败不丢包
- 使用指南：在进行原 VID 与现 VID 的映射关系翻译时，如时没有配置相应翻译关系，缺省
          为不丢包。当使用本命令后，翻译失败会丢掉 tag 报文，此命令对 untag 报文无效。 交换机的
          access 口不支持此功能
- 举例： 设置端口1入口翻译失败丢包

  ```text
  Switch(Config-If-Ethernet1/0/1)#vlan-translation miss drop in 
  ```
  
## 1.2 动态vlan配置命令

### 1.2.1 dynamic-vlan mac-vlan prefer

- 命令：`dynamic-vlan mac-vlan prefer`
- 功能：设置 MAC-based VLAN 优先
- 参数：无。
- 命令模式：全局配置模式
- 缺省情况：MAC-based VLAN 优先
- 使用指南：设置交换机动态 VLAN 的优先顺序。缺省优先顺序为 MAC-based VLAN、
          IP-subnet-based VLAN、Protocol-based VLAN，即多种动态 VLAN 在同时使
          用时优先匹配的顺序。在设置 IP-subnet-based VLAN 优先后，若恢复 MAC-based 
          VLAN 优先时使用本命令。
- 举例：设置 MAC-based VLAN 优先。

  ```text
  Switch#config 
  Switch(config)#dynamic-vlan mac-vlan prefer
  ```
  
### 1.2.2 dynamic-vlan subnet-vlan prefer

- 命令：`dynamic-vlan subnet-vlan prefer`
- 功能：设置 IP-subnet-based VLAN 优先。
- 参数：无。
- 命令模式：全局配置模式。
- 缺省情况：MAC-based VLAN 优先。
- 使用指南：设置交换机动态 VLAN 的优先顺序。缺省优先顺序为 MAC-based VLAN、IP-subnet-based VLAN、
        Protocol-based VLAN，即多种动态 VLAN 在同时使用时优先匹配的顺序。本命令为设置IP-subnet-based 
        VLAN 优先。
- 举例： 设置 IP-subnet-based VLAN 优先。

  ```text
  Switch#config 
  Switch(config)#dynamic-vlan subnet-vlan prefer
  ```
  
### 1.2.3 mac-vlan
- 命令：`mac-vlan mac <mac-addrss> <mac-mask> vlan <vlan-id> priority <priority-id> no mac-vlan {mac <mac-addrss> <mac-mask>|all}`
- 功能：添加 MAC 地址与 VLAN 的对应关系，即指定 MAC 地址加入指定 VLAN ；本命令的 no 命令为删除 MAC 地址与 VLAN 的对应关系。
- 参数：mac-address 为 MAC 地址，格式为XX-XX-XX-XX-XX-XX，mac-mask 为 mac 地址掩码，
       格式为 XX-XX-XX-XX-XX-XX，vlan-id 为 VLAN 号，取值范围为1~4094；priority-id 为优
       先级，用于 VLAN tag 中，取值范围0~7；all 为所有 MAC 地址。命令模式：全局配置模式。
- 缺省情况：没有MAC地址加入 VLAN。
- 使用指南：该命令将指定的MAC地址加入到指定 VLAN 中。若有指定的MAC地址的无 VLAN 标签
          数据包从交换机端口进入，它将匹配到指定的 VLAN 号，从而进入指定的 VLAN，不管该数据包
          从哪个端口进入，其所属 VLAN 是一致的。该命令设置后不对有 VLAN 标签的数据包进行干涉。
- 举例：将 MAC 地址为00-03-0f-11-22-33的网络设备划入 VLAN 100.

  ```text
  Switch#config 
  Switch(config)#mac-vlan mac 00-03-0f-11-22-33 ff-ff-ff-ff-ff-ff vlan 100 priority 0
  ``` 
  
### 1.2.4 mac-vlan vlan

- 命令：`mac-vlan vlan <vlan-id> no mac-vlan vlan <vlan-id>`
- 功能：设置指定 VLAN 为 MAC VLAN；本命令的 no 命令为取消该 VLAN 为 MAC VLAN。
- 参数：`<vlan-id>` 为指定的 VLAN 号。
- 命令模式：全局配置模式。
- 缺省情况：没有 MAC VLAN。
- 使用指南：设置指定 VLAN 为 MAC VLAN。
- 举例：将 docsVLAN100 设置为 MAC VLAN。

  ```text
  switch#config 
  switch(config)#mac-vlan vlan 100
  ```
  
### 1.2.5 protocol-vlan

- 命令：`protocol-vlan mode {ethernetii etype <etype-id>|llc {dsap <dsap-id> ssap <ssap-id>}|snap etype <etype-id>} vlan <vlan-id> priority <priority-id> no protocol-vlan {mode {ethernetii etype <etype-id>|llc {dsap <dsap-id> ssap <ssap-id>}|snap etype <etype-id>}|all}`
- 功能：添加协议与 VLAN 的对应关系，即指定协议加入指定 VLAN；本命令的 no 命令为删除协议与 VLAN 的对应关系。
- 参数：mode为配置封装类型，为 ethernetii、llc、snap；ethernetii 为EthernetII封装格式；etype-id 为报文协议类型，
     取值范围为 1536~65535；llc 为 LLC 封装格式；dsap-id 为目的服务接入点，取值范围为0~255；ssap-id 为源服务接入点，取值
     范围为0~255；snap 为 SNAP 封装格式；etype-id 为报文协议类型，取值范围为1536~65535；vlan-id 为 VLAN 号，取值范围为1~4094；
     priority 为优先级，取值范围为0~7；all 为所有封装类型下的协议。
- 命令模式：全局配置模式。
- 缺省情况：没有协议加入 VLAN 。
- 使用指南：该命令将指定的协议加入到指定 VLAN 中。若有指定的协议的无 VLAN 标签数据包从交换机端口进入，它将匹配到指定的 VLAN 号，
        从而进入指定的 VLAN，不管该数据包从哪个端口进入，其所属 VLAN 是一致的。该命令设置后不对有 VLAN 标签的数据包进行干涉。在配置 IP 协议时建议将ARP协议一并配置，否则某些应用会受到影响。
- 举例：将以太网II封装的IP协议数据包划入 VLAN 200。

  ```text
  Switch#config 
  Switch(config)#protocol-vlan mode ethernetii etype 2048 vlan 200
   ```  

###  1.2.6 show dynamic-vlan prefer

- 命令`show dynamic-vlan prefer` 
- 功能：显示动态 VLAN 的优先顺序。
- 参数：无。 
- 命令模式：特权和配置模式。
- 使用指南：显示动态 VLAN 的优先顺序。
- 举例：显示当前动态 VLAN 的优先顺序。

  ```text
  Switch#show dynamic-vlan prefer 
  Mac Vlan/Voice Vlan 
  IP Subnet Vlan 
  Protocol Vlan
  ```
  
### 1.2.7 show mac-vlan

- 命令：`show mac-vlan`
- 功能：显示交换机 MAC-based VLAN 的配置情况。
- 参数：无。
- 命令模式：特权和配置模式。
- 使用指南：显示交换机 MAC-based VLAN 的配置情况。
- 举例：显示当前 MAC-based VLAN 的配置情况。

  ```text
  Switch#show mac-vlan
  MAC-Address         VLAN_ID      Priority
  ------------------ ------------ -------
  00-e0-4c-77-ab-9d       2            2
  00-0a-eb-26-8d-f3       2            2
  00-03-0f-11-22-33     5            5
  ```
  
### 1.2.8 show mac-vlan interface
- 命令：`show mac-vlan interface`
- 功能：显示处于 MAC-based VLAN 的端口。
- 参数：无。
- 命令模式：特权和配置模式。
- 使用指南：显示使能了 MAC-based VLAN 的端口，端口后括号内标出了端口的属性，A 表示 Access 口，T 表示 Trunk 口，H表示 Hybrid 口。
- 举例：显示当前使能了 MAC-based VLAN 的端口。

  ```text
  Switch#show mac-vlan interface 
  Ethernet1/0/1(A)        Ethernet1/0/2(A) 
  Ethernet1/0/3(A)        Ethernet1/0/4(A) 
  Ethernet1/0/5(H)        Ethernet1/0/6(T)
  ```
  
### 1.2.9 show protocol-vlan

- 命令：`show portocol-vlan`
- 功能：显示交换机 Protocol-based VLAN 的配置情况。
- 参数：无。
- 命令模式：特权和配置模式。
- 使用指南：显示交换机 Protocol-based VLAN 的配置情况。
- 举例：显示当前 Protocol-based VLAN 的配置情况。
 
  ```text
  Switch#show protocol-vlan
  Protocol_Type                    VLAN_ID          Priority
  ------------------            ------------        ---------
  mode ethernetii etype 0x800        200               4
  mode ethernetii etype 0x860        200               4
  mode snap etype 0xabc              100               5
  mode llc dsap 0xac ssap 0xbd       100               5
  ```

### 1.2.10 show subnet-vlan

- 命令：`show subnet-vlan` 
- 功能：显示交换机 IP-subnet-based VLAN 的配置情况。
- 参数：无。 
- 命令模式：特权和配置模式。
- 使用指南：显示交换机 IP-subnet-based VLAN 的配置情况。
- 举例：显示当前 IP-subnet-based VLAN 的配置情况。
 
  ```text
  Switch#show subnet-vlan
  IP-Address              Mask          VLAN_ID
  ------------------ -----------------  -------
  192.168.1.165       255.255.255.0       2
  202.200.121.21      255.255.0.0         2
  10.0.0.1            255.248.0.0         5
  ```
  
###   1.2.11 show subnet-vlan interface

- 命令：`show subnet-vlan interface` 
- 功能：显示处于 IP-subnet-based VLAN 的端口。
- 参数：无。 
- 命令模式：特权和配置模式。
- 使用指南：显示使能了 IP-subnet-based VLAN 的端口，端口后括号内标出了端口的属性，A表示 Access 口，T 表示 Trunk 口，H 表示 Hybrid 口。
- 举例：显示当前使能了 IP-subnet-based VLAN 的端口。
 
  ```text
  Switch#show subnet-vlan interface 
  Ethernet1/0/1(A)         Ethernet1/0/2(A) 
  Ethernet1/0/3(A)         Ethernet1/0/4(A) 
  Ethernet1/0/5(H)         Ethernet1/0/6(T)
  ```
### 1.2.12 subnet-vlan

- 命令：`subnet-vlan ip-address <ipv4-addrss> mask <subnet-mask> vlan <vlan-id> priority <priority-id> no subnet-vlan {ip-address <ipv4-addrss> mask <subnet-mask>|all}` 
- 功能： 添加IP子网与 VLAN 的对应关系，即指定IP子网加入指定 VLAN ；本命令的 no 命令为删除该/全部对应关系。
- 参数： ipv4-address 为 IPv4 地址，格式为点分十进制，每段取值范围0~255；subnet-mask 为子网掩码，
        格式为点分十进制，每段取值范围0~255；priority-id 为优先级，用于 VLAN tag 中，取值范围0~7；
        vlan-id 为 VLAN 号，取值范围为1~4094；all 为所有子网。
- 命令模式： 全局配置模式。
- 缺省情况： 没有IP子网加入 VLAN。
- 使用指南： 该命令将指定的IP子网加入到指定 VLAN 中。若有指定的IP子网的无 VLAN 标签数据包从交换机端口进入，它将匹配到指定的 VLAN 号，从而进入指定的 VLAN，不管该数据包从哪个端口进入，其所属 VLAN 是一致的。该命令设置后不对有 VLAN 标签的数据包进行干涉。
- 举例： 将IP子网为192.168.1.0/24的网络设备划入 VLAN 300。

  ```text
  Switch#config 
  Switch(config)#subnet-vlan ip-address 192.168.1.1 mask 255.255.255.0 vlan 300 priority 0
  ```
  
### 1.2.13 switchport mac-vlan enable

- 命令：`switchport mac-vlan enable
     no switchport mac-vlan enable`
- 功能：打开端口的 MAC-based VLAN 功能；本命令的no命令为关闭端口的 MAC-based VLAN 功能。
- 参数：无。 
- 命令模式：端口配置模式。
- 缺省情况：端口打开 MAC-based VLAN 功能。
- 使用指南：当添加MAC地址属于指定 VLAN 后，缺省全局使能 MAC-based VLAN 功能，此命令可将在指定端口上关闭 MAC-based VLAN 功能，以适应用户特定的应用。
- 举例：关闭端口1的 MAC-based VLAN 功能。

  ```text
  Switch#config 
  Switch(config)#interface ethernet 1/0/1 
  Switch(Config-If-Ethernet1/0/1)#no switchport mac-vlan enable  
  ```
  
### 1.2.14 switchport subnet-vlan enable

- 命令：`switchport subnet-vlan enable no switchport subnet-vlan enable`
- 功能：打开端口的IP-subnet-based VLAN功能；本命令的no命令为关闭端口的IP-subnet-based VLAN功能。
- 参数：无。 
- 命令模式：端口配置模式。
- 缺省情况：端口打开IP-subnet-based VLAN功能。
- 使用指南：当添加IP子网属于指定VLAN后，缺省全局使能IP-subnet-based VLAN功能，此命令可将在指定端口上关闭IP-subnet-based VLAN功能，以适应用户特定的应用。
- 举例：关闭端口1的IP-subnet-based VLAN功能。

  ```text
  Switch#config 
  Switch(config)#interface ethernet 1/0/1 
  Switch(Config-If-Ethernet1/0/1)#no switchport subnet-vlan enable
  ```
## 1.3 Voice VLAN配置命令

### 1.3.1 show voice-vlan

- 命令：`show voice-vlan `
- 功能：显示交换机 Voice VLAN 的配置情况。
- 参数：无。 
- 命令模式：特权和配置模式。
- 使用指南：显示交换机 Voice VLAN 的配置情况。
- 举例：显示当前 Voice VLAN 的配置情况。

  ```text
  switch#show voice-vlan 
  Voice VLAN ID:2
  Ports:ethernet1/0/1;ethernet1/0/3
  Voice name       MAC-Address           Mask  Priority 
  -------------- -----------------     ------- -------- 
  financePhone    00-e0-4c-77-ab-9d       0xff     5
  manager         00-0a-eb-26-8d-f3       0xfe     6 
  Mr_Lee          00-03-0f-11-22-33       0x80     5 
  NULL            00-03-0f-11-22-33       0x0      5
  ```

### 1.3.2 switchport voice-vlan enable

- 命令：`switchport voice-vlan enable no switchport voice-vlan enable`
- 功能：打开端口的 Voice VLAN 功能；本命令的 no 命令为关闭端口的 Voice VLAN 功能。
- 参数：无。
- 命令模式：端口配置模式。
- 缺省情况：端口打开 Voice VLAN 功能。
- 使用指南：当添加语音设备到 Voice VLAN 后，缺省全局使能 Voice VLAN 功能，此命令可在指定端口上关闭 Voice VLAN 功能，以适应用户特定的应用。
- 举例：关闭端口3的 Voice VLAN 功能。

  ```text
  switch#config 
  switch(config)#interface ethernet 1/0/3 
  switch(Config-If-Ethernet1/0/3)#no switchport voice-vlan enable
  ```
  
### 1.3.3 voice-vlan

- 命令：`voice-vlan mac <mac-address> mask <mac-mask> priority <priority-id> [name <voice-name>] no voice-vlan {mac <mac-address> mask <mac-mask>|name <voice-name> |all}` 
- 功能：指定某语音设备加入 Voice VLAN ；本命令的 no 命令为设备离开 Voice VLAN 。
- 参数：mac-address 为语音设备 MAC 地址，格式为”xx-xx-xx-xx-xx-xx”；mac-mask为MAC地址后8位的掩码，取值：0xff, 0xfe, 0xfc, 0xf8, 0xf0, 0xe0, 0xc0,0x80, 0x0；priority-id 为语音流的优先级，取值范围0~7；voice-name 为语音设备的名字，便于管理；all 为所有语音设备 MAC 地址。
- 命令模式：全局配置模式。缺省情况：没有语音设备加入 Voice VLAN。
- 使用指南：该命令将指定的语音设备加入到 Voice VLAN 中。若有指定语音设备的无 VLAN 标签数据包从交换机端口进入，不管该数据包从哪个端口进入，其均属于 Voice VLAN。该命令设置后不对有 VLAN 标签的数据包进行干涉。
- 举例：将研发部MAC地址为00-03-0f-11-22-00到00-03-0f-11-22-ff的256台语音设备加入 Voice VLAN。

  ```text
  switch#config
  switch(config)#voice-vlan vlan 100
  ```
  
### 1.3.4 voice-vlan vlan

- 命令：`voice-vlan vlan <vlan-id> no voice-vlan` 
- 功能：设置指定VLAN为 Voice VLAN ；本命令的no命令为取消该 VLAN 为 Voice VLAN。
- 参数：vlan-id 为指定的 VLAN 号。
- 命令模式：全局配置模式。
- 缺省情况：没有 Voice VLAN。
- 使用指南：设置指定 VLAN 为 Voice VLAN，同一时刻只能有一个 Voice VLAN 。Voice VLAN 与 MAC-based VLAN 不能同时使用。
- 举例：将 VLAN100 置为 Voice VLAN。

  ```text
  switch#config 
  switch(config)#voice-vlan vlan 100
  ```
  
## 1.4 Multi-to-One VLAN translation配置命令

### 1.4.1 vlan-translation n-to-1

- 命令：`vlan-translation n-to-1 <WORD> to <new-vlan-id> no vlan-translation n-to-1 <WORD>` 
- 功能：开启/关闭端口的 Multi-to-One VLAN translation 功能。
- 参数：WORD为映射前的 VLAN ID ，取值范围为1-4094，用‘;’和‘-’连接。如果在相同端口存在两个 VLAN 范围转化为不同的 VLAN ID，则要求这两个 VLAN 范围不能有重叠部分。 new-vlan-id 为映射后的 VLAN ID，取值范围为1-4094。
- 命令模式：端口配置模式缺省情况：端口默认没有开启该功能。
- 使用指南：Multi-to-One VLAN translation 功能应用在网络边缘层，用于将边缘网络的多个 VLAN 映射到
          主干网一个VLAN，同时当数据流从主干网络返回网络边缘层时，会自动还原数据流边缘层的 VLAN 信息，达到多对一的
          VLAN 映射效果，从而节省主干网络的 VLAN 资源。注意：使用该功能时，设备上必须先建立映射前与映射后的 VLAN ，
          同时开启该功能的下联端口和连接骨干网的上联端口必须以 Tagged 的方式加入到映射前与映射后的 VLAN ；该功能不能同
          时和 dot1q-tunnel，VLAN translation 功能同时使用。
- 注意：该功能要在开启 MAC 软件学习后使用。交换机的 access 口不支持此功能。 
- 举例：在端口Ethernet 1/0/1将 VLAN 范围为1-5的数据流转为 VLAN 100，并将从 VLAN 100 流出的属于 VLAN 范围1-5的数据流分别转换为自己对应的 VLAN ID ；连接骨干网的上联端口为 Ethernet 1/0/5。

  ```text 
  Switch(config)#vlan 1-5;100 
  Switch(config)#interface ethernet 1/0/1
  Switch(Config-If-Ethernet1/0/1)#switchport mode trunk 
  Switch(Config-If-Ethernet1/0/1)#vlan-translation n-to-1 1-5 to 100 
  Switch(config)#interface ethernet 1/0/5 
  Switch(Config-If-Ethernet1/0/5)#switchport mode trunk
  ```
  
### 1.4.2 show vlan-translation n-to-1

- 命令：`show vlan-translation n-to-1 [<interface-name>]` 
- 功能：显示开启该功能的端口配置。
- 参数：interface-name指定要显示的端口名称，如不指定该参数，则显示所有开启该功能的端口信息。
- 命令模式：特权配置模式缺省情况：缺省没有该功能信息。
- 使用指南：把GVRP报文调试信息开关打开。注意：交换机的access口不支持此功能。
- 举例：显示开启该功能的所有端口信息。

  ```text
  Switch# show vlan-translation n-to-1 
  Interface Ethernet1/0/1: 
  vlan-translation n-to-1 enable, vlan 1-4 to 100 
  vlan-translation n-to-1 enable,vlan 5-8;13 to 101 
  Interface Ethernet1/0/2: 
  vlan-translation n-to-1 enable,vlan 1-4 to 100
  ```
  
## 1.5 Super VLAN配置命令

### 1.5.1 supervlan

- 命令：`supervlan no supervlan`
- 功能：将 vlan 设置为 super vlan；本命令的 no 操作为恢复缺省情况。
- 参数：无。
- 命令模式：VLAN 配置模式。
- 缺省情况：不配置。
- 使用指南：将 VLAN 设置为 Super vlan ，必须是普通 VLAN ，此 VLAN 不能是 SUB VLAN，设置 truck 口时自动滤除 Super vlan，不能向 Super vlan 中加入端口。
- 举例：将 vlan 2 设置为 supervlan。 

  ```text
  Swithc#config
  Switch(config)#vlan 2 
  Switch (config-vlan2)#supervlan
  ```
  
### 1.5.2 subvlan

- 命令：`subvlan WORD no subvlan {WORD | all} `
- 功能：将 vlan 设置为 subvlan ；本命令的 no 操作为恢复缺省情况。
- 参数：WORD为 VLAN ID，可以使用“-”和“；”表示更多的 VLAN。
       all表示所有 subvlan。
- 命令模式：VLAN 配置模式。
- 缺省情况：不配置。
- 使用指南：将 VLAN 设置为 Sub vlan，该 VLAN 必须已经存在，必须是普通 VLAN，此 VLAN 不能是其他 SUPER VLAN 的 SUB VLAN，也不能是 SUPER VLAN，每一个 Super VLAN 可以和127个 Sub VLAN 建立映射关系。系统最多允许建立1024个 Super VLAN。
- 举例：将 vlan 3 设置为 subvlan。

  ```text
  Swithc#config 
  Switch(config)#vlan 2-3 
  Switch(config)#vlan 2 
  Switch (config-vlan2)#supervlan 
  Switch (config-vlan2)#subvlan 3
  ```
  
### 1.5.3 arp-proxy subvlan

- 命令：`arp-proxy subvlan {WORD | all} no arp-proxy subvlan {WORD | all}` 
- 功能：使能 subvlan 的 arp proxy 功能，此 VLAN 收到的流量可以被代理到其他 subvlan；本命令的 no 操作为恢复缺省情况。
- 参数：WORD 为 VLAN ID，可以使用“-”和“；”表示更多的 VLAN。 all 表示所有 subvlan。
- 命令模式：接口配置模式。
- 缺省情况：不配置。
- 使用指南：此接口必须是 supervlan 的接口。此 VLAN 收到的流量可以被代理到其他 subvlan --当交换机从这个 VLAN 收到 ARP REQUEST 时，用自己的 MAC 回应 ARP REPLY，以便流量可以通过交换机软转发出去。
- 举例：开启 vlan2 的所有 subvlan 的 arp-proxy 功能。 
 
  ```text
  Swithc#config 
  Switch(config)#interface vlan 2 
  Switch (config-if-vlan2)#arp-proxy subvlan all
  ```
  
### 1.5.4 ip-addr-range subvlan

- 命令：`ip-addr-range subvlan <vlan-id> <ipv4-addrss> to <ipv4-addrss> no ip-addr-range subvlan <vlan-id>`
- 功能：为某个 subvlan 指定地址范围，交换机收到流量后，发送 ARP REQUEST 时需要检查数据包的目的 IP 地址是否在地址范围内，如果不在，不发送 ARP REQUEST；本命令的 no 操作为恢复缺省情况。
- 参数：`<vlan-id>`为 VLAN ID，范围1-4094。`<ipv4-addrss>`表示IPv4地址，格式为点分十进制，每段取值范围0~255。
- 命令模式：接口配置模式。
- 缺省情况：无地址范围。
- 使用指南：交换机从配置了地址范围的 subvlan 收到流量后，发送 ARP REQUEST 时需要检查数据包的目的 IP 地址是否在地址范围内，如果不在，不发送 ARP REQUEST。
- 举例：设置 subvlan3 的地址范围。

  ```text
  Swithc#config 
  Switch(config)#interface vlan 2 
  Switch (config-if-vlan2)#ip-addr-range subvlan 3 1.1.1.1 to 1.1.1.10
  ```

### 1.5.5 ip-addr-range

- 命令：`ip-addr-range <ipv4-addrss> to <ipv4-addrss> no ip-addr-range` 
- 功能：为某个接口指定地址范围，交换机收到流量后，发送 ARP REQUEST 时需要检查数据包的目的 IP 地址是否在地址范围内，如果不在，不发送 ARP REQUEST；本命令的 no 操作为恢复缺省情况。
- 参数： `<ipv4-addrss>`表示IPv4地址，格式为点分十进制，每段取值范围0~255。
- 命令模式：接口配置模式。
- 缺省情况：无地址范围。
- 使用指南：交换机从配置了地址范围的接口收到流量后，发送 ARP REQUEST 时需要检查数据包的目的IP地址是否在地址范围内，如果不在，不发送 ARP REQUEST；如果这个接口是 SUPER VLAN 的接口，从这个接口收到的 ARP REQUEST，所请求的 IP 地址不在地址范围内，则不代理这个 ARP REQUEST。
- 举例：设置 interface vlan 2 的地址范围。 

  ```text
  Swithc#config 
  Switch(config)#interface vlan 2 
  Switch (config-if-vlan2)#ip-addr-range 1.1.1.1 to 1.1.1.10
  ```
  
### 1.5.6 show supervlan

- 命令：`show supervlan [<vlan-id>]` 
- 功能：显示系统的 super vlan。
- 参数：`<vlan-id>`为 VLAN ID ，范围1-4094。
- 命令模式：特权和配置模式。
- 使用指南：如果不指定 VLAN ID ，则显示所有的 supervlan 信息。
- 举例：显示当前 supervlan 信息。

```text
Swithc#show supervlan 
VLAN Name                  Type       sub VLAN Ports
---- -------- ---------- --------- ----------------------------------------
2 VLAN0002               Universal    3       Ethernet1/0/2 
                                      4       Ethernet1/0/3
```










   


  



 



     

  
