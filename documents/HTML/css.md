#### 중앙정렬
   ```
.container {  
    display: flex;  
    justify-content: center;  
}

.todo-text {
    width: 100%;
    margin: 0 auto;
}
```

#### 테두리 살짝 둥글게
    border-radius: 10px;

#### Gradation
```
background: linear-gradient(40deg, #212e6e, #f3df88);
```

#### a tag
- a tag 클릭 시 새창으로 열기
```
target="_blank"
```

#### Scroll 안보이게 하기
```
body {
    -ms-overflow-style:none; 
}

body::-webkit-scrollbar { 
    display:none; 
}
```

#### 요소 겹치기
```
.up{
position: relative;
}
.down {
position: absolute;
}
```

#### 드래그 스타일 변경
```
::selection {
  background: #f7f6f2;
  color: #000;
}
```

#### 원만들기
```
border-radius: 100vw;
    width: 100%;
    max-width: 20rem;
    overflow: hidden;
    background-color: #fe81b9;
}
```
