# Elasticsearch Notes

www.elastic.co

ES7必须使用 jdk1.9及以上的版本

GET请求不能带BODY

```powershell
docker pull elasticsearch

#限制elasticsearch的存储空间， 默认分配2个G
docker run -e ES_JAVA_OPTS="-Xms256m -Xmx256m" -d -p 9200:9200 -p 9300:9300 --name ES01 +镜像名

#查看正在运行的容器
docker ps
```

```http

虚拟机地址:9200
返回带有json数据的即为成功
version: 7.3.1 / 5.6.15都可以
```

#存储数据到Elasticsearch的行为叫索引

#理解：索引是数据库，类型是表，文档是表中的数据

```http
索引是megacorp 表是employee id是1 消息体是文档 发送put请求
虚拟机地址:9200/megacorp/employee/1
{
	json数据
	eg:
	"first_name": "Jaen",
	"last_name":  "Smith",
	"age":	      32, 32修改成27后↓↓
	"about":	  "I like to collect rock albums",
	"interests":  ["music"]	
}

==》
{
	“_index”:  "megacorp",
	"_type":   "employee",
	"_id":     "2",
	"_version": 4,"版本会迭代"
	"result":  "created", 刚创建的
	"_shards":{
		"total":2,
		"successful":1
		"failed":0
	},
	……
}
```

## 检索文档

```http
虚拟机地址:9200/megacorp/employee/1 发送get请求
虚拟机地址:9200/megacorp/employee/1

{
	“_index”:  "megacorp",
	"_type":   "employee",
	"_id":     "1",
	"_version": 1,
	found: true,
	"_source":{
		"first_name": "John",
		"last_name":  "Smith",
		"age": 25,
		"about": "I love to go rock climbing",
		"interests": [
			"sports",
			"music"
		]
	}
}
```

**head** 请求 用于检查数据是否存在

status :200 is ok!

status: 404 not found

## 轻量搜索

GET /megacorp/employee/_search

status:400 将第一行的body（分有上下两个body,这里指消息体）里面的请求数据淦掉就好

***GET请求不能携带消息体***

带条件查询

GET /megacorp/employee/_search?q=last_name:Smith

查询表达式搜索

POST /megacorp/employee/_search

{

​	"query" : {

​		"match : {

​			"last_name": "Smith"

​		}

​	}

}

更复杂的表达式搜索

POST /megacorp/employee/_search

{

​	"query":{

​		"bool":{

​			"must":{

​				"match":{

​					"last_name" : "smith"

​				}

​			},

​			"filter":{

​				"range" : {

​					"age" : { "gt" : 30 }

​				}

​			}

​		}

​	}

}

#得到的数据中"_score" 越高的权重越高

## 短语搜索

POST /megacorp/employee/_search

{

​		"query" : {

​			"match_phrase" : {

​				"about" : "rock climbing"

​			}

​		}

}

它搜索完全匹配的短语 "rock climbing"

## 高亮搜索

POST /megacorp/employee/_search

{

​		"query" : {

​			"match_phrase" : {

​				"about" : "rock climbing"

​			}

​		}，

​		"highlight" : {

​			"fields" : {

​				"about" : {}

​			}

​		}

}

# 整合

SpringBoot 默认支持两种技术来和ES交互；

1、Jest (默认不生效)

​	需要导入jest的工具包（io.searchbox.client.JestClient）

2、SpringData ElasticSearch（ES版本可能不合适）

​	1)、Client  节点信息 clusterNodes; clusterName

​	2)、ElasticsearchTemplate 操作es

​	3)、编写一个ElasticsearchRepository的子接口来操作ES;

**ES版本适配：**

![image-20200903181545443](D:\Java笔记\Typora\img\ES版本适配.png)