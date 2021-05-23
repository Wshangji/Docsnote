> 单片机实质上是一个微型计算机，它是计算机的缩小版。拥有芯片、CPU、随机存储器RAM、只读存储器ROM、多个IO口中断系统、定时器等,主要也是二进制数。

## 单片机的基本组成

> - CPU（中央处理器）
> - RAM（随机存储器）
> - ROM（只读存储器）
> - I/O接口
> - 内部功能组件

### 单片机内部数据传输

单片机内部数据传输通过总线完成，输入数据时会经过三态门，三态门挂在总线上，当需要作为输入时，保持为低电位，输出时为高电位。51单片机内部有16根地址总线，总线与单片机的寻址能力有关，其对应关系为：单片机能够寻找到的地址=2的总线个数的次方

总线（BUS）是计算机各部件之间传送信息的公共通道，单片机中有`内部总线`和`外部总线`两类

> 内部总线：CPU之间的连线
>
> 外部总线：CPU与其它部件之间的连线。包括：
> - 数据总线DB（Data Bus）
> - 地址总线 AB（Address Bus）
> - 控制总线CB（Control  Bus）

### 寄存器

寄存器是有限存贮容量的高速存贮部件，它们可用来暂存指令、数据和地址。

寄存器由触发器构成，缓冲寄存器用于临时存储数据，以输入到RAM或I/O口，例如SBUF。

### CPU系统

CPU：它是按照面向测控对象、嵌入式应用和单芯片结构要求，专门设计的要保证有突出的控制功能。

时钟系统：满足CPU及片内各单元电路对时钟的要求。

复位电路：能满足上电复位、信号复位的最简化电路。

总线控制逻辑：要满足CPU对内部总线和外部总线的控制。

内部总线控制用以实现片内各单元电路的协调操作；外部总线控制用于微控制器外围扩展时的操作管理。

### 输入输出接口I/O

用于连接外部的输入/输出设备、测量与控制对象。

### 特殊功能寄存器SFR

SFR是管理与控制微控制器内部I/O端口、基本功能单元、扩展功能单元运行的寄存器。

通过对SFR的编程，实现的方式设置、启动运行和状态读取等。

### 内部总线

微控制器内部CPU与各功能模块之间传送信息的公共通信通道。分为数据总线DB（Data Bus）、地址总线AB（Address Bus）和控制总线CB（Control Bus）

数据总线：双向，用于传送数据，实现CPU与存贮器、I/O接口、各功能模块之间的信息交换，其方向取决于是读操作还是写操作。

地址总线：单向，由CPU发出地址信息，用来访问存贮器和I/O接口。

控制总线：单向，传送控制或时序信号。如读/写信号、中断申请信号、复位信号等。每个信号都有自己的功能，控制着微控制器有序的运用工作。

## 单片机内部结构

![](https://img-blog.csdnimg.cn/20200307194909899.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

### 各引脚定义

![](https://img-blog.csdnimg.cn/20200307195023825.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

> 组成：
>
> - I/O引脚：4组，每组8个
> - 控制引脚：4个
> - 时钟引脚：2个
> - 电源引脚：2个

> 功能：
>
> - P1口为标准I/O口，无特殊功能
> - P0口可作为准I/O口使用，但必须外接上拉电阻；在外接存储器时也可复用为低8位地址总线（AB）和双向数据总线（DB）
> - P2口可作为准I/O口使用，外接存储器时可作为高8位地址总线（AB）
> - P3每一个引脚都有第二功能，既能单独作标准I/O口或第二功能使用，也可混合使用，即将其中某几个作为I/O口，其他作为第二功能口
> - EA引脚用作外接存储器功能的开闭，当该引脚悬空或接高电平时使用内部存储器，接低电平时只能访问外部存储器
> - RST为复位引脚，当给该引脚持续两个周期的高电平时即可复位

### 并行I/O口的结构

**以下每个图中展示的仅仅是每组I/O口中的一个**

#### P1端口的内部结构

![](https://img-blog.csdnimg.cn/20200307202140185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

P1口只具有标准I/O口功能，因此先对其进行分析，当需要输出数据时，控制三态门2为高电位，三态门1为低电位。内部总线将数据发送到锁存器的D端，锁存器正常工作（该锁存器其实为一D型触发器，当时钟脉冲输入到CP时，Q的次态等于上一态的D端），若输出为1，流程大致如下，首先D端为1，使得Q端为1，Q非端为0，使得后方的场效应管截至，由VCC到GND的电路不导通，使得出口的电压为高电平，因此输出1，若输出0，则Q非端为1，场效应管导通，出口电压被拉低，使得输出为0。当需要输入数据时，则控制三态门1打开，但是如果在进行输入操作之前，曾经输出过0，会使得锁存器Q非端为1，场效应管导通，导致端口一直会被拉低，导致无法正确输入数据，因此在进行输入操作之前，应先输出1

#### P2端口内部结构

![](https://img-blog.csdnimg.cn/20200307203837461.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

相比于P1端口，P2端口多了一个控制线用于控制多路开关MUX进行功能的选择，当多路开关置于1端时作普通I/O使用，置于地址端时，则用于第二功能。分析方法同P1

#### P3端口内部结构

![](https://img-blog.csdnimg.cn/20200307204342325.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

相比于P1端口，P3端口多了一个与非门，当作为普通I/O使用时，第二功能引脚保持高电平，当要用于第二功能输出时，使锁存器Q端置1（也就是输出1，其他端口在进行输入时都需要先输出1），输出数据由第二功能端决定。若用于第二功能输入时，则通过专用第二输入端口进行输入

#### P0端内部结构

![](https://img-blog.csdnimg.cn/20200307205226198.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

P1端口分析方法如前面所述一样，只是该端口内部未接上拉电阻，作为通用I/O口时，需要先外接上拉电阻

### 单片机存储器结构

#### 片内存储

![](https://img-blog.csdnimg.cn/20200307205739412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

**可细分为**

![](https://img-blog.csdnimg.cn/20200307210047909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1NTI1MTQx,size_16,color_FFFFFF,t_70)

左边为低128位地址，其中00H到1FH为工作寄存器区，分为4组，每一组又可分为R0到R7八个地址，在程序调用时，例如 MOV A，R0，但无法确定R0是这四组工作寄存区那一组，其实在PSW特殊功能寄存器中的第3和第4位中的数据来决定，若为01，则为第一组，其余则以此类推（任一时刻，只能使用一个寄存器组；CPU复位后，默认选择第0组）。20H到2FH为位寻址区，即这16个地址中的每一个地址中的每一位布尔数都可以直接改变其值，例如20.3H就表示为20H地址中的第4个数。30H到7FH为数据缓冲区，基本都可以随便使用。右边为高128位地址区，除了其中18个特殊功能寄存器的21个地址可以直接访问外，其余都不行。当特殊功能寄存器的地址为0或8结尾（即能被8整除），则该特殊功能寄存器可以进行位寻址操作

