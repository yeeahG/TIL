alert('message');
window.location = '/some/url';

### ๐Hamburger menu
```
import React,{useState} from 'react';
import './Header.css'
  
const Header = () => {

const [isOpen, setMenu] = useState(false);  // ๋ฉ๋ด์ ์ด๊ธฐ๊ฐ์ false๋ก ์ค์ 

const toggleMenu = () => {
      setMenu(isOpen => !isOpen); // on,off ๊ฐ๋ boolean
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

### onClick event ์ฌ๋ฌ๊ฐ
```
1. ์ธ๋ผ์ธ์ผ๋ก
onClick={() => {
  a();
  b();

});

2. ํจ์ ๋ง๋ค๊ธฐ
const clickMe = () => {
  a();
  b();
};

<Button onClick={clickMe}>
```
๋๋๋ก ํจ์๋ก ๋ง๋ค๊ธฐ

### Select tag์ value๊ฐ ์ ์ฅํ๊ธฐ
```
const designers =[
  {key:1, value:"1๋ฒ ๋์์ด๋"},
  {key:2, value:"2๋ฒ ๋์์ด๋"},
  {key:3, value:"3๋ฒ ๋์์ด๋"},
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

### image ์๋ก๋ ํ๋ input tag
 ```<input type="file" accept="image/*"/>```
 
### styled-component ์ ์ฉํ๊ธฐ
```npm install styled-components``` ์ค์นํด์ค์ผํจ
