# 门店运营看板 - 数据分离版

## 文件结构

```
.
├── dashboard.html      # 看板页面（固定不变）
├── data/
│   ├── main.json           # 主数据（订单、销售等）
│   ├── members.json        # 会员明细数据
│   ├── funnel.json         # 转化漏斗数据
│   ├── waimai.json         # 外卖数据
│   ├── city_scorecard.json # 城市打分卡数据
│   ├── store_cups.json     # 门店日销量分布
│   ├── store_daily_sales.json  # 门店城市映射
│   ├── store_system.json   # 门店体系映射
│   ├── system_channel.json # 体系渠道数据
│   ├── city_delivery.json  # 城市外卖渠道数据
│   └── china_map.json      # 中国地图地理数据
└── README.md
```

## 使用方式

### 方式一：本地预览

1. 使用任意HTTP服务器启动目录
2. 访问 `http://localhost:端口/dashboard.html`

```bash
# Python 3
python -m http.server 8080

# Node.js
npx serve .

# PHP
php -S localhost:8080
```

### 方式二：部署到服务器

将 `dashboard.html` 和 `data/` 目录一起部署到Web服务器：
- Nginx
- Apache
- 任意静态托管服务（GitHub Pages、Vercel、Netlify）

### 方式三：部署到内网

在内网服务器上启动HTTP服务，团队成员通过内网IP访问。

---

## 更新数据

### 更新主数据 (main.json)

主数据结构示例：
```json
[
  {
    "date": "2026-05-01",
    "store_type": "独立店",
    "order_source": "京东到家",
    "sales_volume": 1194.0,
    "revenue": 8614.49,
    "order_count": 1196,
    "store_count": 132,
    "store_ids": ["547481", "541650", ...]
  },
  ...
]
```

### 更新会员数据 (members.json)

会员数据结构：
```json
[
  [门店类型, 首刷日索引, 入会日索引, [消费日索引, 订单数, 营收], ...],
  ...
]
```

### 更新外卖数据 (waimai.json)

```json
{
  "独立店": {
    "京东到家": {
      "2026-05-01": {
        "exposure": 1000,
        "enter": 500,
        "order": 100,
        "score": 4.5
      }
    }
  }
}
```

---

## 注意事项

1. **必须通过HTTP服务器访问**：由于使用fetch加载JSON，本地直接打开HTML文件会因CORS限制无法加载数据
2. **日期格式**：所有日期必须使用 `YYYY-MM-DD` 格式
3. **门店类型**：必须使用 `独立店` 或 `店中店`
4. **数据一致性**：确保各JSON文件的数据日期范围一致

---

## 数据字段说明

### main.json

| 字段 | 说明 |
|------|------|
| date | 日期 (YYYY-MM-DD) |
| store_type | 门店类型 (独立店/店中店) |
| order_source | 订单来源 (京东到家/美团外卖/淘宝闪购) |
| sales_volume | 销量 |
| revenue | 实收金额 |
| order_count | 订单数 |
| store_count | 门店数 |
| store_ids | 门店ID列表 |
| member_count | 会员数 |
| takeout_revenue | 外卖实收 |

---

## 故障排除

### 数据加载失败

1. 检查浏览器控制台错误信息
2. 确认HTTP服务器正常运行
3. 检查data/目录下的JSON文件是否可访问
4. 确认JSON格式正确（可使用JSONLint验证）
