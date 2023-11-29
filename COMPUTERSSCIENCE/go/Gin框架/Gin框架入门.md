# Gin框架入门



## 设置Proxy(代理)

在国内一般会用代理拉取项目

```http
https://goproxy.cn
```



## 获取Gin

```go
go get -u github.com/gin-gonic/gin
```



## JSON渲染

```go
// example

func main() {
    r := gin.Default()
    
    // gin.H 是 map[string]interface{} 的缩写
    r.GET("/someJSON", func(c *gin.Context) {
        // 方式一： 自己拼接JSON
        c.JSON(http.StatusOK, gin.H{"message": "hello world!"})
    })
    
    r.GET("/moreJSON", func(c *gin.Context) {
        var msg struct{
            Name string `json:"user"`
            Message string
            Age	int
        }
        msg.Name="你好"
        msg.Message="hello wrold!"
        msg.Age=18
        c.JSON(http.StatusOK, msg)
    })
    r.Run(":8080")
}
```



## Gin获取 query string 参数

```go
// example

func main() {
    
    // Default 返回一个默认路由引擎
    r := gin.Default()
    r.GET("/user/search", func(c *gin.Context) {
        username := c.DefaultQuery("username", "你好") // 取不到就用默认值"你好"
        // username := c.Query("username")
        address := c.Query("address")
        c.JSON(http.StatusOK, gin.H{
            "message": "ok",
            "username": username,
            "address": address,
        })
    })
    r.Run()
}
```



## Gin获取 form 参数

```go
// example

func main() {
    
    // Default 返回一个默认的路由引擎
    r := gin.Default()
    
    r.POST("user/search", func(c *gin.Context) {
        // DefaultPostForm 取不到值时会返回指定的默认值
        // username := c.DefaultPostForm("username", "你好")
        username := c.PostForm("username")
        address := c.PostForm("address")
        // 输出 json 结果给调用方
        c.JSON(http.StatusOK, gin.H{
            "message": "ok",
            "username": username,
            
        })
    })
    
    
}
```



## Gin获取path参数

```go
// example

func main() {
    
    // Default 返回一个默认的路由引擎
    r := gin.Default()
    r.GET("/user/search/:username/:address", func(c *gin.Context) {
        username := c.Param("username")
        address := c.Param("address")
       	// 输出json结果给调用方
        c.JSON(http.StatusOK, gin.H{
            "message": "ok"
            "username": username
            "address": address
        })
    })
    r.Run(":8080")
}
```



## Gin参数绑定

```go
// example

type UserInfo struct {
    Username string `form:"username json:"username"`
    Password string	`form:"password json:"password"`
}

func main() {
    r := gin.Default()
    
    // get方式form
    r.GET("/user", func(c *gin.Context){
     	var u UserInfo // 声明一个 UserInfo 类型的变量
        
        // err := c.ShouldBuild(u) // ?
        err := c.ShouleBuild(&u)
        if err != nil{
            c.JSON(http.StatusBadRequest, gin.H{
                "error": err.Error(),
            })
        } else {
            c.JSON(http.StatusOK, gin.H{
                "status": "ok",
            })
        }
    })
    
    // post方式form
    r.POST("/form", func(c *gin.Context){
     	var u UserInfo // 声明一个 UserInfo 类型的变量
        
        // err := c.ShouldBuild(u) // ?
        err := c.ShouleBuild(&u)
        if err != nil{
            c.JSON(http.StatusBadRequest, gin.H{
                "error": err.Error(),
            })
        } else {
            c.JSON(http.StatusOK, gin.H{
                "status": "ok",
            })
        }
    })
    
    // json格式
    r.POST("/json", func(c *gin.Context){
     	var u UserInfo // 声明一个 UserInfo 类型的变量
        
        // err := c.ShouldBuild(u) // ?
        err := c.ShouleBuild(&u)
        if err != nil{
            c.JSON(http.StatusBadRequest, gin.H{
                "error": err.Error(),
            })
        } else {
            c.JSON(http.StatusOK, gin.H{
                "status": "ok",
            })
        }
    })
    
    r.Run(":8080")
}
```



## Gin文件上传

```go
// example

// 单文件上传
func main() {
    
    router := gin.Default()
    // 处理multipart forms提交文件时默认的内存限制是 32 Mib
    // 可以通过下面的方式修改
    // router.MaxMultipartMemory = 8 << 20 // 8 Mib
    router.POST("/upload", func(c *gin.Context) {
        // 单个文件
        file, err := c.FormFile("file")
        if err != nil {
            c.JSON(http.StatusInternalServerError, gin.H{
                "message": err.Error(),
            })
            return
        }
        
        log.Println(file.Filename)
        dst := fmt.Sprintf("C:/tmp/%s", file.Filename)
        // 上传文件到指定的目录
        c.SaveUploadedFile(file, dst)
        c.JSON(http.StatusOK, gin.H{
            "message": fmt.Sprintf("'%s' uploaded!", file.Filename),
        })
    })
    router.Run()
}

// 多个文件上传
func main() {
     router := gin.Default()
    // 处理multipart forms提交文件时默认的内存限制是 32 Mib
    // 可以通过下面的方式修改
    // router.MaxMultipartMemory = 8 << 20 // 8 Mib
    router.POST("/upload", func(c *gin.Context) {
        // Multipart form
        file, err := c.MultipartForm()
        files := form.File["file"]
        
        for index, file := range files {
            log.Println(file.Filename)
            dst := fmt.Sprintf("C:/tmp/%s_%d", file.Filename, index)
            // 上传文件到指定的目录
            c.SaveUploadFile(file, dst)
        }
        
        c.JSON(http.StatusOK, gin.H{
            "message": fmt.Sprintf("%d files uploaded!", len(files)),
        })
    })	
    router.Run()
}
```



## 重定向

### HTTP 重定向

```go
r.GET("/test", func(c *gin.Context) {
    c.Readirect(http.StatusMovedPermanently, "http://www.baidu.com/")
})
```

