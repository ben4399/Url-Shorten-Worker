# Url-Shorten-Worker API 文档

## 简介

**Url-Shorten-Worker** 是一个基于 Cloudflare Worker 的短网址生成服务，支持用户生成短网址、删除短网址、查询短网址及相关功能。

## 基础 URL

API 基础地址取决于你的 Worker 部署地址。

**示例：**

```
https://your-cloudflare-worker-url/
```

## 请求头（Request Headers）

- `Content-Type: application/json`（所有请求必须为 JSON 格式）

## API 端点

### 1. 新增短网址

#### 请求方法

```
POST
```

#### 请求路径

```

/
```

#### 请求参数

| 参数       | 类型   | 必须 | 描述                     |
| ---------- | ------ | ---- | ------------------------ |
| `cmd`      | String | 是   | 命令类型：`add`          |
| `url`      | String | 是   | 需要缩短的原始 URL       |
| `key`      | String | 否   | 可选的自定义短链接标识符 |
| `password` | String | 是   | 用于身份验证的密码       |

#### 请求示例

```
{
  "cmd": "add",
  "url": "https://example.com",
  "key": "custom-key", 
  "password": "your-password"
}
```

#### 响应示例

```
{
  "status": 200,
  "key": "custom-key",
  "error": ""
}
```

#### 错误示例

```
{
  "status": 500,
  "key": "",
  "error": "Error: Url illegal."
}
```

------

### 2. 删除短网址

#### 请求方法

```
POST
```

#### 请求路径

```
/
```

#### 请求参数

| 参数       | 类型   | 必须 | 描述                   |
| ---------- | ------ | ---- | ---------------------- |
| `cmd`      | String | 是   | 命令类型：`del`        |
| `key`      | String | 是   | 需要删除的短链接标识符 |
| `password` | String | 是   | 用于身份验证的密码     |

#### 请求示例

```
{
  "cmd": "del",
  "key": "custom-key",
  "password": "your-password"
}
```

#### 响应示例

```
{
  "status": 200,
  "key": "custom-key",
  "error": ""
}
```

#### 错误示例

```
{
  "status": 500,
  "key": "custom-key",
  "error": "Error: Key in protect_keylist."
}
```

------

### 3. 查询短网址

#### 请求方法

```
POST
```

#### 请求路径

```

/
```

#### 请求参数

| 参数       | 类型   | 必须 | 描述                   |
| ---------- | ------ | ---- | ---------------------- |
| `cmd`      | String | 是   | 命令类型：`qry`        |
| `key`      | String | 是   | 需要查询的短链接标识符 |
| `password` | String | 是   | 用于身份验证的密码     |

#### 请求示例

```
{
  "cmd": "qry",
  "key": "custom-key",
  "password": "your-password"
}
```

#### 响应示例

```
{
  "status": 200,
  "key": "custom-key",
  "url": "https://example.com",
  "error": ""
}
```

#### 错误示例

```
{
  "status": 500,
  "key": "custom-key",
  "error": "Error: Key not exist."
}
```

------

### 4. 查询所有短网址

#### 请求方法

```
POST
```

#### 请求路径

```

/
```

#### 请求参数

| 参数       | 类型   | 必须 | 描述               |
| ---------- | ------ | ---- | ------------------ |
| `cmd`      | String | 是   | 命令类型：`qryall` |
| `password` | String | 是   | 用于身份验证的密码 |

#### 请求示例

```
{
  "cmd": "qryall",
  "password": "your-password"
}
```

#### 响应示例

```
{
  "status": 200,
  "kvlist": [
    {
      "key": "custom-key1",
      "value": "https://example.com"
    },
    {
      "key": "custom-key2",
      "value": "https://another-example.com"
    }
  ],
  "error": ""
}
```

#### 错误示例

```
{
  "status": 500,
  "error": "Error: Config.load_kv false."
}
```

------

## 常见错误码

| 状态码 | 描述                                     |
| ------ | ---------------------------------------- |
| 200    | 请求成功                                 |
| 500    | 请求失败，可能是参数错误、密码错误等原因 |

------

## 保护关键字

系统中某些关键字（如 `password`）被列为保护关键字，无法通过 API 进行查询、添加或删除。
