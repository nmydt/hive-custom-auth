# Hive自定义用户名密码验证
## 1，下载仓库并修改pom文件中jdk.tools的SyatemPath
## 2，上传打包好的jar包
打包项目并上传jar包到${HIVE_HOME}/lib目录下
## 3，配置user.password.conf
```shell
mkdir -p ${HIVE_HOME}/libexec/conf/
vim ${HIVE_HOME}/libexec/conf/user.password.conf
```
填写用户名和密码
```
user1,password1
user2,password2
user3,password3
```
## 4，配置${HIVE_HOME}/conf/hive-site.xml
添加以下参数
```xml
    <property>
        <name>hive.server2.authentication</name>
        <value>CUSTOM</value>
    </property>
    <property>
        <name>hive.server2.custom.authentication.class</name>
        <value>com.lylg.CustomHiveServer2Auth</value>
    </property>
    <property>
        <name>hive.server2.custom.authentication.file</name>
        <value>${HIVE_HOME}/libexec/conf/user.password.conf</value>
    </property>

```
## 5, 配置${HADOOP_HOME}/etc/hadoop/core-site.xml
其中确保Linux用户lylg存在并属于Superuser组，还属于HadoopXx机器。 否则，HDFS Namenode和YARN Resource Manager将无法正确处理ProxyUser配置。
```xml
<property>
    <name>hadoop.proxyuser.lylg.hosts</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.lylg.groups</name>
    <value>*</value>
</property>

```
详情：[Proxy user - Superusers Acting On Behalf Of Other Users](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/Superusers.html)

[HiveServer2 User Impersonation Issues](https://www.stefaanlippens.net/hiveserver2-impersonation-issues.html)

