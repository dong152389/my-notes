# 修改 Maven 下载源

Gradle 自带的 Maven 源地址是国外的，该 Maven 源在国内的访问速度是很慢的，除非使用了特别的手段。一般情况下，建议使用国内的第三方开放的 Maven 源或企业内部自建 Maven 源。

## 认识 init.d 文件夹

我们可以在 gradle 的 init.d 目录下创建以.gradle 结尾的文件，.gradle 文件可以实现在 build 开始之前执行，所以你可以在这个文件配置一些你想预先加载的操作。

## 在 init.d 文件夹创建 init.gradle 文件

~~~json
allprojects {
    repositories { 
        mavenLocal()
        maven { name "Alibaba" ; url "https://maven.aliyun.com/repository/public" } 
        maven { name "Bstek" ; url "https://nexus.bsdn.org/content/groups/public/" } 
        mavenCentral()
    }
    buildscript {
        repositories { 
            maven { name "Alibaba" ; url 'https://maven.aliyun.com/repository/public' } 
            maven { name "Bstek" ; url 'https://nexus.bsdn.org/content/groups/public/' } 
            maven { name "M2" ; url 'https://plugins.gradle.org/m2/' }
        }
    }
}
~~~

## 启用 init.gradle 文件的方法有

1. 在命令行指定文件,例如：gradle --init-script yourdir/init.gradle -q taskName。你可以多次输入此命令来指定多个init文件
2. 把init.gradle文件放到 USER_HOME/.gradle/ 目录下
3. 把以.gradle结尾的文件放到 USER_HOME/.gradle/init.d/ 目录下
4. 把以.gradle结尾的文件放到 GRADLE_HOME/init.d/ 目录下  

> 如果存在上面的4种方式的2种以上，gradle会按上面的1-4序号依次执行这些文件，如果给定目录下存在多个init脚本，会按拼音a-z顺序执行这些脚本， 每个init脚本都存在一个对应的gradle实例，你在这个文件中调用的所有方法和属性， 都会委托给这个gradle实例， 每个init脚本都实现了Script接口。  

## 仓库地址说明

* mavenLocal(): 指定使用maven本地仓库，而本地仓库在配置maven时settings文件指定的仓库位置。如E:/repository， gradle查找jar包顺序如下：USER_HOME/.m2/settings.xml >> M2_HOME/conf/settings.xml >> USER_HOME/.m2/repository。
* maven { url 地址：指定maven仓库，一般用私有仓库地址或其它的第三方库【比如阿里镜像仓库地址】。
* mavenCentral()：这是Maven的中央仓库，无需配置，直接声明就可以使用。
* jcenter()：JCenter中央仓库，实际也是是用的maven搭建的，但相比Maven仓库更友好，通过CDN分发，并且支持https访问，<font color="blue">在新版本中已经废弃了，替换为了mavenCentral()。</font>

> 总之, gradle可以通过指定仓库地址为本地maven仓库地址和远程仓库地址相结合的方式，避免每次都会去远程仓库下载依赖库。这种方式也有一定的问题，如果本地maven仓库有这个依赖，就会从直接加载本地依赖，如果本地仓库没有该依赖，那么还是会从远程下载。但是下载的jar不是存储在本地maven仓库中，而是放在自己的<font color="red">缓存目录</font>中， 默认在USER_HOME/.gradle/caches目录，当然如果我们配置过GRADLE_USER_HOME环境变量， 则会放在GRADLE_USER_HOME/caches目录，那么可不可以将gradle caches指向maven repository。 但这是不行的， caches下载文件不是按照maven仓库中存放的方式。  

## 阿里云仓库地址

[使用指南](https://developer.aliyun.com/mvn/guide)。可以直接复制阿里提供的init.gradle。

![](./assets/Snipaste_2022-08-29_10-02-12.png)

![](./assets/Snipaste_2022-08-29_10-04-18.png)



~~~json
allprojects {
    /* 通常仓库放在这里 */
    repositories { 
        mavenLocal()
        maven { name "Alibaba" ; url "https://maven.aliyun.com/repository/public" }
        maven { name "Bstek" ; url "https://nexus.bsdn.org/content/groups/public/" }
        maven { name "Spring" ; url "https://maven.aliyun.com/repository/spring" }
        maven { name "Google" ; url "https://maven.aliyun.com/repository/google" }
        mavenCentral()
    }
    /* 通常插件放在这里 */
    buildscript {
        repositories {
            maven { name "Alibaba" ; url 'https://maven.aliyun.com/repository/public' } 
            maven { name "Bstek" ; url 'https://nexus.bsdn.org/content/groups/public/' } 
            maven { name "M2" ; url 'https://plugins.gradle.org/m2/' }
            maven { name "Spring-Plugin" ; url 'https://maven.aliyun.com/repository/spring-plugin' }
            maven { name "Gradle-Plugin" ; url 'https://maven.aliyun.com/repository/gradle-plugin' }
        }
    }
}
~~~
