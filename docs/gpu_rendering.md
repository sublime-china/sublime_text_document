# 

[DOCUMENTATION](index)

GPU 渲染

Added in:4.0

Sublime Text包括软件渲染模式和通过OpenGL使用GPU的硬件加速模式。OpenGL渲染器可以提高高DPI屏幕上的性能，尽管一些显卡驱动程序可能会产生不正确的结果。*默认情况下，mac将使用OpenGL，而Windows和Linux机器将使用软件。*

*   [Setting](gpu_rendering#setting)
*   [Diagnostics](gpu_rendering#diagnostics)

## Setting

hardware\_acceleration 设置控制渲染模式。`"none"` 会导致Sublime Text使用软件渲染模式，而 `"opengl"` 会导致使用OpenGL渲染器。

## 诊断

当 hardware\_acceleration 设置为 `"opengl"` 时，诊断信息将在启动时打印在控制台中。以下是输出示例:

~~~
OpenGL Context Information:
  GL API Version: 4.1 INTEL-14.4.23
  GLSL Version: 4.10
  Vendor: Intel Inc.
  Renderer: Intel Iris Pro OpenGL Engine
~~~