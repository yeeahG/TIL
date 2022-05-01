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
