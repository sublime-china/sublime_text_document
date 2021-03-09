# 

[DOCUMENTATION](index)

GPU Rendering

Added in:4.0

Sublime Text includes both a software rendering mode, and a hardware-accelerated mode using the GPU via OpenGL. The OpenGL renderer can improve performance on high DPI screens, although some graphics card drivers may produce incorrect results.*By default, Macs will use OpenGL, whereas Windows and Linux machines will use software.*

*   [Setting](gpu_rendering#setting)
*   [Diagnostics](gpu_rendering#diagnostics)

## Setting

The settinghardware\_accelerationcontrols the rendering mode. A value of`"none"`causes Sublime Text to use the software rendering mode, whereas a value of`"opengl"`results in the OpenGL renderer being used.

## Diagnostics

Whenhardware\_accelerationis set to`"opengl"`, diagnostic information will be printed in the Console on startup. The following is an example of the output:

~~~
OpenGL Context Information:
  GL API Version: 4.1 INTEL-14.4.23
  GLSL Version: 4.10
  Vendor: Intel Inc.
  Renderer: Intel Iris Pro OpenGL Engine
~~~