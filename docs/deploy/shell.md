> 创建 /data/test 目录

```
mkdir /data/test
```

> 创建 /data/newtest 目录

```
mkdir /data/newtest
```

> 创建 /data/test/index.html 文件

```
touch /data/test/index.html
```

> 编辑文件

```
vi /data/test/index.html
```

> 查看文件内容

```
cat /data/www/index.html
```

> 将 `test`目录下的所有文件复制到 `newtest`目录 下

```
cp –r /data/test/\* /data/newtest
```

> 删除 /data/newtest/index.html 文件

```
rm -rf /data/newtest/index.html
rm -rf 文件夹名
```

> 将 `test`目录下的所有文件移动到新目录 `newtest`目录 下

```
mv /data/test/\* /data/newtest
```

> 检查端口被哪个进程占用

```
netstat -lnp|grep 88   #88请换为你需要的端口，如：80
```

> 杀掉编号为 1777 的进程（请根据实际情况输入）

```
kill -9 1777
```
