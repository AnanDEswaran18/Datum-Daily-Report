# Datum-Daily-Report
## Day-1 [May-17, 2024]
* Task : Creating first React app Understanding the project structure.
  
![React App & code](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/cb466e59-7fb9-4490-895f-cdd4a38dae58)

## Day-2 [May-18, 2024]
* Task : Creating components with JSX Rendering components to the DOM Embedding expressions in JSX
  
![Components in JSX](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/1fe7b1ee-267e-481e-82c9-37e007cca9b4)

## Day-3 [May-19, 2024]
* Task : Creating and using functional components Passing props to components Default props and prop types validation

![Component with props](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/e64da4ed-922b-418c-8b9b-cb0d365f1c10)
![Component with default props](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/2a134f4c-0138-4c5e-9f5f-46477581bf1a)

## Day-4 [May-20, 2024]
* Task : Managing state in class components Using lifecycle methods in class components
![Managing state in class components using life cycle methods](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/cedb729a-d328-4b0f-8563-ab82409cf7a2)

## Day - 5 and 6[May-21, 2024]
* Task-1 : Handling user input with forms Creating controlled form components Handling form submission
* Task-2 : Implementing conditional rendering Using the map function to render lists Assigning unique keys to list items

![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/7799d9c9-5841-4ab4-b6cd-00e7ff49ce55)

```
import React, { useState, useRef } from "react";
function Form() {
    const [formData, setFormData] = useState({name: "",email: "",items: [],});
    const newRef = useRef(null);

    const handleChange = (event) => {
        const { name, value } = event.target;
        setFormData((prevData) => ({ ...prevData, [name]: value }));
    };
    const submit = (event) => {
        event.preventDefault();
        console.log("Form submitted:", formData);
        // setFormData({ name: "", email: "", items: [] });
    };
    const addItem = (event) => {
        // event.preventDefault();
        const newItem = newRef.current.value;
        if (newItem) {
            setFormData((prevData) => ({...prevData,items: [...prevData.items, newItem],}));
            newRef.current.value = null;
        } else {
            console.error("Enter a valid item");
        }
    };

    return (
        <form onSubmit={submit}>
            <h2>Controlled Form Example</h2>
            <label>Name:
                <input type="text" id="name" name="name" value={formData.name} onChange={handleChange} required/>
            </label>
            <br/>
            <label htmlFor="email">Email:
                <input type="email" id="email" name="email" value={formData.email} onChange={handleChange} required/>
            </label>
            <br/>

            <h3>Add Items</h3>
            <input type="text" name="newItem" ref={newRef} placeholder="Enter an item"/>
            <button type="button" onClick={addItem}>Add Item</button>
            <br/>
            {formData.items.length > 0 && (
                <ul>
                    {formData.items.map((item, index) => (
                        <li key={index}>
                            {item}
                        </li>
                    ))}
                </ul>
            )}
            <br/>
            <button type="submit">Submit</button>
        </form>
    );
}

export default Form;
```

## CRUD Operation with Node, Express, MongoDB and React
```
const express = require("express")
const app = express()

// const bodyParser = require("body-parser")
// app.use(bodyParser.json())
app.use(express.json());

const cors = require("cors");
app.use(cors());

require("dotenv").config()

const port = process.env.PORT
const con = process.env.CONNECTION_STRING

const mongoose = require("mongoose")
const dataSchema =new mongoose.Schema({name:String,age:Number});
const data =mongoose.model("FormData", dataSchema);

// -----------------------------------------------------------//

mongoose
    .connect(con)
    .then(()=>console.log("Connected"))
    .catch((err)=>console.error("Connection Failed",err));

    
//create data
app.post("/data",async(req,res)=>{
    try{
        const {name,age}=req.body;
        await data.create({name,age});
        res.status(200).json({message:"Data saved"});
    }
    catch(err){
        console.error("Failed");
        res.status(500).json({error:`${err}`});
    }
});

//get all data
app.get("/data",async(req,res)=>{
    try{
        const val = await data.find({});
        res.status(200).json({count:val.length,data:val})
    }
    catch(error){
        res.status(500).json({ error: `${error}` });
    }
});

//get single data by id
app.get("/data/:id",async(req,res)=>{
    try{
        const {id} = req.params
        const singleData = await data.findById(id)
        res.status(200).json(singleData)
    }catch(error){
        res.status(500).json({ error: `${error}` });
    }
})

//update data
app.put("/data/:id",async(req,res)=>{
    try{
        const {name,age} = req.body
        const {id} = req.params
        const result = await data.findByIdAndUpdate(id,{name,age})
        res.status(200).json({message:"Data Updated"})
        if(!result){res.json({message:"Book not found"})}
    }catch(error){
        res.status(500).json({ error: `${error}` });
    }
})

//delete data
app.delete("/data/:id",async(req,res)=>{
    try{
        const {id} = req.params
        await data.findByIdAndDelete(id);
        res.status(200).json({message:"Data deleted"})
    }
    catch(error){
        res.status(500).json({ error: `${error}` });
    }
})

app.listen(port,()=>{
    console.log("Server started!!!");
})

```
## React Code
- Create User
```
import React, { useState } from 'react';
import axios from 'axios';

export const Create=()=>{
    const [formData, setFormData] = useState({ name: "", age: "" });
    const [result, setResult] = useState(null)

    const handleChange = (event) => {
        const { name, value } = event.target;
        setFormData({ ...formData, [name]: value });
    };

    const handleSubmit = async (event) => {
        event.preventDefault();
        try {
            const response = await axios.post('http://localhost:3001/data/create', formData);
            if(response.status===200){
                setResult(true)
            }
            console.log(response.data);
            setFormData({name:'',age:''});
        } catch (error) {
            console.error(error);
        }
    };

    return (
        <>
            <h2>Create User</h2>
            <form onSubmit={handleSubmit}>
                <input type="text" placeholder="Name" name="name" value={formData.name} onChange={handleChange}/>
                <br />
                <input type="number" placeholder="Age" name="age" value={formData.age} onChange={handleChange}/>
                <br />
                <button type="submit">Save</button>
            </form>
            {result ? <p>User Created</p> : <></>}
        </>
    );
}


```
-Update User
```
import React, { useState, useEffect } from "react";
import axios from "axios"; // Import Axios

export const Update=()=>{
    const [id,setid] = useState("");
    const [details,setDetails] = useState({name: "",age: ""});
    const [isEditing,setIsEditing] = useState(false);
    const [editedDetails,setEditedDetails] = useState({ name: "",age: "" });
    useEffect(() => {
        const fetchDetails = async () => {
            try {
                const response = await axios.get(`http://localhost:3001/data/${id}`);
                setDetails(response.data);
            } catch (error) {
                console.error("Error fetching details:", error);
            }
        };
        if(id) {
            fetchDetails();
        }
    }, [id]);

    const handleChange = (event) => {
        setEditedDetails({
            ...editedDetails,
            [event.target.name]: event.target.value
        });
    };

    const handleUpdate = async()=>{
        try {
            const response = await axios.put(`http://localhost:3001/data/${id}`,editedDetails);
            if (response.status === 200){
                console.log("Details updated successfully!");
                setIsEditing(false); 
                setDetails(editedDetails); 
                setEditedDetails({ name: "", age: "" }); 
            }else{
                console.error("Error updating details:", await response.text());
            }
        }catch(error){
            console.error("Error updating details:", error);
        }
    };

    return (
        <div>
            <h2>Update User Details</h2>
            <form onSubmit={(e)=>e.preventDefault()}>
                <input type="text" placeholder="Enter object ID" value={id} onChange={(e) => setid(e.target.value)}/>
            </form>
            {details.name&&(
                <div>
                    <p>Name: {details.name}</p>
                    <p>Age: {details.age}</p>
                    {isEditing ?
                        <>
                            <input type="text" name="name" value={editedDetails.name } onChange={handleChange} placeholder="Edit Name"/><br/>
                            <input type="number" name="age" value={editedDetails.age } onChange={handleChange} placeholder="Edit Age"/><br/>
                            <button onClick={handleUpdate}>Update</button>
                        </> :
                        <button onClick={() => setIsEditing(true)}>Edit</button>
                    }
                </div>
            )}
        </div>
    );
};

```
 -Delete User
```
import React, { useState } from "react";
import axios from "axios";

export const Delete = () => {
    const [idToDelete, setIdToDelete] = useState(null);
    const [message, setMessage] = useState(null);
    const handleChange = (event) => {
        setIdToDelete(event.target.value);
    };

    const handleDelete = async () => {
        try {
        const response = await axios.delete(`http://localhost:3001/data/${idToDelete}`);

        if (response.status === 200) {
            //console.log("Data deleted successfully!");
            setMessage("Deletion Success!!")
        } else {
            throw new Error(`Error deleting data: ${response.statusText}`);
        }
        } catch (error) {
            console.error("Error deleting data:", error);
        }
    };

    return (
        <div>
        <h2>Delete User Details</h2>
        <input
            type="text"
            placeholder="Enter ID to Delete"
            onChange={handleChange}
        />
        <button onClick={handleDelete}>Delete</button>
        {message && <p className="error-message">{message}</p>}
        </div>
    );
};

```
- Display all user details
```
import React, { useEffect, useState } from "react";
import axios from "axios";

export const AllData = () => {
    const [data, setData] = useState({});

    useEffect(() => {
        const fetchData = async () => {
          try {
              const response = await axios.get("http://localhost:3001/data-all"); 
              setData(response.data.data);
              console.log(response);
          } catch (error) {
              console.error(error); 
          }
        };
        fetchData();
    }, []); 

    return (
        <div>
          <h2>All User Data</h2>
          {data.length>0?
              <ul>
              {data.map((val) => (
                  <li key={val._id}>
                      Name: {val.name}<br/> Age: {val.age}<br/> Id: {val._id}
                  </li>
              ))}
              </ul>
            :<p>No data found.</p>}
        </div>
    );
};

```  
 
# Creating data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/16b507dd-ead8-479b-8e60-ec93d905fcdf)


# Displaying all data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/c134cc36-3f90-49b6-866f-60e4f84a3afe)


# Updating a data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/f3e079c2-8e6e-43af-be98-f19df4cfdf61)


# Deleting a data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/ccd7c67b-9152-46c6-a585-55281ec283ad)




# Converting Class Components to Functional Components with Hooks(useState and useEffect)

* Class Component
```
class Counter extends React.Component {
  constructor(props){
    super(props)
    this.state ={data:[]}
  }

  componentDidMount(){
      const uData = await axios.get("http://localhost:3000/data")
      this.setState({data:uData})
  }
  render(){
    return(
      <div>
        <>
          {data}
        </>

      </div>
    )
  }
}
```

* Functional Component
```
import React, { useState, useEffect } from 'react'

function Counter(){
  const [uData, setUsers]=useState();
  useEffect(() =>{
    const getData = async()=>{
      try{
        const res = await axios.get("http://localhost:3000/data");
        setUsers(res.data);
      }
      catch(err){
        console.error(err);
      }
    };
    getData();
  }, []);

  return (
    <div>
      <>
        {uData}
      </>
    </div>
  );
}

```  

## ToDo List

* ToDoList.js
```

import React, { useState } from "react";
import TodoItem from "./TodoItems";

const TodoList =()=>{
    const [todo,setTodo] =useState([]);
    const [value,setValue]=useState("");

    const add=()=>{
        if(value){
            const title = value;
            const newTodo = { title, completed: false};
            setTodo([...todo, newTodo]);
            setValue("");
        }
    };

    const toggleTodoComplete=(index)=>{
        const newTodo = [...todo];
        newTodo[index].completed = !newTodo[index].completed;
        setTodo(newTodo);
    };

    const removeTodo=(index)=>{
        const newTodo = [...todo];
        newTodo.splice(index, 1);
        setTodo(newTodo);
    };

    const handleChange=(e)=>{
        setValue(e.target.value);
    };

    const handleEnter=(e)=>{
        if (e.key==="Enter"){
            add();
        }
    };

    return(
        <div className="todo-list">
            <h2>Todo</h2>
            <input type="text" placeholder="Add a Todo" value={value} onChange={handleChange} onKeyDown={handleEnter}/>
            <ul style={{listStyle: "none"}}>
                {todo.map((todo,index)=>(
                    <TodoItem key={index} title={todo.title} completed={todo.completed} onCheck={() => toggleTodoComplete(index)} onRemove={() => removeTodo(index)}/>
                ))}
            </ul>
        </div>
    );
};

export default TodoList;

```

* ListItem.js
```
import React from "react";

const TodoItem=({title, completed, onCheck, remove })=>{
    return (
        <li className={`todo-item ${completed ? "completed" : ""}`}>
            <label>
                <input type="checkbox" checked={completed} onChange={onCheck}/>
                {title}
            </label>
            <button onClick={remove}>Remove</button>
        </li>
    );
};

export default TodoItem;
```

![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/d6b42ea4-d406-4473-8b23-550c42bacff7)
