# 请求头检测
要求爬取100页的数字并计算总和

```python
import httpx

headers = {
  "accept": "application/json, text/javascript, */*; q=0.01",
  "accept-encoding": "gzip, deflate, br, zstd",
  "accept-language": "zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6",
  "cookie": "sessionid=bm6ssythck6aaw8qscxbwx324qhg3fyy",
  "priority": "u=1, i",
  "referer": "https://www.spiderdemo.cn/sec1/header_check/",
  "sec-ch-ua": "\"Microsoft Edge\";v=\"141\", \"Not?A_Brand\";v=\"8\", \"Chromium\";v=\"141\"",
  "sec-ch-ua-mobile": "?0",
  "sec-ch-ua-platform": "\"Windows\"",
  "sec-fetch-dest": "empty",
  "sec-fetch-mode": "cors",
  "sec-fetch-site": "same-origin",
  "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36 Edg/141.0.0.0",
  "x-requested-with": "XMLHttpRequest"
}

total_sum = 0
for i in range(1, 101):
    url = f"https://www.spiderdemo.cn/sec1/api/challenge/page/{i}/?challenge_type=header_check"
    response = httpx.get(url, headers=headers)
    total_sum += sum(response.json()["page_data"])
    print(f"Current sum is: {total_sum}")
print(f"total sum is: {total_sum}")
```