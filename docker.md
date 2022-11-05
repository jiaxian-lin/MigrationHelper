## 注意事项

1. 该容器中封装了一个数据库，数据较多，因此容器大概有3g
2. 该容器需要较高的运行内存


##  docker 拉取镜像
```
docker run -itd --network host -p 22568:22568 --name migrationhelper jiaxianlin/migrationhelper bash /start.sh
```
## API 接口

## 如果请求的库在数据库中没有会报错

```
http://migration-helper.net:22568/recommend?fromLib=something:non-existent&pageNum=0&pageSize=20
```
```json
{
  "timestamp": "2020-10-23T02:28:34.015+0000",
  "status": 404,
  "error": "404 Not Found",
  "message": "fromLib something:non-existent does not exist",
  "path": "/recommend",
  "url": "http://migration-helper.net:22568/recommend?fromLib=something:non-existent&page=0&size=20"
}
```

### 查询一个库的推荐

```
http://localhost:22568/recommend?fromLib=org.json:json&page=0&size=20
```

response
```json
{
	"_embedded": {
		"migrationRecommendations": [{
			"fromLib": "org.json:json",
			"toLib": "com.fasterxml.jackson.core:jackson-databind",
			"confidence": 0.2571263152253326,
			"ruleCount": 20,
			"messageCount": 11,
			"apiCount": 141,
			"ruleSupport": 0.5714285714285714,
			"messageSupport": 3.5849625007211565,
			"distanceSupport": 0.6845531681845369,
			"apiSupport": 0.18335500650195058,
			"refs": [],
			"_links": {
				"self": {
					"href": "http://localhost:22568/recommend-one?fromLib=org.json:json&toLib=com.fasterxml.jackson.core:jackson-databind"
				}
			}
		}, {
			"fromLib": "org.json:json",
			"toLib": "com.google.code.gson:gson",
			"confidence": 0.16696214072825585,
			"ruleCount": 21,
			"messageCount": 12,
			"apiCount": 92,
			"ruleSupport": 0.6,
			"messageSupport": 3.700439718141092,
			"distanceSupport": 0.6285675154320988,
			"apiSupport": 0.11963589076723016,
			"refs": [],
			"_links": {
				"self": {
					"href": "http://localhost:22568/recommend-one?fromLib=org.json:json&toLib=com.google.code.gson:gson"
				}
			}
		}]
	},
	"_links": {
		"self": {
			"href": "http://localhost:22568/recommend?fromLib=org.json:json&page=0&size=20"
		},
		"first": {
			"href": "http://localhost:22568/recommend?fromLib=org.json:json&page=0&size=20"
		},
		"next": {
			"href": "http://localhost:22568/recommend?fromLib=org.json:json&page=1&size=20"
		},
		"last": {
			"href": "http://localhost:22568/recommend?fromLib=org.json:json&page=82&size=20"
		}
	},
	"page": {
		"size": 20,
		"totalElements": 1655,
		"totalPages": 83,
		"number": 0
	}
}
```

<!-- ### 查询两个库之间的迁移记录

```
http://localhost:22568/recommend-one?fromLib=org.json:json&toLib=com.google.code.gson:gson
```

```json
{
	"fromLib": "org.json:json",
	"toLib": "com.google.code.gson:gson",
	"confidence": 0.16696214072825585,
	"ruleCount": 21,
	"messageCount": 12,
	"apiCount": 92,
	"ruleSupport": 0.6,
	"messageSupport": 3.700439718141092,
	"distanceSupport": 0.6285675154320988,
	"apiSupport": 0.11963589076723016,
	"refs": [{
		"repoName": "apache_pulsar",
		"startCommit": "633d6ea38defe9d14849d4e135fb8d77a4206302",
		"endCommit": "633d6ea38defe9d14849d4e135fb8d77a4206302",
		"fileName": "pulsar-client-admin/pom.xml",
		"confirmed": false,
		"possible": true
	}, {
		"repoName": "apache_pulsar",
		"startCommit": "633d6ea38defe9d14849d4e135fb8d77a4206302",
		"endCommit": "633d6ea38defe9d14849d4e135fb8d77a4206302",
		"fileName": "pulsar-websocket/pom.xml",
		"confirmed": false,
		"possible": true
	}],
	"_links": {
		"self": {
			"href": "http://localhost:22568/recommend-one?fromLib=org.json:json&toLib=com.google.code.gson:gson"
		}
	}
}
```
 -->
### 前20个最相近的前缀

```
http://localhost:22568/libraries-with-prefix?prefix=org.jso
```
response
```
["org.json4s:json4s-ast_2.10","org.json4s:json4s-ast_2.11","org.json4s:json4s-core_2.10","org.json4s:json4s-core_2.11","org.json4s:json4s-ext_2.10","org.json4s:json4s-ext_2.12","org.json4s:json4s-jackson_2.10","org.json4s:json4s-jackson_2.11","org.json4s:json4s-jackson_2.12","org.json4s:json4s-native_2.11","org.json4s:json4s-native_2.9.1-1","org.json:json","org.jsondoc:jsondoc-core","org.jsonschema2pojo:jsonschema2pojo-cli","org.jsonschema2pojo:jsonschema2pojo-core","org.jsoup:jsoup"]
```

### 前20个最相近的名字
```
http://localhost:22568/libraries-similar?lib=org.gson
```
```
["org.w3c:dom","org.json:json","org.xbib:oai","org.osgi:core","org.ogce:xsul","org.nhind:xd","org.jpos:jpos","org.ojai:ojai","org.ogce:xpp5","org.ogce:xpp3","org.nutz:nutz","org.mongodb:bson","org.jxls:jxls","org.jooq:joox","org.jooq:joor","org.jooq:jooq","org.jooq:jool","org.jongo:jongo","org.jocl:jocl","org.jdom:jdom"]
```
