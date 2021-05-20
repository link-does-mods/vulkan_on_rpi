# Vulkan on Raspberry Pi 4
A guide to setting up vulkan on the raspberry pi.
![vulkan logo](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/Vulkan%20Logo.jpg?raw=true)

### Table of Contents
- [TLDR](#tldr)
- [Compiling From Scratch](#compiling-from-scratch)
- [Demos](#demos)

### TLDR
You can install PiKISS to make everything much easier and quicker to install.
```
sudo apt install curl
curl -sSL https://git.io/JfAPE | bash
```

Updating PiKISS:
```
cd ~/piKiss
git pull
```
Once installed, run PiKISS and this screen appears. On this screen, select confiure.
![PiKISS-Configure](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/PiKISS%20Screenshots/PiKISS-configure.jpg?raw=true)<br /><br />
Then select vulkan to start compiling the vulkan drivers for your rpi4.
![PiKISS-Vulkan](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/PiKISS%20Screenshots/PiKISS-vulkan.jpg?raw=true)<br /><br />
Once the drivers are finished compiling, you should see this screen which confirms everything went well, and you are done.
![PiKISS-Vulkan Ready](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/PiKISS%20Screenshots/PiKISS-vulkan-ready.jpg?raw=true)<br /><br />

### Compiling From Scratch
First, you should check your architecture before manually compiling Vulkan.
```
uname -a 
```
![Version Check](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/Manual%20Compile%20Screenshots/Version32_64.jpg?raw=true)<br /><br />

Updating is important to do first before continuing.
```
sudo apt update
sudo apt upgrade
``` 

Install dependencies:
```
sudo apt install libxcb-randr0-dev libxrandr-dev libxcb-xinerama0-dev libxinerama-dev libxcursor-dev libxcb-cursor-dev libxkbcommon-dev xutils-dev xutils-dev libpthread-stubs0-dev libpciaccess-dev libffi-dev x11proto-xext-dev libxcb1-dev libxcb-*dev bison flex libssl-dev libgnutls28-dev x11proto-dri2-dev x11proto-dri3-dev libx11-dev libxcb-glx0-dev libx11-xcb-dev libxext-dev libxdamage-dev libxfixes-dev libva-dev x11proto-randr-dev x11proto-present-dev libclc-dev libelf-dev git build-essential mesa-utils libvulkan-dev ninja-build libvulkan1 python-mako libdrm-dev libxshmfence-dev libxxf86vm-dev libunwind-dev valgrind libzstd-dev vulkan-tools vulkan-utils ninja-build
```

Remove old versions of Vulkan if installed already.
```
sudo rm -rf /home/pi/mesa_vulkan
```
Now you can compile Vulkan for your rpi.
```
sudo apt purge meson -y
sudo pip3 install meson
sudo pip3 install mako
cd ~ && git clone https://gitlab.freedesktop.org/mesa/mesa.git mesa_vulkan
cd mesa_vulkan
```
For 64-bit:
```
CFLAGS="-mcpu=cortex-a72" \
CXXFLAGS="-mcpu=cortex-a72" \
```
For 32-bit:
```
CFLAGS="-mcpu=cortex-a72 -mfpu=neon-fp-armv8" \
CXXFLAGS="-mcpu=cortex-a72 -mfpu=neon-fp-armv8" \
```
```
meson --prefix /usr \
-D platforms=x11 \
-D vulkan-drivers=broadcom \
-D dri-drivers= \
-D gallium-drivers=kmsro,v3d,vc4 \
-D buildtype=release build
ninja -C build -j4
sudo ninja -C build install
```
Finally to make sure everything is working:
```
glxinfo -B
```
You should get this screen showing that everything works.
![glxinfo -B Command](https://github.com/link-does-mods/vulkan_on_rpi/blob/main/Manual%20Compile%20Screenshots/glxinfo.jpg?raw=true)<br/><br/>
### Demos
This wouldn't be any useful without running some demos to test out the new drivers.
```
sudo apt install libassimp-dev
git clone --recursive https://github.com/SaschaWillems/Vulkan.git  sascha-willems 
cd sascha-willems && python3 download_assets.py
mkdir build
cd build $ cmake -DCMAKE_BUILD_TYPE=Debug  .. 
make -j4
```
Everything is put into the build/bin folder. Just run the desired demo and see the drivers in action. 
