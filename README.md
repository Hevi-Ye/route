# 简单的HTTP路由解决文案

### 快速开始

1. 客隆项目

```bash
git clone git@github.com:heviye/jerry.git
```

2. 创建自己的项目

```go
package main

import (
	"fmt"

	"github.com/heviye/jerry/context"
	"github.com/heviye/jerry/router"
)

func main() {
	mux := router.NewRouteMux()

	// 设置静态文件路径
	mux.Static("./static/")

	// 创建一个没有中间件的分组
	r := mux.Group()
	r.Get("/users", GetUsers)

	// 创建一个可带多个中间件的分组
	rx := mux.Group(printLog, permission)
	rx.Get("/test", Test)

	mux.Run(":8888")
}

func GetUsers(c *context.Context) {
	c.Write([]byte("Users"))
}

func printLog(c *context.Context) bool {
	fmt.Println(c.Request().URL.Path)

	return true
}

func permission(c *context.Context) bool {
	id := c.Query("id")
	if len(id) == 0 {
		c.Write([]byte("id is empty"))
		return false
	}
	return true
}

func Test(c *context.Context) {
	c.Write([]byte("Test"))
}

```

3. 浏览器打开`http://127.0.0.1:8888/test`
