简单的go连接数据库的示例如下
``` go
package main  
  
import (  
   "database/sql"  
   "encoding/json"   "fmt"   _ "github.com/go-sql-driver/mysql"  
)  
  
func main() {  
   db, _ := openDB()  
   rows, _ := db.Query("select name,id_no from user_base limit 1")  
  
   var user User  
   for rows.Next() {  
      e := rows.Scan(&user.Name, &user.IdNo)  
      if e != nil {  
         fmt.Println(e.Error())  
         continue  
      }  
      d, _ := json.Marshal(user)  
      fmt.Println(string(d))  
   }  
}  
  
type User struct {  
   Name string  
   IdNo string  
}  
  
// 获取db连接  
func openDB() (*sql.DB, error) {  
   db, _ := sql.Open("mysql", "root:1991623@tcp(127.0.0.1:3308)/meeting")  
   db.SetConnMaxLifetime(100)  
   db.SetMaxIdleConns(1)  
   if err := db.Ping(); err != nil {  
      fmt.Println("mysql connect error")  
      return nil, err  
   }  
  
   return db, nil  
}
```


#go #mysql 