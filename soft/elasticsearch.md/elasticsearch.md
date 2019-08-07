Elasticsearch 是一个分布式、可扩展、实时的搜索与数据分析引擎
搜索、分析和探索

基于全文搜索引擎库 Apache Lucene

一个分布式的实时文档存储，每个字段 可以被索引与搜索
一个分布式实时分析搜索引擎
能胜任上百个服务节点的扩展，并支持 PB 级别的结构化或者非结构化数据




10.168.0.221:9200
curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search?pretty' -H 'content-type: application/json' -d '
{"query":{"match":{"fourth_tag":"户外"}}}'

curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search?size=1000'



//删除索引
curl -i -XDELETE 'elasticsearch.in.netwa.cn:9200/_all'
curl -i -XDELETE 'elasticsearch.in.netwa.cn:9200/question_v1_index'
curl -i -XDELETE 'elasticsearch.in.netwa.cn:9200/question_index'
//通过程序从数据库重新导数据到elasticsearch
curl -XPOST 'http://base_question.in.netwa.cn:8004/v1/basequestion/inner/back/reload_elasticsearch' -d '{"cmd":0,"es_index":"question_v1_index","es_type":"question_v1_type"}'

curl -XPOST 'http://base_question.in.netwa.cn:8004/v1/basequestion/inner/user/get_question'  -d '{"es_index":"question_index","es_type":"question_type","category":"综合类-科技-历史-地理","size":6}'

curl -XPOST 'http://127.0.0.1:8003/v1/question/get_room' -H 'user_id: 1' -d '{"entrance_id":1,"category":"综合类-电脑--"}'

curl -XPOST 'http://127.0.0.1:8003/v1/qinner/normal_get_question' -H 'user_id: 1' -d '{"entrance_id":1,"category":"综合类-电脑--"}'

./elasticsearch -d
curl 'http://elasticsearch.in.netwa.cn:9200/?pretty'

//查内容总数
curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/_count?pretty' -H 'content-type: application/json' -d '
{"query":{"match_all":{}}}'



//创建映射索引
curl -X PUT "elasticsearch.in.netwa.cn:9200/question_index" -H 'Content-Type: application/json' -d'
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

//通过映射查询
curl -X POST "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
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
curl -X POST "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
		"must": [
			{"term": {"first_tag": "文化"}},
			{"term": {"second_tag": "文学"}},
			{"term": {"third_tag": "1-1"}}
		]
      }
    }
  }
}
'

curl -X POST "elasticsearch.in.netwa.cn:9200/question_v1_index/question_v1_type/_search?size=6" -H 'Content-Type: application/json' -d'
{
  "query": {
    "term" : { "first_tag" : "pic" }
  }
}
'
curl -X POST "elasticsearch.in.netwa.cn:9200/question_v1_index/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match" : { "first_tag" : "综合类" }
  }
}
'











//插入
megacorp 索引名称
employee 类型名称
1 特定雇员的ID
curl -X PUT "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/1" -H 'Content-Type: application/json' -d'
{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
'

curl -X PUT "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/2" -H 'Content-Type: application/json' -d'
{
    "first_name" :  "Jane",
    "last_name" :   "Smith",
    "age" :         32,
    "about" :       "I like to collect rock albums",
    "interests":  [ "music" ]
}
'
curl -X PUT "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/3" -H 'Content-Type: application/json' -d'
{
    "first_name" :  "Douglas",
    "last_name" :   "Fir",
    "age" :         35,
    "about":        "I like to build cabinets",
    "interests":  [ "forestry" ]
}
'

//检索文档
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/1"


//搜索
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search"
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search?q=last_name:Smith"

//查询表达式
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match" : {
            "first_tag" : "综合"
        }
    }
}
'



//过滤器 filter
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "bool": {
            "must": {
                "match" : {
                    "last_name" : "smith"
                }
            },
            "filter": {
                "range" : {
                    "age" : { "gt" : 30 }
                }
            }
        }
    }
}
'
{"query":{"bool":{"must":{"match":{"id":"200"}},"filter":{"range":{"star":{"gt":2}}}}}}
{"aggs":{"all_stars":{"terms":{"field":"star"}}}}



//全文搜索(相关性 概念)
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
'

//短语 “rock climbing” 的形式紧挨
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
'

//高亮关键字
curl -X GET "elasticsearch.in.netwa.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}
'


localhost
/megacorp/employee
//分析  聚合（aggregations）
//预先设置内存值
curl -X PUT "elasticsearch.in.netwa.cn:9200/question_index_v1/question_type/_mapping" -H 'Content-Type: application/json' -d'
{
  "properties": {
    "first_tag": {
      "type":     "keyword",
      "fielddata": true,
	  "index": "not_analyzed"
    }
  }
}
'

//正式查询
curl -X GET "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "first_tag" }
    }
  }
}
'

//集群健康
curl -X GET "elasticsearch.in.netwa.cn:9200/_cluster/health"



//分配3个主分片和一份副本（每个主分片拥有一个副本分片）
curl -X PUT "elasticsearch.in.netwa.cn:9200/blogs" -H 'Content-Type: application/json' -d'
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 1
   }
}
'




curl -XPOST 'http://base_question.in.netwa.cn:8004/v1/basequestion/inner/user/get_question'  -d '{"es_index":"question_v1_index","es_type":"question_v1_type","third_tag":["词语解释","1.1"],"size":6}'





//创建别名-----------------------------------------------------------------------------------------
curl -X PUT "elasticsearch.in.netwa.cn:9200/question_index_v1/_alias/question_index" -H 'Content-Type: application/json'

检测这个别名指向哪一个索引：
curl -X GET "elasticsearch.in.netwa.cn:9200/*/_alias/question_index?pretty"

哪些别名指向这个索引：
curl -X GET "elasticsearch.in.netwa.cn:9200/question_index_v1/_alias/*?pretty"


将别名指向新的索引
curl -X POST "elasticsearch.in.netwa.cn:9200/_aliases?pretty" -H 'Content-Type: application/json' -d'
{
    "actions": [
        { "remove": { "index": "question_v1_index", "alias": "question_index" }},
        { "add":    { "index": "question_index_v2", "alias": "question_index" }}
    ]
}
'


curl -i -XDELETE 'elasticsearch.in.netwa.cn:9200/question_index_v2'

curl -XPOST 'http://base_question.in.netwa.cn:8004/v1/basequestion/inner/back/reload_elasticsearch' -d '{"es_index":"question_index_v1"}'

curl -XPOST 'http://base_question.in.netwa.cn:8004/v1/basequestion/inner/back/create_alias' -d '{"es_index":"question_index_v1"}'




curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/question_index/question_type/_search?size=1'

curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/question_index/_count?pretty' -H 'content-type: application/json' -d '
{"query":{"match_all":{}}}'

curl -X POST "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
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




GEO================================================================================================
curl -i -XDELETE 'elasticsearch.in.netwa.cn:9200/geo_index'
curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/geo_index/geo_type/_search?size=1'
 curl -i -XGET 'http://elasticsearch.in.netwa.cn:9200/geo_index/geo_type/1000005?pretty'


curl -X PUT "elasticsearch.in.netwa.cn:9200/geo_index?pretty" -H 'Content-Type: application/json' -d'
{
  "mappings": {
    "geo_type": {
      "properties": {
		"uid": {
		  "type": "keyword"
		},
        "location": {
          "type": "geo_point"
        }
      }
    }
  }
}
'

curl -X PUT "elasticsearch.in.netwa.cn:9200/geo_index/geo_type/1" -H 'Content-Type: application/json' -d'
{
  "uid": "xxxxxxxxxxxxxxxxxxx",
  "location": {
    "lat": 41.12,
    "lon": -71.34
  }
}
'

curl -X PUT "elasticsearch.in.netwa.cn:9200/geo_index/geo_type/2" -H 'Content-Type: application/json' -d'
{
  "text": "Geo-point as a string",
  "location": "41.12,-71.34"
}
'

curl -X PUT "elasticsearch.in.netwa.cn:9200/geo_index/geo_type/3" -H 'Content-Type: application/json' -d'
{
  "text": "Geo-point as an array",
  "location": [ -71.34, 41.12 ]
}
'

curl -X GET "elasticsearch.in.netwa.cn:9200/geo_index/geo_type/_search?size=5" -H 'Content-Type: application/json' -d'
{
    "query": {
        "bool" : {
            "filter" : {
                "geo_distance" : {
                    "distance" : "200000km",
                    "location" : {
                        "lat" : -80,
                        "lon" : -176
                    }
                }
            }
        }
    }
}
'

curl -X GET "elasticsearch.in.netwa.cn:9200/geo_index/geo_type/_search" -H 'Content-Type: application/json' -d'
{
    "query": {
        "bool" : {
            "must" : {
                "match_all" : {}
            },
            "filter" : {
                "geo_distance" : {
                    "distance" : "200km",
                    "location" : {
                        "lat" : 40,
                        "lon" : -70
                    }
                }
            }
        }
    }
}
'






==========================Aggre==========================
curl -X POST "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
		"must": [
			{"match":{"star":{"query":1}}},
			{"term": {"first_tag": "娱乐类"}},
			{"term": {"second_tag": "旅游类"}}
		]
      }
    }
}
'





curl -X GET "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
		"term": {
			"first_tag": "自然"
		}
	},
  "aggs": {
    "all_interests": {
      "terms": { "field": "second_tag" }
    }
  }
}
'

curl -X GET "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
	 "bool": {
		"must": [
			{"term": {"first_tag": "自然"}},
			{"term": {"second_tag": "生物"}}
		]
      }
	},
  "aggs": {
    "all_interests": {
      "terms": { "field": "third_tag" }
    }
  }
}
'



curl -X GET "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "first_tag" },
	  "aggs": {
		"all_interests": {
			"terms": { "field": "second_tag" }
		}
	  }
    }
  }
}
'

curl -X GET "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search?size=0" -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "first_tag" },
	  "aggs": {
		"all_interests": {
			"terms": { "field": "second_tag" },
			"aggs": {
				"all_interests": {
					"terms": { "field": "third_tag" },
					"aggs": {
						"all_interests": {
							"terms": { "field": "star" }
						}
					}
				}
			}
		}
	  }
    }
  }
}
'



curl -X GET "elasticsearch.in.netwa.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
{
  "query": {
	 "bool": {
		"must": [
			{"term": {"first_tag": "文化"}},
			{"term": {"second_tag": "文学"}}
		]
      }
	},
  "aggs": {
    "all_interests": {
      "terms": {"field":"third_tag","size":30,"min_doc_count":20}
    }
  }
}
'




curl -X POST "elasticsearch.in.netwa.cn:9200/question_index_v1/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
{
  "query": {
    "bool": {
		"must": [
			{"match":{"star":{"query":1}}},
			{"term": {"first_tag": "文史"}},
			{"term": {"second_tag": "文学"}},
			{"term": {"third_tag": "红楼梦专题"}}
		]
      }
    }
}
'





