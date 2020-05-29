<!-- TOC -->

- [递归遍历目录](#递归遍历目录)

<!-- /TOC -->

## 递归遍历目录

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
