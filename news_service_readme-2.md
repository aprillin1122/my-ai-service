# 📰 新聞搜尋與分析 AI Service

> 整合台灣多個新聞來源的智能新聞聚合、搜尋與 AI 分析平台

[![Python](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/flask-3.0.0-green.svg)](https://flask.palletsprojects.com/)
[![OpenAI](https://img.shields.io/badge/OpenAI-API-412991.svg)](https://openai.com/)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## 🎯 專案簡介

這是一個功能完整的新聞 AI 服務，提供：
- 📡 **多來源新聞聚合**：整合自由時報、Taipei Times、Taiwan News、中央社等台灣主流媒體
- 🔍 **智能搜尋引擎**：關鍵字搜尋、多重篩選、即時抓取
- 🤖 **AI 深度分析**：新聞摘要、5W1H 提取、情緒分析、關鍵字萃取
- 📊 **媒體報導比較**：分析不同媒體對同一事件的報導角度
- 🌐 **RESTful API**：完整的 API 介面，易於整合

## ✨ 核心功能

### 1. 新聞聚合 (News Aggregation)
- 支援多個台灣新聞來源的 RSS feeds
- 即時抓取最新新聞
- 自動分類與標籤

### 2. 智能搜尋 (Smart Search)
- 關鍵字全文搜尋
- 依來源篩選
- 時間範圍過濾
- 相關度排序

### 3. AI 分析 (AI Analysis)
- **摘要生成**：2-3 句話精簡摘要
- **5W1H 提取**：Who, What, When, Where, Why, How
- **情緒分析**：正面/中性/負面傾向判斷
- **關鍵字提取**：自動提取重點關鍵字
- **🆕 深度評析報告**：800-1200字專業多維度分析
  - 📚 關鍵背景補述（歷史脈絡、術語釋疑、利害關係人）
  - 🔍 延伸資訊與多方觀點（自動搜尋相關報導、資訊對照）
  - 🎯 潛在立場與批判性解讀（框架分析、對照觀點建構）
  - 🔮 前瞻性推演（情境優化與惡化分析）

### 4. 媒體比較 (Media Comparison)
- 同主題多媒體報導比較
- 分析報導角度差異
- 統計各媒體報導數量

## 📦 安裝與設定

### 系統需求

- Python 3.10 或更高版本
- OpenAI API Key
- 網路連線（用於抓取 RSS feeds）

### 快速安裝

```bash
# 1. 克隆專案
git clone https://github.com/yourusername/news-ai-service.git
cd news-ai-service

# 2. 建立虛擬環境
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 3. 安裝依賴
pip install -r requirements.txt

# 4. 設定環境變數
export OPENAI_API_KEY="your-api-key-here"

# 5. 啟動服務
python news_service.py
```

### requirements.txt

```txt
flask==3.0.0
openai==1.12.0
python-dotenv==1.0.0
feedparser==6.0.10
requests==2.31.0
beautifulsoup4==4.12.2
lxml==4.9.3
```

## 🔌 API 文檔

### 基礎資訊

- **Base URL**: `http://localhost:5000`
- **Content-Type**: `application/json`
- **編碼**: UTF-8

### API 端點

#### 1. 取得最新新聞

```http
GET /api/news/latest?source={source}&limit={limit}
```

**參數：**
- `source` (optional): 新聞來源 key
  - `liberty_times` - 自由時報
  - `taipei_times` - Taipei Times
  - `taiwan_news` - Taiwan News
  - `focus_taiwan` - 中央社
- `limit` (optional): 數量限制，預設 10

**範例請求：**
```bash
curl "http://localhost:5000/api/news/latest?limit=5"
```

**範例回應：**
```json
{
  "status": "success",
  "count": 5,
  "news": [
    {
      "source": "自由時報",
      "source_key": "liberty_times",
      "title": "新聞標題",
      "link": "https://...",
      "published": "2025-11-17",
      "summary": "新聞摘要...",
      "category": "綜合"
    }
  ]
}
```

#### 2. 搜尋新聞

```http
GET /api/news/search?keyword={keyword}&source={source}&limit={limit}
```

**參數：**
- `keyword` (required): 搜尋關鍵字
- `source` (optional): 指定新聞來源
- `limit` (optional): 結果數量，預設 20

**範例請求：**
```bash
curl "http://localhost:5000/api/news/search?keyword=AI&limit=10"
```

**範例回應：**
```json
{
  "status": "success",
  "keyword": "AI",
  "count": 10,
  "news": [...]
}
```

#### 3. AI 分析新聞

```http
POST /api/news/analyze
Content-Type: application/json
```

**請求 Body：**
```json
{
  "url": "https://example.com/news-article",
  "analysis_type": "summary"
}
```

或直接提供文本：
```json
{
  "text": "新聞內容文字...",
  "analysis_type": "5w1h"
}
```

**analysis_type 選項：**
- `summary` - 摘要生成
- `5w1h` - 5W1H 提取
- `sentiment` - 情緒分析
- `keywords` - 關鍵字提取

**範例請求：**
```bash
curl -X POST http://localhost:5000/api/news/analyze \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com/article",
    "analysis_type": "summary"
  }'
```

**範例回應：**
```json
{
  "status": "success",
  "analysis_type": "summary",
  "result": "這則新聞報導了..."
}
```

#### 4. 比較媒體報導

```http
GET /api/news/compare?keyword={keyword}&limit={limit}
```

**參數：**
- `keyword` (required): 比較主題關鍵字
- `limit` (optional): 每個來源的新聞數量

**範例請求：**
```bash
curl "http://localhost:5000/api/news/compare?keyword=選舉"
```

**範例回應：**
```json
{
  "status": "success",
  "data": {
    "keyword": "選舉",
    "total_news": 15,
    "by_source": {
      "自由時報": 5,
      "Taipei Times": 4,
      "Taiwan News": 3,
      "中央社": 3
    },
    "news_list": [...],
    "analysis": "各媒體報導分析..."
  }
}
```

#### 5. 列出新聞來源

```http
GET /api/sources
```

**範例回應：**
```json
{
  "status": "success",
  "sources": {
    "liberty_times": {
      "name": "自由時報",
      "rss": "https://news.ltn.com.tw/rss/all.xml",
      "category": "綜合"
    },
    ...
  }
}
```

## 💻 使用範例

### Python 客戶端

```python
import requests
import json

BASE_URL = "http://localhost:5000"

# 1. 搜尋新聞
def search_news(keyword):
    response = requests.get(
        f"{BASE_URL}/api/news/search",
        params={"keyword": keyword, "limit": 10}
    )
    return response.json()

# 2. 分析新聞
def analyze_news(url):
    response = requests.post(
        f"{BASE_URL}/api/news/analyze",
        json={
            "url": url,
            "analysis_type": "5w1h"
        }
    )
    return response.json()

# 3. 比較媒體報導
def compare_media(keyword):
    response = requests.get(
        f"{BASE_URL}/api/news/compare",
        params={"keyword": keyword}
    )
    return response.json()

# 使用範例
if __name__ == "__main__":
    # 搜尋 AI 相關新聞
    news = search_news("人工智慧")
    print(f"找到 {news['count']} 則新聞")
    
    # 分析第一則新聞
    if news['news']:
        first_news = news['news'][0]
        analysis = analyze_news(first_news['link'])
        print(json.dumps(analysis, indent=2, ensure_ascii=False))
    
    # 比較不同媒體報導
    comparison = compare_media("台灣")
    print(f"各媒體報導數量: {comparison['data']['by_source']}")
```

### JavaScript (Node.js)

```javascript
const axios = require('axios');

const BASE_URL = 'http://localhost:5000';

// 搜尋新聞
async function searchNews(keyword) {
  try {
    const response = await axios.get(`${BASE_URL}/api/news/search`, {
      params: { keyword, limit: 10 }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.message);
  }
}

// 分析新聞
async function analyzeNews(url) {
  try {
    const response = await axios.post(`${BASE_URL}/api/news/analyze`, {
      url: url,
      analysis_type: 'summary'
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.message);
  }
}

// 使用範例
(async () => {
  const news = await searchNews('科技');
  console.log(`找到 ${news.count} 則新聞`);
  
  if (news.news.length > 0) {
    const analysis = await analyzeNews(news.news[0].link);
    console.log('分析結果:', analysis.result);
  }
})();
```

## 🚀 進階功能

### 自訂新聞來源

在 `news_service.py` 中添加新的新聞來源：

```python
NEWS_SOURCES = {
    # ... 現有來源
    "your_source": {
        "name": "您的新聞來源",
        "rss": "https://your-news-site.com/rss",
        "category": "分類"
    }
}
```

### 批次分析

```python
def batch_analyze_news(news_list, analysis_type="summary"):
    """批次分析多則新聞"""
    results = []
    for news in news_list:
        try:
            result = analyze_news_with_ai(
                fetch_news_content(news['link']),
                analysis_type
            )
            results.append({
                "title": news['title'],
                "analysis": result
            })
        except Exception as e:
            print(f"Error: {e}")
            continue
    return results
```

## 🔧 配置選項

### 環境變數

```bash
# OpenAI API 設定
export OPENAI_API_KEY="your-api-key"

# 服務設定
export FLASK_ENV=production
export FLASK_DEBUG=False
export PORT=5000
export HOST=0.0.0.0

# OpenAI 模型設定
export DEFAULT_MODEL=gpt-3.5-turbo
export MAX_TOKENS=2000
```

### 新聞抓取設定

```python
# 調整每個來源的新聞數量
DEFAULT_LIMIT = 10

# 設定請求超時時間
REQUEST_TIMEOUT = 10  # 秒

# 設定內容最大長度
MAX_CONTENT_LENGTH = 3000  # 字元
```

## 🧪 測試

### 執行測試腳本

```bash
# 安裝測試依賴
pip install pytest requests

# 執行測試
pytest test_news_service.py -v
```

### 手動測試

```bash
# 健康檢查
curl http://localhost:5000/api/sources

# 測試搜尋
curl "http://localhost:5000/api/news/search?keyword=台灣"

# 測試分析
curl -X POST http://localhost:5000/api/news/analyze \
  -H "Content-Type: application/json" \
  -d '{"text": "測試新聞內容", "analysis_type": "summary"}'
```

## 📊 效能考量

### 快取策略

建議實施快取機制以提升效能：

```python
from functools import lru_cache
from datetime import datetime, timedelta

@lru_cache(maxsize=100)
def fetch_rss_news_cached(source_key, limit):
    # 快取 5 分鐘
    return fetch_rss_news(source_key, limit)
```

### 速率限制

對於生產環境，建議添加速率限制：

```python
from flask_limiter import Limiter

limiter = Limiter(
    app=app,
    key_func=lambda: request.remote_addr,
    default_limits=["100 per hour"]
)

@app.route('/api/news/search')
@limiter.limit("30 per minute")
def api_search_news():
    # ...
```

## 🔒 安全建議

1. **API Key 保護**
   - 使用環境變數儲存
   - 定期輪換
   - 監控使用量

2. **輸入驗證**
   - 驗證關鍵字長度
   - 過濾特殊字元
   - 限制請求頻率

3. **內容安全**
   - 清理 HTML 標籤
   - 防止 XSS 攻擊
   - 驗證 URL 格式

## 🐳 Docker 部署

```dockerfile
FROM python:3.10-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY news_service.py .
EXPOSE 5000

CMD ["python", "news_service.py"]
```

```bash
# 建立映像
docker build -t news-ai-service .

# 執行容器
docker run -d -p 5000:5000 \
  -e OPENAI_API_KEY=your-key \
  news-ai-service
```

## 📈 未來發展

- [ ] 支援更多新聞來源
- [ ] 新增圖片識別與分析
- [ ] 實施全文索引搜尋
- [ ] 提供 WebSocket 即時推送
- [ ] 開發前端視覺化介面
- [ ] 添加使用者認證系統
- [ ] 整合資料庫儲存歷史記錄
- [ ] 支援多語言翻譯

## 🤝 貢獻

歡迎提交 Pull Request 或開 Issue！

## 📄 授權

MIT License

## 📧 聯絡方式

- Email: your-email@example.com
- GitHub: [@yourusername](https://github.com/yourusername)

---

**Made with ❤️ for Taiwan News Community**