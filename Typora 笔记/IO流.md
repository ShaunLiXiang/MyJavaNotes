# IO流

1字符=俩字节，1字节=8bit

## 字节流

InputStream OutPutStream

```java
byte bufferZone []= new byte [1024];
FileInputStream is = new FileInputStream("a.txt");//读取文件
OutPutStream ops = new OutPutStream("target.txt");
int len = 0;
while( len = is.read(bufferZone) != -1){
    ops.write(len);
}
ops.close();
is.close();
//先开的后关
//读完了不一定写完
//写完了一定读完
```



## 字符流

**Reader     And     Writer**

FileReader fr = new FileRreader("a.txt");

char [] buffer = new char[1024];

int len = 0;

while( len = fr.read(buffer) !=-1 ){

​	sout(new String(buffer, 0 ,len));

}

fr.close();

FileWriter

写入后需要invoke一下flush（）将读取到内存中的字符给它刷新到目标文件中；

释放资源