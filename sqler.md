---
title: SQLer
date: 2019-01-11 08:17:31
tags:
- golang
categories:
- golang
- GitHub精选
---

> `SQL-er` 是一个微型 `http` 服务器，用 `Go` 语言编写，将旧的 `CGI` 概念应用于 SQL 查询。SQLer 允许编写端点并分配一个 SQL 查询，以便任何人点击它时能执行查询。此外 SQLer 还允许自定义验证规则，可验证请求正文或查询参数。SQLer 使用 nginx 样式配置语言（[HCL](https://github.com/hashicorp/hcl)）。

## 特性

- 无需依赖，可独立使用；
- 支持多种数据可类型，包括：`SQL Server`, `MYSQL`, `SQLITE`, `PostgreSQL`, `Cockroachdb` 等；
- 内置 RESTful 服务器；
- 内置 `RESP Redis 协议`，可以使用任何 `redis 客户端`连接到 SQLer；
- 内置 `Javascript 解释器`，可轻松转换结果；
- 内置验证器；
- 自动使用预备语句；
- 使用（[HCL](https://github.com/hashicorp/hcl)）配置语言；
- 可基于 `unix glob` 模式加载多个配置文件；
- 每条 `SQL` 查询可被命名为`宏`；
- 在每个宏内可使用 Go [text/template](https://golang.org/pkg/text/template/)；
- 每个宏都有自己的 `Context`（`query params` + `body params`）作为.Input(map [string] interface{})，而`.Utils`是辅助函数列表，目前它只包含 SQLEscape;
- 可自定义授权程序，授权程序只是一个简单的 webhook，sqler 使用这个 webhook 验证是否应该完成某请求。

## 下载

- go get github.com/alash3al/sqler

- 源代码：[github.com/alash3al/sqler](http://github.com/alash3al/sqler)

- 二进制版本：[releases](https://github.com/alash3al/sqler/releases)

## 配置概况

```hcl
// create a macro/endpoint called "_boot",
// this macro is private "used within other macros" 
// because it starts with "_".
// this rule only used within `RESTful` context.
_boot {
    // the query we want to execute
    exec = <<SQL
        CREATE TABLE IF NOT EXISTS `users` (
            `ID` INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
            `name` VARCHAR(30) DEFAULT "@anonymous",
            `email` VARCHAR(30) DEFAULT "@anonymous",
            `password` VARCHAR(200) DEFAULT "",
            `time` INT UNSIGNED
        );
    SQL
}

// adduser macro/endpoint, just hit `/adduser` with
// a `?user_name=&user_email=` or json `POST` request
// with the same fields.
adduser {
    // what request method will this macro be called
    // default: ["ANY"]
    // this only used within `RESTful` context.
    methods = ["POST"]

    // authorizers,
    // sqler will attempt to send the incoming authorization header
    // to the provided endpoint(s) as `Authorization`,
    // each endpoint MUST return `200 OK` so sqler can continue, other wise,
    // sqler will break the request and return back the client with the error occurred.
    // each authorizer has a method and a url.
    // this only used within `RESTful` context.
    authorizers = ["GET http://web.hook/api/authorize", "GET http://web.hook/api/allowed?roles=admin,root,super_admin"]

    // the validation rules
    // you can specify separated rules for each request method!
    rules {
        user_name = ["required"]
        user_email =  ["required", "email"]
        user_password = ["required", "stringlength: 5,50"]
    }

    // the query to be executed
    exec = <<SQL
       {{ template "_boot" }}

        /* let's bind a vars to be used within our internal prepared statement */
        {{ .BindVar "name" .Input.user_name }}
        {{ .BindVar "email" .Input.user_email }}
        {{ .BindVar "emailx" .Input.user_email }}

        INSERT INTO users(name, email, password, time) VALUES(
            /* we added it above */
            :name,

            /* we added it above */
            :email,

            /* it will be secured anyway because it is encoded */
            '{{ .Input.user_password | .Hash "bcrypt" }}',

            /* generate a unix timestamp "seconds" */
            {{ .UnixTime }}
        );

        SELECT * FROM users WHERE id = LAST_INSERT_ID();
    SQL
}

// list all databases, and run a transformer function
databases {
    exec = "SHOW DATABASES"

    transformer = <<JS
        // there is a global variable called `$result`,
        // `$result` holds the result of the sql execution.
        (function(){
            newResult = []

            for ( i in $result ) {
                newResult.push($result[i].Database)
            }

            return newResult
        })()
    JS
}
```

## 支持的验证规则

- 没有args的简单验证方法: [这里](https://godoc.org/github.com/asaskevich/govalidator#TagMap)
- 有args高级验证方法: [这里](https://godoc.org/github.com/asaskevich/govalidator#ParamTagMap)

## 支持的Utils

- `.Hash <method> `- 使用指定的方法散列指定的输入 [md5, sha1, sha256, sha512, bcrypt], `{{ "data" | .Hash "md5" }}`
- `.UnixTime` - 以秒为单位返回unix时间, `{{ .UnixTime }}`
- `.UnixNanoTime` - 以纳秒为单位返回unix时间, `{{ .UnixNanoTime }}`
- `.Uniqid` - 返回唯一id, `{{ .Uniqid }}`

## 使用

1. 在release页面中，使用适合您的操作系统的二进制文件安装`sqler`。
1. 假设您下载了`sqler_darwin_amd64`
1. 我们将其重命名为sqler，并将其复制到`/usr/local/bin`
1. 现在运行`sqler -h`，下一个

```bash
                        ____   ___  _
                        / ___| / _ \| |    ___ _ __
                        \___ \| | | | |   / _ \ '__|
                         ___) | |_| | |__|  __/ |
                        |____/ \__\_\_____\___|_|

                     将SQL查询转换为安全有效的RESTful api.


  -config string
        the config file(s) that contains your endpoints configs, it accepts comma seprated list of glob style pattern (default "./config.example.hcl")
  -driver string
        the sql driver to be used (default "mysql")
  -dsn string
        the data source name for the selected engine (default "root:root@tcp(127.0.0.1)/test?multiStatements=true")
  -resp string
        the rest api listen address (default ":3678")
  -rest string
        the rest api listen address (default ":8025")
  -workers int
        the maximum workers count (default 4)
```

1. 您可以指定多个文件 `-config`, 例如 `-config="/my/config/dir/*.hcl,/my/config/dir2/*.hcl"`
1. 你需要指定你需要从以下驱动:

驱动 | DSN
--- | ---
mysql | usrname:password@tcp(server:port)/dbname?option1=value1&...
postgres | postgresql://username:password@server:port/dbname?option1=value1
sqlite3 | /path/to/db.sqlite?option1=value1
sqlserver | sqlserver://username:password@host/instance?param1=value&param2=value
  | sqlserver://username:password@host:port?param1=value&param2=value
  | sqlserver://sa@localhost/SQLExpress?database=master&connection+timeout=30
mssql | server=localhost\\SQLExpress;user id=sa;database=master;app name=MyAppName
  | server=localhost;user id=sa;database=master;app name=MyAppName
  | odbc:server=localhost\\SQLExpress;user id=sa;database=master;app name=MyAppName
  | odbc:server=localhost;user id=sa;database=master;app name=MyAppName
hdb (SAP HANA) | hdb://user:password@host:port

