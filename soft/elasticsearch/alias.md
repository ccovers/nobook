
# _alias

## 创建
curl -X PUT "elasticsearch.in.owner.cn:9200/question_index_v1/_alias/question_index" -H 'Content-Type: application/json'


## 检测这个别名指向哪一个索引
curl -X GET "elasticsearch.in.owner.cn:9200/*/_alias/question_index?pretty"


## 哪些别名指向这个索引
curl -X GET "elasticsearch.in.owner.cn:9200/question_index_v1/_alias/*?pretty"


## 将别名指向新的索引
curl -X POST "elasticsearch.in.owner.cn:9200/_aliases?pretty" -H 'Content-Type: application/json' -d'
{
    "actions": [
        { "remove": { "index": "question_v1_index", "alias": "question_index" }},
        { "add":    { "index": "question_index_v2", "alias": "question_index" }}
    ]
}
'