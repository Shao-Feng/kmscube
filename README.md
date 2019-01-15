kmscube
=======

kmscube is a little demonstration program for how to drive bare metal graphics
without a compositor like X11, wayland or similar, using DRM/KMS (kernel mode
setting), GBM (graphics buffer manager) and EGL for rendering content using
OpenGL or OpenGL ES.

The upstream of kmscube is available at https://gitlab.freedesktop.org/mesa/kmscube/




Build on Android
=======


Patch Kernel
==========

https://android.intel.com/#/c/639800/4

https://android.intel.com/#/c/639798/5

https://android.intel.com/#/c/639796/5


Build minigbm
==========

Depend on libminigbm. which is not built by default in 1A repo (only gralloc will be built).

For building libminigbm, apply the patch '0001-Build-libminigbm.so-for-compiling-kmscube.patch' in minigbm-intel https://github.com/intel/minigbm

and make sure 'libminigbm.so' is deployed on device '/vendor/lib64/' and '/vendor/lib/'.


Build kmscubewrapper
==========

kmscubewrapper is a wrapper for invoking and connecting Android "binder" (C++) from kmscube atomic (C).
also make sure 'libkmscubewrapper.so' is deployed on device '/vendor/lib64/' and '/vendor/lib/'.


Build kmscube
==========

cd <HWC_PATH>

git clone https://github.com/Shao-Feng/kmscube.git

Add 2 line in <HWC_PATH>/Android.mk as below

"""

include $(HWC_PATH)/kmscube/wrap/Android.mk

include $(HWC_PATH)/kmscube/Android.mk

endif

"""
