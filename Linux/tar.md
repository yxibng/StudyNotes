

# tar 解压缩

- -j 标识 bzip2 压缩
- -z 标识 gzip 压缩
- -c 或者 --create 建立新的压缩文件
- -x 或者 --extract 解压文件
- -v 或者 --verbose 
- -t 或者 list 列出备份文件的内容
- -f <备份文件> 或者 --file=<备份文件>  指定备份文件
- -C <目录>：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项。


压缩

```
tar -cvf log.tar log2012.log    仅打包，不压缩！ 
tar -zcvf log.tar.gz log2012.log   打包后，以 gzip 压缩 
tar -jcvf log.tar.bz2 log2012.log  打包后，以 bzip2 压缩 
```

解压缩

```
tar -zxvf /opt/soft/test/log.tar.gz  解压 .gzip
tar -cxvf /opt/soft/test/log.tar.bz2 解压 .bzip2
tar -zxvf /opt/soft/test/log.tar.gz -C  /temp/dir 解压到/temp/dir
```

查看压缩包
```
tar -ztvf log.tar.gz
tar -jtvf log.tar.bz2
```

# zip 解压缩

1. 将 data 目录下的所有文件压缩到 file.zip ，不包含子目录

```
zip file.zip data/*
```
2. 将 data 目录下的所有文件压缩到 file.zip ，包含子目录, 递归压缩

```
zip -r file.zip data/*
```

3. 查看压缩文件

```
uzip -l file.zip
```

4. 解压文件

```
uzip file.zip
```

解压其中一个文件
```
uzip file.zip test.pdf
```

解压到目录

```
uzip file.zip -d /home/images
```

