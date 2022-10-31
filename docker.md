## 注意事项

1. 该容器中封装了一个数据库，数据较多，因此容器大概有3g
2. 该容器需要较高的运行内存


##  docker 拉取镜像
```
docker run -itd --network host -p 22568:22568 --name migrationhelper jiaxianlin/migrationhelper bash /start.sh
```
## API 接口



### Query Migration Recommendations for a Library

```
http://migration-helper.net:22568/recommend?fromLib=org.json:json&page=0&size=20
```

### Query Related Repositories and Commits for a Source/Target Library Pair

```
http://migration-helper.net:22568/recommend-one?fromLib=org.json:json&toLib=com.google.code.gson:gson
```
