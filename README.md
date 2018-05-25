# STM32F303RE-Development-on-Mac
STM32CubeMX + CLion + OpenOCD + arm-none-eabi

# To be installed

### CLion 
I'm using Version 2018.2 EAP (182.2574.4). 
It's an early access program now (5/25/2018), and I get this version from (https://www.jetbrains.com/clion/nextversion/)
### GNU Arm Embedded Toolchain 
    brew install caskroom/cask/gcc-arm-embedded
Then you can find it in /usr/local/Caskroom/gcc-arm-embedded
### OpenOCD
    brew install openocd
Then you can find it in /usr/local/Cellar/open-ocd
### STM32CubeMX
http://www.st.com/en/development-tools/stm32cubemx.html

# Generate code from STM32CubeMX
![Image of STM32CubeMX](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/STM32CubeMX.png)

Project Location should be at ~/Project.
Toolchain/IDE should be SW4STM32.
Generate Under Root should be selected.

# CLion Import project
Select your project created by STM32CubeMX.
![Image of import](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20import.png)
Select everything under the directory.

# CLion install Plugin 
![Image of preference](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/STM32CubeMX.png)
![Image of plugin](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20plugin.png)

Go Preferences -> Plugins -> Browse repositories -> search openocd -> find 'OpenOCD + STM32CubeMX support for ARM embedded development' and install it -> Restart CLion
(I'm using version 1.1 alpha6.[more details for plugin version](https://plugins.jetbrains.com/plugin/10115-openocd--stm32cubemx-support-for-arm-embedded-development))

# CLion setting OpenOCD path and CMake
![Image of openocd path](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20openocd%20path.png)

Go Preferences -> Build,Execution,Deployment -> Bulid Tools -> OpenOCD Support

Create a public toolchain setting file toolchain-arm-eabi-gcc.cmake 
You can create by yourself or download [this](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/toolchain-arm-eabi-gcc.cmake)
Then go Preferences -> Build,Execution,Deployment -> CMake 
![Image of cmake](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20cmake.png)
Add this parameter to CMake options

    -DCMAKE_TOOLCHAIN_FILE=<path to toolchain-arm-eabi-gcc.cmake>
For me, I put the file under ~/Project/

    -DCMAKE_TOOLCHAIN_FILE=~/Project/toolchain-arm-eabi-gcc.cmake

# CLion setting OpenOCD configuration

Run -> Edit Configurations... -> OCD {Project name} ->Board config file 
![Image of board config](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20board%20config.png)

Those config file will be found in /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/board/.
We are using STM32F303RE, so we may choose st_nucleo_f3.cfg or stm32f3discovery.cfg.
However, if you choose stm32f3discovery.cfg, there may be some errors.
To fix it(from [here](http://www.openstm32.org/forumthread562)), you can find stlink-v2.cfg in /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/interface/, and then find this line:

    hla_vid_pid 0x0483 0x3748
Change the line to:

    hla_vid_pid 0x0483 0x374b

# CLion Update CMake project with STM32CubeMX project

![Image of update project](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20update%20project.png)

Go Tools -> Update CMake project with STM32CubeMX project

# CLion Reimport your project

Close CLion, and delete your project at welcome page. Then import it as a new CMake project. Remember to select everything under the directory.

# CLion Update CMake project with STM32CubeMX project again

# CLion Build project

Go run -> Build 

# Finish! Your Binary file will be in ~/Project/cmake-build-debug/. Just drag it in to your ARM device.





