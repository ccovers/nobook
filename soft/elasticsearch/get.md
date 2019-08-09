
## `GET`

### 查内容总数
curl -i -XGET 'http://elasticsearch.in.owner.cn:9200/_count?pretty' -H 'content-type: application/json' -d '{"query":{"match_all":{}}}'

curl -i -XGET 'http://elasticsearch.in.owner.cn:9200/question_index/_count?pretty' -H 'content-type: application/json' -d '{"query":{"match_all":{}}}'

curl -i -XGET 'http://elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search?pretty' -H 'content-type: application/json' -d '{"query":{"match":{"fourth_tag":"户外"}}}'

curl -i -XGET 'http://elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search?size=1000'


## 检索文档
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/1"


## 搜索
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search"
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search?q=last_name:Smith"


## 查询表达式
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match" : {
            "first_tag" : "综合"
        }
    }
}
'



## 过滤器 filter
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
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



## 全文搜索(相关性 概念)
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
'


## 短语 “rock climbing” 的形式紧挨
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
'


## 高亮关键字
curl -X GET "elasticsearch.in.owner.cn:9200/question_v1_list/question_v1_type/_search" -H 'Content-Type: application/json' -d'
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
