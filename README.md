
> maven工程目录结构
> 
> 根目录：工程名
> 
> |---src：源码
> 
> |---|---main:存放主程序
> 
> |---|---|---java：java源码文件
> 
> |---|---|---resource：存放框架的配置文件
> 
> |---|---test：存放测试程序
> 
> |---pop.xml：maven的核心配置文件

maven仓库默认地址~/.m2/repository

compile --> target

package --> jar包

clean --> 清除target

## 仓库和坐标

pom.xml(Project Object Model, 项目对象模型)是maven的核心配置文件，所有构建的配置都在这里

使用下面的三个向量在仓库中唯一定位一个maven工程

```pom
<dependency>
    <!-- 1、公司或组织域名倒序+项目名 -->
    <groupId>commons-io</groupId>
    <!-- 2、模块名 -->
    <artifactId>commons-io</artifactId>
    <!-- 3、版本 -->
    <version>2.4</version>
</dependency>
```
