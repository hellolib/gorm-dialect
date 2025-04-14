# gorm-dialect
Gorm v1 方言包

## 版本 
v1-dm8 （gorm v1版本 达梦8 数据库方言包）

## 使用说明
1. 拉取代码
```go
go get -u github.com/hellolib/gorm-dialect
```
2. 示例
```go
package main
import (
	_ "github.com/hellolib/gorm-dialect/dm" // DM驱动包路径
	"github.com/jinzhu/gorm"
)
type Product struct {
	gorm.Model
	Code string
	Price uint
}
func main() {
	db, err := gorm.Open("dm", "dm://SYSDBA:*****@localhost:5238")
	if err != nil {
		panic("failed to connect database")
	}
	defer db.Close()

	// Migrate the schema
	db.AutoMigrate(&Product{})

	// 创建
	db.Create(&Product{Code: "L1212", Price: 1000})

	// 读取
	var product Product
	//此处数字好像无任何作用，只占位，表示取一条数据，与和下一句合并查询
	db.First(&product,1)
	db.First(&product, "\"code\" = ?", "L1212") // 查询code为l1212的product

	db.Model(&product).Update("code", "D42")
	// 更新 - 更新product的price为2000
	db.Model(&product).Update("price", 2000)

	// 删除 - 删除product，运行时会加上删除时间
	db.Delete(&product,1)

	//全部删除
	//db.Unscoped().Delete(&product)
}
```


