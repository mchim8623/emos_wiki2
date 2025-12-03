# 使用

系统分为 `/api` 和 `/server` 入口

未写在本 `wiki` 上的属于内部接口 改动可能比较频繁

todo...

## 安装配置

### R2

#### CORS

```json
[
  {
    "AllowedOrigins": [
      "https://emos.lol",
      "http://localhost:5173"
    ],
    "AllowedMethods": [
      "GET",
      "POST",
      "PUT"
    ],
    "AllowedHeaders": [
        "*"
    ]
  }
]
```