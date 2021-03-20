# IO

Node.js 是以 IO 密集型业务著称

> Buffer

> TypedArray

> String Decoder

# Stream

```
int copy(const char *src, const char *dest)
{
    FILE *fpSrc, *fpDest;
    char buf[BUF_SIZE] = {0};
    int lenSrc, lenDest;

    // 打开要 src 的文件
    if ((fpSrc = fopen(src, "r")) == NULL)
    {
        printf("文件 '%s' 无法打开\n", src);
        return FAILURE;
    }

    // 打开 dest 的文件
    if ((fpDest = fopen(dest, "w")) == NULL)
    {
        printf("文件 '%s' 无法打开\n", dest);
        fclose(fpSrc);
        return FAILURE;
    }

    // 从 src 中读取 BUF_SIZE 长的数据到 buf 中
    while ((lenSrc = fread(buf, 1, BUF_SIZE, fpSrc)) > 0)
    {
        // 将 buf 中的数据写入 dest 中
        if ((lenDest = fwrite(buf, 1, lenSrc, fpDest)) != lenSrc)
        {
            printf("写入文件 '%s' 失败\n", dest);
            fclose(fpSrc);
            fclose(fpDest);
            return FAILURE;
        }
        // 写入成功后清空 buf
        memset(buf, 0, BUF_SIZE);
    }

    // 关闭文件
    fclose(fpSrc);
    fclose(fpDest);
    return SUCCESS;
}
```

> stream 类型

| 类            |    使用场景    |             重写方法 |
| ------------- | :------------: | -------------------: |
| [Readable]()  |      只读      |               \_read |
| [Writable]()  |      只写      |              \_write |
| [Duplex]()    |      读写      |      \_read, \_write |
| [Transform]() | 操作被写入数据 | \_transform, \_flush |

# 缓冲区

> 可读流

当一个可读实例调用 stream.push() 方法的时候, 数据将会被推入缓冲区. 如果数据没有被消费, 即调用 stream.read() 方法读取的话, 那么数据会一直留在缓冲队列中. 当缓冲区中的数据到达 highWaterMark 指定的阈值, 可读流将停止从底层汲取数据, 直到当前缓冲的报备成功消耗为止.

> 可写流

在一个在可写实例上不停地调用 writable.write(chunk) 的时候数据会被写入可写流的缓冲区. 如果当前缓冲区的缓冲的数据量低于 highWaterMark 设定的值, 调用 writable.write() 方法会返回 true (表示数据已经写入缓冲区), 否则当缓冲的数据量达到了阈值, 数据无法写入缓冲区 write 方法会返回 false, 直到 drain 事件触发之后才能继续调用 write 写入.

```
// Write the data to the supplied writable stream one million times.
// Be attentive to back-pressure.
function writeOneMillionTimes(writer, data, encoding, callback) {
  let i = 1000000;
  write();
  function write() {
    var ok = true;
    do {
      i--;
      if (i === 0) {
        // last time!
        writer.write(data, encoding, callback);
      } else {
        // see if we should continue, or wait
        // don't pass the callback, because we're not done yet.
        ok = writer.write(data, encoding);
      }
    } while (i > 0 && ok);
    if (i > 0) {
      // had to stop early!
      // write some more once it drains
      writer.once('drain', write);
    }
  }
}
``
```

> pipe

stream 的 .pipe(), 将一个可写流附到可读流上, 同时将可写流切换到流模式, 并把所有数据推给可写流. 在 pipe 传递数据的过程中, objectMode 是传递引用, 非 objectMode 则是拷贝一份数据传递下去
[通过源码解析 Node.js 中导流（pipe）的实现](https://cnodejs.org/topic/56ba030271204e03637a3870)

> Console

```
let print = (str) => process.stdout.write(str + '\n');

print('hello world');
```

# File
