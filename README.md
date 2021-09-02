
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

### 依赖范围

`<scope></scope>`

- compile，默认值，适用于所有阶段（开发、测试、部署、运行），本jar会一直存在所有阶段。
  
- provided，只在开发、测试阶段使用，目的是不让Servlet容器和本地仓库的jar包冲突，如servlet.jar。
  
- runtime，只在运行时使用，如JDBC驱动，适用运行和测试阶段。
  
- test，只在测试时使用，用于编译和运行测试代码，不会随项目发布。
  
- system，类似provided，需要显式提供包含依赖的jar，Maven不会在Repository中查找它。

## 生命周期

- Clean Lifecycle

- Default LifeCycle

- Site LifeCycle

## 高级特性

- 依赖的传递性
  - 路径最短者优先
  - 路径相同先声明优先原则
  
- 统一管理依赖的版本
  - 为了统一管理版本号，可以使用properties标签，里面可以自定义版本的标签名。在使用的地方使用${自定义标签名}
  
## build

```xml
作者：芋道源码
链接：https://zhuanlan.zhihu.com/p/150709572
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

<build>
  <!-- 项目的名字 -->
  <finalName>WebMavenDemo</finalName>
  <!-- 描述项目中资源的位置 -->
  <resources>
    <!-- 自定义资源1 -->
    <resource>
      <!-- 资源目录 -->
      <directory>src/main/java</directory>
      <!-- 包括哪些文件参与打包 -->
      <includes>
        <include>**/*.xml</include>
      </includes>
      <!-- 排除哪些文件不参与打包 -->
      <excludes>
        <exclude>**/*.txt</exclude>
          <exclude>**/*.doc</exclude>
      </excludes>
    </resource>
  </resources>
  <!-- 设置构建时候的插件 -->
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>2.1</version>
      <configuration>
        <!-- 源代码编译版本 -->
        <source>1.8</source>
        <!-- 目标平台编译版本 -->
        <target>1.8</target>
      </configuration>
    </plugin>
    <!-- 资源插件（资源的插件） -->
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-resources-plugin</artifactId>
      <version>2.1</version>
      <executions>
        <execution>
          <phase>compile</phase>
        </execution>
      </executions>
      <configuration>
        <encoding>UTF-8</encoding>
      </configuration>
    </plugin>
    <!-- war插件(将项目打成war包) -->
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-war-plugin</artifactId>
      <version>2.1</version>
      <configuration>
        <!-- war包名字 -->
        <warName>WebMavenDemo1</warName>
      </configuration>
    </plugin>
  </plugins>
</build>
```

配置好build后，执行mvn package之后，在maven工程指定的target目录里war包和文件都按照配置的生成了
  

# Ref

1、https://www.zhihu.com/search?type=content&q=maven