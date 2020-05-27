USB104A7-dpmutil Demo
====================

Description
-----------

This project demonstrates the use of the Digilent Platform Management Utility (dpmutil) on the USB104A7.

Dpmutil provides a command line interface for discovering information about the features and configuration of a Digilent platform board. Commands are also provided for discovering the Zmods (SYZYGY pods) attached to the SmartVIO port(s) of the board and for manipulating various Platform MCU settings.

| Command			       | Function						                                                                  |
| ---------------------    | ------------------------------------------------------------------------------------------------ |
| getinfo   |  Get general configuration and information about the supported features of the PlatformMCU (PMCU). This command communicates with the PMCU over the I2C bus to retrieve general information about the capabilities of the PMCU and the board configuration. This information includes the PMCU firmware revision, SmartVIO port count, power supply group counts (5V0, 3V3, VADJ), the number of temperature probes supported by the board, and the number of fans supported by the board. If the board supports one or more temperature probe, then the capabilities of each supported probe and the most recent temperature measurement of that probe are displayed via the console. If the board supports one or more fan, then the capabilities of each supported fan are displayed and if a fan supports RPM measurement then the most recent RPM measurement is also displayed. |
| getinfo5v0					   | Get information about the on board 5V0 power supplies that are associated with the on board SmartVIO ports. This function communicates with the Platform MCU (PMCU) over I2C to determine the number of on board 5V0 power supplies, to retrieve the amount of current that each supply is capable of providing, and to retrieve the sum of current requested by all SmartVIO ports that are associated with each supply.<br/><br/>The "-chanid" option can be used to specify the channel identifier (0-7, a-f, or A-F) of a specific power supply in order to limit the information that is displayed to a specific channel. If no channel identifier is provided, then this command will retrieve and display information for every channel supported by the board.  |
| getinfo3v3					   | Get information about the on board 3V3 power supplies that are associated with the on board SmartVIO ports. This function communicates with the Platform MCU (PMCU) over I2C to determine the number of on board 3V3 power supplies, to retrieve the amount of current that each supply is capable of providing, and to retrieve the sum of current requested by all SmartVIO ports that are associated with each supply.<br/><br/>The "-chanid" option can be used to specify the channel identifier (0-7, a-f, or A-F) of a specific power supply in order to limit the information that is displayed to a specific channel. If no channel identifier is provided, then this command will retrieve and display information for every channel supported by the board. |
| getinfovio    | Get information about the on board VIO (VADJ) power supplies that are associated with the on board SmartVIO ports. This function communicates with the Platform MCU (PMCU) over I2C to determine the number of on board VIO power supplies, to retrieve the amount of current that each supply is capable of providing, to retrieve the sum of current requested by all SmartVIO ports that are associated with each supply, and to retrieve all status and configuration information associated with each supply.<br/><br/>The "-chanid" option can be used to specify the channel identifier (0-7, a-f, or A-F) of a specific power supply in order to limit the information that is displayed to a specific channel. If no channel identifier is provided, then this command will retrieve and display information for every channel supported by the board  |
| getinfopower | Get information about the on board power supplies (5V0, 3V3, VIO) that are associated with the on board SmartVIO ports. This command communicates with the Platform MCU (PMCU) over I2C to determine the number of on board 5V0, 3V3, and VIO power supplies that are associated with the on board VIO ports and to retrieve various information about each of these supplies. Executing this command is equivalent to sequentially executing "getinfo5v0", "getinfo3v3", and "getinfovio".  |
| enum                      | Enumerate SmartVIO ports. This command communicates with the Platform MCU over the I2C bus to determine how many SmartVIO ports the board contains and to retrieve the configuration and status for each of those ports. If a SmartVIO port has a SYZYGY pod installed then the I2C bus is used to retrieve the Standard SYZYGY firmware registers, as well as the SYZYGY DNA (including all string fields), and output that information to the console. Additional information may be displayed when a Digilent Zmod is attached to the port. |
| setplatcfg                     | Modify one or more field of the Platform MCU (PMCU) Platform Configuration Register. This function uses the I2C bus to retrieve the contents of the PMCU’s Platform Configuration Register, modifies the specified field(s) of the register, and then writes the new settings to the register. Settings that may be modified include enforcing the 5V0 current limit, enforcing the 3V3 current limit, enforcing the VIO current limit, and performing CRC checks of SYZYGY headers. Please note that the Platform Configuration is stored in the PMCU’s EEPROM and is only read during firmware initialization. Therefore, any changes made to the Platform Configuration Register will not take effect until the next time the PMCU is reset. The PMCU may be reset by issuing the "resetpmcu" command.<br/><br/>The "-enforce5v0" option can be used to enable or disable enforcement of 5V0 current limits. If enforcement is enabled and the sum of the current requested by all associated SmartVIO pods exceeds the total current that the onboard supply can provide then the VIO supply associated with the SmartVIO port will not be enabled.<br/><br/>The "-enforce3v3" option can be used to enable or disable enforcement of 3V3 current limits. If enforcement is enabled and the sum of the current requested by all associated SmartVIO pods exceeds the total current that the supply can provide then the VIO supply associated with the SmartVIO port will not be enabled.<br/><br/>The "-enforcevio" option can be used to enable or disable enforcement of VIO current limits. If enforcement is enabled and the sum of the current requested by all associated SmartVIO pods exceeds the total current that the supply can provide then the VIO supply associated with the SmartVIO port will not be enabled.<br/><br/>The "-checkcrc" option can be used to enable or disable SYZYGY header DNA checks. If CRC checks are enabled and the CRC computed does not match, then the VIO supply associated with the SmartVIO port will not be enabled.  |
| setviocfg          | Modify one or more field of the Platform MCU (PMCU) VADJ_n_OVERRIDE register. The VADJ_n_OVERRIDE register can be used to override the state of a specific VIO supply. This includes enabling or disabling the supply, as well as setting the output voltage. When a VADJ_n_OVERRIDE register is written the PMCU will check to make sure that the specified settings do not conflict with the requirements of any SmartVIO port associated with the specified supply. If there aren’t any conflicts, then the specified settings will be applied immediately. Howev er, if there is a conflict then the changes to the VADJ_n_OVERRIDE register, and the associated power supply, will be restricted to meet the requirements of all associated SmartVIO ports.<br/><br/>The "-chanid" option must be used to specify the channel identifier (0-7, a-f, or A-F) of the VIO supply.<br/><br/>The "-override" option can be used to enable (’y’) or disable (’n’) overriding the VIO supply configuration. If override is enabled, then the associated VIO supply will be configured based on the enable and voltage fields of the VADJ_n_OVERRIDE register.<br/><br/>The "-enable" option can be used to enable (’y’) or disable (’n’) the associated VIO supply. This setting has no impact when the override field of the VADJ_n_OVERRIDE register is cleared.<br/><br/>The "-voltage" option can be used to specify the voltage (in millivolts) of the associated VIO supply. This setting has no impact when the override field of the VADJ_n_OVERRIDE register is cleared. |
| setfancfg | Modify one or more field of the Platform MCU (PMCU) FAN_n_CONFIGURATION register. The FAN_n_CONFIGURATION register is used to specify the settings of the associated fan. This may include the enable state of the fan, the fan’s speed, and the associated temperature probe. Please note that not all fan ports support enable/disable, fixed speed control, or automatic speed control (temperature based). Changes to a FAN_n_CONFIGURATION register will be restricted to the be within the supported capabilities of the port and take effect immediately after the register is written. Additionally, the FAN configuration is written to EEPROM and will restored each time the PMCU is reset or power cycled.<br/><br/>The "-fanid" option must be used to specify the identifier (1-4) of the fan configuration to be modified.<br/><br/>The "-enable" option can be used to enable (’y’) or disable (’n’) the associated fan.<br/><br/>The "-speed" option can be used to specify the speed ("minimum", "medium", "maximum", or "auto") of the associated fan. Please note that not all fans support this functionality and some ports that do support this functionality may not support automatic fan speed control.<br/><br/>The "-probe" option can be used to specify the temperature probe ("none","p1","p2","p3","p4") associated with a fan if that fan supports automatic speed control. |
| resetpmcu | This command uses the I2C bus to write a positive value to the software reset register of the Platform MCU (PMCU), which causes the processor to perform a software reset. |

| Option | Function |
| ------ | -------- |
| -chanid | Specify the channel identifier to get information about a specific power supply when executing one of the getinfo commands or to specify the VIO channel that’s being configured by the "setviocfg" command. The channel identifier may be specified using the numbers 0 through 7, the characterse ’a’ through ’h’, or the characters ’A’ through ’H’. The number of VIO supplies, or channels, may vary from product to product. Use the "getinfopower" command to determine how many 5V0, 3V3, and VIO channels the device supports. |
| -fanid | Specify the fan identifier used when setting a fan configuration with the "setfancfg". The fan identifier may be specified using the numbers 1 through 4. The number of fans support may vary from product to product. Use the "getinfo" command to determine how many fans the device supports. |
| -port | Specify the port identifier for the physical port containing the pod whose DNA you wish to write when executing the "writedna" command. The port identifier command may be speified using numbers 0 through 25, the characterse ’a’ through ’z’, or the characters ’A’ through ’Z’. The number of physical ports supported may vary from product to product. Use the "getinfo" or "enum" commands to determine how many SmartVIO ports the device suppports |
| -enable |  Specify whether a function is enabled (’y’) or disabled (’n’). |
| -override | Specify whether the configuration of a VADJ supply is overriden (’y’) or not (’n’) by an alernative configuration using the "setviocfg" command. |
| -voltage | Specify the VIO voltage (in millivolts) to set in the VADJ_n_OVERRIDE register when executing the "setviocfg" command. |
| -enforce5v0 | Specify if the 5V0 current limit is enforced (’y’) or ignored (’n’). This setting is written to the Platform MCU’s Platform Configuration Register by issuing the "setplatcfg" command. |
| -enforce3v3 | Specify if the 3V3 current limit is enforced (’y’) or ignored (’n’). This setting is written to the Platform MCU’s Platform Configuration Register by issuing the "setplatcfg" command. |
| -enforcevio | Specify if the VIO current limit is enforced (’y’) or ignored (’n’). This setting is written to the Platform MCU’s Platform Configuration Register by issuing the "setplatcfg" command. |
| -checkcrc | Specify if a CRC check is performed (’y’) or skipped (’n’) for each SYZYGY header. This option must be issued alongside the "setplatcfg" command to specify if the Platform MCU performs CRC checks when it enumerates SmartVIO ports. This option may also be used to tell the "enum" command to skip CRC checks when enumerating SmartVIO ports. |
| -speed | Specify the fan speed setting used when the "setfancfg" command is issued. The fan speed may be specified as "minimum", "medium", "maximum", or "auto". Not all fan headers support setting a fixed speed or automatic speed control. Use the "getinfo" command to determine how many fan headers the device contains and if a fixed speed or automatic speed setting is supported for each header |
| -probe | Specify the temperature source, or probe, utilized when performing automatic speed control. Temperature probes may be specified as "none", "p1", "p2", "p3", or "p4". Use the "getinfo" command to determine how many temperature probe’s the device supports. |
| -?,-help | Display typical application usage, a list of supported commands, and a list of supported options. |

Requirements
------------
* **USB104A7**: To purchase a USB104A7, see the [Digilent Store](https://store.digilentinc.com/usb104a7/)
* **Vivado 2019.1 Installation with Xilinx SDK**: To set up Vivado, see the [Installing Vivado and Digilent Board Files Tutorial](https://reference.digilentinc.com/vivado/installing-vivado/start).
* **Serial Terminal Emulator Application**: For more information see the [Installing and Using a Terminal Emulator Tutorial](https://reference.digilentinc.com/learn/programmable-logic/tutorials/tera-term).
* **USB A Cable**
* **5V DC Adapter**

Demo Setup
----------

1. Download the most recent release ZIP archive ("USB104A7-dpmutil-*.zip") from the repo's [releases page](https://github.com/Digilent/USB104A7-dpmutil/releases).

2. Extract the downloaded ZIP.

3. Open the XPR project file, found at \<archive extracted location\>/vivado_proj/USB104A7-dpmutil.xpr, included in the extracted release archive in Vivado 2019.1.

4. Launch Xilinx SDK directly (not through the Vivado file menu). When prompted for a workspace, select "\<archive extracted location\>/sdk_workspace".

5. Once the workspace opens, click the **Import** button. In the resulting dialog, first select *Existing Projects into Workspace*, then click **Next**. Navigate to and select the same sdk_workspace folder.

6. Build the project. **Note**: *Errors are sometimes seen at this step. These are typically resolved by right-clicking on the BSP project and selecting Regenerate BSP Sources.*

7. Plug the 5v DC adapter into the USB104A7 and connect it to the PC using the USB cable.

8. Open a serial terminal application (such as [TeraTerm](https://ttssh2.osdn.jp/index.html.en) and connect it to the USB104A7's serial port, using a baud rate of 115200.

9. In the toolbar at the top of the SDK window, select *Xilinx -> Program FPGA*. Leave all fields as their defaults and click "Program".

10. In the Project Explorer pane, right click on the "USB104A7-dpmutil" application project and select "Run As -> Launch on Hardware (System Debugger)".

11. The application will now be running on the USB104A7. It can be interacted with as described in the first section of this README.

12. Lastly, the hardware platform must be linked to a hardware handoff, so that changes to the Vivado design can be brought into the SDK workspace. In Vivado, in the toolbar at the top of the window, select *File -> Export -> Export Hardware*. Any Exported Location will do, but make sure to remember the selection, and make sure that the **Include bitstream** box is checked. Click **OK**.

13. In SDK, right click on the \*_hw_platform_\* project, and select *Change Hardware Platform Specification*. Click **Yes** in response to the warning. In the resulting dialog, navigate to and select the .hdf hardware handoff file exported in the previous step, then click **OK**. Now, whenever a modified design is exported from Vivado, on top of the .hdf file, it can be applied to the hardware platform.

Next Steps
----------

This demo can be used as a basis for other projects by modifying the hardware platform in the Vivado project's block design or by modifying the SDK application project.

Check out the USB104A7's [Resource Center](https://reference.digilentinc.com/reference/programmable-logic/USB104A7/start) to find more documentation, demos, and tutorials.

For technical support or questions, please post on the [Digilent Forum](forum.digilentinc.com).

Additional Notes
----------------
For more information on how this project is version controlled, refer to the [digilent-vivado-scripts repo](https://github.com/digilent/digilent-vivado-scripts).
