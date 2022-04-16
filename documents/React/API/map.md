## How to use Map API

## 🗺️Kakao Map

### Setting
- .env file  
REACT_APP_KAKAOMAP_API_KEY=발급받은key입력
- index.html  
src="//dapi.kakao.com/v2/maps/sdk.js?appkey=%REACT_APP_KAKAOMAP_API_KEY%"


### Code
```
import React, { useEffect } from 'react';
import './Map.css'

const Map= ({searchPlace}) => {
  const { kakao } = window
  /*global kakao*/
   
  // 마커를 담을 배열
  var markers = [];
  var ps;
  var infowindow;
  var map;

  useEffect(() => {
    keywordlistMap();
  }, []);
    
  const keywordlistMap = () => { // 지도를 표시할 div 
    var mapContainer = document.getElementById('map'),
    mapOption = {
      center: new kakao.maps.LatLng(33.37245636567239, 126.52833144852052), // 지도의 중심좌표
      level: 9 // 지도의 확대 레벨
    };  
    
    // 지도를 생성합니다
    map = new kakao.maps.Map(mapContainer, mapOption); 
    
    
    // 장소 검색 객체를 생성
    ps = new kakao.maps.services.Places();

    // 검색 결과 목록이나 마커를 클릭했을 때 장소명을 표출할 인포윈도우를 생성
    infowindow = new kakao.maps.InfoWindow({zIndex:1});
    // 키워드로 장소를 검색
    searchPlaces();
  }
  
  // 키워드 검색을 요청하는 함수
  const searchPlaces = () => {
    var keyword = document.getElementById('keyword').value;
    
    if (!keyword.replace(/^\s+|\s+$/g, '')) {
      alert('키워드를 입력해주세요!');
        return false;
    }
        
    // 장소검색 객체를 통해 키워드로 장소검색을 요청
    ps.keywordSearch( keyword, placesSearchCB); 
  }
  
  // 장소검색이 완료됐을 때 호출되는 콜백함수
  const placesSearchCB = (data, status, pagination) => {
    if (status === kakao.maps.services.Status.OK) {
    // 정상적으로 검색이 완료됐으면 검색 목록과 마커를 표출
    displayPlaces(data);
    // 페이지 번호를 표출
    displayPagination(pagination);

    } else if (status === kakao.maps.services.Status.ZERO_RESULT) {
      alert('검색 결과가 존재하지 않습니다.');
      return;

    } else if (status === kakao.maps.services.Status.ERROR) {
      alert('검색 결과 중 오류가 발생했습니다.');
      return;
    }
  }
  
  // 검색 결과 목록과 마커를 표출하는 함수
  const displayPlaces = (places) => {
    var listEl = document.getElementById('placesList'), 
    menuEl = document.getElementById('menu_wrap'),
    fragment = document.createDocumentFragment(), 
    bounds = new kakao.maps.LatLngBounds(), 
    listStr = '';
    
    // 검색 결과 목록에 추가된 항목들을 제거
    removeAllChildNods(listEl);
    // 지도에 표시되고 있는 마커를 제거
    removeMarker();
            
    for ( var i=0; i<places.length; i++ ) {
    // 마커를 생성하고 지도에 표시
      var placePosition = new kakao.maps.LatLng(places[i].y, places[i].x);
      var marker = addMarker(placePosition, i);
      var itemEl = getListItem(i, places[i]); // 검색 결과 항목 Element를 생성

      // 검색된 장소 위치를 기준으로 지도 범위를 재설정하기위해 LatLngBounds 객체에 좌표를 추가
      bounds.extend(placePosition);
    
    
        // 마커와 검색결과 항목에 mouseover 했을때 해당 장소에 인포윈도우에 장소명을 표시하고 mouseout 했을 때는 인포윈도우를 닫음
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
      
      // 검색결과 항목들을 검색결과 목록 Element에 추가
      listEl.appendChild(fragment);
      menuEl.scrollTop = 0;
     // 검색된 장소 위치를 기준으로 지도 범위를 재설정
      map.setBounds(bounds);
    }
  
  // 검색결과 항목을 Element로 반환하는 함수
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
  
    // 마커를 생성하고 지도 위에 마커를 표시하는 함수
    const addMarker = (position, idx, title) => {
    // 마커 이미지 url, 스프라이트 이미지를 씀
      var imageSrc = 'https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/marker_number_blue.png',
      imageSize = new kakao.maps.Size(36, 37),
      imgOptions =  {
        spriteSize : new kakao.maps.Size(36, 691), 
        spriteOrigin : new kakao.maps.Point(0, (idx*46)+10), 
        offset: new kakao.maps.Point(13, 37) 
      },
      markerImage = new kakao.maps.MarkerImage(imageSrc, imageSize, imgOptions),
      marker = new kakao.maps.Marker({
        position: position, // 마커의 위치
        image: markerImage 
      });
  
      marker.setMap(map); // 지도 위에 마커를 표출
      markers.push(marker); // 배열에 생성된 마커를 추가
  
      return marker;
    }
  
// 지도 위에 표시되고 있는 마커를 모두 제거
    const removeMarker = () => {
      for ( var i = 0; i < markers.length; i++ ) {
        markers[i].setMap(null);
      }   
      markers = [];
    }
  
    //// 검색결과 목록 하단에 페이지번호를 표시는 함수
    const displayPagination = (pagination) => {
      var paginationEl = document.getElementById('pagination'),
      fragment = document.createDocumentFragment(),
      i; 
  
      // 기존에 추가된 페이지번호를 삭제
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
    
  // 검색결과 목록 또는 마커를 클릭했을 때 호출되는 함수
  // 인포윈도우에 장소명을 표시
    const displayInfowindow = (marker, title) => {
      var content = '<div style="padding:5px;z-index:1;">' + title + '</div>';
        infowindow.setContent(content);
        infowindow.open(map, marker);
    }
  
   //검색결과 목록의 자식 Element를 제거하는 함수
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
              키워드 : <input type="text" defaultValue="서귀포 호텔" id="keyword" size="15" /> 
              <button onClick={searchPlaces}>검색하기</button> 
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
  
  출처 : https://apis.map.kakao.com/web/sample/keywordList/
  <br>
  
## 🗺️Google Map
### Error
npm install부터 에러가 많았다  
아마 version이 맞지않는 것 같았다  
<br>
🔍Solution
Install @react-google-maps/api with NPM

출처 : https://github.com/JustFly1984/react-google-maps-api/tree/master/packages/react-google-maps-api
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

- .env 파일에서는
```
REACT_APP_GOOGLEMAP_API_KEY=발급받은 Google map API key
```

- .gitignore에서는
```
#API KEY
.env
```
  
