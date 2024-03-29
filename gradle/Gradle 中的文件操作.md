# Gradle 中的文件操作

几种常见的文件操作方式：

* 本地文件
* 文件集合
* 文件树
* 文件拷贝
* 归档文件

## 本地文件

使用 Project.<font color="red">file(java.lang.Object)</font>方法，通过指定文件的<font color="red">相对路径或绝对路径</font>来对文件的操作,其中相对路径为相对当前project[根 project 或者子 project]的目录。<font color="red">其实使用 Project.file(java.lang.Object)方法创建的 File 对象就是 Java 中的 File 对象</font>，我们可以使用它就像在 Java 中使用一样。示例代码如下：  

~~~groovy
//使用相对路径
File configFile = file('src/conf.xml')
configFile.createNewFile()
// 使用绝对路径
configFile = file('D:\\conf.xml')
println(configFile.createNewFile())
// 使用一个文件对象
configFile = new File('src/config.xml')
println(configFile.exists())
~~~

## 文件集合

文 件 集 合 就 是 一 组 文 件 的 列 表 , 在 Gradle 中 , 文 件 集 合 用 FileCollection 接 口 表 示 。 我 们 可 以 使 用
Project.files(java.lang.Object[])方法来获得一个文件集合对象，如下代码创建一个 FileCollection 实例：

~~~groovy
def collection = files('src/test1.txt', new File('src/test2.txt'), ['src/test3.txt', 'src/test4.txt'])

collection.forEach() { File it ->
    it.createNewFile() //创建该文件
//    println it.name //输出文件名
}
Set set1 = collection.files // 把文件集合转换为java中的Set类型
Set set2 = collection as Set
List list = collection as List// 把文件集合转换为java中的List类型
//for (item in list) {
//    println item.name
//}
def union = list + files('src/test5.txt') // 添加或者删除一个集合
def minus = union - files('src/test3.txt')

minus.forEach() { File it ->
    println it.name
}
~~~

对于文件集合我们可以遍历它；也可以把它转换成 java 类型；同时还能使用+来添加一个集合，或使用-来删除集合。

## 文件树

<font color="red">文件树是有层级结构的文件集合</font>，一个文件树它可以代表一个目录结构或一 ZIP 压缩包中的内容结构。<font color="blue">文件树是从文件集合继承过来的</font>，<font color="red">所以文件树具有文件集合所有的功能</font>。我们可以使用 <font color="blue">Project.fileTree(java.util.Map)</font>方法来创建文件树对象，还可以使用过虑条件来包含或排除相关文件。示例代码如下

~~~groovy
// 第一种方式:使用路径创建文件树对象，同时指定包含的文件
def tree = fileTree('src/main').include('**/*.java')
//第二种方式:通过闭包创建文件树:
//tree = fileTree('src/main') {
//    include '**/*.java'
//}
//第三种方式:通过路径和闭包创建文件树：具名参数给map传值
//tree = fileTree(dir: 'src/main', include: '**/*.java') 
tree = fileTree(dir: 'src/main', includes: ['**/*.java', '**/*.xml', '**/*.txt'], exclude: '**/*test*/**')
tree.each { File file -> // 遍历文件树的所有文件
    println file
    println file.name
}
~~~

## 文件拷贝

我们可以使用 Copy 任务来拷贝文件，通过它可以过虑指定拷贝内容，还能对文件进行重命名操作等。Copy 任务必须指定一组需要拷贝的文件和拷贝到的目录，这里使用 CopySpec.from(java.lang.Object[])方法指定原文件；使用 CopySpec.into(java.lang.Object)方法指定目标目录。示例代码如下

~~~groovy
//将resources下的文件拷贝到build的config文件夹下
task copyTask(type: Copy) {
    from 'src/main/resources'
    into 'build/config'
}
~~~

from()方法接受的参数和文件集合时 files()一样。

* 当参数为一个目录时，该目录下所有的文件都会被拷贝到指定目录下（目录自身不会被拷贝）。
* 当参数为一个文件时，该文件会被拷贝到指定目录。
* 如果参数指定的文件不存在，就会被忽略；当参数为一个 Zip 压缩文件，该压缩文件的内容会被拷贝到指定目录。  

into()方法接受的参数与本地文件时 file()一样。 示例代码如下：

~~~groovy
task copyTask2(type: Copy) {
    // 拷贝src/main/webapp目录下所有的文件
    from 'src/main/webapp'
    // 拷贝单独的一个文件
//    from 'src/main/java/com/example/boot/CcProperties.java'
    // 从Zip压缩文件中拷贝内容
    from zipTree('src/main/assets.zip')
    // 拷贝到的目标目录
    into 'build/explodedWar'
}
~~~

在拷贝文件的时候还可以添加过虑条件来指定包含或排除的文件，示例如下：

~~~groovy
task copyTaskWithPatterns(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
    include '**/*.html'
    include '**/*.jsp'
    exclude { details -> details.file.name.endsWith('.html') }
}
~~~

在拷贝文件的时候还可以对文件进行重命名操作，示例如下：

~~~groovy
task rename(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
    // 使用一个闭包方式重命名文件
    rename { String fileName ->
        fileName.replace('-staging-', '')
    }
}
~~~

在上面的例子中我们都是使用 Copy 任务来完成拷贝功能的，那么有没有另外一种方式呢？答案是肯定的，那就是Project.copy(org.gradle.api.Action)方法。下面示例展示了 copy()方法的使用方式：  

~~~groovy
task copyMethod {
    doLast {
        copy {
            from 'src/main/webapp'
            into 'build/explodedWar'
            include '**/*.html'
            include '**/*.jsp'
        }
    }
}
~~~

或者使用 project 对象的 copy 方法：

~~~groovy
copy {
    //相对路径或者绝对路径
    from file('src/main/resources/ddd.txt') //file也可以换成new File()
    into this.buildDir.absolutePath
}
~~~

执行 `gradle build` 指令即可。去 build 目录的本地磁盘查看，就能看到。

## 归档文件

通常一个项目会有很多的 Jar 包，我们希望把项目打包成一个 WAR，ZIP 或 TAR 包进行发布，这时我们就可以使用 Zip，Tar，Jar，War 和 Ear 任务来实现，不过它们的用法都一样，所以在这里我只介绍 Zip 任务的示例。首先，创建一个 Zip 压缩文件,并指定压缩文件名称，如下代码所示：  

~~~groovy
apply plugin: 'java'
version = 1.0
task myZip(type: Zip) {
    from 'src/main'
    into 'build' //保存到build目录中
    baseName = 'myZIP'
}
println myZip.archiveName
~~~

执行命令 `gradle -q myZip`，会在 build 文件夹 distributions 中生成 myZIP-1.0.zip

最后，我们可以使用 Project.zipTree(java.lang.Object)和 Project.tarTree(java.lang.Object)方法来创建访问 Zip 压缩包的文件树对象，示例代码如下：

~~~groovy
// 使用zipTree
FileTree zip = zipTree('build/distributions/myZIP-1.0.zip')
// 使用tarTree
FileTree tar = tarTree('someFile.tar')
~~~

在这里，我们介绍了 Gradle 对本地文件、文件集合、文件树、文件拷贝和归档文件的操作方式。更详细的请参考官方文档：[Working With Files (gradle.org)](https://docs.gradle.org/current/userguide/working_with_files.html) 
