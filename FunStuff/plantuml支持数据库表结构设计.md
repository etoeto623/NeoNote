由于`plantuml`官方对数据库表结构设计的支持比较简单，要比较好的绘制表结构，可以通过第三方扩展来实现
一个第三方扩展是：[https://github.com/chippyash/db-plantuml](https://github.com/chippyash/db-plantuml)

一个例子如下：
``` plantuml
!includeurl https://raw.githubusercontent.com/chippyash/db-plantuml/master/dist/DatabaseLogical.iuml

Table(user, user, User in our system) {
	primary()
	idx(tag, string(30))
	unique(username, string(30))
	not_null(bar, text())
	not_null(password, string(30))
	not_null(guid, char(36))
	unsigned(count)
	idx2(username, password)
}
note right of user
It's debatable as to whether we should be identifying
indexes and unique indexes (keys) at the logical level. Identifying
a primary key is usually required. If the PK is a string then
it is as well to create the psuedo int PK and add the actual
key as a unique key element for clarity.
end note
Table(session, session, session for user) {
	primary()
	not_null(data, text)
}
onemany(user,session,'will have')
note right on link
A one to many
relationship
end note

Entity(ac, account) {
    idx(logon, string())
}
zeromany(user,ac,may have)
note right on link
0+ to many
relationship
end note

Table(profile, profile, Some info of user) {
age int()
not_null(birthday, datetime())
}
oneone(user,profile,'has a')
note right on link
A one to one relationship.
Usually a reason to refactor
at the physical design stage
end note

Table(group, group, group of users) {
	primary()
	not_null(name, string(20))
}
manymany(user, group, 'can be in', 'can have')

note top on link
A many to many
relationship
end note

Type(gender, Gender) {
	MALE
	FEMALE
}
depends(profile, gender, gender)
note right on link
Denotes a dependency.
i.e. profile has a gender attribute
that depends on being a specific Type (value)
end note

Set(fav_colours,Colour) {
	RED
	BLUE
	GREEN
}
depends(profile, fav_colours, fav_colours)
note right on link
Denotes a dependency.
i.e. profile has a fav_colours attribute
that depends on being zero,one or more of a Set
end note

```

``` plantuml
@startuml
!includeurl https://raw.githubusercontent.com/chippyash/db-plantuml/master/dist/DatabasePhysical.iuml

Table(user, user, User in our system) {
	primary()
	idx(tag, string(30))
	unique('username', string(30))
	not_null(bar, text())
	not_null(password, string(30))
    not_null(guid, char(36))
    unsigned(count)
	idx2(username, password)
}

Table(session, session, session for user) {
	primary()
	not_null(data, text)
}
onemany(user,session,'will have')

Entity(ac, account) {
    idx(logon, string())
}
zeromany(user,ac,may have)

Table(profile, profile, Some info of user) {
age SMALLINT
not_null(birthday, datetime())
}
oneone(user,profile,'has a')

Table(group, group, group of users) {
	primary()
	not_null(name, string(20))
}
manymany(user, group, 'can be in', 'can have')

Type(gender, Gender) {
	MALE
	FEMALE
}
note left of gender
The actual implementation of this is db dependent.
In MySql et al, the gender column would be defined as
""gender enum('MALE','FEMALE')""
end note
depends(profile, gender, gender)

Set(fav_colours,Colour) {
	RED
	BLUE
	GREEN
}
depends(profile, fav_colours, fav_colours)
note right of fav_colours
The actual implementation of this is db dependent.
In MySql et al, the  column fav_colours would be defined as
""fav_colours set('RED','BLUE','GREEN')""
end note
/'
This is the extra functionality that the physical rendering provides
'/
'comment out next line to not see trigger and proc methods
show methods

View(v1, sessions) {
select(user_id, data from user\njoin session on\n(user.id = session.user_id))
}

Trigger(t1, 'UserUpdate') {
	beforeUpdate()
	afterUpdate()
}
triggers(user, t1)
Proc(p1, StoredProcs) {
	addUser(IN uid INT, IN guid INT)
}
uses(p1, user)
uses(p1, group)

@enduml
```

另一个自动给数据库生成plantuml图的工具: [https://github.com/Hywan/Database-to-PlantUML](https://github.com/Hywan/Database-to-PlantUML)

其他工具
| 地址                                                                   | 说明                           |
| ---------------------------------------------------------------------- | ------------------------------ |
| [https://github.com/achiku/planter](https://github.com/achiku/planter) | 给postgres数据库生成plantuml图 |
| [https://github.com/k1LoW/tbls](https://github.com/k1LoW/tbls)         | 生成表结构视图                               |

