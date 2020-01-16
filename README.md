kmscube
=======

This application is updated for Android based on kmscube.
kmscube is a little demonstration program for how to drive bare metal graphics
without a compositor like X11, wayland or similar, using DRM/KMS (kernel mode
setting), GBM (graphics buffer manager) and EGL for rendering content using
OpenGL or OpenGL ES.

The upstream of kmscube is available at https://gitlab.freedesktop.org/mesa/kmscube/




Build on Android
=======


Dependency
=======

Dependent on libminigbm, libhwcservice, and libkmscubewrapper.
minigbm is not built by default in Celadon repo (only gralloc will be built).


Build minigbm manually
==========

By default, only gralloc is built on 1A repo tree. For building libminigbm, 

apply the patch '0001-Build-libminigbm.so-for-compiling-kmscube.patch' in minigbm-intel https://github.com/intel/minigbm


cd <Minigbm_HOME>

git am <Commitswitch_HOME>/0001-Build-libminigbm.so-for-compiling-kmscube.patch


Once minigbm is built successfully. 

Push the 'libminigbm.so' file on the device as below

adb root

adb remount

cd <1A_OUT_PRODUCT_HOME>/vendor/lib64/hw/

adb push libminigbm.so /vendor/lib64/

adb push ../../lib/hw/libminigbm.so /vendor/lib/

adb reboot


Clone kmscube in HWC tests
===========

cd <HWC_PATH>/tests
git clone https://github.com/Shao-Feng/kmscube.git


Build kmscube with Dependency
==========

libkmscubewrapper is a wrapper for invoking and connecting Android "binder" (C++) from kmscube (C).

libhwcservice provide the API for syncing DRM commit with HWC.

For building kmscube, libkmscubewrapper and libhwcservice add 2 line in the bottom of <HWC_PATH>/Android.mk as below

"""
include $(HWC_PATH)/tests/kmscube/wrap/Android.mk
include $(HWC_PATH)/tests/kmscube/Android.mk

endif
"""


After building, push the corresponding '.so' file to device


adb root

adb remount

cd <1A_OUT_PRODUCT_HOME>/vendor/lib64/

adb push libkmscubewrapper.so /vendor/lib64/

adb push ../lib/libkmscubewrapper.so /vendor/lib/

adb push libhwcservice.so /vendor/lib64/

adb push ../lib/libhwcservice.so /vendor/lib/

adb push hw/hwcomposer.broxton.so /vendor/lib64/hw/

adb push ../lib/hw/hwcomposer.broxton.so /vendor/lib/hw/

adb push ../bin/kmscube /vendor/bin/



Launch kmscube
===============

This demo must be launched as root. 

(Celadon only begin)

On Celadon, need to create hwc lock.

adb root

adb remount

adb shell touch /vendor/hwc.lock

(Celadon only end)

adb root

adb shell

/vendor/bin/kmscube

