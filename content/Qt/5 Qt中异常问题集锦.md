# 中文读写乱码

&emsp;&emsp;设置本地编码为 utf-8 即可，如下：

```c++
QTextCodec::setCodecForLocale(QTextCodec::codecForName("Utf-8"));
```

# 隐藏菜单 QMenu

```c++
QMenu::menuAction()->setVisible(false);
```

&emsp;&emsp;Note:如果直接使用 QMenu->setVisible(false)是无法隐藏的；
