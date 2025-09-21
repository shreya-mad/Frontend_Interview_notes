1.useMemo Hook?
ANS: It is nothing but one of the react hooks.here use meaning its a hook becuase use word is used in all the hooks and memo means Memoisation.
imagine there are two person.person a is teacher and person b is student so teacher asked student ,tell me the ans of 2*3 so student didnt know its ans so calculated its ans and decided to store it somewhere so that if someone will ask later then simply will ans from the stored part  and then replied thats ans to teacher,its ans would be 6.
and similarly for 4*7 and 5+102 as well so here student stores all the ans somewhere so that if it will be asked later then will reply by picking from that location so student dont need to perform operation again and again.
this process of storing value somewhere so that can be used later is called as memoisation.so relate it will coding with as well.
we commonly use it in DP.
so in simple word,useMemo is a kind of hook which is used to prevent unnecesary expensive operations.we can say expensive opention in terms of memory or time anything.
lets take a example of expensive operation:-

import React, { useState, useMemo } from "react";
function App() {
  const [count, setCount] = useState(0);


  function expensiveTask(num) {
    console.log("Inside Expensive Task");
    <!-- addded below line to make it expensive because it will take some time to interate in the loop as it will interate for 1000000000 times -->
    for (let i = 0; i <= 1000000000; i++) {} 
    return num * 2;
  }

  let doubleValue = useMemo(() => {
    return expensiveTask(4);
  }, []); 

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      <div>
        Count: {count}
      </div>

      <div>
        Double: {doubleValue}
      </div>
    </div>
  );
}

export default App;

when we on Increment button then it will increase counter one so after changing useState ,it will re-render whole component so while rerender it will take some time to run expensiveTask fucntion so if we increase counter multiple times then it will take much time as compare our expectation due to running expensiveTask fucntion on every re-render if counter increses so here we are passing 4 for every time so to make it fast we store(Memomoise) expensiveTask(4) value somewhere so that irt will not run on every re-render and our component become faster as comapre without memoisation. but if we chnage value passed inside expensiveTask fucntion then it will take that much time. below one code is for variable input value 


import React, { useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(0); 

  function expensiveTask(n) {
    console.log("Inside Expensive Task");
    for (let i = 0; i <= 1000000000; i++) {} 
    return n * 2;
  }
  const doubleValue = expensiveTask(num);
  return (
    <div style={{ padding: "20px" }}>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <div>Count: {count}</div>

      <div style={{ marginTop: "20px" }}>
        <input
          type="number"
          value={num}
          onChange={(e) => setNum(Number(e.target.value))}
          placeholder="Enter a number"
        />
      </div>

      <div>Double of {num}: {doubleValue}</div>
    </div>
  );
}

export default App;

now to make above task fatser ,we need to use useMemo so inside usememo, we need to pass two things one is function for which we want to apply useMemo and then varible by changing that we need to re-call that function so here first we need to import useMemo use useState
like below
import React, { useState, useMemo } from "react";
then simply call  expensiveTask inside useMemo bu below mentioned code

 const doubleValue = useMemo(()=>
       expensiveTask(num)
    ,[num]);

if we are thinking that useMemo store result of every varible then that is wrong ,it store response of only last task or variable,like if we have selected 3 then 4 then then 5 and we are expecting that usememo has allready stored value of 3 and 4 both then that is wrong ,i stotred only respnse of 4,not 3. so if we enter 3 after 5 then it recall that function so simple that if value of dependency dones change then it will not call that function and it get changed then will call that functionagain and store response of last one.



2.useCallback Hook?
ANS:-it is a kind of hook which memoize function reference so it doesn't get recreated every time  similar to memoization of the value in useMemo.
useCallback is useful when you pass a function down to a child component. Without useCallback, the function reference is recreated on every render, causing the child to re-render even if its props didn’t change.
when page re-renders then reference of any  function used in that component get change like if earlier reference was a then after first re-render it will will be b after third re-render it will be c and so on.
useCallback is used for two things:-
a.unnecessary re-rendering of child component
b.handling expensive operations

Expaliantion of a:-
PARENT COMPONENT CODE
import React, { useState } from "react";
import Child from "./Child";

function Parent() {
  const [count, setCount] = useState(0);
const handleClick=()=>{
  setCount(count+1);
}
  return (
    <div>
      <div>
        <p>Count: {count}</p>
        <button
          onClick={handleClick}
          className="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600"
        >
          Increment
        </button>
      </div>
      <Child buttonName="Click Me" />
    </div>
  );
}
export default Parent;

CHILD COMPONENT CODE

import React from "react";
function Child(props) {
  console.log("Child component called");
  return (
    <div>
      <h3>Child Component</h3>
      <button>
        {props.buttonName}
      </button>
    </div>
  );
}
export default Child;

so here if we increase our couter then child coponent get re-render every time and console "Child component called" on every re-render but it was unnesecerry re-rendering so for preventing this re-render we simply wrap-up child coponent in React.memo ...like below code

import React from "react";
const Child=React.memo((props) {
  console.log("Child component called");
  return (
    <div>
      <h3>Child Component</h3>
      <button>
        {props.buttonName}
      </button>
    </div>
  );
});
export default Child;

so by wrapping-up child copone tit will stop re-rendering the child coponent because by using React.memo child coponent will re-render only when props passed in child coponent get chnaged so here Click me text is not chnaging thats why child component will not re-render but we were studing useCallback then why we came to React.memo and then what is pupose of useCallback.
So if we pass any function in child coponent from parent component then we will observe that child copoment re-rendering hapeen again.
so this happen because as we know that if  child component is wrapped up in React.memo then it re-render when props change so what happen here is that if we increase couter then parent page re-renders and if page re-render then reference of the function get chnaged ,like here we have definend handleClick for increasing couter so on incressing counter value, function reference of handleClick stored in derent variable so this  function is passed as props so fucntion get chnaged every time so props is also get changed so due to pros chnage React.memo re-renders child compoent.
so to overcome this issue when we have pass any function as a props we basically freeze that function by wrapping-up in useCallback hook  and then pass dependency and it will not create new function reference for calling function on re-render,it simply store on same refence,like here count is dependency in parent compoent. like below code.if we dont pass depedency like count here then it will simply freeze the function because it will run only once and due to that it will take 0 count value and then increase that by 1 every time and will only 1 for every click for count increament.so simply we pass dependecy and if dependency varible changes then simply it will re-create function every time and child coponenet get re-renders so to avoid this we have to use setCount(pre=>pre+1) rather than setCount(count+1) and then we dont need to pass count as dependency as it will re-create function.concept for dependency in the beginning was bit wrong and this is correct so passing dependency will simply re-create function reference bu chnaging that variable. 

import React, { useState } from "react";
import Child from "./Child";

function Parent() {
  const [count, setCount] = useState(0);
const handleClick=useCallback(()=>{
  setCount(pre=>pre+1);
},[]);
  return (
    <div>
      <div>
        <p>Count: {count}</p>
        <button
          onClick={handleClick}
        >
          Increment
        </button>
      </div>
      <Child buttonName="Click Me"/>
    </div>
  );
}
export default Parent;


EXPLAINATION of  usecase b of useCallback:-
function will not re-create on every render and you can see by below example,here increment function dont need to recreate on every render when we changre value of count.

import React, { useState, useCallback } from "react";
function App() {
  const [count, setCount] = useState(0);

  // useCallback memoizes the function so it doesn't get recreated on every render
  const increment = useCallback(() => {
    setCount((prev) => prev + 1);
  }, []);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+1</button>
    </div>
  );
}

export default App;















