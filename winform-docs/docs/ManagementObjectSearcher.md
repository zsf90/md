我们在很多情况下想要获得计算机的硬件或操作系统的信息，比如获得CPU序列号给自己的软件添加机器码锁绑定指定电脑。又或者想要获得硬盘分区，声卡显卡等信息。

我们用到的主要类是ManagementObjectSearcher,该类在System.Management命名空间下。
有时候我们可以通过Environment获得一些简单的系统信息。
如：Environment.MachineName;获得计算机名。
Environment.UserName;获得操作系统登录用户名。
不过在这篇文章中主要讨论ManagementObjectSearcher获取计算机硬件及操作系统的信息。

## 用法步骤

1. 添加引用：System.Management
2. 引入命名空间：using System.Management;
3. 创建ManagementObjectSearcher对象
   anagementObjectSearcher searcher = new ManagementObjectSearcher("select * from " + Key);
   其中的key见下面key列表：
4. 通过searcher.Get()获得ManagementObjectCollection集合
5. 遍历ManagementObjectCollection集合获得ManagementObject
6. 通过managementObject[name]或ManagementObject.GetPropertyValue(name)获得想要的属性

## 例子

```csharp
private void getPortDeviceName()
{
    // 需要返回的结果：
    // 1. 即插即用设备ID
    // 2. 串口的端口 COM1..2..3等
    // 3. 判断即插即用设备ID来显示设备名称
    using (var searcher = new System.Management.ManagementObjectSearcher
    ("select * from Win32_SerialPort"))
    {
        var hardInfos = searcher.Get();
        foreach (var hardInfo in hardInfos)
        {
            string? DeviceID = hardInfo?.Properties["DeviceID"].Value.ToString();
            string? PNPDeviceID = hardInfo?.Properties["PNPDeviceID"].Value.ToString();
            string? DeviceName = hardInfo?.Properties["name"].Value.ToString();

            string? Caption = hardInfo?.Properties["Caption"].Value.ToString();

            textBox_send.Text += Caption;
        }
              
}
```
