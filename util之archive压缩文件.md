本质是调用tarfile和zipfile完成解压
全局的extract作为入口函数，传入压缩文件和想要解压到的位置
压缩文件的类型有很多种，通过Archive类判断文件的扩展名(extension)，找到对应类如：TarArchive，ZipArchive
压缩包内部的文件可能是:
```
/oo/1.txt
/oo/2.txt
/oo/dd/3.txt
```
也有可能是
```
1.txt
2.txt
dd/3.txt
```

有引导目录的要去掉第一层引导目录split_leading_path(path)。
首先判断是否有leading目录has_heading_dir(path):所有文件在开头有相同的目录，该目录被叫做leading_dir,如上面的/oo/
由于以上两个函数在TarArchive，ZipArchive中都要用到，设置了一个BaseArchive基类
