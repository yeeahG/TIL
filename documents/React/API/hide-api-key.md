# ๐APPI KEY๋ฅผ ์จ๊ธฐ๊ธฐ
### .js ํ์ผ
ํ์ผ์ ์๋จ์
```
const API_KEY = process.env.REACT_APP_TRAVEL_ADVISOR_KEY;

const ํจ์ = {
    
    headers: {
      'X-RapidAPI-Host': 'travel-advisor.p.rapidapi.com',
      'X-RapidAPI-Key': API_KEY
    }
  };
```

- .envํ์ผ
```
REACT_APP_TRAVEL_ADVISOR_KEY=์ง์ ํ๋ ค๋ API KEY
```
