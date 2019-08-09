# elasticsearch
`elasticsearch`是一个分布式、可扩展、实时的搜索与数据分析引擎，基于全文搜索引擎库`Apache Lucene`，实现了分布式的实时文档存储，每个字段可以被索引与搜索，能胜任上百个服务节点的扩展，并支持`PB`级别的结构化或者非结构化数据



## `POST` 改变对象的当前状态
- 通过映射查询
curl -X POST "elasticsearch.in.owner.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
		"must": [
			{"term": {"first_tag": "综合"}},
			{"term": {"second_tag": "文学"}},
			{"term": {"third_tag": "文化"}},
			{"term": {"third_tag": "语文"}}
		]
      }
    }
  }
}
'


curl -X POST "elasticsearch.in.owner.cn:9200/question_v1_index/question_v1_type/_search?size=6" -H 'Content-Type: application/json' -d'
{
  "query": {
    "term" : { "first_tag" : "pic" }
  }
}
'


curl -X POST "elasticsearch.in.owner.cn:9200/question_v1_index/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match" : { "first_tag" : "综合类" }
  }
}
'


curl -X POST "elasticsearch.in.owner.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
		"must": [
			{"match":{"star":{"query":1}}},
			{"term": {"first_tag": "趣味"}},
			{"term": {"second_tag": "趣味常识"}}
		]
      }
    }
}
'



## `PUT` 创建一个对象
1. 创建映射索引
curl -X PUT "elasticsearch.in.owner.cn:9200/question_index" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "question_type": {
      "properties": {
        "first_tag": {"type": "keyword"},
		"second_tag": {"type": "keyword"}
      }
    }
  }
}
'


2. 插入
curl -X PUT "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/1" -H 'Content-Type: application/json' -d'{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
'


## `DELETE` 销毁对象
curl -i -XDELETE 'elasticsearch.in.owner.cn:9200/_all'
curl -i -XDELETE 'elasticsearch.in.owner.cn:9200/question_index'


## `HEAD` 请求获取对象的基础信息






























