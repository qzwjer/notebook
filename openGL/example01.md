```
#include <QOpenGLWidget>
#include <QOpenGLFunctions>
#include <QOpenGLShaderProgram>
#include <QOpenGLBuffer>
#include <QMatrix4x4>
#include <cmath>

class MyOpenGLWidget : public QOpenGLWidget, protected QOpenGLFunctions
{
public:
    MyOpenGLWidget(QWidget *parent = nullptr)
        : QOpenGLWidget(parent)
    {
    }

protected:
    void initializeGL() override
    {
        initializeOpenGLFunctions();
        glClearColor(0.0f, 0.0f, 0.0f, 1.0f);

        // 初始化着色器程序
        shaderProgram.addShaderFromSourceCode(QOpenGLShader::Vertex,
            "#version 330 core\n"
            "layout (location = 0) in vec2 position;\n"
            "void main()\n"
            "{\n"
            "    gl_Position = vec4(position.x, position.y, 0.0, 1.0);\n"
            "}\n");

        shaderProgram.addShaderFromSourceCode(QOpenGLShader::Fragment,
            "#version 330 core\n"
            "out vec4 fragColor;\n"
            "void main()\n"
            "{\n"
            "    fragColor = vec4(1.0, 0.0, 0.0, 1.0);\n"
            "}\n");

        shaderProgram.link();
    }

    void paintGL() override
    {
        glClear(GL_COLOR_BUFFER_BIT);

        // 使用着色器程序
        shaderProgram.bind();

        // 定义圆形的半径和圆心坐标
        float radius = 0.5f;
        float centerX = 0.0f;
        float centerY = 0.0f;

        // 设置圆形的片段数，影响圆的光滑度
        int segments = 100;

        // 计算并绘制圆形
        float angleIncrement = 2.0f * M_PI / static_cast<float>(segments);
        QVector<QVector2D> vertices;

        for (int i = 0; i <= segments; ++i)
        {
            float angle = static_cast<float>(i) * angleIncrement;
            float x = centerX + radius * std::cos(angle);
            float y = centerY + radius * std::sin(angle);
            vertices.append(QVector2D(x, y));
        }

        // 创建顶点缓冲对象
        QOpenGLBuffer vbo(QOpenGLBuffer::VertexBuffer);
        vbo.create();
        vbo.bind();
        vbo.allocate(vertices.constData(), vertices.size() * sizeof(QVector2D));

        // 启用顶点属性数组
        shaderProgram.enableAttributeArray(0);
        shaderProgram.setAttributeBuffer(0, GL_FLOAT, 0, 2);

        // 绘制圆形
        glDrawArrays(GL_TRIANGLE_FAN, 0, segments + 2);

        // 解绑VBO和着色器程序
        vbo.release();
        shaderProgram.disableAttributeArray(0);
        shaderProgram.release();
    }

    void resizeGL(int width, int height) override
    {
        glViewport(0, 0, width, height);
    }

private:
    QOpenGLShaderProgram shaderProgram;
};

int main(int argc, char *argv[])
{
    QApplication app(argc, argv);
    MyOpenGLWidget widget;
    widget.resize(800, 600);
    widget.show();
    return app.exec();
}
```
