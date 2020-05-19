------


# 打包编译

# maven

dependencyManagement:
	只是声明依赖，并不实现引入

dependencies:
	依赖都会自动引入，并默认被所有的子项目继承	

	org.springframework.beans.factory.support.DefaultListableBeanFactory@a307a8c: defining beans [org.springframework.context.annotation.internalConfigurationAnnotationProcessor,org.springframework.context.annotation.internalAutowiredAnnotationProcessor,org.springframework.context.annotation.internalCommonAnnotationProcessor,org.springframework.context.event.internalEventListenerProcessor,org.springframework.context.event.internalEventListenerFactory,simpleJdbcDemoApplication,org.springframework.boot.autoconfigure.internalCachingMetadataReaderFactory,batchFooDao,fooDao,simpleJdbcInsert,namedParameterJdbcTemplate,org.springframework.boot.autoconfigure.AutoConfigurationPackages,org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,org.springframework.boot.autoconfigure.condition.BeanTypeRegistry,propertySourcesPlaceholderConfigurer,org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,mbeanExporter,objectNamingStrategy,mbeanServer,org.springframework.boot.autoconfigure.jdbc.DataSourceConfiguration$Hikari,dataSource]; root of factory hierarchy



packing默认是jar类型，

<packaging>pom</packaging>   --------->   父类型都为pom类型

<packaging>jar</packaging>      --------->   内部调用或者是作服务使用

<packaging>war</packaging>    --------->   需要部署的项目



scope编译范围:
	1、compile: 缺省值，适用于所有阶段，项目测试、打包发布期间都有效

	2、provided: 已提供，该依赖包已经由目标容器（如tomcat）和JDK提供，只在编译、测试环境下有效，打包时不会加入。如servlet-api和jsp-api等jar

	3、test: 测试范围，只在测试环境下使用，用于编译和运行测试代码，项目打包时不会加入，如junit

	4、runtime: 运行范围，运行和测试环境使用，编译时用不到（不加入classpath），打包时候会加入

	5、system: 系统范围，与provided 类似，但是你必须显式的提供一个对于本地系统中JAR 文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库的一部分
  配合systemPath使用
  
  6、import：
  

<dependencyManagement>
      <dependencies>
        <dependency>
            <groupId>com.youzhibing.account</groupId>
              <artifactId>account-aggregator</artifactId>
              <version>1.0.0-SNAPSHOT</version>
              <type>pom</type>
              <scope>import</scope>
        </dependency>
      </dependencies>
  </dependencyManagement>
导入account-aggregator里定义的dependencyManagement，避免重复定义



mvn archetype:generate -DgroupId=com.uft.edas -DartifactId=pandora_boot  -Dversion=1.0.0-SNAPSHOT  -Dpackage=com.uft.edas



生成.ipr文件: mvn idea:project
生成.iws文件: mvn idea:workspace
生成.iml文件: mvn idea:module






## 自定义jar丢入私有仓库
mvn install:install-file -Dfile=/Users/yubin/workspace/coding/eap/out/artifacts/wdk_api/wdk_api.jar -DgroupId=com.longj -DartifactId=wdk_api -Dversion=1.0.0 -Dpackaging=jar



mvn install:install-file -Dfile=/Users/yubin/workspace/coding/eap/app/WEB-INF/lib/wdklib.jar -DgroupId=com.longj -DartifactId=wdklib -Dversion=1.0.0 -Dpackaging=jar


## 强制下载依赖的jar
mvn dependency:tree
mvn dependency:list 列出当前已解析依赖
mvn dependency:analyze 分析当前项目的依赖
mvn dependency:copy-dependencies
mvn dependency:resolve-plugins
mvn dependency:resolve  tells Maven to resolve all dependencies and displays the version



# ant
