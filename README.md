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
```
import React, { useState } from 'react';
import axios from 'axios';

const App = ()=>{
    const [formData, setFormData] =useState({name:'',age:''});

    const handleChange = (e)=>{
        setFormData({...formData,[e.target.name]: e.target.value });
    };

    const handleSubmit=async(event)=>{
        event.preventDefault();
        try{
            await axios.post('http://localhost:3001/data', formData);
            console.log('Data saved');
            setFormData({name:'',age:''});
        } catch (error){
            console.error('Data not saved', error);
        }
        // console.log(formData);
        // setFormData({ name: "", age: "" });
    };

    return(
        <div>
            <form onSubmit={handleSubmit}>
                <input type="text" placeholder="Name" name="name" value={formData.name} onChange={handleChange}/>
                <br/><input type="number" placeholder="Age" name="age" value={formData.age} onChange={handleChange}/>
                <br/><button type="submit">Save</button>
            </form>
        </div>
    );
};

export default App;
```

# Creating data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/c4e77f8f-b743-4a6d-b702-064ca9b7c188)

# Displaying all data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/dc37997f-7d73-4e12-afe1-eb2fea9871db)

# Updating a data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/4bbf51cc-e27a-4789-bbfb-ed73add630e2)

# Deleting a data
![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/03fc03c5-1871-4596-8295-9573b428c11f)
