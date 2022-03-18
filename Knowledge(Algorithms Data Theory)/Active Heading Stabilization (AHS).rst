

.. _doc_android_plugin:

`Active Heading Stabilization (AHS) 主动航向稳定 (AHS) <https://xsenstechnologies.force.com/knowledgebase/s/article/Active-Heading-Stabilization-AHS-1605869706072>`__
========================

Jul 13, 2021•Knowledge Article


免责声明：AHS 仍处于 GNSS/INS 设备的测试阶段。 尽管该功能可用，但并未针对这些设备进行设计或广泛测试。

主动航向稳定 (AHS) 是传感器融合引擎中的一个软件组件，旨在即使在受干扰的磁环境中也能提供低漂移的非参考（非北参考）偏航解决方案。 它旨在解决不随传感器移动的磁畸变，即临时或空间畸变。 磁范数(magnetic norm)可用于识别这些磁畸变。

AHS 适用于所有产品和滤波器配置文件。 因此，即使使用 VRU 产品，用户也可以获得稳定的偏航解决方案。 当 AHS 应用于使用磁场作为参考的滤波器配置文件时，磁场将不再用作参考。 偏航输出将参考启动航向方向，而不是北参考。 在传感器初始化时，偏航估计将因此为 0 度。 启用 AHS 后，偏航漂移可低至每小时 1-3 度。 然而，这取决于应用场景的类型。 AHS 最适合偶尔静止的应用，例如仓库机器人和其他地面车辆。

当应用场景预计 MTi 或磁场旋转非常慢时，建议不要启用 AHS。
 
如何启用 AHS
------------
在最新一代的 MTi 设备（例如 MTi 600 系列）中，AHS 可以作为滤波器配置文件“VRU-AHS”启用。

在早期的 MTi 设备（例如 MTi 10/100 系列）中，可以通过设置一个标志来启用 AHS：

- **Low Level Communication低级通信**
在低级通信中配置设备。 请参阅 MT 低级通信协议文档中的 SetOptionFlags 部分。

- **XDA**	
使用 setDeviceOptionFlags 函数。 有关更多信息，请参阅 XDA 库（位于您的 MT SDK 文档文件夹中）。

- **MT Manager**

  - 打开Device Settings窗口 -> Device Settings。
  - 勾选“Active Heading Stabilization (AHS)”。
  - 写入设备。
  .. image:: image/ahs_settings.jpg
  此方法不适用于 GNSS/INS 设备。
  

 
