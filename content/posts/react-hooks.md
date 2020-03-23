+++
date = "2019-11-08"
title = "Using React Hooks - useState and useEffect"
slug = "using-reactjs-hooks-introduction-useState-useEffect"
tags = [
    "reactjs",
    "react-hooks",
    "javascript",
    "programming",
    "development",
]
+++


With the release of [Hooks](https://reactjs.org/docs/hooks-intro.html "React Hooks") in React 16.8, it is now possible to store **state** in a **function**. We can add react features like useState and useEffect into the function directly without needing to create **class**.

## useState

The **useState** hook adds state to the functional components. useState hook allows you to declare one state variable at a time.

```
import React, { useState } from 'react';

function Counter() {
  //Declare the "counter" state variable
  const [counter, setCounter] = useState(0);

  return (
      <div>
        <p>Clicked {counter} times</p>

        //increment the counter by 1 on every click
        <button onClick={() => setCounter(counter + 1)}>Click here</button>
      </div>
  );
}
``` 

## useEffect

The **useEffect** allows to manage side effects in a functional component. Some of the examples are fetching external data using api, manipulating DOM in react. It is similar to componentDidMount and componentDidUpdate from class component.

```
import React, { useState, useEffect } from 'react';

function LoadProducts() {
  //Declare the "products" state variable
  const [products, setProducts] = useState([]);

  useEffect(() => {
    fetch('www.example.com/api/getProducts')
      .then(response => response.json())
      .then(data => {
        setProducts(data); // add product list to the state array.
      });
  }, []); 

  return (
    <div>
        {products.map((product, index) => (
            <div key={index}>
                <p>{product.name}</p>
                <p>{product.price}</p>
            </div>
        ))}
    </div>
  );
}
```