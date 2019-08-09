# cluster

## 集群健康
curl -X GET "elasticsearch.in.owner.cn:9200/_cluster/health"



## 分配3个主分片和一份副本（每个主分片拥有一个副本分片）
curl -X PUT "elasticsearch.in.owner.cn:9200/blogs" -H 'Content-Type: application/json' -d'
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 1
   }
}
'