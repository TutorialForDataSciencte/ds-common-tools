# 使用postgresql做数据存储

postgresql(简称pg)是目前最先进的开源关系型通用数据库,它完全支持标准sql语句,在其上还有一定的功能扩展;天生支持插件,有许多额外的实用功能由一些第三方组织已经做了很不错的实现;并且支持自定义数据包装器,将pg仅作为interface,数据则实际存在被包装的数据库上.

在性能上,pg从来不在任何一个场景下是最好的选择.但永远是第二好的选择,加上它是个多面手,可以一套工具应付绝大多数场景,不用考虑多种工机的异构系统集成问题,因此技术选型选它还是比较合适的.

## 应用领域

pg生态下大致的应用领域有:

+ [OLTP](): 事务处理是PostgreSQL的本行

+ [OLAP](): 并行计算,数据分区,ANSI SQL兼容，窗口函数，CTE，CUBE等高级分析功能，任意语言写UDF

+ 时序数据:timescaledb时序数据库插件,分区表,BRIN索引

+ 流处理: PipelineDB扩展，Notify-Listen，物化视图，规则系统，灵活的存储过程与函数编写

+ 搜索引擎: 全文搜索索引足以应对简单场景;配合插件zhparser等分词工具可以实现中文支持,丰富的索引类型,支持函数索引,条件索引

+ 文档数据库: JSON,JSONB,XML,HStore原生支持,可以替代mongodb

+ 图数据库: 递归查询,更有AgensGraph扩展实现完整的图数据库功能

+ 空间数据: PostGIS扩展(杀手锏),内建的几何类型支持,GiST索引.

+ 缓存: 物化视图

+ 作为交互界面包装外部数据: 可以自定FDW包装外部数据,也就是说pg可以什么也不存,只是作为对其他外部数据对象的sql交互界面

本文会按顺序逐次介绍

## 本文使用的工具

+ 本文使用docker作为pg的部署工具,我会在docker中模拟pg和插件的运行环境,部署用的docker-compose文件会放在[code]()文件夹下

+ 本文使用的pg版本为`11.5`

+ 执行SQL语句这边使用jupyter notebook的[postgres_kernel](http://blog.hszofficial.site/TutorialForPython/%E5%B7%A5%E5%85%B7%E9%93%BE%E7%AF%87/%E4%BA%A4%E4%BA%92%E7%8E%AF%E5%A2%83jupyter/%E5%9F%BA%E4%BA%8Eweb%E7%9A%84%E5%8F%AF%E4%BA%A4%E4%BA%92%E8%BF%90%E8%A1%8C%E7%8E%AF%E5%A2%83jupyter.html#postgresql)


