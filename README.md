# HadoopClientWin10Env

## 客户端环境准备

### windows 环境配置

windows 下客户端环境准备

1. 配置好java 环境，要求jdk1.8及以上版本
2. 将hadoop 压缩包，配置HADOOP_HOME环境变量

![a123569c1645aa8f9ee2fadc8263a200.png](evernotecid://C3A6FC58-7944-4D08-A565-9CB56987F3D5/appyinxiangcom/2253078/ENResource/p1552)

3. 配置Path环境变量。如果环境变量不起作用，可以重启电脑试试。

![eb192d14658a295ed07a2c029cfe96bf.png](evernotecid://C3A6FC58-7944-4D08-A565-9CB56987F3D5/appyinxiangcom/2253078/ENResource/p1553)

验证Hadoop环境变量是否正常。双击winutils.exe，如果报如下错误。说明缺少微软运行库（正版系统往往有这个问题）。再资料包里面有对应的微软运行库安装包双击安装即可。

![d18a977ed9e0f766f705983b3106e988.png](evernotecid://C3A6FC58-7944-4D08-A565-9CB56987F3D5/appyinxiangcom/2253078/ENResource/p1554)


### idea 工程准备

1. 在IDEA中创建一个Maven工程HdfsClientDemo，并导入相应的依赖坐标+日志添加

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client</artifactId>
        <version>3.1.3</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.30</version>
    </dependency>
</dependencies>
```

2. 在项目的 `src/main/resources` 目录下，新建一个文件，命名为 `log4j.properties`，在文件中填入

```properties
log4j.rootLogger=INFO, stdout  
log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n  
log4j.appender.logfile=org.apache.log4j.FileAppender  
log4j.appender.logfile.File=target/spring.log  
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout  
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```

3. 创建包名：`com.atguigu.hdfs` 和 `HdfsClient` 类

```java
public class HdfsClient {

    @Test
    public void testMkdirs() throws IOException, URISyntaxException, InterruptedException {

        // 1 获取文件系统
        Configuration configuration = new Configuration();

        // FileSystem fs = FileSystem.get(new URI("hdfs://hadoop102:8020"), configuration);
       f
        // 2 创建目录
        fs.mkdirs(new Path("/xiyou/huaguoshan/"));

        // 3 关闭资源
        fs.close();
    }
}
```

注意：

客户端去操作HDFS时，是有一个用户身份的。默认情况下，HDFS客户端API会从采用Windows默认用户访问HDFS，会报权限异常错误。所以在访问HDFS时，一定要配置用户。

```txt
org.apache.hadoop.security.AccessControlException: Permission denied: user=56576, access=WRITE, inode="/xiyou/huaguoshan":atguigu:supergroup:drwxr-xr-x
```
