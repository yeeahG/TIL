## How to use Map API

## ๐บ๏ธKakao Map

### Setting
- .env๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด dotenv ์ค์น
```npm install dotenv```
- .env file  
REACT_APP_KAKAOMAP_API_KEY=๋ฐ๊ธ๋ฐ์key์๋ ฅ
- index.html  
src="//dapi.kakao.com/v2/maps/sdk.js?appkey=%REACT_APP_KAKAOMAP_API_KEY%"


### Code
```
import React, { useEffect } from 'react';
import './Map.css'

const Map= ({searchPlace}) => {
  const { kakao } = window
  /*global kakao*/
   
  // ๋ง์ปค๋ฅผ ๋ด์ ๋ฐฐ์ด
  var markers = [];
  var ps;
  var infowindow;
  var map;

  useEffect(() => {
    keywordlistMap();
  }, []);
    
  const keywordlistMap = () => { // ์ง๋๋ฅผ ํ์ํ  div 
    var mapContainer = document.getElementById('map'),
    mapOption = {
      center: new kakao.maps.LatLng(33.37245636567239, 126.52833144852052), // ์ง๋์ ์ค์ฌ์ขํ
      level: 9 // ์ง๋์ ํ๋ ๋ ๋ฒจ
    };  
    
    // ์ง๋๋ฅผ ์์ฑํฉ๋๋ค
    map = new kakao.maps.Map(mapContainer, mapOption); 
    
    
    // ์ฅ์ ๊ฒ์ ๊ฐ์ฒด๋ฅผ ์์ฑ
    ps = new kakao.maps.services.Places();

    // ๊ฒ์ ๊ฒฐ๊ณผ ๋ชฉ๋ก์ด๋ ๋ง์ปค๋ฅผ ํด๋ฆญํ์ ๋ ์ฅ์๋ช์ ํ์ถํ  ์ธํฌ์๋์ฐ๋ฅผ ์์ฑ
    infowindow = new kakao.maps.InfoWindow({zIndex:1});
    // ํค์๋๋ก ์ฅ์๋ฅผ ๊ฒ์
    searchPlaces();
  }
  
  // ํค์๋ ๊ฒ์์ ์์ฒญํ๋ ํจ์
  const searchPlaces = () => {
    var keyword = document.getElementById('keyword').value;
    
    if (!keyword.replace(/^\s+|\s+$/g, '')) {
      alert('ํค์๋๋ฅผ ์๋ ฅํด์ฃผ์ธ์!');
        return false;
    }
        
    // ์ฅ์๊ฒ์ ๊ฐ์ฒด๋ฅผ ํตํด ํค์๋๋ก ์ฅ์๊ฒ์์ ์์ฒญ
    ps.keywordSearch( keyword, placesSearchCB); 
  }
  
  // ์ฅ์๊ฒ์์ด ์๋ฃ๋์ ๋ ํธ์ถ๋๋ ์ฝ๋ฐฑํจ์
  const placesSearchCB = (data, status, pagination) => {
    if (status === kakao.maps.services.Status.OK) {
    // ์ ์์ ์ผ๋ก ๊ฒ์์ด ์๋ฃ๋์ผ๋ฉด ๊ฒ์ ๋ชฉ๋ก๊ณผ ๋ง์ปค๋ฅผ ํ์ถ
    displayPlaces(data);
    // ํ์ด์ง ๋ฒํธ๋ฅผ ํ์ถ
    displayPagination(pagination);

    } else if (status === kakao.maps.services.Status.ZERO_RESULT) {
      alert('๊ฒ์ ๊ฒฐ๊ณผ๊ฐ ์กด์ฌํ์ง ์์ต๋๋ค.');
      return;

    } else if (status === kakao.maps.services.Status.ERROR) {
      alert('๊ฒ์ ๊ฒฐ๊ณผ ์ค ์ค๋ฅ๊ฐ ๋ฐ์ํ์ต๋๋ค.');
      return;
    }
  }
  
  // ๊ฒ์ ๊ฒฐ๊ณผ ๋ชฉ๋ก๊ณผ ๋ง์ปค๋ฅผ ํ์ถํ๋ ํจ์
  const displayPlaces = (places) => {
    var listEl = document.getElementById('placesList'), 
    menuEl = document.getElementById('menu_wrap'),
    fragment = document.createDocumentFragment(), 
    bounds = new kakao.maps.LatLngBounds(), 
    listStr = '';
    
    // ๊ฒ์ ๊ฒฐ๊ณผ ๋ชฉ๋ก์ ์ถ๊ฐ๋ ํญ๋ชฉ๋ค์ ์ ๊ฑฐ
    removeAllChildNods(listEl);
    // ์ง๋์ ํ์๋๊ณ  ์๋ ๋ง์ปค๋ฅผ ์ ๊ฑฐ
    removeMarker();
            
    for ( var i=0; i<places.length; i++ ) {
    // ๋ง์ปค๋ฅผ ์์ฑํ๊ณ  ์ง๋์ ํ์
      var placePosition = new kakao.maps.LatLng(places[i].y, places[i].x);
      var marker = addMarker(placePosition, i);
      var itemEl = getListItem(i, places[i]); // ๊ฒ์ ๊ฒฐ๊ณผ ํญ๋ชฉ Element๋ฅผ ์์ฑ

      // ๊ฒ์๋ ์ฅ์ ์์น๋ฅผ ๊ธฐ์ค์ผ๋ก ์ง๋ ๋ฒ์๋ฅผ ์ฌ์ค์ ํ๊ธฐ์ํด LatLngBounds ๊ฐ์ฒด์ ์ขํ๋ฅผ ์ถ๊ฐ
      bounds.extend(placePosition);
    
    
        // ๋ง์ปค์ ๊ฒ์๊ฒฐ๊ณผ ํญ๋ชฉ์ mouseover ํ์๋ ํด๋น ์ฅ์์ ์ธํฌ์๋์ฐ์ ์ฅ์๋ช์ ํ์ํ๊ณ  mouseout ํ์ ๋๋ ์ธํฌ์๋์ฐ๋ฅผ ๋ซ์
      (function(marker, title) {
        kakao.maps.event.addListener(marker, 'mouseover', function() {
          displayInfowindow(marker, title);
        });
    
          kakao.maps.event.addListener(marker, 'mouseout', function() {
            infowindow.close();
          });
  
          itemEl.onmouseover =  function () {
            displayInfowindow(marker, title);
          };
    
          itemEl.onmouseout =  function () {
            infowindow.close();
          };
         
        })(marker, places[i].place_name);
        fragment.appendChild(itemEl);
                
      }
      
      // ๊ฒ์๊ฒฐ๊ณผ ํญ๋ชฉ๋ค์ ๊ฒ์๊ฒฐ๊ณผ ๋ชฉ๋ก Element์ ์ถ๊ฐ
      listEl.appendChild(fragment);
      menuEl.scrollTop = 0;
     // ๊ฒ์๋ ์ฅ์ ์์น๋ฅผ ๊ธฐ์ค์ผ๋ก ์ง๋ ๋ฒ์๋ฅผ ์ฌ์ค์ 
      map.setBounds(bounds);
    }
  
  // ๊ฒ์๊ฒฐ๊ณผ ํญ๋ชฉ์ Element๋ก ๋ฐํํ๋ ํจ์
    const getListItem = (index, places) => {
      var el = document.createElement('li'),
      itemStr = '<span class="markerbg marker_' + (index+1) + '"></span>' 
                + '   <h5>' + places.place_name + '</h5>';
  
      if (places.road_address_name) {
        itemStr += '    <span>' + places.road_address_name + '</span>' 
      } 
        
      itemStr += '  <span class="tel">' + places.phone  + '</span>' + '</div>';           
  
      el.innerHTML = itemStr;
      el.className = 'item';
  
      return el;
    }
  
    // ๋ง์ปค๋ฅผ ์์ฑํ๊ณ  ์ง๋ ์์ ๋ง์ปค๋ฅผ ํ์ํ๋ ํจ์
    const addMarker = (position, idx, title) => {
    // ๋ง์ปค ์ด๋ฏธ์ง url, ์คํ๋ผ์ดํธ ์ด๋ฏธ์ง๋ฅผ ์
      var imageSrc = 'https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/marker_number_blue.png',
      imageSize = new kakao.maps.Size(36, 37),
      imgOptions =  {
        spriteSize : new kakao.maps.Size(36, 691), 
        spriteOrigin : new kakao.maps.Point(0, (idx*46)+10), 
        offset: new kakao.maps.Point(13, 37) 
      },
      markerImage = new kakao.maps.MarkerImage(imageSrc, imageSize, imgOptions),
      marker = new kakao.maps.Marker({
        position: position, // ๋ง์ปค์ ์์น
        image: markerImage 
      });
  
      marker.setMap(map); // ์ง๋ ์์ ๋ง์ปค๋ฅผ ํ์ถ
      markers.push(marker); // ๋ฐฐ์ด์ ์์ฑ๋ ๋ง์ปค๋ฅผ ์ถ๊ฐ
  
      return marker;
    }
  
// ์ง๋ ์์ ํ์๋๊ณ  ์๋ ๋ง์ปค๋ฅผ ๋ชจ๋ ์ ๊ฑฐ
    const removeMarker = () => {
      for ( var i = 0; i < markers.length; i++ ) {
        markers[i].setMap(null);
      }   
      markers = [];
    }
  
    //// ๊ฒ์๊ฒฐ๊ณผ ๋ชฉ๋ก ํ๋จ์ ํ์ด์ง๋ฒํธ๋ฅผ ํ์๋ ํจ์
    const displayPagination = (pagination) => {
      var paginationEl = document.getElementById('pagination'),
      fragment = document.createDocumentFragment(),
      i; 
  
      // ๊ธฐ์กด์ ์ถ๊ฐ๋ ํ์ด์ง๋ฒํธ๋ฅผ ์ญ์ 
      while (paginationEl.hasChildNodes()) {
        paginationEl.removeChild (paginationEl.lastChild);
      }
  
      for (i=1; i<=pagination.last; i++) {
        var el = document.createElement('a');
        //el.href = "#";
        el.innerHTML = i;
  
        if (i===pagination.current) {
        el.className = 'on';
        } else {
        el.onclick = (function(i) {
          return function() {
            pagination.gotoPage(i);
          }
        })(i);
      }
  
      fragment.appendChild(el);
      }
    paginationEl.appendChild(fragment);
    }
    
  // ๊ฒ์๊ฒฐ๊ณผ ๋ชฉ๋ก ๋๋ ๋ง์ปค๋ฅผ ํด๋ฆญํ์ ๋ ํธ์ถ๋๋ ํจ์
  // ์ธํฌ์๋์ฐ์ ์ฅ์๋ช์ ํ์
    const displayInfowindow = (marker, title) => {
      var content = '<div style="padding:5px;z-index:1;">' + title + '</div>';
        infowindow.setContent(content);
        infowindow.open(map, marker);
    }
  
   //๊ฒ์๊ฒฐ๊ณผ ๋ชฉ๋ก์ ์์ Element๋ฅผ ์ ๊ฑฐํ๋ ํจ์
    const removeAllChildNods = (el) => {   
      while (el.hasChildNodes()) {
        el.removeChild (el.lastChild);
      }
    }
  
    return (
      <div className='mapContainer'>
        <div id="map" style={{ width: "100%", height: "500px" }}></div>
      
        <div id="menu_wrap" className="bg_white">
          <div className="option">
            <div>
              ํค์๋ : <input type="text" defaultValue="์๊ทํฌ ํธํ" id="keyword" size="15" /> 
              <button onClick={searchPlaces}>๊ฒ์ํ๊ธฐ</button> 
            </div>
          </div>
          <ul id="placesList"></ul>
          <div id="pagination"></div>
        </div>
      </div>
    );
  
  }
  
  export default Map;
  ```
  
  ์ถ์ฒ : https://apis.map.kakao.com/web/sample/keywordList/
  <br>
  
  - json data๋ฅผ ์ง๋์ ํ์ํ๊ธฐ
  ```
  /*global kakao */ 
import React, { useEffect } from 'react'
import { placeData } from './placeData'
import './Map.css'

const Map = () => {

  useEffect(() => {
    const container = document.getElementById("map");

    const options = {
      center: new window.kakao.maps.LatLng(35.85133, 127.734086),
      level: 11,
    };

    const map = new window.kakao.maps.Map(container, options);

    console.log("loading kakaomap");

    // ์ฃผ์-์ขํ ๋ณํ ๊ฐ์ฒด๋ฅผ ์์ฑํฉ๋๋ค
  var geocoder = new kakao.maps.services.Geocoder();

  //foreach loop
  placeData.forEach((el) => {
    // ์ฃผ์๋ก ์ขํ๋ฅผ ๊ฒ์ํฉ๋๋ค
    geocoder.addressSearch(el.addr, function(result, status) {
    
      // ์ ์์ ์ผ๋ก ๊ฒ์์ด ์๋ฃ๋์ผ๋ฉด 
      if (status === kakao.maps.services.Status.OK) {
    
        var coords = new kakao.maps.LatLng(result[0].y, result[0].x);
    
        // ๊ฒฐ๊ณผ๊ฐ์ผ๋ก ๋ฐ์ ์์น๋ฅผ ๋ง์ปค๋ก ํ์ํฉ๋๋ค
        var marker = new kakao.maps.Marker({
          map: map,
          position: coords
        });
    
        // ์ธํฌ์๋์ฐ๋ก ์ฅ์์ ๋ํ ์ค๋ช์ ํ์ํฉ๋๋ค
        var infowindow = new kakao.maps.InfoWindow({
          content: '<div style="width:150px;text-align:center;padding:6px 0;">' +el.title+'</div>'
        });
        infowindow.open(map, marker);
    
        // ์ง๋์ ์ค์ฌ์ ๊ฒฐ๊ณผ๊ฐ์ผ๋ก ๋ฐ์ ์์น๋ก ์ด๋์ํต๋๋ค
        map.setCenter(coords);
        }
    })
      
  });




  }, []);

  return (
    <div className='map__container'>
        <div id="map" style={{height:"50vh"}}></div>
    </div>
  )
}

export default Map
```
  
## ๐บ๏ธGoogle Map
### Error
npm install๋ถํฐ ์๋ฌ๊ฐ ๋ง์๋ค  
์๋ง version์ด ๋ง์ง์๋ ๊ฒ ๊ฐ์๋ค  
<br>
๐Solution
Install @react-google-maps/api with NPM

์ถ์ฒ : https://github.com/JustFly1984/react-google-maps-api/tree/master/packages/react-google-maps-api
```
npm i -S @react-google-maps/api
```
```
import React from 'react'
import { GoogleMap, useJsApiLoader } from '@react-google-maps/api';

const containerStyle = {
  width: '400px',
  height: '400px'
};

const center = {
  lat: -3.745,
  lng: -38.523
};

const Map = () => {
  const { isLoaded } = useJsApiLoader({
    id: 'google-map-script',
    googleMapsApiKey: process.env.REACT_APP_GOOGLEMAP_API_KEY
  })

  const [map, setMap] = React.useState(null)

  const onLoad = React.useCallback(function callback(map) {
    const bounds = new window.google.maps.LatLngBounds();
    map.fitBounds(bounds);
    setMap(map)
  }, [])

  const onUnmount = React.useCallback(function callback(map) {
    setMap(null)
  }, [])

  return isLoaded ? (
      <GoogleMap
        mapContainerStyle={containerStyle}
        center={center}
        zoom={10}
        onLoad={onLoad}
        onUnmount={onUnmount}
      >
        { /* Child components, such as markers, info windows, etc. */ }
        <></>
      </GoogleMap>
  ) : <></>
}
export default Map
```

- .env ํ์ผ์์๋
```
REACT_APP_GOOGLEMAP_API_KEY=๋ฐ๊ธ๋ฐ์ Google map API key
```

- .gitignore์์๋
```
#API KEY
.env
```
  
