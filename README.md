# Vulkan on Raspberry Pi
A guide to setting up vulkan on the raspberry pi.
![vulkan logo](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/Vulkan%20Logo.jpg?raw=true)
Note: this may not work due to wayland-client on raspberry pis being out of date and there is no update as of now.   
### get necessary updates
This is to make sure everything is up-to date and to make sure you have all the necessary packages before starting the setup process.
```
sudo apt update
sudo apt upgrade
```
### unlock build dependency on apt
```
sudo nano /etc/apt/sources.list
```
Once inside the file, uncomment the following line:
```
deb-src http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi
```
Then hit ctrl+x, y, enter to save the changes to the file.
### update repository list
```
sudo apt update
```
### install vulkan tools
This is needed to test and see if vulkan has been installed correctly.
```
sudo apt install vulkan-tools
```
### build mesa libraries
This is necessary to build the vulkan libraries.
```
sudo apt build-dep mesa
```
Then this grabs the libraries. 
```
git clone https://gitlab.freedesktop.org/mesa/mesa
```
Once the libraries are downloaded, make a directory inside the mesa folder called build and go to the directory.
```
cd mesa
mkdir build
cd build
```
Once inside the build directory, run the following command
```
meson --libdir lib -Dplatforms=x11,auto -Dvulkan-drivers=broadcom -Ddri-drivers= -Dgallium-drivers=v3d,kmsro,vc4 -Dbuildtype=debug _buuild ..
```
If you get an error saying you need xcb-shm run the following command then the previous one again.
```
sudo apt install libxcb-shm0-dev
```
Once completed, it should say found ninja along with your version of ninja.
### install vulkan libraries
You first need to go into the other build directory.
```
cd _build
```
Then run the following command:
```
sudo ninja install
```
Once completed, vulkan should be installed on your rpi.
### testing vulkan
The last step is to test and see if everything was done correctly.
```
vkcube
```
If the following window shows a spinning cube, then you have sucessfully built and installed vulkan on your rpi.
