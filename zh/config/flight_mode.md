# 飞行模式有关配置

This topic explains how to map [flight modes](../getting_started/flight_modes.md) and other functions to the switches on your radio control transmitter.

:::tip
You must already have [configured your radio](../config/radio.md) in order to assign flight modes and functions.
:::

:::note PX4 allows you to select flight modes from a ground station (tablet or desktop) or from a radio control transmitter. If radio control and tablet are both connected, either system can change the mode and override the previous setting.
:::

## What Flight Modes and Switches Should I Set?

*Flight Modes* provide different types of *autopilot-assisted flight*, and *fully autonomous flight*. You can set any (or none) of the flight modes [described here](../getting_started/flight_modes.md). Most users should set the following functions, as these make the vehicle easier and safer to fly:

* **Position mode** ([multicopter](../getting_started/flight_modes.md#position-mode-mc), [fixed-wing](../getting_started/flight_modes.md#position-mode-fw)) - Easiest and safest mode for manual flight.
* [Return mode](../flight_modes/return.md) - Return to launch position by safe path and land (by default).
* **VTOL Transition Switch** - Toggle between fixed-wing and multicopter flight configuration on VTOL vehicles.

It is also common to map switches to:

* [Mission](../flight_modes/mission.md) - This mode runs a pre-programmed mission sent by the ground control station.
* <span id="kill_switch"></span> [Kill Switch](../config/safety.md#kill_switch) - Immediately stops all motor outputs (the vehicle will crash, which may in some circumstances be more desirable than allowing it to continue flying).

## 多通道飞行模式选择 vs 单通道飞行模式选择

*PX4* (*QGroundControl*) supports two modes for mapping flight modes to transmitter switches/dials:

* **单通道模式选择：** 单通道编码模式下最多分配 6 个飞行模式到遥控器的不同开关位置上。
* **多通道模式选择：** 把不同飞行模式分配到不同的遥控器开关位置，来使用一个或者多个通道的编码。 一些模式被硬编码以共享通道，或者是根据其他选择的模式被自动指定（多通道模式选择的方式很多时候会导致困惑）。 

:::tip
The recommended approach is use *Single Channel Mode Selection* because it easy to understand and configure.
:::

<span id="single_channel"></span>

## 单通道飞行模式选择

The single-channel selection mode allows you to specify a "mode" channel and select up to 6 flight modes that will be activated based on the PWM value of the channel. You can also separately specify channels for mapping a kill switch, return to launch mode, and offboard mode.

:::note
In order to use this approach you will first need to configure your *transmitter* to encode the physical positions of your mode switch(es) into a single channel. We provide a video guide of how this is done for the popular *Taranis* transmitter [below](#taranis_setup) (check your documentation if you use a different transmitter).
:::

To configure single-channel flight mode selection:

1. 打开 *QGroundControl* 并连接上飞机。
2. 打开您的 RC 遥控器发射机。
3. 点击上方工具栏的 **Gear** 图标（飞行器设置），然后在左侧边栏选择 **Flight Modes** 。
    
    ![Flight modes single-channel](../../assets/qgc/setup/flight_modes/flight_modes_single_channel.jpg)
    
:::tip
If the screen opens in *Multi Channel Mode* click the **Use Single Channel Mode Selection** button to change screen.
:::

4. 进行 *飞行模式设置* :
    
    * 选择 **Mode channel** （上面显示的是 Channel 5 ，但是这是取决于你遥控器的设置的）。 
    * 最多可以选择 6 个 **飞行模式** 。
5. 进行 *遥控器开关设置* ： 
    * 选择你想要映射的特点飞行模式的通道 - 例如：*自动返航*，*Kill switch*，*机外控制（offboard）* 等模式。 （如果你的遥控器发射机上有空闲的开关或者通道的话）。
6. 测试模式是否映射到正确的发射器开关： 
    * 检查 *Channel Monitor* 以确认改变每个开关可以改变预期的通道。
    * 拨动你遥控器上刚刚映射的飞行模式有关的开关，并检查对应的飞行模式已被激活（ *QGroundeControl* 上对应的通道的字体在被激活的情况下变为黄色 ）

All values are automatically saved as they are changed.

<span id="taranis_setup"></span>

### 单通道模式配置的视频演示（包括遥控器相关设置）

It is common to use the positions of a 2- and a 3-position switch on the transmitter to represent the 6 flight modes, and encode each combination of switches as a particular PWM value for the mode that will be sent on a single channel.

The video below shows how this is done with the *FrSky Taranis* transmitter (a very popular and highly recommended RC transmitter). The process involves assigning a "logical switch" to each combination of positions of the two real switches. Each logical switch is then assigned to a different PWM value on the same channel.

The video then shows how to use *QGroundControl* to specify the mode channel and map modes to each of the 6 "slots".

@[youtube](https://youtu.be/scqO7vbH2jo)

### 单通道模式设置示例

This example shows how you can configure a transmitter and PX4 with:

* 一个在单通道模式下用于选择飞行模式（手动，高度，特技）的三段开关。
* A 2-way switch that invokes some function (arm/disarm) (via a [Radio switch](../advanced_config/parameter_reference.md#radio-switches) parameter).

:::note
This example shows how to set up the popular *FrSky Taranis* transmitter. Configuration will be slightly different for other transmitters.
:::

First set up your transmitter. Below we show how to map the Taranis "SD" switch to channel 5. This is done in the Taranis UI 'mixer' page, as shown below:

![Taranis - Map channel to switch](../../assets/qgc/setup/flight_modes/single_channel_mode_selection_1.png)

![Taranis - Configure channel](../../assets/qgc/setup/flight_modes/single_channel_mode_selection_2.png)

You can then select the channel and the flight modes in single channel mode selection option in *QGroundControl*:

![QGC - Set mode channel](../../assets/qgc/setup/flight_modes/single_channel_mode_selection_3.png)

The [Radio switch](../advanced_config/parameter_reference.md#radio-switches) parameters map a particular function to a channel. Assuming you have already mapped a channel in your transmitter you can assign the channel by [setting the parameter](../advanced_config/parameters.md).

For example, below we map channel 6 to the [RC_MAP_ARM_SW](../advanced_config/parameter_reference.md#RC_MAP_ARM_SW) parameter in *QGroundControl*.

![QGC - Map ARM switch to channel](../../assets/qgc/setup/flight_modes/single_channel_mode_selection_4.png)

<span id="multi_channel"></span>

## 多通道飞行模式选择

:::tip
We recommend you use [Single Channel Flight Mode](#single_channel) selection because the Multi Channel selection user interface can be confusing. If you do choose to use this method, then the best approach is to start assigning channels and take note of information displayed by *QGroundControl* following your selection.
:::

The multi-channel selection user interface allows you to map one or more modes to one or more channels. There are some modes (and hence switches) that must always be defined, and the channel to which they must be allocated.

To configure flight modes using the multi-channel UI:

1. 打开您的 RC 遥控器发射机。
2. 打开 *QGroundControl* 并连接上飞机。
3. 点击上方工具栏的 **Gear** 图标（飞行器设置），然后在左侧边栏选择 **Flight Modes** 。
    
    ![Flight modes multi-channel](../../assets/qgc/setup/flight_modes/flight_modes_multi_channel.jpg)
    
:::tip
If the screen opens in *Single Channel Mode* click the **Use Multi Channel Mode Selection** button to change screen.
:::

4. Select the modes you want to assign to your switches and select the associated channel (selected modes will *move* in the user interface to be grouped by channel). There are a number of complications on the mode to channel assignments:
    
    * Some modes cannot be manually edited (are grayed out) because their channel and threshold level are automatically defined based on the values of other mode settings. 例如： 
        * *Mission* mode - is automatically assigned the same channel number as *Hold* (if the channel for *Hold* is defined by the user). If the channel for *Hold* is not defined, *Mission* mode is automatically assigned the same channel as *Stabilized/Main* mode. This, for example, prevents the user from defining *Stabilized/Main* and *Mission* mode on different channels, to ensure that the user cannot switch both modes ON at the same time. 
        * *Altitude* mode - is automatically assigned the same channel number as *Position Control* (if it is defined), or otherwise the same channel as *Stabilized/Main* mode.
    * *Assist* mode - This mode is added to the same channel as *Stabilized/Main* mode if (and only if) *Position Control* is enabled and defined on a different channel than *Stabilized/Main*.
5. Click the **Generate Thresholds** button. 
    * This will automatically create threshold values for all modes, spread evenly across each channel for its assigned modes. For example, in the mode assignment shown above, most modes are assigned to mode 5, and you can see that the channel thresholds for each mode are spread evenly across the channel. 

This mode is demonstrated in the [autopilot setup video here](https://youtu.be/91VGmdSlbo4?t=6m53s) (youtube).

:::note
This flight mode selection mechanism is relatively complicated due to the way that PX4 works out which mode should be selected. You may be able to gain some insight from this [flow chart](../concept/flight_modes.md#flight-mode-evaluation-diagram).
:::

## 更多信息

* [Flight Modes Overview](../flight_modes/README.md)
* [QGroundControl > Flight Modes](https://docs.qgroundcontrol.com/en/SetupView/FlightModes.html#px4-pro-flight-mode-setup)
* [PX4 Setup Video - @6m53s](https://youtu.be/91VGmdSlbo4?t=6m53s) (Youtube)