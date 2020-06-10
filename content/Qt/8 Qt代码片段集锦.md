# 递归遍历目录

```c++
//参数含义（当前遍历目录、当前遍历目录上一级目录name、记录当前目录相对于顶级父目录深度）
bool reverseDir(const QDir& dir,QString parentName,int parentLevel)
{
    QString dirname = parentName + QString("%1/").arg(dir.dirName());
    parentLevel++;
    QFileInfoList list = dir.entryInfoList();

    for(QFileInfo &info : list)
    {
        //在Qt10.1中发现，如果不过滤将会有这两个目录，可以认为是父目录和二级父目录，如果遍历当前目录则必须过滤掉
        if(!QString::compare(info.fileName(),".") || !QString::compare(info.fileName(),".."))
            continue;

        if(info.isDir()) //当前为目录
        {
            ...
            reverseDir(QDir(info.filePath()),dirname,parentLevel,in);

        }
        else //当前为文件
        {
           ...
        }
    }
    return 1;
}
```

# ini 文件读写

```c++
QSettings setting(QString("path.ini"),QSettings::IniFormat);
setting.setValue("path/content-path","");  //写
contentPath = setting.value("path/content-path").toString();  //读
```

Note：

- 如果 ini 文件本身不存在，只读，那么文件不会创建，除非我们有写入操作；
- QSettings::sync()的作用是：将所有未保存的内容进行保存，在对象析构时自动调用；所以有写操作时，如果我们使用临时变量，那么临时变量一旦析构，就会生成 ini 文件；但如果我们自己分配内存，并且还忘了释放空间，那么将不生成 ini 文件，此时可以调用 sync()进行同步，但为了避免内存泄漏，建议使用 QSharedPointer；

# 常用路径获取

```c++
QString QCoreApplication::applicationDirPath();                     //获取当前运行exe所在文件夹的绝对路径
qApp->applicationFilePath();                                        //获取程序完整名称
QString QDir::currentPath();                                        //获取当前工作目录
QDir::homePath();                                                   //获取当前用户目录
QStandardPaths::standardLocations(QStandardPaths::DesktopLocation); //获取桌面路径
QStandardPaths::writableLocation(QStandardPaths::AppConfigLocation);//程序数据存放路径
QDir::tempPath();                                                   //临时文件路径
```

Note:

- 程序数据存放路径，由 Qt 给我们自动分配的，例如针对程序 WriteSummary，对应数据存放路径是："C:/Users/ritu/AppData/Local/WriteSummary"

# 文件(夹)相关操作

```c++
QFile::exists("c/users/administrator/desktop/2.png");             //判断文件是否存在
QFile::permissions("c:/users/administrator/desktop/2.png");       //判断文件读写权限
QFile::remove("c:/users/administrator/desktop...");               //删除文件
QFile::remove(oldname,newname);                                   //重命名文件
QFile::copy(oldname,newname));                                    //拷贝文件

bool QDir::exists() const;                                        //判断文件夹是否存在
bool QDir::mkpath(const QString &dirPath) const;                  //创建文件夹包括其涉及到的父目录
bool QDir::rmdir(const QString &dirName) const;                   //删除目录，前提是它们为空
bool QDir::rmpath(const QString &dirPath) const;                  //删除目录包括其设计到的父目录，前提是它们为空
bool QDir::rename(const QString &oldName, const QString &newName);//重命名文件夹
```

Note:

- 注意 rmdir 和 rmpath 的区别，前者只删除当前这个目录，而后者会把这个路径涉及到的所有父目录都删除，当然删除时都要求目录为空；redir 似乎在删除时更安全；
- Qt 中文件夹的拷贝并没有像文件拷贝一样提供 copy 函数，这要求我们如果要拷贝文件夹那么就要遍历文件夹并在新路径下创建必要的文件夹，然后再拷贝文件；
