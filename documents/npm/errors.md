- run npm Error
```
node:internal/modules/cjs/loader:936
throw err;
^
```
๐
```
npm cache clean --force
delete node_modules folder
delete package-lock.json file
npm install
```
ํ๋๊น ๋๋ค

- Node Error  
```
[Node] Cannot find module / 'MODULE_NOT_FOUND'
```
nodemon์ ์ค์นํ๋ค 
``` npm install nodemon --save-dev```  
๊ทธ๋ฆฌ๊ณ  package.json์์ ๊ฒฝ๋ก์ค์ ์ ์ด๋ ๊ฒ ํด์คฌ๋ค  
```
  "scripts": {
    "dev": "nodemon src/server.js"
  },
```
๊ทธ๋ฌ๊ณ ๋์ ์คํ์
```npm run dev```
  
(nodemon์ ์ ์ฅ๋ง ํ๋ฉด ๋ฐ๋ก๋ฐ๋ก ์คํ์ด ๋จ ์์ ํธํด)
  
- nodemon ERR_CONNECTION_REFUSED  
์๋ฌ..์ฐ๋์ด ์ฐ์ด๋ผ๊ณ ..ํ๋ ํด๊ฒฐํ๋๊น port๊ฐ ๋จน์ง ์์๋ค..ํด..  
```server.listen(3000, handleListen); ``` ๋ก ์ค์ ํด์ฃผ๋๊น ํ๋ฉด์ด ๋ด๋ค..
