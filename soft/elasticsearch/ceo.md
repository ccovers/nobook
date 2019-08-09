
# GEO
curl -i -XDELETE 'elasticsearch.in.owner.cn:9200/geo_index'
curl -i -XGET 'http://elasticsearch.in.owner.cn:9200/geo_index/geo_type/_search?size=1'
curl -i -XGET 'http://elasticsearch.in.owner.cn:9200/geo_index/geo_type/1000005?pretty'


curl -X PUT "elasticsearch.in.owner.cn:9200/geo_index?pretty" -H 'Content-Type: application/json' -d'
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


curl -X PUT "elasticsearch.in.owner.cn:9200/geo_index/geo_type/1" -H 'Content-Type: application/json' -d'
{
  "uid": "xxxxxxxxxxxxxxxxxxx",
  "location": {
    "lat": 41.12,
    "lon": -71.34
  }
}
'


curl -X PUT "elasticsearch.in.owner.cn:9200/geo_index/geo_type/2" -H 'Content-Type: application/json' -d'
{
  "text": "Geo-point as a string",
  "location": "41.12,-71.34"
}
'


curl -X PUT "elasticsearch.in.owner.cn:9200/geo_index/geo_type/3" -H 'Content-Type: application/json' -d'
{
  "text": "Geo-point as an array",
  "location": [ -71.34, 41.12 ]
}
'


curl -X GET "elasticsearch.in.owner.cn:9200/geo_index/geo_type/_search?size=5" -H 'Content-Type: application/json' -d'
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


curl -X GET "elasticsearch.in.owner.cn:9200/geo_index/geo_type/_search" -H 'Content-Type: application/json' -d'
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