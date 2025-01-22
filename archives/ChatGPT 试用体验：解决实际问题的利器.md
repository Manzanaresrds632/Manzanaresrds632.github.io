结合实际问题，我试用了一下当前非常火的 ChatGPT，测试了它在解决 Go 语言处理 MongoDB 数据去重查询问题上的表现。结果令人满意，ChatGPT 确实能够实际帮助解决问题。

---

## 使用截图

以下是测试过程中截取的部分截图：

![截图1](https://i-blog.csdnimg.cn/blog_migrate/95c99e29997bda5bb7d8f5daf276e301.png)  
![截图2](https://i-blog.csdnimg.cn/blog_migrate/63a7d250f5cee4da3787a31ca8fc8601.png)  
![截图3](https://i-blog.csdnimg.cn/blog_migrate/d4b2e889e054cd4c582508464e03809d.png)  
![截图4](https://i-blog.csdnimg.cn/blog_migrate/97424d7112470b45f4d17da23f485974.png)

---

## Go 语言 MongoDB 去重查询方法

在使用 `go.mongodb.org/mongo-driver` 进行 MongoDB 数据库查询时，可以使用 `Distinct` 方法实现去重查询。以下是具体用法示例：

go
// 创建查询条件
filter := bson.M{"age": bson.M{"$gt": 18}}
// 执行去重查询
cursor, err := collection.Distinct(context.Background(), "name", filter, options.Distinct())
if err != nil {
    // 处理错误
}
var names []string
// 将结果存入切片中
if err = cursor.All(context.Background(), &names); err != nil {
    // 处理错误
}


在这个例子中，我们根据年龄大于 18 的条件创建了一个查询过滤器 `filter`，然后使用 `collection.Distinct()` 方法查询 `name` 字段的不同值。最后将查询结果存入一个字符串切片 `names` 中，并对返回的错误进行了处理。

### 注意事项

- **方法更新**：`Distinct` 方法在 `go.mongodb.org/mongo-driver/v1` 中已被弃用，建议使用 `go.mongodb.org/mongo-driver/v2` 版本中的 `DistinctDocuments` 方法替代，具体用法可参考官方文档。
- **返回值类型**：`Distinct` 方法返回的是一个 `mongo.Cursor` 类型的游标，包含查询结果中指定字段的所有不同值。可以使用 `All` 或 `Next` 方法逐个获取值。

---

## 去重后返回多个字段

如果需要在查询结果中返回多个字段并进行去重，可以将需要查询的字段传递给 `collection.Distinct()` 方法的第一个参数。以下是示例代码：

go
// 创建查询条件
filter := bson.M{"age": bson.M{"$gt": 18}}
// 指定多个查询字段
fields := []string{"name", "gender"}
// 执行去重查询
cursor, err := collection.Distinct(context.Background(), fields, filter, options.Distinct())
if err != nil {
    // 处理错误
}
var results []bson.M
// 将结果存入文档切片中
if err = cursor.All(context.Background(), &results); err != nil {
    // 处理错误
}


在这个例子中，我们将查询结果中的两个字段（`name` 和 `gender`）加入到 `fields` 数组中，然后传递给 `collection.Distinct()` 方法。查询结果是一个文档切片，每个文档由指定字段的唯一组合构成。

---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)