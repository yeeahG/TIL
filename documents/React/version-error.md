### React 18 version
```Warning: ReactDOM.render is no longer supported in React 18. Use createRoot instead.```

index.js 에서
```
import ReactDOM from 'react-dom.client'  
  
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />)
```
이렇게 바꿔줘야 함


### React router dom
import {useHistory} from 'react-router-dom'를 사용하려 했는데 에러가 발생했다
```'useHistory' is not exported from 'react-router-dom' (imported as 'useHistory').```  

- react-router-dom의 버전이 6이상이라서 버전이 업데이트 되면서 명령어가 변경된 것

🔍 해결
v6 미만인 경우에는 useHistory 그대로 사용, v6 이상인 경우는 useNavigate 로 사용
or
router dom을 다운그레이드 해줘야 한다. >npm i react-router-dom@5.0.0

or
import {useNavigate} from 'react-router-dom';
const navigate = useNavigate()

그런데 내가 6버전에서 변경된 명령어를 사용하고 있다는 사실을 모르고 있다가 다운그레이드 해버려서 에러가 많이 발생했다.  
