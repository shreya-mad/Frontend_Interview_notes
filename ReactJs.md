1.useMemo Hook?
ans: It is nothing but one of the react hooks.here use meaning its a hook becuase use word is used in all the hooks and memo means Memoisation.
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






