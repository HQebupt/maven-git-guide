# Maven构建多模块Java工程

## 概述
1. 为构建多模块的maven工程做简单指导.
2. git简单的指导

## 软件环境
- JDK: 1.7
- Maven: 3.3.3
- OS: OSX

## 项目模块说明
示例工程分为多个模块，分别是：

* maven_git_guide：父工程，聚合各模块。
* App：主模块，依赖ModuleA和ModuleCom
* ModuleA：模块A，依赖ModuleCom 
* ModuleCom：公共模块

多模块项目整体构建使用Maven的父子继承能力。


## 创建工程
首先创建工程和各个模块,然后配置各模块之间的依赖关系. 项目的第三方构件的依赖关系配置在父工程的`pom.xml`中,
这样各个模块可以省去第三方构建的依赖配置.

### 1 创建父工程
`mvn archetype:create -DgroupId=com.github.hqebupt -DartifactId=maven-git-guide -DarchetypeArtifactId=maven-archet-quickstart`

### 2 修改父工程为Maven的父工程
创建成功后,需要将父工程改为Maven的父工程,修改`pom.xml`文件的 *packageing* 属性为`pom`, 如下
```
	<packaging>pom</packaging>
```

### 3 创建各模块工程
父工程创建成功后,创建各模块工程.分别创建模块 *app*, *module-a*, *module-com*.
```
cd maven_git_guide
mvn archetype:generate -DgroupId=com.github.hqebupt -DartifactId=app -DarchetypeArtifactId=maven-archetype-quickstart
mvn archetype:generate -DgroupId=com.github.hqebupt -DartifactId=module-a -DarchetypeArtifactId=maven-archetype-quickstart
mvn archetype:generate -DgroupId=com.github.hqebupt -DartifactId=module-com -DarchetypeArtifactId=maven-archetype-quickstart
```


创建各模块后，可以看到父工程*maven_git_guide* 的`pom.xml`文件中添加了子工程的信息：
```
<modules>  
    <module>app</module>  
    <module>module-a</module>  
    <module>module-b</module>  
    <module>module-com</module>  
</modules>  
```


### 4 配置各模块的依赖关系
创建各模块工程后，需要对各模块工程之间的依赖关系进行配置。如之前说明的模块依赖关系：
 1. ModuleA依赖ModuleCom
 2. 主模块app依赖ModuleA和ModuleCom

#### 配置ModuleA依赖ModuleCom
修改ModuleA的pom.xml文件，添加ModuleCom的依赖：
``` xml
<dependency>
    <groupId>com.github.hqebupt</groupId>
    <artifactId>module-com</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

#### 配置主模块app依赖ModuleA和ModuleCom
修改app的pom.xml文件，添加ModuleA和ModuleCom的依赖：
```
<dependency>
    <groupId>com.github.hqebupt</groupId>
    <artifactId>module-a</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>

<dependency>
    <groupId>com.github.hqebupt</groupId>
    <artifactId>module-com</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

## 编译工程
`mvn clean compile`

## 测试工程
`mvn clean test`

## 打包工程
`mvn clean package`

## 最佳实践
- 父工程不应该有代码，所有的代码都应该查分到子模块中开发
- 工程共同依赖的第三方构件应该在父工程的`pom.xml`中维护，各模块依赖的特殊构件可以在各模块的`pom.xml`中维护


## maven安装jar包到本地库或私服
### 1 安装到本地库
```
mvn install:install-file -DgroupId=com.github.hqebupt -DartifactId=app -Dversion=1.0 -Dpackaging=jar -Dfile=[path to file]
```
> 如果是在maven工程里面,直接使用`mvn install`便可以安装到本地.

### 2 安装到私服
```
mvn deploy:deploy-file -DgroupId=com.github.hqebupt -DartifactId=app -Dversion=1.0 -Dpackaging=jar -Dfile=[path to file] -Durl=[url] -DrepositoryId=[id]
```
> 如果是在maven工程里面,在`pom.xml`里面配置`distributionManagement`的信息, 直接用`mvn deploy`可行.

## 注意
1. 这个工程要先`mvn deploy`,将jar包安装到私服,才可以为别的模块使用.
2. `mvn assembly:assembly`需要所依赖的插件,还有第1步中的依赖.


