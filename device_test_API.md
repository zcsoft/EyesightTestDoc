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
| start_time | Date | | 要获取的训练计划的起始日期,2025-09-25 |
| end_time | Date | | 要获取的训练计划的结束日期，2025-09-26 |
| total | integer | | 要获取的训练计划数量 |

- 最多获取total条数据。
- start_id：返回的数据大于start_id（返回结果不包含此ID）。
- start_time,end_time：返回日期范围内的数据（包含start_time和end_time，end_time为空返回start_time日期后的所有数据）。
- 如果start_id和start_time均为空，则从第一条数据开始返回。
- total默认为20条。
- start_id优先于start_time。

#### 返回数据

| 参数名称 | 类型 | 描述 |
| :--- |:---| :--- |
| id | integer | 训练ID |
| time | Date | 训练时间，2025-10-13 |
| updateTime | Date | 记录编辑时间，2025-09-02 16:17:34:875 |
| mode_name | String | 模式名称 |
| complete | Bool | 是否已完成训练（有训练记录） |
| params | [key:value] | 训练参数列表 |
| remark | String | 备注 |

```
GET http://47.94.34.9/v1/user/training/plan?start_time=2025-08-31&end_time=2025-09-02

{
    "code": 0,
    "message": "",
    "plans": [
        {
            "id": 3,
            "time": "2025-09-01",
            "mode_name": "Eshape",
            "updateTime": "2025-09-24 16:17:34:875",
            "params": {
                "Brightness": "0.1",
                "Size": "3",
                "Distance": "0.2"
            },
            "complete": false
        },
        {
            "id": 4,
            "time": "2025-09-01",
            "mode_name": "Eshape",
            "updateTime": "2025-09-24 16:17:34:875",
            "params": {
                "Brightness": "0.1",
                "Size": "3",
                "Distance": "0.2"
            },
            "complete": false
        }
    ]
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
| dev_record_id | integer | 否 | 数据在设备中的记录ID |
| type | String | | 训练类型名 |
| plan_id | Integer | | 对应训练计划ID |
| time | Data | | 训练时间 |
| params | [key:value] | | 训练参数 |
| params.accuracy | Float | | 准确率 |

```
{
    "records": [
        {
            "type" : "ShapeCompareRT",
            "plan_id": 3,
            "time": "2025-02-02 02:22:22:222",
            "params": {
                "accuracy": 0.23,
                "question" : "较小物体",
                "answer" : "较小物体",
                "latency" : "2.140065",
                "shape" : "三角形",
                "eye" : "2",
                "luminance" : "1",
                "size" : "1",
                "level" : "5"
            }
        }
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

