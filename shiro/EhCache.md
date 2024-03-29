# EhCache

## 缓存工具 EhCache

EhCache是一种广泛使用的开源Java分布式缓存。主要面向通用缓存,Java EE和轻量级容器。可以和大部分Java项目无缝整合，例如：Hibernate中的缓存就是基于EhCache实现的。

EhCache支持内存和磁盘存储，默认存储在内存中，如内存不够时把缓存数据同步到磁盘中。EhCache支持基于Filter的Cache实现，也支持Gzip压缩算法。

EhCache直接在JVM虚拟机中缓存，速度快，效率高

EhCache缺点是缓存共享麻烦，集群分布式应用使用不方便

## EhCache 搭建使用

### 添加依赖

~~~xml
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>2.6.11</version>
    <type>pom</type>
</dependency>
~~~

### 添加配置文件 ehcache.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache>
    <!--磁盘的缓存位置-->
    <diskStore path="java.io.tmpdir/ehcache"/>
    <!--默认缓存-->
    <defaultCache
            maxEntriesLocalHeap="10000"
            eternal="false"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            maxEntriesLocalDisk="10000000"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
        <persistence strategy="localTempSwap"/>
    </defaultCache>
    <!--helloworld 缓存-->
    <cache name="HelloWorldCache"
           maxElementsInMemory="1000"
           eternal="false"
           timeToIdleSeconds="5"
           timeToLiveSeconds="5"
           overflowToDisk="false"
           memoryStoreEvictionPolicy="LRU"/>
    <!--
    defaultCache：默认缓存策略，当 ehcache 找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
    -->
    <!--
        name:缓存名称。
        maxElementsInMemory:缓存最大数目
        maxElementsOnDisk：硬盘最大缓存个数。
        eternal:对象是否永久有效，一但设置了，timeout 将不起作用。
        overflowToDisk:是否保存到磁盘，当系统宕机时
        timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当
        eternal=false 对象不是永久有效时使用，可选属性，默认值是 0，也就是可闲置时间无穷大。
        timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当 eternal=false 对象不是永久有效时使用，默认是 0.也就是对象存活时间无穷大。
        diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
        diskSpoolBufferSizeMB：这个参数设置 DiskStore（磁盘缓存）的缓存区大小。默认是 30MB。每个 Cache 都应该有自己的一个缓冲区。
        diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120 秒。
        memoryStoreEvictionPolicy：当达到 maxElementsInMemory 限制时，Ehcache 将会根据指定的策略去清理内存。默认策略是 LRU（最近最少使用）。你可以设置为 FIFO（先进先出）或是 LFU（较少使用）。
        clearOnFlush：内存数量最大时是否清除。
        memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
            FIFO，first in first out，这个是大家最熟的，先进先出。
            LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个 hit 属性，hit 值最小的将会被清出缓存。
            LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
    -->
</ehcache>
~~~

### 创建测试类

~~~java
@Test
public void testEH() throws IOException {
    //获取编译目录下的资源的流对象
    InputStream stream = ResourceUtil.getStream("ehcache.xml");
    //获取 EhCache 的缓存管理对象
    CacheManager cacheManager = new CacheManager(stream);
    //获取缓存对象
    Cache cache = cacheManager.getCache("HelloWorldCache");
    //创建缓存对象
    Element element = new Element("name","zhang3");
    //存入缓存
    cache.put(element);
    //从缓存中取出
    Element element1 = cache.get("name");
    System.out.println(element1.getObjectValue());
}
~~~

![image-20221221160336118](./assets/image-20221221160336118.png)