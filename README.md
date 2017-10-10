## 需要班次定义提供的接口

### 获取班次列表（用于排班管理选择给某个员工排什么班）

接口形式：

.getTotalShifts()

返回信息示例：

```js
{
    "error": 0,
    "data": [
        {
            "name": "默认排班",
            "timeZone": [
                {
                    "startTime": "9:00",
                    "endTime": "18:00"
                },
                {
                    "startTime": "9:30",
                    "endTime": "18:30"
                }
            ]
        },
        {
            "name": "审核组早班",
            "timeZone": [
                {
                    "startTime": "1:00",
                    "endTime": "9:00"
                }
            ]
        }
    ]
}
```

## 需要排班管理提供的接口

### 获取某个员工的排班信息（用于考勤结果管理判定某天某个员工的考勤状态以及统计一段时间该员工的工作时间）

接口形式：

.getSchedulingByEmployeeIds( employeeIds, duration )

**employeeIds**

Type: Array

员工的工号列表

**duration**

Type: String or Array

如果是String，则查询这些员工某一天的排班信息，如果是数组，那么数组的第一个元素是开始时间，数组的第二个元素是截止时间，这个时候查询的是这个时间段的排班列表信息

返回信息示例：

```js
{
    "error": 0,
    "data": [
        {
            "employeeId": "012222",
            "scheduling": [
                {
                    "date": "2017-10-09",
                    "shiftsName": "默认排班",
                    "timeZone": [
                        {
                            "startTime": "9:00",
                            "endTime": "18:00"
                        },
                        {
                            "startTime": "9:30",
                            "endTime": "18:30"
                        }
                    ]
                },
                {
                    "date": "2017-10-10",
                    "shiftsName": "早班",
                    "timeZone": [
                        {
                            "startTime": "1:00",
                            "endTime": "9:00"
                        }
                    ]
                }
            ]
        }
    ]
}
```

## 需要公休日管理提供的接口

### 获取一段时间的工作日和公休日情况

接口形式：

.getDays( startDate, endDate )

**startDate**

Type: String

查询的起始日期

**endDate**

Type: String

查询的终止日期

返回信息示例：

```js
{
    "error": 0,
    "data": {
        "workDayLength": 20, // 工作日的天数
        "holidayLength": 10, // 公休日的天数
        "holidayList": ["2017-10-01", "2017-10-02"] // 此处待定，这里可能会需要返回这个时间段每一天是工作日还是公休日，因为排班和考勤统计都需要日期列表，建议工作日和公休日都由公休日管理模块统一管理，其他模块不作自行判断，便于以后调整算法的时候好维护
    }
}
```

## 需要年假管理提供的接口

### 获取某些员工的年假信息（用于考勤结果管理做统计）

.getAnnualLeaveByEmployeeIds( employeeIds )

**employeeIds**

Type: Array

员工的工号列表

返回信息示例：

```js
{
    "error": 0,
    "data": [
        {
            "employeeId": "01111",
            "total": 5, // 总共年假
            "surplus": 3.5 // 剩余年假
        },
        {
            "employeeId": "02222",
            "total": 6,
            "surplus": 4
        }
    ]
}
```

## 需要请假模块提供的接口

### 获取一段时间某些员工的请假信息（用于排班的时候排除这些天）

.getLeaveInfoByEmployeeIds( employeeIds, startDate, endDate )

**employeeIds**

Type: Array

员工的工号列表

**startDate**

Type: String

查询的起始日期

**endDate**

Type: String

查询的截止日期

返回信息示例：

```js
{
    "error": 0,
    "data": [
        {
            "employeeId": "01111",
            "leaveInfo": [
                {
                    "date": "2017-10-12",
                    "startTime": "10:00",
                    "endTime": "15:00"
                },
                {
                    "date": "2017-10-15",
                    "startTime": "9:00",
                    "endTime": "18:00"
                }
            ]
        },
        {
            "employeeId": "02222",
            "leaveInfo": [
                {
                    "date": "2017-10-12",
                    "startTime": "10:00",
                    "endTime": "15:00"
                },
                {
                    "date": "2017-10-15",
                    "startTime": "9:00",
                    "endTime": "18:00"
                }
            ]
        }
    ]
}
```
