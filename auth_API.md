# 用户授权API
---

### 1.1：登录
> 接口地址：`POST` `/v1/auth/login`  
>
>使用用户名与密码换取一对令牌（访问令牌 + 刷新令牌）。

#### 请求参数

| 参数名称 | 类型  | 必须 | 描述 |
| :--- |:---|:---|:---|
| username | String | 是 | 登录用户名 |
| password | String | 是 | 登录密码 |

##### 请求示例
```json
{
  "username": "demo",
  "password": "123456"
}
```

#### 返回数据

| 参数名称 | 类型 | 描述 |
| :--- |:---| :--- |
| userId | integer | 用户ID |
| role | String | 用户角色 |
| accessToken | String | 访问token |
| refreshToken | String | 刷新token |
| accessTokenExpiresInSec | integer | accessToken过期时间（秒） |
| refreshTokenExpiresInSec | integer | refreshToken过期时间（秒） |

成功示例：

```json
{
    "code": 200,
    "message": "success",
    "data": {
        "userId": 12,
        "role": "ADMIN",
        "accessToken": "70b98915779c493a8b6d1e187eca049c",
        "refreshToken": "67c5123a6bf443c8919d6b64eb446673",
        "accessTokenExpiresInSec": 600,
        "refreshTokenExpiresInSec": 604800
    },
    "count": 0
}
```

失败示例：
```json
{
    "code": 401,
    "message": "用户名或密码错误",
    "data": null,
    "count": 0
}
```

---

### 1.2：刷新令牌
> 接口地址：`POST` `/v1/auth/refresh`  
>
>使用刷新令牌获取新的访问令牌/刷新令牌对。

#### 请求参数

| 参数名称 | 类型  | 必须 | 描述 |
| :--- |:---|:---|:---|
| refreshToken | String | 是 | 刷新令牌 |

##### 请求示例
```json
{
  "refreshToken": "6c70904248b34a04b4dcaaab920ab455"
}
```

#### 返回数据

成功示例：
```json
与`POST` `/v1/auth/login` 相同
```

失败示例：
```json
{
    "code": 401,
    "message": "刷新失败，请重新登录",
    "data": null,
    "count": 0
}
```

---

### 1.3：登出
> 接口地址：`POST` `/v1/auth/logout`  
>
>作废当前访问令牌与（可选）刷新令牌。



#### 请求参数

无


#### 返回数据

```json
{
  "code": 0,
  "message": null,
  "data": null
}
```

---

### 1.4：获取当前用户信息
> 接口地址：`GET` `/v1/auth/me`  
>
>获取当前登录用户的认证信息。

#### 请求参数

- 无

#### 返回数据

- 成功：返回 `AuthUser`
- 未登录：`400 未登录`

成功示例：
```json
{
    "code": 200,
    "message": "success",
    "data": {
        "userId": 12,
        "username": "admin",
        "role": "ADMIN"
    },
    "count": 0
}
```

未登录示例：
```json
{
    "code": 401,
    "message": "Unauthorized"
}
```

---

## 通用说明

- 身份校验：需要登录的接口必须在 `Authorization` 头中携带 `Bearer {accessToken}`。
- accessToken用于访问授权，refreshToken用于刷新accessToken，过期时间需客户端自行检查处理。
- 令牌撤销：登出会使访问令牌和刷新令牌立即失效。
- 登录流程：
    - 调用登录接口获取accessToken，并在后续所有请求头中携带Authorization；
    - 每次请求之前，判断accessToken是否过期，如果过期，则调用POST /v1/auth/refresh获取新的accessToken；
    - 用户退出登录时，会使所有令牌失效；