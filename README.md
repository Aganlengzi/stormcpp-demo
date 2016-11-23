http://blog.csdn.net/aganlengzi/article/details/53261391

默认已经安装好apache-storm-1.0.2和apache-maven3

主要参照：
```
[1] http://blog.csdn.net/jmppok/article/details/16827837 
[2] http://demeter.inf.ed.ac.uk/cross/stormcpp.html 
[3] http://blog.csdn.net/lybingo/article/details/52873673 
```
###1.工程文件放置：
```
cd /path/to/apache-storm-1.0.2/examples/storm-starter/multilang/resources
git clone git@github.com:Aganlengzi/stormcpp-demo.git
cd stormcpp-demo
mv WordCountTopologyCpp.java /path/to/storm-starter/src/jvm/org/apache/storm/starter/
mv *.sh ../
```
###2.jsoncpp配置
下载Jsoncpp-src-0.6.0-rc2并解压至/path/to/Jsoncpp-src-0.6.0-rc2
百度网盘链接：http://pan.baidu.com/s/1dE4j8jR
下载scons-2.1.0并解压至/path/to/scons-2.1.0
```
wget http://sourceforge.net/projects/scons/files/scons/2.1.0/scons-2.1.0.tar.gz/download
tar –xvf scons-2.1.0.tar.gz
vi /etc/profile
export MYSCONS=/path/to/scons-2.1.0
export SCONS_LIB_DIR=$MYSCONS/engine
source /etc/profile
```
###3.编译jsoncpp
```
cd /path/to/Jsoncpp-src-0.6.0-rc2
python /path/to/scons-2.1.0/script/scons platform=linux-gcc
```
将生成的库文件拷到stormcpp的deps中
```
cd /path/to/ Jsoncpp-src-0.6.0-rc2/libs/linux-gcc-xxx
cp ./libjson_linux-gcc-x.x.x_libmt.a /path/to/storm-starter/multilang/resources/stormcpp/deps/libjson-cpp.a
cp ./libjson_linux-gcc-x.x.x_libmt.so /path/to/storm-starter/multilang/resources/stormcpp/deps/libjson-cpp.so
```
###4.编译生成可执行文件
并将其与shell文件放到/path/to/storm-starter/multilang/resources
```
cd /path/to/storm-starter/multilang/resources/stormcpp-demo
cd build
cmake ..
make
```
成功后在/path/to/storm-starter/multilang/resources/中会看到shell文件和可执行文件。

###5.打包和执行
利用maven打包storm-starter，如果有权限问题，利用sudo提升当前账户权限
构建和打包
```
mvn clean install -DskipTests=true 
```
在/path/to/storm-starter/target/下会看到最新的jar包其中已经包含我们添加的WordCountTopologyCpp
运行
```
cd /path/to/storm/bin
./storm jar path/to/generated.jar org.apache.storm.starter.WordCountTopologyCpp
```
###6.查看结果
通过打印信息和SplitSentence.h中指定的文件路径查看

其他参照http://blog.csdn.net/aganlengzi/article/details/53261391
