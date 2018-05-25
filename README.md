# STM32F303RE-Development-on-Mac
光機電實驗
STM32CubeMX + CLion + OpenOCD + gcc-arm-embedded

# To be installed

### CLion 
I'm using Version 2018.2 EAP (182.2574.4). 

It's an early access program now (5/25/2018), and I get this version from (https://www.jetbrains.com/clion/nextversion/)

(EAP version doesn't require license. Stable version requires license. If you're a student, you can get license by signing up using school email address. [more information](https://www.jetbrains.com/student/))

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

Project Location should be at /Users/{User Name}/Project.

Toolchain/IDE should be SW4STM32.

Generate Under Root should be selected.

# CLion 

### Import project
Select your project created by STM32CubeMX.

![Image of import](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20import.png)

Just press OK.

### Install Plugin 

![Image of preference](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20preference.png)

![Image of plugin](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20plugin.png)

Go Preferences -> Plugins -> Browse repositories -> search openocd -> find 'OpenOCD + STM32CubeMX support for ARM embedded development' and install it -> Restart CLion

(I'm using version 1.1 alpha6. [more details about plugin version](https://plugins.jetbrains.com/plugin/10115-openocd--stm32cubemx-support-for-arm-embedded-development))

### Setting OpenOCD path and CMake

![Image of openocd path](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20openocd%20path.png)

Go Preferences -> Build,Execution,Deployment -> Bulid Tools -> OpenOCD Support

Create a public toolchain setting file toolchain-arm-eabi-gcc.cmake 

You can create it by yourself or download [this](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/toolchain-arm-eabi-gcc.cmake)

Then go Preferences -> Build,Execution,Deployment -> CMake 

![Image of cmake](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20cmake.png)

Add this parameter to CMake options

    -DCMAKE_TOOLCHAIN_FILE=<path to toolchain-arm-eabi-gcc.cmake>
For me, I put the file in ~/Project/

    -DCMAKE_TOOLCHAIN_FILE=~/Project/toolchain-arm-eabi-gcc.cmake

### Update CMake project with STM32CubeMX project

![Image of update project](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20update%20project.png)

Go Tools -> Update CMake project with STM32CubeMX project

![Image of board config2](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20board%20config2.png)

Choose st_nucleo_f3.cfg or stm32f3discovery.cfg, or you can set this later.

### Setting OpenOCD configuration

Run -> Edit Configurations... -> OCD {Project name} ->Board config file 
![Image of board config](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20board%20config.png)

Those config file will be found in /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/board/

We are using STM32F303RE, so we can choose st_nucleo_f3.cfg or stm32f3discovery.cfg

However, if you choose stm32f3discovery.cfg, there might be some errors.

To fix it (imformation from [here](http://www.openstm32.org/forumthread562)), you can find stlink-v2.cfg in /usr/local/Cellar/open-ocd/0.10.0/share/openocd/scripts/interface/.

Find this line:

    hla_vid_pid 0x0483 0x3748
Change the line to:

    hla_vid_pid 0x0483 0x374b
    
### Add your code

Change your code in main.c or other source file if needed
    
### Build project

Go run -> Build 

### Finish! 

Your Binary file will be in ~/Project/cmake-build-debug/. Just drag it in to your ARM device.

# Common Errors

When you encounter an error like this:

![Image of board config](https://github.com/b04505009/STM32F303RE-Development-on-Mac/blob/master/CLion%20error.png)

This error seems to be caused by update CMake project with STM32CubeMX project repeatly.

## To fix this:

### Reimport your project

Close CLion, and delete your project at welcome page. Then import your project as a new CMake project. 

### Update CMake project with STM32CubeMX project

Go Tools -> Update CMake project with STM32CubeMX project

### Add your code

Change your code in main.c or other source file if needed

### Build project

Go run -> Build 



