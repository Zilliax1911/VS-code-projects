Qt项目工作中相关问题及解决

## 界面ui部分
### QLabel相关
1. 使字体居中：在右边属性管理器，找到‘alignment'，下拉菜单中水平部分选择AlignCenter
1. 代码实现：

```c++
MainWindow::MainWindow(QWidget *parent) :
QMainWindow(parent),
ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    QWidget *widget = new QWidget(this);
    this->setCentralWidget(widget);
	QLabel *label = new QLabel(widget);
	label->setText("Demo_QLabel");
	label->setAlignment(Qt::AlignCenter);//居中显示
	QGridLayout *gridlayout = new QGridLayout(widget);
	gridlayout->addWidget(label,0,0,1,1);//将QLabel添加到网格布局的0行0列，占用1行1列
	widget->setLayout(gridlayout);
}
```
2.1 设置字体

```c++
MainWindow::MainWindow(QWidget *parent) :
QMainWindow(parent),
ui(new Ui::MainWindow)
{
ui->setupUi(this);
QWidget *widget = new QWidget(this);
this->setCentralWidget(widget);
QLabel *label = new QLabel(widget);
label->setText("Demo_QLabel");
label->setAlignment(Qt::AlignCenter);//居中显示
//设置字体
QFont font("微软雅黑",22,QFont::Bold,true);
label->setFont(font);
 
//设置字体颜色，常用的两种方法//    QPalette plt;
//    plt.setColor(QPalette::WindowText,QColor(Qt::red));//1.利用QPalette调色板设置字体颜色
//    label->setPalette(plt);
 label->setStyleSheet("color:red");//2.利用样式表设置字体颜色，如果对样式表熟悉也可以使用样式表设置字体风格、边框等等
 label_image->setStyleSheet("image: url(:/img/积跬步至千里.png)");   //显示图片方法2：利用样式表，特点：图片原始长宽比例始终不变

QGridLayout *gridlayout = new QGridLayout(widget);
gridlayout->addWidget(label,0,0,1,1);//将QLabel添加到网格布局的0行0列，占用1行1列
widget->setLayout(gridlayout);
}
```

## 界面相关

### 主窗口与子窗口相互切换

1. 主界面.cpp写槽函数：点击主界面按钮响应子界面的槽函数
```C++
void MainWindow::on_pushButton_clicked()
{
    this->hide();//主界面隐藏
    video *videodlg = new video(this);//新建子界面
connect(videodlg,SIGNAL(sendsignal()),this,SLOT(reshow()));//当点击子界面时，调用主界面的reshow()函数
    videodlg->setGeometry(this->geometry());	//获取当前窗口的xy值，设置窗口
    videodlg->show();//子界面出现
}
void reshow()
{
	this->show();
}
```
2. 子窗口头函数中，声明一个槽函数（点击显示父窗口），声明一个信号（点击后发送）
```C++
#ifndef VIDEO_H
#define VIDEO_H
#include <QDialog> 
namespace Ui {
class video;
}
class video : public QDialog
{
    Q_OBJECT
 
public:
    explicit video(QWidget *parent = 0);
    ~video();
private slots:
    void on_pushButton_clicked();
signals:
    void sendsignal();//这个函数用户向主界面通知关闭的消息
private:
    Ui::video *ui;
};
#endif // VIDEO_H
```
3. 子界面.cpp：实现声明的槽函数：
```C++
void son::on_pushButton_clicked()
{
	emit sendsignal();	//发送声明的信号
	this->close();	//
}
```

 ### a.setStyle(QStyleFactory::create("Windows"));

1. 参数有：WindowsXP、Windows、Macintosh、	

### QStringList 的笔记
#### QStringList 初始化
```C++
QStringList qstrList;
qstrList<<"Android" << "Qt Creator" << "Java" << "C++";
QStringListIterator strIterator(qstrList);
while (strIterator.hasNext())
    qDebug()<<strIterator.next()<<endl;
```

#### 增加字符串 append()

- QStringList可以通过append(),或者使用<<来添加List元素，如

  ```C++
  qstrList.append("python");
  qstrList << "PHP";
  ```

  

### QString

#### QString 的 arg ( ) 方法

QString.arg()用于填充字符串中的**%1**，**%2** ...为给定的参数，如

`QString m = tr("%1:%2:%3").arg("12").arg("60").arg("80");		//m = "12:60:80"`

它还有另外一种重载方法：

`QString QString::arg(int a, int fieldWidth = 0, int base = 10, QChar fillChar = QLatin1Char(''))const `

其中第一个参数是要填充的数字，第二个参数是最小宽度（位数），第三个参数是进制，第四个参数是当原始数字的宽度不足时用于填充的字符。

如

```c++
    QString text = QString("%1:%2")
                   .arg(123, 5, 10, QChar('0'))
                   .arg(456, 5, 10, QChar('0'));    // text = "00123:00456"
```



#### 首部添加和尾部添加

- `str.append("abc");`添加在尾部；
- `str.prepend("abc");`添加在头部；

#### QString 的toLocal8bit与toLatin1的区别

问题：将QString赋值给char数组。

方法：QString ---> QByteArray ---> char数组

```c++
QString str("123456");
QByteArray arr = str.toLocal8Bit();	//Unicode编码
QByteArray arr = str.toLatin1();	//ASCII编码

char data[64]{0};
strcpy_s(data, arr.size() + 1, arr.data());
```



#### QString 转为 QDateTime

```C++
//头文件
#include <QTime>
QString str = "2022-09-14 13:32:44";
QDateTime _time = QDateTime::fromString(str, "yyyy-MM-dd hh:mm:ss");
auto secs = _time.toTime_t();	//到1970之间的秒数
```

- 获取从1970-01-01 起经过的秒数

```C++
#include <QDateTime>
#include <QString>

QDateTime current_date_time = QDateTime::currentDateTime();
QString current_date = current_date_time.toString("yyyy-MM-dd hh:mm:ss");
long Secnum = current_date_time.toMSecsSinceEpoch();
qDebug()<<current_date<<"=-=-=-=-=-=-=-=-="<<current_date_time.currentMSecsSinceEpoch()<<"Secnum = " << Secnum << endl;
```



#### 将字符串时、分、秒转为秒单位时间

```C++
 QString time_str="01:20:30"; //时分秒
QTime time=QTime::fromString(time_str);
qDebug()<<"时:"<<time.hour();
qDebug()<<"分:"<<time.minute();
qDebug()<<"秒:"<<time.second();
qDebug()<<"总秒数:"<<time.hour()*60*60+time.minute()*60+time.second();
/*
时: 1
分: 20
秒: 30
总秒数: 4830
*/
```





### QProcess的使用

#### 作用

- 有时需要在应用程序中调用外部的编译器，执行相关指令并获得结果。

- `QProcess`类可以使得在应用程序中启动一个外部程序并与之交互通信。主要包括三种启动外部命令的方式：

  - ##### QProcess::start()

    一体式，会随着主程序的退出而退出

  - ##### QProcess::startDetached()

    分离式，当主程序退出时并不退出，而是继续运行

  - ##### QProcess::execute()

    阻塞



#### 可执行文件.exe的生成

- Release执行源代码，会生成一个release文件夹
- 找到其中的.exe后缀文件，复制到空白文件夹下
- 打开Qt终端，切换到上一步的文件夹下
- 输入命令`windeployqt 程序名.exe` （目的是：将bin文件中的.dll文件拷贝封装到.exe文件中）
- 回到文件夹，程序可执行
