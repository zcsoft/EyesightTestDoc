# API
---

### 1.1：获取训练计划
> 接口地址：`GET` `/v1/user/training/plan`
>
>获取训练计划
>

#### 请求参数

| 参数名称 | 类型  | 必须 | 描述 |
| :--- |:---|:---| :--- |
| start_id | integer | | 要获取的训练计划的起始id |
| start_time | Date | | 要获取的训练计划的起始时间 |
| total | integer | | 要获取的训练计划数量 |

- 从start_id或start_time开始，获取total条数据。
- 返回的数据大于start_id或start_time（不包含）。
- 如果start_id和start_time均为空，则从第一条数据开始返回。
- total默认为20条。
- start_id优先于start_time。

#### 返回数据

| 参数名称 | 类型 | 描述 |
| :--- |:---| :--- |
| id | integer | 训练ID |
| time | Date | 训练时间 |
| mode_name | String | 模式名称 |
| params | [key:value] | 训练参数列表 |
| remark | String | 备注 |

```
{
    "code": 0
    "message": null
    "data": {
        "plans": [
            {
                "mode_name": "VisionRT",
                "time": "2025-09-02 16:17:34:875",
                "params": {
                    "brightness": "1",
                    "size": "1",
                    "upDown": "0",
                    "leftRight": "0",
                    "frequency": "10"
                }
            },
            {
                "mode_name": "Grating",
                "time": "2025-09-02 16:17:34:875",
                "params": {
                    "size": "0.15",
                    "brightness": "1",
                    "frequency": "10",
                    "wave": "0",
                    "contrast": "1"
                }
            }
        ]
    }
}
```


---

### 1.2：上传训练记录
> 接口地址：`POST` `/v1/user/training/record`
>
>上传训练记录
>

#### 请求参数

| 参数名称 | 类型  | 必须 | 描述 |
| :--- |:---|:---| :--- |
| dev_record_id | integer | | 数据在设备中的记录ID |
| type | String | | 训练类型名 |
| time | Date | | 训练时间 |
| params | [key:value] | | 训练参数 |
| duration | integer | | 训练持续时间（秒） |

```
{
    "records": [
        {
            "dev_record_id" : 2,
            "type" : "ShapeCompareRT",
            "time" : "2025-09-02 16:17:34:875",
            "duration" : 11,
            "params": {
                "question" : "较小物体",
                "answer" : "较小物体"，
                "latency" : "2.140065",
                "shape" : "三角形"，
                "eye" : "2",
                "luminance" : "1",
                "size" : "1",
                "level" : "5"
            }
        },
        ...
    ]
}
```

#### 返回数据

- 无

```
{
    "code": 0
    "message": null
}
```

---

### 2.1：创建训练计划
> 接口地址：`POST` `/v1/admin/training/plan`
>
>创建训练计划
>

- 当前测试接口，通过POST提交计划json后，可通过/v1/user/training/plan获取。
- 测试数据如下：
```
{
    "plans": [
        {
            "mode_name": "VisionRT",
            "time": "2025-09-02 16:17:34:875",
            "params": {
                "brightness": "1",
                "size": "1",
                "upDown": "0",
                "leftRight": "0",
                "frequency": "10"
            }
        },
        {
            "mode_name": "Grating",
            "time": "2025-09-02 16:17:34:875",
            "params": {
                "size": "0.15",
                "brightness": "1",
                "frequency": "10",
                "wave": "0",
                "contrast": "1"
            }
        }
    ]
}

```

---

### 2.2：获取训练记录
> 接口地址：`GET` `/v1/admin/training/record`
>
>创建训练计划
>

- 当前测试接口，通过GET请求，可以获取用户通过/v1/user/training/record提交的数据。


