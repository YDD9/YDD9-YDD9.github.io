# Basica tutorial to setup environment on linux   
https://www.datacamp.com/community/tutorials/apache-spark-python   
   
# Part Java JDK   
Open terminal in linux:   
verify if you have already java jdk `java -version`   
if you want to use a newer version, please uninstall old ones.   
   
To uninstall Java: https://www.java.com/en/download/help/linux_uninstall.xml   
type: `rpm -e jre--fcs`   
   
Unpack the tarball and install the JDK.   
   
`tar zxvf jdk-8uversion-linux-x64.tar.gz`     
The Java Development Kit files are installed in a directory called jdk1.8.0_version in the **current** directory.   
   
Delete the .tar.gz file if you want to save disk space.   
   
official guide: https://docs.oracle.com/javase/8/docs/technotes/guides/install/linux_jdk.html#BJFJJEFG      
Java guide from elastic search: https://docs.wso2.com/display/ELB201/Installing+Java+Development+Kit+%28JDK%29+on+Linux   
   
   
# Part Spark   
download https://www.apache.org/dyn/closer.lua/spark/spark-2.2.0/spark-2.2.0-bin-hadoop2.7.tgz     
unzip `tar zxvf spark-2.2.0-bin-hadoop2.7.tgz`   
type `mv spark-2.1.0-bin-hadoop2.7 /usr/local/spark`   
   
Open README.MD in /usr/local/spark and follow instruction to build if needed.   
   
start pyspark shell interface, go to path /usr/local/spark, type `./bin/pyspark`      
start pyspark web interface that the application UI is available at localhost:4040, type `./bin/pyspark --master local[*]`   
   
Set up path: http://tsarx.com/2017/06/how-to-set-path-for-apache-spark-in-linux/   
```   
SPARK_HOME="/usr/local/spark"   
export SPARK_HOME   
PATH=$PATH:$SPARK_HOME/bin   
   
# similar for JAVA_HOME   
export JAVA_HOME="path that you found"   
export PATH=$JAVA_HOME/bin:$PATH   
```   
   
https://unix.stackexchange.com/questions/129143/what-is-the-purpose-of-bashrc-and-how-does-it-work   
export PATH="$PATH:/some/addition"     
If you put above in .bashrc instead, every time you launched an interactive sub-shell, :/some/addition would get tacked on to the end of the PATH again.   
   
   
# Use Jupyter Notebook python web sdk tool   
as new Jupyter has stop supporting Python2.7, please use python3. Jupyter tutorial https://www.datacamp.com/community/tutorials/tutorial-jupyter-notebook    
install via Anaconda: download https://www.anaconda.com/download/#linux and install https://conda.io/docs/user-guide/install/linux.html   
type: `bash Anaconda-latest-Linux-x86_64.sh`  you can uninstall old conda if you like.   
then make the installation path in ~/.bashrc type `export PATH=/predix/anaconda3/bin:$PATH`   
then activate this file `source ~/.bashrc`   
   
   
# Spark in Jupyter   
Method 2 — FindSpark package https://blog.sicara.com/get-started-pyspark-jupyter-guide-tutorial-ae2fe84f594f   
```   
pip install findspark   
jupyter notebook   
```   
   
inside your jupyter notebook   
```   
import findspark   
findspark.init()   
   
import pyspark   
   
def fn():   
    pass   
```   
or just indicate clearly the path `findspark.init('/path/to/spark_home')`   
   
   
# Solve pyspark ip loopback issue   
If you run your env in a virtualbox linux, you don't have localhost as ip for pyspark   
To change IP: add the SPARK_LOCAL_IP in the /usr/local/spark/spark-env.sh file `export SPARK_LOCAL_IP="<IP address>"`   
This file is located at $SPARK_HOME/conf/, where $SPARK_HOME is the location of your Apache Spark installation.    
https://support.datastax.com/hc/en-us/articles/204675669-Spark-hostname-resolving-to-loopback-address-warning-in-spark-worker-logs   

example file  https://gist.github.com/berngp/10793284   

You will find a template spark-env.sh.template and use it.   
```   
WARN Utils: Your hostname, Linuxdevbox resolves to a loopback address: 127.0.0.1; using 10.0.2.15 instead (on interface enp0s3)   
17/12/07 16:17:50 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address   
```   
   
# Further troubleshoot   
WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable   
   
https://stackoverflow.com/questions/40015416/spark-unable-to-load-native-hadoop-library-for-your-platform

set HADOOP_HOME to point to that directory.
add $HADOOP_HOME/lib/native to LD_LIBRARY_PATH
   
