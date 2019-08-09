
# aggre

curl -X POST "elasticsearch.in.owner.cn:9200/question_index/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
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


curl -X GET "elasticsearch.in.owner.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
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


curl -X GET "elasticsearch.in.owner.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
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


curl -X GET "elasticsearch.in.owner.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
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


curl -X GET "elasticsearch.in.owner.cn:9200/question_index/question_type/_search?size=0" -H 'Content-Type: application/json' -d'
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


curl -X GET "elasticsearch.in.owner.cn:9200/question_index/question_type/_search" -H 'Content-Type: application/json' -d'
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


curl -X POST "elasticsearch.in.owner.cn:9200/question_index_v1/question_type/_search?size=10" -H 'Content-Type: application/json' -d'
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