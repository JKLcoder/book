# QFileInfo

```c++
//以QFileInfo fi("c:/tmp/archive.tar.gz")为例
QString QFileInfo::canonicalPath() const;  // 返回"c:/tmp"
QString QFileInfo::baseName() const;       // 返回"archive"
QString QFileInfo::suffix() const;         // 返回"tar.gz"
```

# QDir

```c++
void QDir::setSorting(SortFlags sort);  //排序
void QDir::setFilter(Filters filters);  //过滤
QFileInfoList QDir::entryInfoList(Filters filters = NoFilter, SortFlags sort = NoSort) const; //获取该目录下的文件列表
QStringList QDir::entryList(const QStringList &nameFilters, Filters filters = NoFilter, SortFlags sort = NoSort) const; //获取该目录下的文件列表
```

Note:

- entryInfoList 和 entryList 存在区别，前者返回的是文件的绝对路径，后者则是 basename，并且 entryInfoList 中即使设置不排序，其获取的列表也和系统中本来的排序可能不同，目前有一种解决思路是：使用 entryList，然后使用 soft 排序，自定义 soft 规则；
