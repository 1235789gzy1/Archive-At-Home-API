# Archive-At-Home-API
[统一归档（A@H）](https://github.com/taskmgr818/archive-at-home)项目API

已适配项目：

[[JHenTai]](https://github.com/jiangtian616/JHenTai)

# 接口规范文档

该文档描述了基于 FastAPI 构建的接口服务，包括用户验证、解析下载链接、查询 GP 余额和签到功能。所有接口均使用 POST 方法，数据格式为 JSON。

---

## 通用返回格式

```json
{
  "code": 0,
  "msg": "提示信息",
  "data": {}
}
```

- `code`: 整型，状态码，0 表示成功，其他为错误码。
- `msg`: 字符串，提示信息。
- `data`: 字典，接口返回的具体数据内容。

---

## 1. `/resolve` 解析下载链接

- **方法**：POST
- **描述**：解析画廊下载地址，消耗 GP。
- **请求参数**：

```json
{
  "apikey": "string",
  "gid": "string",
  "token": "string"
}
```

- **返回字段**：

成功时：

```json
{
  "code": 0,
  "msg": "解析成功",
  "data": {
    "archive_url": "string"
  }
}
```

错误码说明：
- 1: 参数不完整
- 2: 无效的 API Key
- 3: 您已被封禁
- 4: 获取画廊信息失败
- 5: GP 不足
- 6: 解析失败

---

## 2. `/balance` 查询 GP 余额

- **方法**：POST
- **描述**：查询当前用户剩余 GP。
- **请求参数**：

```json
{
  "apikey": "string"
}
```

- **返回字段**：

```json
{
  "code": 0,
  "msg": "查询成功",
  "data": {
    "current_GP": int
  }
}
```

错误码说明：
- 1: 参数不完整
- 2: 无效的 API Key
- 3: 您已被封禁

---

## 3. `/checkin` 签到领取 GP

- **方法**：POST
- **描述**：每日签到领取 GP。
- **请求参数**：

```json
{
  "apikey": "string"
}
```

- **返回字段**：

签到成功：

```json
{
  "code": 0,
  "msg": "签到成功",
  "data": {
    "get_GP": int,
    "current_GP": int
  }
}
```

已签到：
```json
{
  "code": 7,
  "msg": "今日已签到",
  "data": {}
}
```

错误码说明：
- 1: 参数不完整
- 2: 无效的 API Key
- 3: 您已被封禁

---

## 公共异常码

- 99: 服务器内部错误

