### React 18 version
```Warning: ReactDOM.render is no longer supported in React 18. Use createRoot instead.```

index.js 에서
```
import ReactDOM from 'react-dom.client'  
  
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />)
```
이렇게 바꿔줘야 함
