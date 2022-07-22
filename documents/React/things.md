alert('message');
window.location = '/some/url';

### 🍔Hamburger menu
```
import React,{useState} from 'react';
import './Header.css'
  
const Header = () => {

const [isOpen, setMenu] = useState(false);  // 메뉴의 초기값을 false로 설정

const toggleMenu = () => {
      setMenu(isOpen => !isOpen); // on,off 개념 boolean
  }

  return(
      <div className="header">
            <div className="menu-container">
                <p onClick={()=>toggleMenu()}>menu</p>
            </div>
            <div className={isOpen ? "show-menu" : "hide-menu"}>
                <p><a href='/'>Home</a></p>
                <p><a href='/styles'>Style</a></p>
                <p><a href='/reserve'>Reserve</a></p>
            </div>
      </div>
  )
}

export default Header
```

### onClick event 여러개
```
1. 인라인으로
onClick={() => {
  a();
  b();

});

2. 함수 만들기
const clickMe = () => {
  a();
  b();
};

<Button onClick={clickMe}>
```
되도록 함수로 만들기

### Select tag의 value값 저장하기
```
const designers =[
  {key:1, value:"1번 디자이너"},
  {key:2, value:"2번 디자이너"},
  {key:3, value:"3번 디자이너"},
]

  const [Content, setContent] = useState();

  const designerChoiceHandler=(e)=>{
    setContent(e.currentTarget.value)
  }
  console.log(Content);
  
  return (
   <select onChange={designerChoiceHandler} value={Content}>
      {designers.map((item)=>(
    <option key={item.key} value={item.value}>{item.value}</option>
    ))}
   </select>
```

### image 업로드 하는 input tag
 ```<input type="file" accept="image/*"/>```
 
### styled-component 적용하기
```npm install styled-components``` 설치해줘야함
