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
* 如果ini文件本身不存在，只读，那么文件不会创建，除非我们有写入操作；
* QSettings::sync()的作用是：将所有未保存的内容进行保存，在对象析构时自动调用；所以有写操作时，如果我们使用临时变量，那么临时变量一旦析构，就会生成ini文件；但如果我们自己分配内存，并且还忘了释放空间，那么将不生成ini文件，此时可以调用sync()进行同步，但为了避免内存泄漏，建议使用QSharedPointer；
