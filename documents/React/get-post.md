### Spring과 연동하기 위한 설정

## GET




```
import React, { useEffect, useState } from 'react';
import Accom from './Accom';

const ACCOM = [
    {
        id: "dum1",
        city: "jeju",
        accommodationName: "효리네민박", 
        address: "제주시 어쩌구 저쩌구",
        phoneNumber: "000-000-0000" ,
    }
];

const BASE_URL = 'http://Getmapping으로 설정된 url주소';

const List = (click,data) => {
    const [input, setInput] = useState();
    
    const passData =(input)=> {
        setInput(data.input);
    }

    const [lists, setLists] = useState(ACCOM);

    useEffect(() => {
        console.log('호출');
        const fetchAccoms = async () => {
            const response = await fetch(BASE_URL);
            
            console.log(response.ok);
            const responseData =  await response.json();
            
            const listsData = [];
            
            for (const key in responseData) {
                
                localStorage.setItem("업소명", responseData[key].accommodationName);
            if(localStorage.getItem("위치")===responseData[key].city){
                listsData.push({
                    id: key,
                    city: responseData[key].city,
                    accommodationName: responseData[key].accommodationName,
                    address: responseData[key].address,
                    phoneNumber: responseData[key].phoneNumber,
                });
            } else if(localStorage.getItem("위치")===null) {
                listsData.push({
                    id: key,
                    city: responseData[key].city,
                    accommodationName: responseData[key].accommodationName,
                    address: responseData[key].address,
                    phoneNumber: responseData[key].phoneNumber,
                });}
        }

        setLists(listsData);
        }

        fetchAccoms().catch(error => {
        console.log(error);
        })
    }, []);

    const accomsList = lists.map((list) => (
        <Accom click={click.click} data = {passData}
          key={list.id}
          id={list.id}
          city={list.city}
          accommodationName={list.accommodationName}
          address={list.address}
          phoneNumber={list.phoneNumber}
        />
      ));
      
  return (
    <div>
        {accomsList}
    </div>

  )
}

export default List
```

## POST
```
import React, { useState, useEffect} from 'react';
import List from './List';

const ACCOM = [
    {
        city: localStorage.getItem("위치"),
        checkIn: localStorage.getItem("체크인날짜"), 
        checkOut: localStorage.getItem("체크아웃날짜"),
        people: localStorage.getItem("총사람수") ,
        accommodationId : localStorage.getItem("업소명"),
    },
];

const data =() => {
}

const BASE_URL = 'http://PostMapping으로 설정된 url';
const Reserve = () => {
  const [lists, setLists] = useState(ACCOM);

  const reserveHandler = async () => {

    const data = {
      city: localStorage.getItem("위치"), 
      checkIn: localStorage.getItem("체크인날짜"),
      checkOut: localStorage.getItem("체크아웃날짜"),
      people: localStorage.getItem("총사람수"),
      accommodationName : localStorage.getItem("업소명"),
    };
  
    console.log(data);

    await fetch(BASE_URL, 
      {
        method: 'POST',
        headers: {
          'Content-Type' : 'application/json',
        },
        body: JSON.stringify(data),
      });
      setLists();
      
      localStorage.clear();
      localStorage.setItem("총사람수",0);
    }
    
    return (
      <div >
        <List click={reserveHandler} data = {data}/>
      </div>
  )
}

export default Reserve
```
