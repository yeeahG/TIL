## data crawling

### get요청  
request payload 요청
```
import requests
url= 'https://'
payload = {}
r = requests.post(url, data=payload)
data = r.json()['list']
```

payload?  
사용에 있어서 전송되는 데이터를 뜻한다. 페이로드는 전송의 근본적인 목적이 되는 데이터의 일부분으로 그 데이터와 함께 전송되는 헤더와 메타데이터와 같은 데이터는 제외한다. 많은 데이터들 중 흥미있는 데이터만 구별하는 것  
출처 : https://ko.wikipedia.org/wiki/%ED%8E%98%EC%9D%B4%EB%A1%9C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85)

custom header
```
import requests
from bs4 import BeautifulSoup
head = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/100.0.4896.60 Safari/537.36"}
r = requests.get("https://www.melon.com/song/detail.htm?songId=34845949",headers=head)
bs = BeautifulSoup(r.text)
```

head사용?  
프로그래밍 언어로 사용할 수 없는 사이트의 경우 head를 지정하여 crawling을 할 수 있는 환경을 만들어준다
