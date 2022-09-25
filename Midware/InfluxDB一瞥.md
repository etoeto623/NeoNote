>InxluxDB是一款时序数据库（Time Serial Data Base），主要用来存储系统监控数据、IOT数据等写多读少、不需要事务的海量数据
# 开始使用
## 安装
可以通过Docker来安装，docker hub官网是[influxDB docker hub](https://hub.docker.com/_/influxdb)，安装命令如下：
``` shell
docker pull influxdb:2.2.0
```
启动命令如下：
``` shell
docker run -p 8086:8086 \
      -v influxdb:/var/lib/influxdb \
      influxdb:2.2.0
```
启动完成以后，可以通过`localhost:8086`访问influxDB 2.x版本自带的管理页面
``` ad-note
user: neo
pwd: 1qaz!QAZ
```
## 数据格式
根据官网的描述，向InfluxDB写的数据要符合`Line Protocol`，具体的要求如下：
**[Line Protocol](https://docs.influxdata.com/influxdb/cloud/reference/syntax/line-protocol/)**
. Verify your line protocol file adheres to the following conventions:
-   Each line represents a data point.
-   Each data point requires a:
    -   [_measurement_](https://docs.influxdata.com/influxdb/cloud/reference/syntax/line-protocol/#measurement)
    - (Optional) [_tag set_](https://docs.influxdata.com/influxdb/cloud/reference/syntax/line-protocol/#tag-set)
    -   [_field set_](https://docs.influxdata.com/influxdb/cloud/reference/syntax/line-protocol/#field-set)
    -   [_timestamp_](https://docs.influxdata.com/influxdb/cloud/reference/syntax/line-protocol/#timestamp)
一条合法的数据如下：
``` text
measurementName,tagKey=tagValue fieldKey="fieldValue" 1465839830100400200
--------------- --------------- --------------------- -------------------
       |               |                  |                    |
  Measurement       Tag set           Field set            Timestamp
```
## 使用文档
官网的使用文档地址是：[https://docs.influxdata.com/influxdb/v2.2/](https://docs.influxdata.com/influxdb/v2.2/)
## 简单操作
### 写数据
InfluxDB提供了写数据的api接口，使用示例如下：
``` shell
curl --location --request POST 'http://localhost:8086/api/v2/write?org=NEO&bucket=pot&precision=ns' \
--header 'Accept: application/json' \
--header 'Authorization: Token wsQ6j79fw0daouJUNlWzHtkAO0FvD-Q8eKJwQbkKWAA4_hQIvfk3nZRBHt4xJQcPn0lqvnd_FROnyb9Gid-WAA==' \
--header 'Content-Type: text/plain' \
--data-binary 'cpuUsage,type=osx usage=0.55 1655383996543000000'
```
## 插件
InfluxDB提供了各种插件，进行各种数据的收集。插件列表见[plugins](https://docs.influxdata.com/telegraf/v1.22/plugins/#input-plugins)
### telegraf插件
插件的安装方法参考：[https://portal.influxdata.com/downloads/](https://portal.influxdata.com/downloads/)
Mac的安装方法如下：
``` shell
brew install telegraf
```
使用方式为：Data -> Telegraf -> Create Configuration -> Choose Plugin
#### 完整的示例
- Create Configuration
- 使用telegraf开启数据收集
``` shell
# 设置服务器端的token
export INFLUX_TOKEN=8PtSr_N-URD3JWzY9dyY2ATiIYP9UmWPcsU9-k7UmPhmC1R31DU-jCvj4BgsQt2SdO51c-F3qNiks__NLEwEuQ==
# 从服务器端拉取配置，并根据配置运行数据收集
telegraf --config http://localhost:8086/api/v2/telegrafs/09872a5da531e000
```

# 详解
## 一些重要的概念
- organization: 数据所属的组织
- bucket: 数据所属的bucket
- token: 操作授权的token，可以在influxDB的后台页面中创建，也可以通过`influx`命令行创建

#influxdb