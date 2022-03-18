https://xsenstechnologies.force.com/knowledgebase/s/article/Synchronization-with-the-MTi-1605869709711

**Synchronization with the MTi(与 MTi 同步)**

Nov 18, 2021•Knowledge Article

One of the best features of the MTi is the possibility to synchronize
with other devices and sensor systems. With 6 functions (a SendLatest, a
TriggerIndication, a SyncOut, a ClockSync, a StartSampling and a GNSS 1
PPS output), there is always a way to get other sensor systems aligned
with the MTi. Most of these functions can also be combined to achieve an
even more versatile functionality.

| MTi 的最佳功能之一是可以与其他设备和传感器系统同步。 借助 6
  个功能（SendLatest、TriggerIndication、SyncOut、ClockSync、StartSampling
  和 GNSS 1 PPS 输出），总有一种方法可以使其他传感器系统与 MTi
  保持一致。 这些功能中的大部分也可以组合起来以实现更加通用的功能。
| Not all functions are available for all MTi devices. See the following
  table for an overview of available synchronization options per
  device. 

| 并非所有功能都适用于所有 MTi 设备。
  有关每个设备可用同步选项的概述，请参阅下表。
|  

+-------------+-------------+-----------+-------------+-------------+
| **Sync      | **MTi       | **MTi-7** | **MTi       | **M         |
| function**  | 1-series**  |           | 10/100/     | Ti-670/680, |
|             |             |           | 600-series, | MTi-G-710** |
|             |             |           | incl.       |             |
|             |             |           | MTi-        |             |
|             |             |           | 670G/680G** |             |
+=============+=============+===========+=============+=============+
| SendLatest  | X           | X         | X           | X           |
+-------------+-------------+-----------+-------------+-------------+
| ClockSync   |             | X         | X           | X           |
| (Clock Bias |             |           |             |             |
| Estimation) |             |           |             |             |
+-------------+-------------+-----------+-------------+-------------+
| SyncOut     |             |           | X           | X           |
| (Interval   |             |           |             |             |
| Transition  |             |           |             |             |
| M           |             |           |             |             |
| easurement) |             |           |             |             |
+-------------+-------------+-----------+-------------+-------------+
| Trigge      |             |           | X           | X           |
| rIndication |             |           |             |             |
+-------------+-------------+-----------+-------------+-------------+
| St          |             |           | X           | X           |
| artSampling |             |           |             |             |
+-------------+-------------+-----------+-------------+-------------+
| GNSS 1 PPS  |             |  X        |             | X           |
+-------------+-------------+-----------+-------------+-------------+

 

In case multiple systems are used during a measurement it is important
to have the measurement data synchronized between the systems.
Processing synchronized data is much easier because there is no need to
resample the data to compensate for timing inaccuracies like clock drift
and clock deviations. Synchronization using multiple systems involves 2
important issues: starting the measurement at the same time and having a
fixed time relationship of the sampling instances. This section will
explain how the MTi must be setup when using multiple measurement
systems.

| 如果在测量期间使用多个系统，那么在系统之间同步测量数据很重要。
  处理同步数据要容易得多，因为无需重新采样数据以补偿时钟漂移和时钟偏差等时序误差。
  使用多个系统的同步涉及两个重要问题：同时开始测量和采样实例的固定时间关系。
  本节将解释在使用多个测量系统时必须如何设置 MTi。
|  

**External device triggers MTi: SendLatest(外部设备触发
MTi：SendLatest)**

SendLatest is used when an external device triggers the MTi.

| 当外部设备触发 MTi 时使用 SendLatest。
| In the following figure, a possible configuration is shown where a
  Motion Tracker and Device A are synchronised. In this example, a clock
  generator triggers device A and an MTi ensuring that the two devices
  are synchronized with each other.

在下图中，显示了一种可能的配置，其中MTi传感器和设备 A 同步。
在此示例中，时钟发生器触发设备 A 和 MTi，确保这两个设备彼此同步。

| |Diagram Description automatically generated|
| The output of the clock generator can be directly connected to the
  MTi.

| 时钟发生器的输出可以直接连接到 MTi。
| **NOTE:** Always check if the SyncIn specification matches with the
  trigger signal.

**注意**\ ：始终检查 SyncIn 参数是否与触发信号匹配。

Once a SyncIn signal or a ReqData message is received, the MTi will
output the latest available data. It is possible to delay the data to be
sent, to choose whether the SyncIn signal needs to be triggered on
rising edge or falling edge etc. The internal clock determines when data
is available. This data is transmitted only if a trigger is detected on
the SyncIn line or when polled (ReqData software trigger). This means
that the trigger instance will not coincide with the availability of the
data. Because two different clocks are used the time difference between
the trigger instance and the last sampling instance may vary during the
measurement and at most with a time equal to the used sampling period.

一旦接收到 SyncIn 信号或 ReqData 消息，MTi
将输出最新的可用数据。可以延迟要发送的数据，选择是否需要在上升沿或下降沿触发
SyncIn 信号等。内部时钟决定数据何时可用。仅当在 SyncIn
线路上检测到触发或轮询（ReqData
软件触发）时才会传输此数据。这意味着触发器实例将与数据的可用性不一致。因为使用了两个不同的时钟，所以在测量期间触发实例和最后一个采样实例之间的时间差可能会发生变化，并且最多与所使用的采样周期相等。

SendLatest does not apply to AccelerationHR and RateOfTurnHR data
output. In case of PVT data, SendLatest will send the last received PVT
data only once. Subsequent triggers will not output any PVT data until
new PVT data is available. The SendLatest functionality is not available
when using the CAN interface.

| SendLatest 不适用于 AccelerationHR 和 RateOfTurnHR 数据输出。如果是
  PVT 数据，SendLatest 将只发送最后接收到的 PVT 数据一次。在新的 PVT
  数据可用之前，后续触发器不会输出任何 PVT 数据。使用 CAN
  接口时，SendLatest 功能不可用。
|  

**Marker in MT Data: TriggerIndication (MT
Data中的标记：TriggerIndication)**

Next to let the MTi send data to the computer, it is also possible to
incorporate a trigger indication in the MTData2 packet (Status Word).
The data will not be affected by the trigger indication; the data is
marked with the pulse received. With an MTi 600-series device, the user
can configure the MTi to output also a TriggerIndication message through
the MtData2 stream. The advantage is that this message is timestamped
with the trigger moment, so it has better accuracy than just the status
flag.

| 接下来让 MTi 向计算机发送数据，还可以在 MTData2
  数据包（状态字）中加入触发指示。 数据不会受到触发指示的影响；
  数据用接收到的脉冲标记。 对于 MTi 600 系列设备，用户可以配置 MTi
  以通过 MtData2 流输出 TriggerIndication 消息。
  优点是该消息带有触发时刻的时间戳，因此它比仅使用状态标志具有更好的准确性。
| The TriggerIndication functionality is not available when using the
  CAN interface.

| 使用 CAN 接口时，TriggerIndication 功能不可用。
|  

**MTi triggers external device: SyncOut (MTi 触发外部设备：SyncOut)**

The SyncOut (called Interval Transition Measurement as the SyncOut is
generated on the transition between two 400 Hz intervals of the SDI) is
used to trigger other devices.

| SyncOut（称为间隔转换测量，因为 SyncOut 是在捷联积分算法( SDI) 的两个
  400 Hz 间隔之间的转换时生成）用于触发其他设备。
| In case the clock specification of the MTi is accurate enough for the
  measurement, the MTi can provide a sync pulse which is generated based
  on its internal clock at a frequency of 400 Hz, regardless of the
  frequency of the data outputted. For example, when Interval Transition
  Measurement is set with a skip factor of 3 and a pulse width of 1000
  µs, the following will be outputted: 1 ms sync pulse, 9 ms no sync
  pulse, 1 ms sync pulse, 9 ms no sync pulse and so on. Three pulses
  will be skipped after every pulse, resulting in a 100 Hz output
  signal. The data output (e.g. orientation) and frequency is irrelevant
  for the functionality of Interval Transition Measurement.

如果 MTi 的时钟规格对于测量来说足够准确，则 MTi 可以提供基于其内部时钟以
400 Hz 频率（2.5ms）生成的同步脉冲，而不管实际选择的输出数据的频率如何。
例如，当 Interval Transition Measurement
（间隔转换测量）设置为跳跃因子为 3 且脉冲宽度为 1000 µs
时，将输出以下内容：1 ms 同步脉冲、9 ms 无同步脉冲、1 ms 同步脉冲、9 ms
无同步脉冲 等等。 每个脉冲后将跳过三个脉冲，从而产生 100 Hz 的输出信号。
数据输出（例如方向）和频率与间隔转换测量的功能无关。

.. image:: vertopal_62a9d09047c248febcc3f1e34f6f8dca/media/image2.png
   :alt: Shape Description automatically generated
   :width: 6.3in
   :height: 2.90625in

上图显示了使用1ms脉冲宽度，跳跃因子为3的示意

A SyncOut marker is outputted in the data stream that shows the exact
time of the transmission of the SyncOut pulse. The signal can be set to
either pulse or toggle mode and in case of pulse mode the polarity can
be set to negative or positive. The Low Level Communication Protocol
(`Xsens MTi
Documentation <https://www.xsens.com/xsens-mti-documentation>`__)
describes the different settings. 

| SyncOut 标记在数据流中输出，显示 SyncOut 脉冲传输的准确时间。
  信号可以设置为脉冲或切换模式，在脉冲模式的情况下，极性可以设置为负或正。
  低级通信协议（Xsens MTi 文档）描述了不同的设置。
| To connect the SyncOut signal to an external device you can either
  make a custom cable that wires the SyncOut pin (see the User
  Manual `Xsens MTi
  Documentation <https://www.xsens.com/xsens-mti-documentation>`__ for
  pin configurations) directly from the MTi/MTi-OEM or in case you use a
  multi-purpose cable you can connect directly to the appropriate pin of
  the termination header.

要将 SyncOut 信号连接到外部设备，您可以直接从 MTi/MTi-OEM
制作一根定制电缆来连接 SyncOut 引脚（有关引脚配置，请参阅用户手册 Xsens
MTi 文档），或者如果您使用多用途
电缆，您可以直接连接到终端接头的相应引脚。

.. image:: vertopal_62a9d09047c248febcc3f1e34f6f8dca/media/image3.png
   :alt: A picture containing text, electronics, kitchen appliance
   Description automatically generated
   :width: 3.42222in
   :height: 1.61616in

Always check if the input voltage levels and the input impedance of the
external device matches the SyncOut specifications.

| 始终检查输入电压电平和外部设备的输入阻抗是否符合 SyncOut 规格。
|  

**1 PPS output directly from GNSS receiver (1 PPS 直接从 GNSS
接收器输出)**

Another possibility on the SyncOut line is the 1 PPS signal, that is
coming directly from the GNSS receiver. This 1 PPS pulse has a duration
of 100 us and is outputted at exactly the integer second (1.00000,
2.00000, etc.) with an accuracy of 30 ns. This functionality is only
available on GNSS-enabled MTi devices. The MTi-680(G) does not support a
true 1 PPS signal as described above, however it can generate its own 1
PPS pulse using the Interval Transition Measurement function. This pulse
is then synchronized with the 1 PPS pulse of the internal GNSS receiver,
but it does not appear exactly at the integer second.

| SyncOut 线上的另一种可能性是 1 PPS 信号，它直接来自 GNSS 接收器。 这个
  1 PPS 脉冲的持续时间为 100 us，并以 30 ns
  的精度精确输出整数秒（1.00000、2.00000 等）。 此功能仅适用于支持 GNSS
  的 MTi 设备。 MTi-680(G) 不支持上述真正的 1 PPS
  信号，但它可以使用间隔转换测量功能生成自己的 1 PPS 脉冲。
  该脉冲随后与内部 GNSS 接收器的 1 PPS
  脉冲同步，但它不会准确出现在整数秒处。
|  

**Synchronizing two clocks: ClockSync（同步两个时钟：ClockSync）**

The MTi features clock synchronization: it is possible to adjust the
bias of the MTi’s internal clock with an external clock of which the
frequency is known. Note that the adjusted bias is also used in the
calibration of the inertial sensors, so that no additional errors are
introduced. When a pulse is missed, e.g. because it was not sent or was
lost on the input line, this will not have a bad influence on the
performance. The maximum time that the pulses may be absent is 30
seconds.

| MTi 具有时钟同步功能：可以使用频率已知的外部时钟来调整 MTi
  内部时钟的偏差。
  请注意，调整后的偏差也用于校准惯性传感器，因此不会引入额外的误差。
  当错过一个脉冲时，例如
  因为它没有发送或丢失在输入线上，这不会对性能产生不良影响。
  脉冲可能不存在的最长时间为 30 秒。
| The clock synchronization can be used for two distinctive use cases:

时钟同步可用于两个不同的用例：

-  When a precise external clock is available (e.g. a GPS time pulse),
   this frequency can be sent to the MTi to make sure that the time of
   the MTi follows the UTC time.

-  当精确的外部时钟可用（例如 GPS 时间脉冲）时，可以将此频率发送到 MTi
   以确保 MTi 的时间遵循 UTC 时间。

-  When an external device has a time constant that differs from the
   MTi, the sensor readings will at some point no longer be aligned to
   each other. If the external device accepts synchronization pulses, it
   is possible to use SyncOut; if the external device can send
   synchronization pulses at a frequency that is the same as the
   required output frequency of the MTi, it is possible to use SyncIn.
   If these two options are not possible, the Clock Sync is an
   alternative.

-  当外部设备的时间常数与 MTi
   不同时，传感器读数在某些时候将不再相互对齐。
   如果外部设备接受同步脉冲，则可以使用SyncOut； 如果外部设备可以以与
   MTi 所需输出频率相同的频率发送同步脉冲，则可以使用 SyncIn。
   如果这两个选项都不可能，则时钟同步是一种替代方法。

 

+---------------------------------------------------+-----------------+
| **Specification**                                 | **Value**       |
+===================================================+=================+
| MTi’s internal clock accuracy                     | 10 ppm          |
|                                                   |                 |
| MTi 的内部时钟精度                                |                 |
+---------------------------------------------------+-----------------+
| Input frequency输入频率                           | 0.1 – 1000 Hz\* |
+---------------------------------------------------+-----------------+
| Maximum deviation from MTi’s internal clock       | 900 ppm         |
|                                                   |                 |
| 与 MTi 内部时钟的最大偏差                         |                 |
+---------------------------------------------------+-----------------+
| Initialisation time (per ppm difference between   | 0.72 ms/ppm     |
| internal clock and external clock)                |                 |
|                                                   |                 |
| 初始化时间（内部时钟和外部时钟之间的每 ppm 差异） |                 |
+---------------------------------------------------+-----------------+

Once ClockSync is active, its corresponding bit in the Status Word
output of the MTi will be raised. Refer to the `MT Low-Level
Communication Protocol
Document <http://xsens.com/xsens-mti-documentation>`__ for more details.

| 一旦 ClockSync 被激活，它在 MTi 的状态字输出中的相应位将被升高。
  有关更多详细信息，请参阅 MT 低级通信协议文档。
| The MTi-7, MTi-670(G), MTi-680(G) and MTi-G-710 use the clock bias
  estimation function to synchronize the MTi with the GPS time (1 ppm).
  This synchronization is set by default, and although not recommended,
  it is possible to disable this synchronization setting. 

| MTi-7、MTi-670(G)、MTi-680(G) 和 MTi-G-710 使用时钟偏差估计功能将 MTi
  与 GPS 时间 (1 ppm) 同步。
  默认情况下会设置此同步，尽管不推荐，但可以禁用此同步设置。
| \*Please note that not all reference clock frequencies are supported
  by the ClockSync functionality. This is because the reference
  clock *period* needs to be configured in integer milliseconds. For
  example, a reference clock frequency of 100 Hz (period = 10 ms) is
  supported, but 60 Hz (period ~ 16.67 ms) is not supported. The
  ClockSync functionality can cope with small deviations\ * (*-0.5%)
  between the configured and actual reference clock periods, but a 60 Hz
  reference clock signal will exceed this when a 17 ms clock period (f ~
  58.8 Hz) is configured.

| \*请注意，并非所有参考时钟频率都受 ClockSync 功能支持。
  这是因为需要以整数毫秒为单位配置参考时钟周期。 例如，支持 100 Hz（周期
  = 10 ms）的参考时钟频率，但不支持 60 Hz（周期 ~ 16.67 ms）。 ClockSync
  功能可以应对配置的参考时钟周期和实际参考时钟周期之间的小偏差
  (-0.5%)，但是当配置 17 ms 时钟周期 (f ~ 58.8 Hz) 时，60 Hz
  参考时钟信号将超过此值。
|  

**StartSampling开始采样**

One of the advanced timing features of the MTi is the StartSampling
synchronization function. StartSampling will trigger the MTi to start
processing data, so that the start time for sampling can be chosen. This
is useful when the timing of a data needs to be aligned with an external
sensor or sensor system at an accuracy of better than 2.5 ms. Timing
specification is as following:

| MTi 的高级计时功能之一是 StartSampling 同步功能。 StartSampling 将触发
  MTi 开始处理数据，从而可以选择采样的开始时间。 当数据的时序需要以优于
  2.5 ms 的精度与外部传感器或传感器系统对齐时，这非常有用。
  时序规范如下：
|  

+----------------+----------------+----------------+----------------+
| 0 ms           | 0.69 +/- 0.05  | 3.19 +/-0.05   | 10.69 +/- 0.05 |
|                | ms             | ms             | ms             |
+================+================+================+================+
| External pulse | First sample   | First inertial | First          |
| received at    | (10kHz)        | data available | orientation    |
| MTi            | received for   | (acc/gyr, 400  | available (400 |
|                | signal         | Hz)            | Hz)            |
| 在 MTi         | processing     |                |                |
| 接             |                | 第一个         | 第一个         |
| 收到的外部脉冲 | 接收           | 可用的惯性数据 | 可用的姿态数据 |
|                | 到的第一个样本 | （acc/gyr，400 | (400 Hz)       |
|                | (10kHz)        | Hz）           |                |
|                | 用于信号处理   |                |                |
+----------------+----------------+----------------+----------------+

*Note: The specifications in this table apply to the MTi 10/100-series
only.*

| *注意：此表中的规格仅适用于 MTi 10/100 系列。*
| It is possible to delay the “First sample received”, and with that the
  entire data output, with up to 0.65536 seconds. For example, setting a
  delay of 6810 us (6.81 ms) will output data at exactly 10 ms after the
  external pulse has been received.  
| 可以延迟“接收到的第一个样本”以及整个数据输出，最长可达 0.65536 秒。
  例如，设置 6810 us (6.81 ms) 的延迟将在接收到外部脉冲后 10 ms
  时输出数据。
|  

**Combining sync functions结合同步功能**

It is possible to configure multiple synchronization functions on the
MTi. This can be useful if you need to synchronize multiple devices,
e.g. a GPS device (providing a 1 pulse per second (PPS) pulse), an
MTi-300 and an external camera that needs 0.2 seconds to make a
picture. 

可以在 MTi 上配置多个同步功能。 如果您需要同步多个设备，这会很有用，例如
一个 GPS 设备（提供每秒 1 个脉冲 (PPS) 脉冲）、一个 MTi-300 和一个需要
0.2 秒来拍摄照片的外部相机。

 |image1|

In this example, you could use the GPS pulse to synchronize the clock of
the MTi with the GPS clock (use Clock Bias Estimation), but you also
need to know the timing difference between the GPS and MTi (so connect
the 1 PPS to Trigger Indication as well: the 1 PPS trigger will be
inside the MT Data2 packet). If you need orientation at a different rate
than the camera images, you can send the Interval Transition Measurement
(SyncOut) at a SkipFactor and with an offset to give the camera time to
make the picture.

| 在本例中，您可以使用 GPS 脉冲将 MTi 的时钟与 GPS 时钟同步（使用 Clock
  Bias Estimation），但您还需要知道 GPS 和 MTi 之间的时间差（因此将 1
  PPS 连接到 Trigger 也有指示：1 PPS 触发器将在 MT Data2 数据包内）。
  如果您需要以与相机图像不同的速率进行定向，您可以以 SkipFactor
  和偏移量发送间隔转换测量 (SyncOut)，以便让相机有时间制作图片。
| A list of possible sync combinations is shown below. There are many
  more use cases, and Xsens can advise you on this.

| 可能的同步组合列表如下所示。 还有更多用例，Xsens
  可以在这方面为您提供建议。
|  

+----------------------------------+----------------------------------+
| **Example use case示例用例**     | **Functions to combine           |
|                                  | 要组合的功能**                   |
+==================================+==================================+
| Two separate sensor systems that | `ClockSync and                   |
| need to run at the same          | StartSampli                      |
| frequency with their own clocks  | ng <https://base.xsens.com/knowl |
| and where no bias between the    | edgebase/s/article/ClockSync-and |
| devices is allowed               | -StartSampling-1605869706645>`__ |
|                                  |                                  |
| 两个独立的传感器系               |                                  |
| 统需要使用自己的时钟以相同的频率 |                                  |
| 运行，并且设备之间不允许存在偏差 |                                  |
+----------------------------------+----------------------------------+
| Two separate systems of which    | ClockSync and TriggerIndication  |
| the clocks need to run at the    |                                  |
| same frequency and where a       |                                  |
| manual trigger for the other     |                                  |
| system must be shown in the MTi  |                                  |
| data                             |                                  |
|                                  |                                  |
| 两个独立的系统，它们的时钟       |                                  |
| 需要以相同的频率运行，并且必须在 |                                  |
| MTi                              |                                  |
| 数据中显示另一个系统的手动触发   |                                  |
+----------------------------------+----------------------------------+
| A system where the data has to   | StartSampling, ClockSync and     |
| be available at exactly the      | SendLatest (via ReqData message, |
| round second, and where the 3rd  | as there are only 2 SyncIn).     |
| party devices polls the MTi for  |                                  |
| the latest data                  | StartSampling、ClockSync 和      |
|                                  | SendLatest（通过 ReqData         |
| 一个系统，其中                   | 消息，因为只有 2 个 SyncIn）。   |
| 数据必须恰好在第二轮可用，并且第 |                                  |
| 3 方设备轮询 MTi 以获取最新数据  |                                  |
+----------------------------------+----------------------------------+
| A system where device A can only | ClockSync (from Device A) and    |
| generate a 1 Hz pulse, but where | Interval Transition Measurement  |
| device B requires a 10 Hz pulse  | with skip factor 39 (to Device   |
|                                  | B)                               |
| 设备 A 只能生成 1 Hz             |                                  |
| 脉冲，但设备 B 需要 10 Hz        | ClockSync（从设备 A）和Interval  |
| 脉冲的系统                       | Transition                       |
|                                  | Measure                          |
|                                  | ment（间隔转换测量），跳跃因子为 |
|                                  | 39（到设备 B）                   |
+----------------------------------+----------------------------------+

**
Common ground for Sync applications同步应用设备的共同接地**

Just as with the communication interface, it is required to have a
common ground. This means that the ground of the MTi must be connected
to the ground of the serial interface (USB or serial), the power source
(USB or external) and the sync/clock. When this common ground is
neglected, the Sync interface may become irreparably damaged or the
communication may not start up. See the schematic below.

| 就像通信接口一样，需要有一个共同接地点。 这意味着 MTi
  的地必须连接到串行接口（USB 或串行）、电源（USB
  或外部）和同步/时钟的地。
  如果忽略这个共同接地，同步接口可能会受到不可修复的损坏或通信可能无法启动。
  请参阅下面的示意图。
|  

|Diagram, schematic Description automatically generated| 

电源、同步、主机（数据接口）和 MTi 的共同接地

.. |Diagram Description automatically generated| image:: vertopal_62a9d09047c248febcc3f1e34f6f8dca/media/image1.png
   :width: 3.65in
   :height: 2.1719in
.. |image1| image:: vertopal_62a9d09047c248febcc3f1e34f6f8dca/media/image4.png
   :width: 6.3in
   :height: 4.03958in
.. |Diagram, schematic Description automatically generated| image:: vertopal_62a9d09047c248febcc3f1e34f6f8dca/media/image5.png
   :width: 4.61667in
   :height: 4.63333in
