
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

仓库分类

- 本地仓库：当前电脑上的仓库

- 远程仓库
    - 私服：搭建在局域网中，一般公司都会有私服，私服一般使用nexus来搭建
    - 中央仓库：架设在Internet上，像刚才的springframework就是在中央仓库上

maven坐标和仓库对应的映射关系：[groupId][artifactId][version][artifactId]-[version].jar

## 依赖

### 查找顺序

maven解析依赖信息时会到本地仓库中取查找被依赖的jar包对于本地仓库中没有的会去
中央仓库去查找maven坐标来获取jar包，获取到jar之后会下载到本地仓库对于中央仓
库也找不到依赖的jar包的时候，就会编译失败了

如果依赖的是自己或者团队开发的maven工程，需要先使用install命令把被依赖的maven
工程的jar包导入到本地仓库中

比如现在我再创建第二个maven工程HelloFriend，其中用到了第一个Hello工程里类的
sayHello(String name)方法。我们在给HelloFriend项目使用 mvn compile 命令
进行编译的时候，会提示缺少依赖Hello的jar包。

怎么办呢？到第一个maven工程中执行 mvn install 后，你再去看一下本地仓库，你会
发现有了Hello项目的jar包。一旦本地仓库有了依赖的maven工程的jar包后，你再到
HelloFriend项目中使用 mvn compile 命令的时候，可以成功编译
