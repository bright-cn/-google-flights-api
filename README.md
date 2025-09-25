# Google 航班爬取器

[![Promo](https://github.com/bright-cn/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/products/web-scraper/google-flights)

本仓库提供两种从 Google Flights 提取航班数据的方式：

1. 免费 Google Flights 爬虫：适合小规模数据提取
2. Google Flights 爬取 API：面向高并发、实时数据提取且请求不限量的生产级方案。隶属于 Bright Data 的 [SERP Scraping API](https://www.bright.cn/products/serp-api)。

## 目录
- [免费爬虫](#免费爬虫)
  - [环境准备](#环境准备)
  - [快速开始](#快速开始)
  - [示例输出](#示例输出)
  - [限制](#限制)
- [Google 航班爬取 API](#google-航班爬取-api)
  - [关键特性](#关键特性)
  - [先决条件](#先决条件)
  - [直接 API 访问](#直接-api-访问)
  - [原生代理访问](#原生代理访问)
- [其他参数](#其他参数)
  - [本地化参数](#本地化参数)
  - [货币参数](#货币参数)
- [支持与资源](#支持与资源)

## 免费爬虫
一个用于从 Google Flights 进行小规模数据提取的快速、简单的爬虫。

<img width="800" alt="google-flights-scraper" src="https://github.com/bright-cn/-google-flights-api/blob/main/images/424383720-44ae10b1-4974-497e-9a7c-c1a762614f0e.png" />

### 环境准备
- [Python 3.9+](https://www.python.org/downloads/)
- 用于浏览器自动化的 [Playwright](https://playwright.dev/)

```bash
pip install playwright
playwright install chromium
```

> 抓取新手？可先阅读我们的[Python 网页抓取入门指南](https://www.bright.cn/blog/how-tos/web-scraping-with-python)

### 快速开始
1. 打开 [google-flights-scraper.py](https://github.com/bright-cn/google-flights-api/blob/main/google-flights-scraper/google-flights-scraper.py)
2. 更新以下变量：
    - `url`：粘贴 Google Flights 的 URL（通常包含 `tfs`）
3. 运行脚本

💡 专业提示：将 `HEADLESS = False` 可降低被 Google 反抓取措施检测的概率。

### 示例输出
```json
{
  "airline": "Emirates",
  "departure_time": "4:15 AM",
  "arrival_time": "2:00 PM",
  "duration": "22 hr 15 min",
  "stops": "1 stop in DXB",
  "price": "$1,139",
  "co2_emissions": "1,092 kg CO2e",
  "emissions_variation": "+6% emissions"
}
```

👉 [查看完整输出示例](https://github.com/bright-cn/google-flights-api/blob/main/google-flights-results/flight_results.json)

### 限制
免费爬虫存在以下限制：
- IP 被封风险高
- 请求量能力有限
- 容易频繁触发 CAPTCHA
- 不适合生产环境

如需稳定、可扩展的抓取能力，请参考下方的专用 API。👇

## Google 航班爬取 API
[Bright Data 的 Google Flights 爬取 API](https://www.bright.cn/products/web-scraper/google-flights) 集成在 [SERP Scraping API](https://www.bright.cn/products/serp-api) 中，并依托我们广泛的[代理网络](https://www.bright.cn/proxy-types)，可在规模化场景下不触发 CAPTCHA 或 IP 封禁的情况下，实时提取价格、时刻表、航司信息等航班数据。

### 关键特性

- 全球准确性：为特定位置返回定制化结果
- 成功即付费：仅为成功请求付费
- 实时数据：数秒内获取最新航班数据
- 无限扩展性：轻松应对高并发抓取
- 成本高效：无需自建昂贵基础设施
- 稳定可靠：内置防封与反阻断技术
- 7x24 专家支持：随时获得协助

### 先决条件

1. [创建 Bright Data 账户](https://www.bright.cn/)（新用户可获 $5 额度）
2. 生成你的 [API 密钥](https://docs.brightdata.com/general/account/api-token)
3. 按照我们的[分步指南](https://github.com/bright-cn/google-flights-api/blob/main/setup-serp-api-guide.md)配置 SERP API 并完成凭据设置

### 直接 API 访问

向 API 端点直接发起请求。

cURL 示例：

```bash
curl https://api.brightdata.com/request \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_TOKEN" \
-d '{
zone: "ZONE_NAME",
url: "https://www.google.com/travel/flights/search?tfs=CBwQAhojEgoyMDI1LTA0LTAxagcIARIDREVMcgwIAxIIL20vMDRqcGxAAUgBcAGCAQsI____________AZgBAg",
format: "raw"
}'
```

Python 示例：

```python
import requests

url = "https://api.brightdata.com/request"
headers = {"Content-Type": "application/json", "Authorization": "Bearer API_TOKEN"}
payload = {
zone: "ZONE_NAME",
url: "https://www.google.com/travel/flights/search?tfs=CBwQAhojEgoyMDI1LTA0LTAxagcIARIDREVMcgwIAxIIL20vMDRqcGxAAUgBcAGCAQsI____________AZgBAg",
format: "raw",
}

response = requests.post(url, headers=headers, json=payload)

with open("google-flights-data.html", "w", encoding="utf-8") as file:
file.write(response.text)
print("HTML response saved to 'google-flights-data.html'.")
```

### 原生代理访问

或者，使用 Bright Data 的代理路由方式。

cURL 示例：

```bash
curl -i \
--proxy brd.superproxy.io:33335 \
--proxy-user "brd-customer-<customer-id>-zone-<zone-name>:<zone-password>" \
-k \
https://www.google.com/travel/flights/search?tfs=CBwQAhojEgoyMDI1LTA0LTAxagcIARIDREVMcgwIAxIIL20vMDRqcGxAAUgBcAGCAQsI____________AZgBAg
```

Python 示例：

```python
import requests
import urllib3

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

host = "brd.superproxy.io"
port = 33335
username = "brd-customer-<customer-id>-zone-<zone-name>"
password = "<zone-password>"
proxy_url = f"http://{username}:{password}@{host}:{port}"

proxies = {"http": proxy_url, "https": proxy_url}
url = "https://www.google.com/travel/flights/search?tfs=CBwQAhojEgoyMDI1LTA0LTAxagcIARIDREVMcgwIAxIIL20vMDRqcGxAAUgBcAGCAQsI____________AZgBAg"
response = requests.get(url, proxies=proxies, verify=False)

with open("google-flights-data.html", "w", encoding="utf-8") as file:
file.write(response.text)

print("Response saved to 'google-flights-data.html'.")
```

👉 查看[完整 HTML 输出](https://github.com/bright-cn/google-flights-api/blob/main/google-flights-api-output/google-flights-data.html)。

注意：用于生产时，请按照[SSL 证书指南](https://docs.brightdata.com/general/account/ssl-certificate)加载 Bright Data 的 SSL 证书。

## 其他参数
通过以下可选参数，精细化你的 Google Flights 数据提取。

### 本地化参数
<img width="800" alt="bright-data-google-flights-api-localization" src="https://github.com/bright-cn/-google-flights-api/blob/main/images/424454961-e77f10c9-8e44-46aa-be3d-64c756741479.png" />

可基于位置与语言定制结果：

| 参数 | 描述 | 示例 |
| --- | --- | --- |
| gl | 两字母国家代码 | `gl=us`（美国） |
| hl | 两字母语言代码 | `hl=en`（英语） |

示例：以法语检索巴黎到伦敦的航班：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
https://www.google.com/travel/flights/search?tfs=CBwQAhojEgoyMDI1LTA0LTAxagcIARIDQ0RHcgwIAxIIL20vMDRqcGxAAUgBcAGCAQsI____________AZgBAg&hl=fr&gl=fr
```

### 货币参数

<img width="800" alt="bright-data-google-flights-api-currency" src="https://github.com/bright-cn/-google-flights-api/blob/main/images/424820088-c571e99f-b854-449e-abc2-60149611ad5b.png" />

使用 `curr` 参数指定返回价格的货币。

示例：以美元（USD）返回价格。

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
https://www.google.com/travel/flights/search?tfs=CBwQAhojEgoyMDI1LTA0LTAxagcIARIDQ0RHcgwIAxIIL20vMDRqcGxAAUgBcAGCAQsI____________AZgBAg&hl=fr&gl=fr&curr=USD
```

## 支持与资源

- 文档：[SERP API 文档](https://docs.brightdata.com/scraping-automation/serp-api/)
- 相关 API： [Web Unlocker API](https://github.com/bright-cn/web-unlocker-api)、[SERP API](https://github.com/bright-cn/serp-api)、[Google Search API](https://github.com/bright-cn/google-search-api)、[Google News Scraper](https://github.com/bright-cn/Google-News-Scraper)、[Google Trends API](https://github.com/bright-cn/google-trends-api)、[Google Reviews API](https://github.com/bright-cn/google-reviews-api)、[Google Hotels API](https://github.com/bright-cn/google-hotels-api)
- Google 抓取教程：
- [如何抓取 Google Flights](https://www.bright.cn/blog/web-data/how-to-scrape-google-flights)
- [如何抓取 Google 搜索结果](https://www.bright.cn/blog/web-data/scraping-google-with-python)
- [如何抓取 Google 地图](https://www.bright.cn/blog/web-data/how-to-scrape-google-maps)
- 使用场景：
- [SEO 与 SERP 追踪](https://www.bright.cn/use-cases/serp-tracking)
- [旅游行业数据](https://www.bright.cn/use-cases/travel)
- 延伸阅读：[最佳 SERP API](https://www.bright.cn/blog/web-data/best-serp-apis)
- 联系支持：[support@brightdata.com](mailto:support@brightdata.com)
