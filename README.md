### Project based on a 6-DOF Robotic ARM & Jetson Nano & Kinect360
<p align="center" >
   <img width="200" height="90" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Logos/kinect360.jpg">
 </p>
 Assemble of Robotic Arm parts:
 <p align="center" >
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/1.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/2.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/3.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/4.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/5.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/6.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/7.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/8.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/9.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/10.jpg">
 
</p>

### Connect Kinect360 to Linux(Ubuntu 18.04) 

Required Libraries :  **Freenect**   **OpenNI**   **PrimeSensor Modules**  **NITE**


Some additional notes about different models:

**Kinect 1414:** This is the original kinect and works with the library documented on this page in the Processing 3.0 beta series.
**Kinect 1473:** This looks identical to the 1414, but is an updated model. It should work with this library, but I don’t have one to test.

**(Installation):**

**I) :**

       $ sudo apt-get install cmake libglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev python
       
if you get “Unable to locate package libglut3-dev” error, use this command instead:

       $ sudo apt-get install cmake freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev python
       $ sudo apt-get install doxygen mono-complete graphviz
 

**II) :** **Freenect Library:** 

libfreenect is a cross-platform library that provides the necessary interfaces to activate, initialize, and communicate data with the Kinect hardware. Currently, the library supports access to RGB and depth video streams, motors, accelerometer and LED and provide binding in different languages (C++, Python...)

This library is the low level component of the OpenKinect project which is an open community of people interested in making use of the Xbox Kinect hardware with PCs and other devices.[1](http://neuro.debian.net/pkgs/freenect.html)

       $ git https://github.com/OpenKinect/libfreenect 
       $ cd libfreenect
       $ mkdir build
       $ cd build
       $ cmake ..
Check that it marks no errors and no missing applications. Install whatever it marks and then proceed with the making.   
       
       $ make
The make command will connect to the Internet in order to download the audios.bin which is a firmware needed for the MS Kinect to work, without it the device won't be able to function properly.  

       $ sudo make install
       $ cd ../src
       $ python fwfetcher.py
       
This will create audios.bin file.I should copy this into a directory then the system will open it as firmware .

       $ sudo ldconfig /usr/local/lib64/
       
Now I should create a new directory to add firmware to it.

       $ cp -r /usr/local/include/libfreenect /usr/include/libfreenect   
       $ sudo cp audios.bin /usr/local/share/libfreenect/
       
       
 Now plug the Kinect-360(v1) to the computer usb port and check the attached devices :
 
       $ lsusb | grep Xbox
       
  <p align="center" >
    <img width="360" height="37"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/usb_list.png">
  </p>

**Test by an example :**
       
        $ cd ../build/bin
        $ freenect-glview
        
   <p align="center" >
    <img width="300" height="200"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/libfreenect_0.png">
    <img width="350" height="280"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/libfreenect_1.png">
  </p>

Note:

if you get an error :
 
     libusb couldn’t open USB device /dev/bus/usb/001/006: Permission denied.
Then run these commands:

         $ sudo adduser $USER video
         $ sudo nano /etc/udev/rules.d/51-kinect.rules
         
     # ATTR{product}=="Xbox NUI Motor" SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02b0", MODE="0666"
     # ATTR{product}=="Xbox NUI Audio" SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ad", MODE="0666"
     # ATTR{product}=="Xbox NUI Camera" SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ae", MODE="0666"

Then Logout and then Login again .

**III) :** **OpenNI Library:** 

       $ git https://github.com/OpenNI/OpenNI
       $ cd OpenNI
       $ cd Platform/Linux/CreateRedist
       $ chmod +x ./RedistMaker
       $ ./RedistMaker
       $ cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.7.10/
       $ sudo ./install.sh

**IV) :** **PrimeSensor Modules for OpenNI :**
  
OpenNI is primarly developed by PrimeSence, which is the company behind Kinect's depth sensor's technology.

       $ git https://github.com/avin2/SensorKinect
       $ cd SensorKinect
       $ cd Platform/Linux/CreateRedist
       $ chmod a+x RedistMaker
       $ sudo ./RedistMaker
       $ cd ../Redist/Sensor-Bin-Linux-x64-v5.1.2.1/
       $ sudo sh install.sh

**V) :** **NITE :**(a SDK for joint tracking with the Microsoft Kinect)

       $ git https://github.com/arnaud-ramey/NITE-Bin-Dev-Linux-v1.5.2.23
       $ cd NITE-Bin-Dev-Linux-v1.5.2.23
       $ cd x64
       $ sudo bash install.sh
       
       
**VI) :** **Test the Kinect360 configuration by running an example**
 
Inside the OpenNI library :
 
       $ cd OpenNI/Platform/Linux/Bin/x64-Release/
       $ ./ Sample-NiHandTracker

<p align="center" >
    <img width="300" height="250"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/example_1.png">
</p>
       
       
### Project Development :

I want to develop a "Mobile Robotic ARM" and use PCL Library for SLAM section.
**I)** Project Folder is [Kinect_PCL]()

CmakeLists.txt File :

     cmake_minimum_required(VERSION 3.0.0) 
     project(Kinect_PCL VERSION 0.1 LANGUAGES CXX)
     set(CMAKE_INCLUDE_CURRENT_DIR ON)
     set(CMAKE_AUTOMOC ON)
     find_package(PCL 1.2 REQUIRED)
     find_package(libfreenect REQUIRED)
     include_directories(${PCL_INCLUDE_DIRS})
     include_directories("/usr/include/libusb-1.0/")
     link_directories(${PCL_LIBRARY_DIRS})
     add_definitions(${PCL_DEFINITIONS})
     add_executable(${PROJECT_NAME} "main.cpp")
     target_link_libraries (${PROJECT_NAME} ${PCL_LIBRARIES}
                                       ${FREENECT_LIBRARIES})

