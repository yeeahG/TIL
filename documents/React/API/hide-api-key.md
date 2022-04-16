# 🔐APPI KEY를 숨기기
### .js 파일
파일의 상단에
```
const API_KEY = process.env.REACT_APP_TRAVEL_ADVISOR_KEY;

const 함수 = {
    
    headers: {
      'X-RapidAPI-Host': 'travel-advisor.p.rapidapi.com',
      'X-RapidAPI-Key': API_KEY
    }
  };
```

- .env파일
```
REACT_APP_TRAVEL_ADVISOR_KEY=지정하려는 API KEY
```
