# mapping
分析聚合`aggregations`


## 预先设置内存值
curl -X PUT "elasticsearch.in.owner.cn:9200/question_index_v1/question_type/_mapping" -H 'Content-Type: application/json' -d'
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


## 正式查询
curl -X GET "elasticsearch.in.owner.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "first_tag" }
    }
  }
}
'

