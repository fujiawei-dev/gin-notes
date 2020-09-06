# 在 Gin 中集成 Swagger

- [在 Gin 中集成 Swagger](#在-gin-中集成-swagger)
  - [安装 swaggo](#安装-swaggo)
  - [注释基本信息](#注释基本信息)
  - [注释接口信息](#注释接口信息)
  - [常用请求示例](#常用请求示例)
    - [多个路径参数](#多个路径参数)
    - [表单上传文件](#表单上传文件)
    - [指明参数属性格式](#指明参数属性格式)
  - [常用响应示例](#常用响应示例)
    - [数组类型数据](#数组类型数据)
    - [在注释中组合结构体](#在注释中组合结构体)
    - [添加 Headers](#添加-headers)
    - [给出示例值](#给出示例值)
    - [给出字段描述](#给出字段描述)

## 安装 swaggo

```
go get -u github.com/swaggo/swag/cmd/swag
```

在安装完 Swagger 关联库后，就需要项目里的 API 接口编写注解，以便后续在生成时能够正确地运行。

![](../imgs/swagger.png)

最后执行以下命令生成相关文档：

```
swag init
```

## 注释基本信息

基本信息必须在 main.go 的 main 函数上添加注释，或者 `-g` 参数指定文件。

```
swag init -g http/api.go
```

```go
// @title Gin Swagger
// @version 1.0
// @description Gin Swagger 示例项目
// @termsOfService https://github.com/fujiawei-dev

// @contact.name Rustle Karl
// @contact.url https://github.com/fujiawei-dev
// @contact.email fu.jiawei@outlook.com

// @license.name Apache 2.0
// @license.url http://www.apache.org/licenses/LICENSE-2.0.html

// @host localhost:8080
// @basePath /api/v1/

// @securityDefinitions.basic BasicAuth
// @securityDefinitions.apikey ApiKeyAuth
// @in header
// @name Authorization


func main() {
 // 省略其他代码
}
```

## 注释接口信息

| 注释属性 | 描述 |
| ----------- | ----------- |
| description | 描述 |
| id | 全局唯一标识符 |
| tags | 接口标注 |
| summary | 简述 |
| accept | 请求类型 |
| produce | 响应类型 |
| param | 参数 `param name`,`param type`,`data type`,`is required`,`comment` `attribute(optional)` |
| success | 请求成功后返回 `return code`,`{param type}`,`data type`,`comment` |
| failure | 请求失败后返回 `return code`,`{param type}`,`data type`,`comment` |
| router | 请求路由及请求方式 `path`,`[httpMethod]` |

| 数据类型 | 注释可填 |
| ----------- | ----------- |
| application/json | application/json, json |
| text/xml | text/xml, xml |
| text/plain | text/plain, plain |
| html | text/html, html |
| multipart/form-data | multipart/form-data, mpfd |
| application/x-www-form-urlencoded | application/x-www-form-urlencoded, x-www-form-urlencoded |
| application/vnd.api+json | application/vnd.api+json, json-api |
| application/x-json-stream | application/x-json-stream, json-stream |
| application/octet-stream | application/octet-stream, octet-stream |
| image/png | image/png, png |
| image/jpeg | image/jpeg, jpeg |
| image/gif | image/gif, gif |

## 常用请求示例

### 多个路径参数

```go
// @Param group_id path int true "Group ID"
// @Param account_id path int true "Account ID"
// @Router /examples/groups/{group_id}/accounts/{account_id} [get]
```

### 表单上传文件

```go
// @Accept mpfd
// @Param file formData file true "上传文件"
```

### 指明参数属性格式

```go
// @Param q query string false "email" Format(email)
```

## 常用响应示例

### 数组类型数据

```go
// @Success 200 {array} response.Bottle
```

### 在注释中组合结构体

```go
type JSONResult struct {
    Code    int          `json:"code" `
    Message string       `json:"message"`
    Data    interface{}  `json:"data"`
}

type Order struct { //in `proto` package
    Id  uint            `json:"id"`
    Data  interface{}   `json:"data"`
}

// JSONResult's data field will be overridden by the specific type proto.Order
// @success 200 {object} jsonresult.JSONResult{data=proto.Order} "desc"
// @success 200 {object} jsonresult.JSONResult{data=string} "desc"
// @success 200 {object} jsonresult.JSONResult{data=[]string} "desc"
```

### 添加 Headers

```go
// @Success 200 {string} string	"ok"
// @Header 200 {string} Location "/entity"
// @Header 200 {string} Token "qwerty"
```

### 给出示例值

```go
type Account struct {
    ID   int    `json:"id" example:"1"`
    Name string `json:"name" example:"account"`
    PhotoUrls []string `json:"photo_urls" example:"http://image/1.jpg,http://image/2.jpg"`
}
```

### 给出字段描述

```go
type Account struct {
	// ID this is userid
	ID   int    `json:"id"`
	Name string `json:"name"` // This is Name
}
```