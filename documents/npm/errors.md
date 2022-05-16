- run npm Error
```
node:internal/modules/cjs/loader:936
throw err;
^
```
🔑
```
npm cache clean --force
delete node_modules folder
delete package-lock.json file
npm install
```
하니까 됐다

- Node Error  
```
[Node] Cannot find module / 'MODULE_NOT_FOUND'
```
nodemon을 설치했다 
``` npm install nodemon --save-dev```  
그리고 package.json에서 경로설정을 이렇게 해줬다  
```
  "scripts": {
    "dev": "nodemon src/server.js"
  },
```
그러고나서 실행은
```npm run dev```
  
(nodemon은 저장만 하면 바로바로 실행이 됨 완전편해)
  
- nodemon ERR_CONNECTION_REFUSED  
에러..산넘어 산이라고..하나 해결하니까 port가 먹지 않았다..휴..  
```server.listen(3000, handleListen); ``` 로 설정해주니까 화면이 떴다..
