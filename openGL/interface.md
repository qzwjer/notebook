在Qt中，你可以使用Qt的QOpenGL模块来绘制图形，它提供了一些用于OpenGL渲染的接口和类。以下是一些常用的Qt OpenGL绘制图形的接口和类：

1. **QOpenGLWidget**：这是一个Qt小部件类，继承自QWidget，用于创建一个OpenGL上下文，并在其中绘制OpenGL图形。你可以子类化QOpenGLWidget，并在其`initializeGL`、`paintGL`和`resizeGL`函数中编写OpenGL渲染代码。

    **Qt的UI设计有OpenGL Widget控件，将该控件拖动到要显示的地方后，在源文件中创建继承QOpenGLWidget的类，实现initializeGL,resizeGL,paintGL，选择控件promote到该类即可在代码中使用。**

2. **QOpenGLFunctions**：这是一个辅助类，它提供了OpenGL核心功能的接口。当你需要在QOpenGLWidget或其他OpenGL上下文中使用OpenGL函数时，通常会继承自QOpenGLFunctions，并在你的类中调用OpenGL函数。

3. **QOpenGLShaderProgram**：这个类用于管理着色器程序。你可以使用它来创建、编译、链接和使用顶点着色器和片段着色器，以进行高级的图形渲染。

4. **QOpenGLBuffer**：这个类用于管理顶点缓冲对象（VBO），它允许你在GPU上存储顶点数据，以加速渲染过程。

5. **QOpenGLVertexArrayObject**：这个类用于管理顶点数组对象（VAO），它用于封装顶点属性设置，以便轻松地切换和共享渲染状态。

6. **QOpenGLTexture**：用于管理OpenGL纹理对象，允许你加载和绘制纹理。

7. **QOpenGLFramebufferObject**：这个类用于创建和管理帧缓冲对象（FBO），允许你在离屏渲染中进行高级渲染和后期处理。

8. **QOpenGLContext**：用于管理OpenGL上下文的类，通常不需要直接使用，因为QOpenGLWidget会处理上下文的创建和管理。

以上是一些用于在Qt中进行OpenGL绘制的常用接口和类。你可以根据你的需求选择适当的接口和类来实现你的OpenGL渲染代码。