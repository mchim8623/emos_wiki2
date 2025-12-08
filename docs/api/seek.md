# 点播

在 [`视频列表`](./library.md#视频列表) 返回了 `seek_id` 点播ID

在 [`获取电视集列表`](./library.md#获取电视集列表) 返回了 `seek` 认领信息

## 参数字典

`status` 状态

- `default` 未认领
- `upload` 已认领
- `complete` 完成
- `cancel` 取消
- `forget` 遗忘

## 获取点播记录

`GET`
`/api/seek/history?video_list_id=[video_list_id]&video_season_id=[video_season_id]&video_episode_id=[video_episode_id]`

- `video_list_id` 视频ID
- `video_season_id` 视频季ID
- `video_episode_id` 视频集ID

三个参数为合并查询关系 使用时必须传一个

```json
[
    {
        // 用户名
        "username": "test1",
        // 本次悬赏的萝卜
        "carrot": 10,
        // 点播时间
        "created_at": "2025-11-29T16:31:39.000000Z"
    },
    {
        "username": "test2",
        "carrot": 0,
        "created_at": "2025-11-29T16:31:39.000000Z"
    }
]
```

## 查询认领信息

只有存在 `seek_id` 时才可进行查询 

> 根据 `video_type` 字段 `movie`单独查 `tv`可以用`seek`中内容

`GET` `/api/seek/query?seek_id=[seek_id]`

- `seek_id` 点播ID

```json
// 可认领时
{
    "seek_id": 1,
    // 状态
    "status": "default",
    // 认领用户名
    "upload_username": null,
    // 认领过期时间
    "upload_expired_at": null,
    // 是否可以认领
    "is_can_claim": true,
    // 点播总数
    "request_count": 1,
    // 悬赏萝卜
    "seek_carrot": 70,
    // 更新时间
    "updated_at": "2025-12-08T17:02:58.000000Z"
}
// 已被认领时 如果 status 为 upload 并且 is_can_claim 为 true 则是自己认领的
{
    "seek_id": 1,
    "status": "upload",
    "upload_username": "test1",
    "upload_expired_at": "2025-12-08T23:01:57.000000Z",
    "is_can_claim": false,
    "request_count": 1,
    "seek_carrot": 100,
    "updated_at": "2025-12-08T17:02:58.000000Z"
}
// 已完成时
{
    "seek_id": 1,
    "status": "complete",
    "upload_username": "test1",
    "upload_expired_at": null,
    "is_can_claim": false,
    "request_count": 1,
    "seek_carrot": 0,
    "updated_at": "2025-12-08T17:02:58.000000Z"
}
```

## 认领/取消认领

可以操作 认领信息中 `status` 状态为 `default` 或者是自己认领的 

`PUT` `/api/seek/claim`

### 请求

```json
{
    // 点播ID
    "seek_id": 1,
    // 类型 confirm 认领 cancel 取消认领
    "type": "confirm"
}
```

### 响应

```json
{
    "status": "upload"
}
```

## 催上片

只有当 `seek_id` 不为空时并且 `medias_count` 资源总数为0时 才可以催

`PUT` `/api/seek/claim?seek_id=[seek_id]&carrot=[carrot]`

### 请求

- `seek_id` 点播ID
- `carrot` 本次悬赏的萝卜 `int`类型

### 响应

```json
{
    // 此资源总共悬赏的萝卜
    "seek_carrot": 0   
}
```
