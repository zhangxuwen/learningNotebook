# gorm入门(v1)



## 安装

```go
go get -u github.com/jinzhu/gorm
```



## 连接数据库

```go
import _ "github.com/jinzhu/gorm/dialects/mysql"
// import _ "github.com/jinzhu/gorm/dialects/postgres"
// import _ "github.com/jinzhu/gorm/dialects/sqlite"
// import _ "github.com/jinzhu/gorm/dialects/mssql"
```



## 连接MySQL

```go
import (
	"github.com/jinzhu/gorm"
    _ "github.com/jinzhu/gorm/dialects/mysql"
)

func main() {
    db, err := gorm.Open("mysql", "user:password@(localhost/dbname?charset=utf8mb4&parseTime=True&loc=Local)")
    defer db.Close()
}
```



## GROM Model定义

### gorm.Model

```go
// example

// gorm.Model 定义
type Model struct {
    ID			uint `gorm:"primary_key"`
    CreatedAt 	time.Time
    UpdatedAt	time.Time
    DeletedAt	*time.Time
}
```

#### 模型定义示例

```go
// example

type User struct {
    gorm.Model		// 内嵌gorm.Model
    Name			string
    Age				sql.NullInt64
    Birthday 		*time.Time
    Email			string `gorm:"type:varchar(100);unique_index"`
    Role			string `gorm:"size:255"` // 设置字段大小为255
    MemberNumber	*string `gorm:"unique; not null"` // 设置会员号 (member number) 唯一并且不为空
    Num				int	`gorm:"AUTO_INCREATEMENT"` // 设置 num 为自增类型
    Address			string `gorm:"index:addr"` // 给 address 字段创建名为 addr 的索引
    IgnoreMe		int `gorm:"-"` // 忽略本字段
}
```



## 主键、表名、列名的约定

### 主键（Primary Key）

GROM默认会使用名为ID的字段作为表的主键

```go
// example

type User struct {
    ID string // 名为`ID`的字段会默认作为表的主键
    Name string
}

// 使用`Animal`作为主键
type Animal struct {
    AnimalID 	int64 `gorm:"primary_key"`
    Name		string
    Age			int64
}
```

### 表名（Table Name）

表名默认就是结构体名称的复数

```go
// example

type User struct {} // 默认表名是`users`

// 将User的表名设置为`prfiles`
func (User) TableName() string {
    return "profiles"
}

func (u User) TableName() string {
    if u.Role == "admin" {
        return "admin_users"
    } else {
        return "users"
    }
}

// 禁用默认表名的复数形式，如果置为true，则`User`的默认表名是`user`
db.SingularTable(true)
```

也可以通过**Table()**指定表名

```go
// example

// 使用User结构体创建名为`deleted_users`的表
db.Table("deleted_users").CreateTable(&User{})

var deleted_users []User
db.Table("deleted_users").Find(&deleted_users)
/// SELECT * FROM deleted_users;

db.Table("deleted_users").Where("name = ?", "jinzhu").Delete()
/// DELETE FROM deleted_users WHERE name = 'jinzhu';
```

### GORM还支持更改默认表名称规则

```go
// example

gorm.DefaultTableNameHandler = func(db *gorm.DB, defaultTableName string) string {
    return "prefix_" + defaultTableName;
}
```

### 列名

列名由字段名称进行下划线分割来生成

```go
type User struct {
    ID			uint // column name is `id`
    Name	 	string // column name is `name`
    Birthday	time.Time // column name is `birthday`
    CreatedAt	time.Time // column name is `created_at`
}
```

可以使用结构体**tag**指定列名

```go
type Animal struct {
    AnimalId	int64 `gorm:"column:beast_id"` // set column name to `best_id`
    Birthday	time.Time `gorm:"column:day_of_the_beast"` // set column name to `day_of_the_beast`
}
```

### 时间戳跟踪

#### CreateAt

如果模型有**CreateAt**字段，该字段的值将会是初次创建记录的时间

```go
db.Create(&user) // `CreateAt`将会是当前时间

// 可以使用`Update`方法来改变`CreateAt`的值
db.Model(&user).Update("CreateAt", time.Now())
```

#### UpdatedAt

如果模型有**UpdatedAt**字段，该字段的值将会是每次更新记录的时间

```go
db.Save(&user) // `UpdatedAt`将会是当前时间

// 可以使用`Update`方法来改变`CreateAt`的值
db.Model(&user).Update("name", "jinzhu") // `UpdatedAt`将会是当前时间
```

#### DeletedAt

如果模型有**DeletedAt**字段，调用**Delete**删除该记录时，将会设置**DeletedAt**字段为当前时间，而不是直接将记录从数据库中删除



## CRUD（增删改查）

### 创建

#### 创建记录

首先定义模型

```go
type User struct {
    ID		int64
    Name	string
    Age		int64
}
```

使用使用**NewRecord()**查询主键是否存在，主键为空使用**Create()**创建记录

```go
user := User{Name: "hello", Age: 18}

db.NewRecord(&user) // 主键为空返回`true`
db.Create(&user) // 在数据库中创建了user
db.NewRecord(&user) // 创建`user`后返回`false`
```

#### 默认值

可以通过**tag**定义字段的默认值

```go
type User struct {
    ID		int64
    Name	string `gorm:"default:'hello'"`
    Age		int64
}
```

***注意：通过`tag`定义字段的默认值，在创建记录时生成的SQL会排除没有值或值为零值的字段，在将记录插入到数据库后，Gorm会从数据库加载那些字段的默认值***

***所有字段的零值，比如`0`，`""`，`false`或者其它`零值`，都不会保存到数据库内，但会使用他们的默认值。如果要避免这种情况，可以考虑使用指针或实现`Scanner/Valuer`接口***

##### 使用指针方式将零值存入数据库

```go
// example

// 使用指针
type User struct {
    ID 		int64
    Name	*string `gorm:"default:'hello'"`
    Age		int64
}
user := User{Name: new(string), Age: 18}
db.Create(&user) // 此时数据库中该条记录name字段的值就是''
```

##### 使用Scanner/Valuer

```go
// example

// 使用Scanner/Valuer
type User struct {
    ID int64
    Name sql.NullString `gorm:"default:'hello'"` // sql.NullString实现了Scanner/valuer接口
    Age int64
}
user := User{Name: sql.NullString{"", true}, Age: 18}
db.Create(&user) // 此时数据库中该记录name字段的值就是''
```



## GORM查询操作

### 一般查询

```go
// example

// 根据主键查询第一条记录
db.First(&user)
//// SELECT * FROM users ORDER BY id LIMIT 1;

// 随机获取一条记录
db.Take(&user)
//// SELECT * FROM users LIMIT 1;

// 根据主键查询最后一条记录
db.Last(&user)
//// SELECT * FROM users ORDER BY id DESC LIMIT 1;

// 查询所有的记录
db.Find(&users)
//// SELECT * FROM users;

// 查询指定的某条记录（仅当主键为整型时可用）
db.First(&user, 10)
//// SELECT * FROM users WHERE id = 10;
```

### Where 条件

#### 普通SQL查询

```go
// example

// Get first matched record
db.Where("name = ?", "jinzhu").First(&user)
//// SELECT * FROM users WHERE name = 'jinzhu' limit 1;

// Get all matched records
db.Where("name = ?", "jinzhu").Find(&users)
//// SELECT * FROM users WHERE name = "jinzhu";

// <>
db.Where("name <> ?", "jinzhu").Find(&users)
//// SELECT * FROM users WHERE name <> 'jinzhu';

// IN
db.Where("name IN (?)", []string{"jinzhu", "jinzhu 2"}).Find(&users)
//// SELECT * FROM users WHERE name in ('jinzhu', 'jinzhu 2');

// LIKE
db.Where("name LIKE ?", "%jin%").Find(&users)
//// SELECT * FROM users WHERE name LIKE '%jin%';

// AND 
db.Where("name = ? AND age >= ?". "jinzhu", "22").Find(&users)
//// SELECT * FROM users WHERE name = 'jinzhu' AND age >= 22;

// Time
db.Where("update_at > ?", lastWeek).Find(&users)
//// SELECT * FROM users WHERE updated_at > '2000-01-01 00:00:00';

// BETWEEN
db.Where("created_at BETWEEN ? AND ?", lastWeek, today).Find(&users)
//// SELECT * FROM users WHERE created_at BETWEEN '2000-01-01 00:00:00' AND '2000-01-08 00:00:00'
```

#### Struct & Map查询

```go
// Struct
db.Where(&User{Name: "jinzhu", Age: 20}).First(&user)
//// SELECT * FROM users WHERE name = "jinzhu" AND age = 20 LIMIT 1;

// Map
db.Where(map[string]interface{}{"name": "jinzhu", "age": 20}).Find(&users)
//// SELECT * FROM users WHERE name = "jinzhu" AND age = 20;

// 主键的切片
db.Where([]int64{20, 21, 22}).Find(&users)
//// SELECT * FROM users WHERE id IN (20, 21, 22);
```

***当通过结构体进行查询时，GORM将会通过非零值字段查询，这意味着如果你的字段为`0`，`false`或者其它零值时，将不会被用于构建查询条件***

```go
db.Where(&user{Name: "jinzhu", Age: 0}).Find(&users)
//// SELECT * FROM users WHERE name = "jinzhu";
```

可以使用指针或实现**Scanner/Valuer**接口来避免这个问题

```go
// example

// 使用指针
type User struct {
    gorm.Model
    Name string
    Age *int
}

// 使用Scanner/Valuer
type User struct {
    gorm.Model
    Name string
    Age	sql.NullInt64	// sql.NullInt64 实现了 Scanner/Valuer 接口
}
```

#### Not条件

```go
// example

db.Not("name", "jinzhu").First(&user)
//// SELECT * FROM users WHERE name <> "jinzhu" LIMIT 1;

// Not In
db.Not("name", []string{"jinzhu", "jinzhu 2"}).Find(&users)
//// SELECT * FROM users WHERE name NOT IN("jinzhu", "jinzhu 2");

// Not In slice of primary keys
db.Not([]int64{1, 2, 3}).First(&user)
//// SELECT * FROM users WHERE id NOT IN (1,2,3);

db.Not([]int64{}).First(&user)
//// SELECT * FROM users;

// Plain SQL
db.Not("name = ?", "jinzhu").First(&user)
//// SELECT * FROM users WHERE NOT(name = "jinzhu");

// Struct
db.Not(User{Name: "jinzhu"}).First(&user)
//// SELECT * FROM users WHERE name <> "jinzhu";
```

#### Or条件

```go
db.Where("role = ?", "admin").Or("role = ?", "super_damin").Find(&users)
//// SELECT * FROM users WHERE role = 'admin' OR role = 'super_damin';

// Struct
db.Where("name = 'junzhu'").Or(User{Name: "jinzhu 2"}).Find(&users)
//// SELECT * FROM users WHERE name = 'jinzhu' OR name = 'jinzhu 2';

// Map
db.Where("name = 'jinzhu'").Or(map[string]interface{}{"name": "jinzhu 2"}).Find(&users)
//// SELECT * FROM users WHERE name = 'jinzhu' OR name = 'jinzhu 2';
```

#### 内联条件

作用与**Where**查询类似，当内联条件与多个**立即执行方法**一起使用时，内联条件不会传递给后面的立即执行

```go
// example

// 根据主键获取记录（只适用于整型主键）
db.First(&user, 23)
//// SELECT * FROM users WHERE id = 23 LIMIT 1;
// 根据主键获取记录，如果它是一个非整型主键
db.First(&user, "id = ?", "string_primary_key")
//// SELECT * FROM users WHERE id = 'string_primary_key' LIMIT 1;

// Plain SQL
db.Find(&user, "name = ?", "jinzhu")
//// SELECT * FROM users WHERE name = 'jinzhu';

db.Find(&users, "name <> ? AND age > ?", "jinzhu", "20")
//// SELECT * FROM users WHERE name <> "jinzhu" AND age > 20;

// Struct
db.Find(&users, User{Age: 20})
//// SELECT * FROM users WHERE age = 20;

// Map
db.Find(&users, map[string]interface{}{"age": 20})
//// SELECT * FROM users WHERE age = 20;
```

#### 额外查询选项

```go
// 为查询 SQL 添加额外的 SQL 操作
db.Set("gorm:query_option". "FOR UPDATE").First(&user, 10)
//// SELECT * FROM users WHERE id = 10 FOR UPDATE;
```

#### FirstOrInit

获得匹配的第一条记录，否则根据给定的条件初始化一个新的对象（仅支持**struct**和**map**条件）

```go
// example

// 未找到
db.FirstOrInit(&user, User{Name: "non_existing"})
//// user -> User{Name: "non_existing"}

// 找到
db.Where(User{Name: "Jinzhu"}).FirstOrInit(&user)
//// user -> User{Id: 111, Name: "Jinzhu", Age: 20}
db.FirstOrInit(&user, map[string]interface{}{"name": "Jinzhu"})
//// user -> User{Id: 111, Name: "Jinzhu", Age: 20}
```

#### Attrs

如果记录未找到，将使用参数初始化**struct**

```go
// example

// 未找到
db.Where(User{Name: "non_existing"}).Attrs(User{Age: 20}).FirstOrInit(&user)
//// SELECT * FROM USERS WHERE name = 'non_existing';
//// user -> User{Name: "non_existing", Age: 20}

db.Where(User{Name: "non_existing"}).Attrs("age", 20).FirstOrInit(&user)
//// SELECT * FROM USERS WHERE name = 'non_existing'
//// user -> User{Name: "non_existing", Age: 20}

// 找到
db.Where(User{Name: "jinzhu"}).Attrs(User{Age: 30}).FirstOrInit(&user)
//// SELECT * FROM USERS WHEWE name = 'jinzhu';
//// user -> User{Id: 111, Name: "Jinzhu", Age: 20}
```

#### Assign

不过记录是否找到，都将参数赋值给**struct**

```go
// example

// 未找到
db.Where(User{Name: "non_existing"}).Assign(User{Age: 20}).FirstOrInit(&user)
//// user -> User{Name: "non_existing", Age: 20}

// 找到
db.Where(User{Name: "Jinzhu"}).Assign(User{Age: 30}).FirstOrInit(&user)
//// SELECT * FROM USERS WHERE name = 'Jinzhu';
//// user -> User{Id: 111, Name: "Jinzhu", Age: 30}
```

#### FirstOrCreate

获取匹配的第一条记录，否则根据给定的条件创建一个新的记录（仅支持**struct**和**map**条件）

```go
// example

// 未找到
db.FirstOrCreate(&user, User{Name: "non_existing"})
//// INSERT INTO "users" (name) VALUES ("non_existing");
//// user -> User{Id: 112, Name: "non_existing"}

// 找到
db.Where(User{Name: "Jinzhu"}).FirstOrCreate(&user)
//// user ->  User{Id: 111, Name: "Jinzhu"}
```

***一样可以使用Attrs和Assign***



### 高级查询

#### 子查询

基于**\*grom.expr**的子查询

```go
// example

db.Where("amount > ?", DB.Table("orders").Select("AVG(amount)").Where("state = ?", "paid").QueryExpr())
// SELECT * 
// FROM `orders` 
// WHERE `orders`.`deleted_at` IS NULL 
// AND (
//	amount > (
//  	SELECT AVG(amount) 
//  	FROM `orders` 
//  	WHERE (sate = 'paid')
//	)
//)
```

#### 选择字段

**Select**，指定想从数据库中检索出的字段，默认会选择全部字段

```go
// example

db.Select("name, age").Find(&users)
//// SELECT name, age FROM users;

db.Select([]stirng{"name", "age"}).Find(&users)
//// SELECT name, age FROM users;

db.Table("users").Select("COALESCE(age,?)", 42).Rows()
//// SELECT COALESCE(age, '42') FROM users;
```

#### 排序

**Order**，指定从数据库中检索出记录的顺序。设置第二个参数**reorder**为**true**，可以覆盖前面定义的排序条件

```go
db.Order("age desc, name").Find(&users)
//// SELECT * FROM users ORDER BY age desc, name;

// 多字段排序
db.Order("age desc").Order("name").Find(&users)
//// SELECT * FROM users ORDER BY age desc, name;

// 覆盖排序
db.Order("age desc").Find(&users1).Order("age", true).Find(&users2)
//// SELECT * FROM users ORDER BY age, desc; (users1)
//// SELECT * FROM users ORDER BY age; (users2)
```

#### 数量

**Limit**，指定从数据库检索出的最大记录数

```go
db.Limit(3).Find(&users)
//// SELECT * FROM users LIMIT 3;

// -1 取消 Limit 条件
db.Limit(10).Find(&users1).Limit(-1).Find(&users2)
//// SELECT * FROM users LIMIT 10; (users1)
//// SELECT * FROM users; (users2)
```

#### 偏移

**Offset**，指定开始返回记录前要跳过的记录数

```go
db.Offset(3).Find(&users)
//// SELECT * FROM users OFFSET 3;

// -1 取消 Offset 条件
db.Offset(10).Find(&users1).Offset(-1).Find(&users2)
//// SELECT * FROM users OFFSET 10; (users1)
//// SELECT * FROM users; (users2)
```

#### 总数

**Count**，该**model**能获取的记录总数

```go
db.Where("name = ?", "jinzhu").Or("name = ?", "jinzhu 2").Find(&users).Count(&count)
//// SELECT * FROM users WHERE name = 'jinzhu' OR name = 'jinzhu 2'; (users)
//// SELECT COUNT(*) FROM users WHERE name = 'jinzhu' OR name = 'jinzhu 2'; (count)

db.Model(&User{}).Where("name = ?", "jinzhu").Count(&count)
//// SELECT COUNT(*) FROM users WHERE name = 'jinzhu'; (count)

db.Table("deleted_users").Count(&count)
//// SELECT COUNT(*) FROM deleted_users;

db.Table("deleted_users").Select("count(distinct(name))").Count(&count)
//// SELECT COUNT( DISTINCT(name) ) FROM deleted_users; (count)
```

***注意：`COUNT`必须是链式查询的最后一个操作，因为它会覆盖前面的`SELECT`，但如果里面使用了`count`时不会覆盖***

#### Group & Having

```go
rows, err := db.Table("orders").Select("date(created_at) as date, sum(amount) as total").Group("date(created_at)").Rows()
for rows.Next() {
    ...
}
```

#### 连接

**Joins**，指定连接条件

```go
rows, err := db.Table("users").Select("users.name, emails.email").Joins("left join emails on emails...")
for rows.Next() {
    ...
}
```

#### Pluck

**Pluck**，查询**model**中的一个列作为切片，如果想要查询多个列，应该使用`Scan`

```go
var ages []int64
db.Find(&users).Pluck("age", &ages)

var names []string
db.Model(&User{}).Pluck("name", &names)

db.Table("deleted_users").Pluck("name", &names)

// 想查询多个字段
db.Select("name, age").Find(&users)
```

#### 扫描

**Scan**，扫描结果至一个**struct**

```go
type Result struct {
    Name 	string
    Age		int
}
var result Result
db.Table("users").Select("name, age").Where("name = ?", "Antonio").Scan(&result)

// 原生
db.Row("SELECT name, age FROM users WHERE name = ?", "Antonio").Scan(&result)
```

### 链式操作相关

#### 链式操作

**Method Chaining**，**Gorm**实现了链式操作接口

```go
// example

// 创建一个查询
tx := db.Where("name = ?", "jinzhu")

// 添加更多条件
if someCondition {
    tx = tx.Where("age = ?", 20)
} else {
    tx = tx.Where("age = ?", 30)
}

if yetAnotherCondition {
    tx = tx.Where("active = ?", 1)
}
```

在调用立即执行方法前不会生成`Query`语句，借助这个特性可以创建一个函数来处理一些通用逻辑

### 立即执行方法

立即执行方法是指那些会立即生成`SQL`语句并发送到数据库的方法，他们一般是`CRUD`方法

比如：

`Create`，`First`，`Find`，`Take`，`Save`，`UpdateXXX`，`Delete`，`Scan`，`Row`，`Rows`...

这有一个基于上面链式方法代码的立即执行方法的例子

```go
tx.Find(&user)
```

生成的**SQL**语句如下

```go
SELECT * FROM users WHERE name = 'jinzhu' AND age = 30 AND active = 1;
```

### 范围

`Scopes`，**Scope**是建立在链式操作的基础之上的

基于它，可以抽取一些通用逻辑，写出更多可重用的函数库

```go
func AmountGreaterThan1000(db *gorm.DB) *gorm.DB {
    return db.Where("amount > ?", 1000)
}

func PaidWithCod(db *gorm.DB) *gorm.DB {
    return db.Where("pay_mode_sign = ?", "C")
}

db.Scopes(AmountGreaterThan1000, PainWithCrediCard).Find(&orders)
// 查找所有金额大于 1000 的信用卡订单
```

### 多个立即执行方法

**Multiple Immediate Methods**，在**GORM**中使用多个立即方法时，后一个立即执行方法会复用前一个**立即执行方法**的条件（不包含内联条件）

```go
db.Where("name LIKE ?", "jinzhu%").Find(&user, "id IN (?)", []int{1, 2, 3}).Count(&count)
```

生成的**SQL**

```mysql
SELECT * FROM users WHERE name LIKE 'jinzhu' AND id IN (1, 2, 3)

SELECT COUNT(*) FROM users WHERE name LIKE 'jinzhu%'
```



## GORM更新操作

### 更新

#### 更新所有字段

`Save()`默认会更新该对象的所有字段，即使没有赋值

```go
db.First(&user)

user.Name = "hello"
user.Age = 99
db.Save(&user)

//// UPDATE `users` SET `created_at` = '2023-01-16 12:52:20', `updated_at` = '2023-01-16 12:55:00'
```

#### 更新修改字段

如果只希望更新指定字段，可以使用`Update`或者`Updates`

```go
// 更新单个属性，如果有变化
db.Model(&user).Update("name", "hello")
//// UPDATE users SET name = 'hello', updated_at = '2020-12-31 00:00:00' WHERE `users`.`deleted_at` IS NULL AND `users`.`id` = 1;

// 使用 map 更新多个属性，只会更新其中变化的属性
db.Model(&user).Updates(map[string]interface{}{"name": "hello", "age": 18, "actived": false})

// 使用 struct 更新多个属性，只会更新其中有变化且为非零值的字段
db.Model(&user).Update(User{Name: "hello", Age: 18})
```

#### 更新选定字段

```go
// m1列出来的所有字段都会更新
db.Model(&user).Updates(m1)

// 只更新age字段
db.Model(&user).Select("age").Update(m1)

// 排除m1中的active更新其他的字段
db.Model(&user).Omit("active").Updates(m1)
```

#### 无**Hooks**更新

上面的更新操作会自动运行**model**的`BeforeUpdate`，`AfterUpdate`方法，更新`UpdateAt`时间戳，在更新时保存其`Associations`，如果你不想调用这些方法，你可以使用`UpdateColumn`，`UpdateColumns`

``` go
// example

// 更新单个属性，类似于`Update`
db.Model(&user).UpdateColumn("name", "hello")
//// UPDATE users SET name = 'hello' WHERE id = 1;

// 更新多个属性，类似于`Update`
db.Model(&user).UpdateColumns(User{Name: "hello", Age: 18})
//// UPDATE users SET name = 'hello', age = 18 WHERE id = 1;
```

#### 批量更新

批量更新时`Hooks(钩子函数)`不会运行

```go
db.Table("users").Where("id IN (?)", []int{10, 11}).Update(map[string]interface{"name": "hello", "age": 18})
//// UPDATE users SET name = 'hello', age = 18 WHERE id IN (10, 11);

// 使用 struct 更新时，只会更新非零值字段，若想更新所有字段，请使用map[string]interface{}
db.Model(User{}).Updates(User{Name: "hello", Age: 18})
//// UPDATE users SET name = 'hello', age = 18;

// 使用 `RowsAffected` 获取更新记录总数
db.Model(User{}).Updates(User{Name: "hello", Age: 18}).RowsAffected
```

#### 使用SQL表达式更新

先查询表中的第一条数据保存至`user`变量

```go
var user User
db.First(&user)
```

```go
// 让users表中所有的用户的年龄在原来的基础上 + 2
db.Model(&User{}).Update("age", gorm.Expr("age+?", 2))

db.Model(&user).UpdateColumn("age", gorm.Expr("age - ?", 1))
//// UPDATE "users" SET `age` = age - 1 WHERE `id` = 1;

db.Model(&user).Where("age > 10").UpdateColumn("age", gorm.Expr("age - ?", 1))
//// UPDATE `users` SET `age` = age - 1 WHERE `id` = 1 AND quantity > 10;
```

#### 修改Hooks中的值

如果想修改`BeforeUpdate`，`BeforeSave`等**Hooks**中更新的值，你可以使用`scope.SetColumn`

```go
func (user *User) BeforeSave(scope *gorm.Scope) (err error) {
    if pw, err := bcrypt.GenerateFromPassword(user.Password, 0); err == nil {
        scope.SetColumn("EncryptedPassword", pw)
    }
}
```

#### 其它更新选项

```go
// 为 update SQL 添加其它的 SQL
db.Model(&user).Set("gorm:update_option", "OPTION (OPTIMIZE FOR UNKNOWN)").Update("name", "hello")
//// UPDATE users SET name = 'hello', updated_at = 'xxxx-xx-xx xx:xx:xx' WHERE id = 1 OPTION (OPTIMIZE FOR UNKONOWN)
```



## GORM删除操作

### 删除记录

**警告**删除记录时，请确保主键字段有值，**GORM**会通过主键去删除记录，如果主键为空，**GORM**会删除该**model**的所有记录

```go
// 删除现有记录
db.Delete(&email)
//// DELETE FROM emails WHERE id = 10;

// 为删除 SQL 添加额外的 SQL 操作
db.Set("gorm:delete_option", "OPTION (OPTIMIZE FOR UNKNOWN)").Delete(&eamil)
//// DELETE FROM emails WHERE id = 10 OPTION (OPTIMIZE FOR UNKONWN);
```

### 批量删除

删除全部匹配的记录

```go
db.Where("email LIKE ?", "%jinzhu%").Delete(Email{})
//// DELETE FROM emails WHERE email LIKE '%jinzhu%';

db.Delete(Email{}, "email LIKE ?", "%jinzhu%")
//// DELETE FROM emails WHERE LIKE '%jinzhu%';
```

### 软删除

如果一个**model**有**DeleteAt**字段，将自动获得软删除的功能。当调用**Delete**方法时，记录不会真正的从数据库中被删除，只会将**DeletedAt**字段的值会被设置为当前时间

```go
db.Delete(&user)
//// UPDATE users SET deleted_at = 'xxxx-xx-xx xx:xx' WHERE id = 1;

// 批量删除
db.Where("age = ?", 20).Delete(&User{})
//// UPDATE usersSET deleted_at = 'xxxx-xx-xx xx:xx' WHERE age = 20;

// 查询记录时会忽略被软删除的记录
db.Where("age = 20").Find(&user)
//// SELECT * FROM users WHERE age = 20 AND deleted_at IS NULL;

// Unscoped 方法可以查询被软删除的记录
db.Unscoped().Where("age = 20").Find(&users)
//// SELECT * FROM uesrs WHERE age = 20;
```

### 物理删除

```go
// Unscoped 方法可以物理删除记录
db.Unscoped().Delete(&order)
//// DELETE FROM orders WHERE id = 10;
```

