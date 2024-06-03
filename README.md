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


## CRUD Operation
# React Components


* Create.js


```
import React, { useState } from 'react';
import axios from 'axios';

const departments = [
    {id:'0001',name:'Human Resource'},
    {id:'0002',name:'Accounting and Finance'},
    {id:'0003',name:'Marketing'},
    {id:'0004',name:'Sales'},
    {id:'0005',name:'IT'},
    {id:'0006',name:'Production'},
    {id:'0007',name:'Research and Development'},
];

export const Create=()=>{
    const [formData,setFormData] = useState({name:"",employee_id:"",department_id:"",position:"",salary:""});
    const [result, setResult] = useState("");

    const handleChange=(event)=>{
        const {name,value}=event.target;
        setFormData({...formData,[name]:value});
    };
    const handleSubmit =async(event)=>{
        event.preventDefault();
        try{
            const response = await axios.post('http://localhost:3001/employees',formData);
            if (response.status===200) {
                setResult(true);
            }
            console.log(response.data);
            setFormData({ name: '',employee_id:'',department_id:'', position:'', salary:''});
        }catch (error){
            console.error(error);
        }
    };

    return (
        <>
            <h2>Create User</h2>
            <form onSubmit={handleSubmit}>
                <label for="name">name:</label>
                <input type="text" placeholder="name" name="name" value={formData.name} onChange={handleChange} required />
                <br />
                <label for="employee_id">Employee ID:</label>
                <input type="text" placeholder="Employee ID" name="employee_id" value={formData.employee_id} onChange={handleChange} required />
                <br />
                <label htmlFor="department_id">Department ID:</label>
                <select id="department_id" name="department_id" value={formData.department_id} onChange={handleChange} required>
                    <option value="">Select Department</option>
                    {departments.map((dept) =>(
                        <option key={dept.id} value={dept.id}>
                            {dept.name}
                        </option>
                    ))}
                </select>
                <br/>
                <label for="position">Position:</label>
                <input type="text" placeholder="Position" name="position" value={formData.position} onChange={handleChange} required />
                <br/>
                <label for="salary">Salary:</label>
                <input type="number" placeholder="Salary" name="salary" value={formData.salary} onChange={handleChange} required />
                <br/>
                <button type="submit">Submit</button>
            </form>
            {result?<p>User Created</p>:<></>}
        </>
    );
};
```


* Update.js


```
import React, { useState, useEffect } from "react";
import axios from "axios";

export const Update = () => {
    const [id, setId] = useState("");
    const [details, setDetails] = useState({name:"",position:"",salary:""});
    const [isEditing, setIsEditing] = useState(false);
    const [editedDetails, setEditedDetails] = useState({name:"",position:"",salary:"" });

    useEffect(() => {
        const fetchDetails = async () => {
            try {
                const response = await axios.get(`http://localhost:3001/employees/${id}`);
                setDetails(response.data);
            }
            catch(error){
                console.error("Error fetching details:", error);
            }
        };
        if (id) {
            fetchDetails();
        }
    }, [id]);

    const handleChange=(event)=>{
        setEditedDetails({...editedDetails,[event.target.name]: event.target.value});
    };

    const handleUpdate = async () => {
        try {
            const response = await axios.put(`http://localhost:3001/employees/${id}`, editedDetails);

            if (response.status === 200) {
                console.log("Details updated successfully!");
                setIsEditing(false);
                setDetails(editedDetails);
                setEditedDetails({ name: "", position: "", salary: "" }); 
            } else {
                console.error("Error updating details:", await response.text());
            }
        } catch (error) {
        console.error("Error updating details:", error);
        }
    }; 
    return (
        <div>
        <h2>Update Employee Details</h2>
        <form onSubmit={(e) => e.preventDefault()}>
            <input type="text" placeholder="Enter employee ID" value={id} onChange={(e) => setId(e.target.value)} />
        </form>
        {details.name && (
            <div>
            <p>Name: {details.name}</p>
            <p>Position: {details.position}</p>
            <p>Salary: {details.salary}</p>
            {isEditing ? (
                <>
                <input type="text" name="name" value={editedDetails.name} onChange={handleChange} placeholder="Edit Name" />
                <br />
                <input type="text" name="position" value={editedDetails.position} onChange={handleChange} placeholder="Edit Position" />
                <br />
                <input type="number" name="salary" value={editedDetails.salary} onChange={handleChange} placeholder="Edit Salary" />
                <br />
                <button onClick={handleUpdate}>Update</button>
                </>
            ) : (
                <button onClick={() => setIsEditing(true)}>Edit</button>
            )}
            </div>
        )} 
        </div>
    );
};

```


* Delete.js


```
import React, { useState } from "react";
import axios from "axios";

export const Delete=()=>{
    const [idToDelete, setIdToDelete] = useState(null); 
    const [message, setMessage] = useState(null);

    const handleChange=(event)=>{
        setIdToDelete(event.target.value); 
    };
    const handleDelete=async()=>{
        try {
        const response=await axios.delete(`http://localhost:3001/employees/${idToDelete}`);

        if (response.status===200) {
            console.log("Employee deleted successfully!");
            setMessage("Deletion Successful!");
            setIdToDelete(null);
        } else {
            throw new Error("Error deleting user"); 
        }
        } 
        catch(error) {
            console.error("Error deleting employee:", error);
            setMessage(`Error: ${error.message}`); 
        }
    };

    return (
        <div>
            <h2>Delete Employee Details</h2>
            <input type="text"placeholder="Enter Employee ID to Delete"onChange={handleChange}/>
            <button onClick={handleDelete}>Delete</button>
            {message && <p className="error-message">{message}</p>}
        </div>
    );
};

```


* GetAllEmployeeDetails.js


```
import React, { useEffect, useState } from "react";
import axios from "axios";

export const AllData=()=>{
    const [data, setData]=useState([]); // Update state type to array

    useEffect(()=>{
        const fetchData =async()=> {
            try {
                const response = await axios.get("http://localhost:3001/employees");
                setData(response.data);
                console.log(response.data);
            }
            catch(error){
                console.error(error);
            }
        };
        fetchData();
    }, []);

    return (
        <div>
        <h2>All User Data</h2>
        <table style={{ border: "1px solid black", borderCollapse: "collapse" }}>
            <thead>
            <tr>
                <th style={{ border: "1px solid black", padding: "5px" }}>
                    Name
                    </th>
                <th style={{ border: "1px solid black", padding: "5px" }}>
                    Department
                </th>
                <th style={{ border: "1px solid black", padding: "5px" }}>
                    Manager
                </th>
                <th style={{ border: "1px solid black", padding: "5px" }}>
                    Position
                </th>
                <th style={{ border: "1px solid black", padding: "5px" }}>
                    Salary
                </th>
                <th style={{ border: "1px solid black", padding: "5px" }}>
                    ID
                </th>
            </tr>
            </thead>
            <tbody>
                {data.length > 0 &&
                    data.map((employee) => (
                    <tr key={employee._id} style={{ border: "1px solid black" }}>
                        <td style={{ border: "1px solid black", padding: "5px" }}>
                        {employee.name}
                        </td>
                        <td style={{ border: "1px solid black", padding: "5px" }}>
                        {employee.department
                            ? employee.department.department_name
                            : "Not Found"}
                        </td>
                        <td style={{ border: "1px solid black", padding: "5px" }}>
                            {employee.department
                                ? employee.department.manager
                                : "Not Found"}
                        </td>
                        <td style={{ border: "1px solid black", padding: "5px" }}>
                            {employee.position}
                        </td>
                        <td style={{ border: "1px solid black", padding: "5px" }}>
                            {employee.salary}
                        </td>
                        <td style={{ border: "1px solid black", padding: "5px" }}>
                            {employee.employee_id}
                        </td>
                    </tr>
                    ))}
                {data.length === 0 && (
                    <tr>
                        <td colSpan="6">No data found.</td>
                    </tr>
                )}
            </tbody>
        </table>
        </div>
    );
};
```


* GetAEmployeeDetails.js


```
import React, { useEffect, useState } from "react";
import axios from "axios";

export const SingleUser = ()=>{
    const [id, setId]= useState("");
    const [user, setUser]= useState(null); 
    const [error, setError] = useState(null); 

    const handleChange = (event)=>{
        setId(event.target.value);
    };

    const fetchUser =async() =>{
        try {
            const response = await axios.get(`http://localhost:3001/employees/${id}`);
            setUser(response.data);
            setError(null); 
        } catch (error) {
            setError("Error fetching user data");
            console.error("Error:", error);
        }
    };

    useEffect(() => {
        if (id) {
            fetchUser();
        }
    }, [id]);

    return (
        <div>
        <h2>Employee Details</h2>
            <input type="text" placeholder="Enter employee ID" value={id} onChange={handleChange}/>
            {error ? (
                <p className="error-message">{error}</p>
            ):user?(
                    <div>
                    <p>Name: {user.name}</p>
                    <p>
                        Department:{" "}
                        {user.department ? user.department.department_name : "Not Found"}
                    </p>
                    <p>
                        Manager: {user.department ? user.department.manager : "Not Found"}
                    </p>
                    <p>Position: {user.position}</p>
                    <p>Salary: {user.salary}</p>
                    <p>ID: {user.employee_id}</p>
                    </div>
            ):null}
        </div>
    );
};
```


# Backend

* index.js

  
```
const express = require("express");
const app = express();
const cors = require("cors");
app.use(express.json());
app.use(cors());
require("dotenv").config();
const port = process.env.PORT;
const con = process.env.CONNECTION_STRING;
const mongoose = require("mongoose");

const employeeSchema = new mongoose.Schema({
    name: String,
    employee_id: { type: String, unique: true },
    department_id: String,
    position: String,
    salary: Number,
});

const Employee = mongoose.model("Employee", employeeSchema);

const DepartmentSchema = new mongoose.Schema({
    department_name: String,
    department_id: String,
    manager: String,
},{
    collection:"department"
});

const Department = mongoose.model("Department", DepartmentSchema)


mongoose
    .connect(con)
    .then(() => console.log("Connected"))
    .catch((err) => console.error("Connection Failed", err));



app.get("/employees", async (req, res) => {
    try {
        const employees = await Employee.find();
        const employeesWithDepartments = await Promise.all(
            employees.map(async (employee) => {
                const department = await Department.findOne({ department_id: employee.department_id });
                console.log(`Employee ${employee.name} (ID: ${employee.employee_id}) - Department: ${department ? department.department_name : 'Not found'}`);
                return { ...employee.toObject(), department };
            })
        );
        res.status(200).json(employeesWithDepartments);
    } catch (error) {
        res.status(500).json({ error: `${error}` });
    }
});


app.get("/employees/:id", async (req, res) => {
    try {
        const { id } = req.params;
        const employee = await Employee.findOne({ employee_id: id });
        if (!employee) return res.status(404).json({ message: "Employee not found" });
        const department = await Department.findOne({department_id: employee.department_id});
        const employeeWithDepartment = { ...employee.toObject(), department };
        res.status(200).json(employeeWithDepartment);
    } catch (error) {
        res.status(500).json({ error: `${error}` });
    }
});

app.post("/employees", async (req, res) => {
    try {
        const { name, employee_id, department_id, position, salary } = req.body;
        const existingDepartment = await Department.findOne({ department_id });
        if (!existingDepartment)
            return res.status(404).json({ message: `Department not found for ID: ${department_id}` });
        const newEmployee = new Employee({name,employee_id,department_id,position,salary});
        const createdEmployee = await newEmployee.save();
        res.status(201).json(createdEmployee);
    } catch (error) {
        res.status(500).json({ error: `${error}` });
    }
});

app.put("/employees/:id", async (req, res) => {
    try {
        const { id } = req.params;
        const { name, position, salary } = req.body;
        const updatedEmployee = await Employee.findOneAndUpdate({employee_id: id},{$set: { name, position, salary }},{ new: true });
        if (!updatedEmployee)   return res.status(404).json({ message: "Employee not found" });
        res.status(200).json(updatedEmployee);
    } catch (error) {
        res.status(500).json({ error: `${error}` });
    }
});

app.delete("/employees/:id", async (req, res) => {
    try {
        const { id } = req.params;
        const deletedEmployee = await Employee.findOneAndDelete({employee_id: id});
        if (!deletedEmployee)   return res.status(404).json({ message: "Employee not found" });
        res.status(200).json({ message: "Employee deleted" });
    } catch (error) {
        res.status(500).json({ error: `${error}` });
    }
});

app.listen(port, () => {
    console.log("Server started!!!");
});

```


* Department Collection

![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/bc349439-5158-4599-b7f2-7c2cd4b2359d)

* Employee Collection

![image](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/afa35e96-5f4a-479d-9280-cce5d7cd56dc)

* Create Employee

![Screenshot (20)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/5d6a677f-e904-4e8a-a029-72c49f3ba387)

* Update Employee

![Screenshot (21)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/bf35882b-40eb-4312-94cb-4ddaf4c2d29a)

* Delete Employee

![Screenshot (22)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/b4554696-69fb-4d9b-bf4f-392f2ead6778)

* Display All Empployee Data

![Screenshot (23)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/2007231a-97a9-405e-9180-2bb932d8971a)

* Display A Single Employee Data

![Screenshot (24)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/342ea554-c5f6-4221-8ea1-38326e4b3062)




## Email sending using nodemailer

# Backend

* Server.js

```

const express = require("express")
const mongoose = require("mongoose")
const cors = require("cors")
const nodemailer = require("nodemailer")
const app = express()


require("dotenv").config()
app.use(express.json())
app.use(cors())


mongoose
    .connect(process.env.CONNECTION_STRING)
    .then(() => console.log("Database Connected"))
    .catch((error) => console.log(error))

const Events = require("./models/Events")
const Customer = require("./models/Customer")

// creating event
app.post("/events", async(req,res)=>{
    try{
        const eventData =req.body
        const event = new Events(eventData)
        await event.save()
        res.status(201).json(event)
    }catch(error){
        console.error(error)
        res.status(500).json({ error: "Server error" })
    }
})

// fetch event
app.get("/events", async(req,res)=>{
    try{
        const Event=await Events.find()
        res.json(Event)
    }catch(error){
        console.error(error)
        res.status(500).json({error:"Server error"})
    }
})

// sending 
app.post("/events/:eventId/sendmail", async(req, res)=>{
    try{
        const eventId = req.params.eventId
        const event=await Events.findOne({eventId:eventId}) 
        if(!event){
            return res.status(404).json({message:"Event not found"})
        }
        const customers = await Customer.find()
        let current = 0
        for (const customer of customers){
            try{
                await transporter.sendMail({
                    to: customer.customer_mail,
                    subject: `Happy ${event.name}! Mr/Ms.${customer.customer_name}!`,
                    text: event.message,
                })
                console.log(`Mail sent to ${customer.customer_name}, Total mails sent ${++current}`);

            }catch(error){
                console.error(error)
            }
        }
        console.log({ message: "Emails sent successfully" })
    }catch(error){
        console.error(error) 
    }
})


app.get("/customers", async(req,res)=>{
    try{
        const customers= await customer.find()
        res.json(customers)
    }catch (error){
        console.error(error)
    }
})

const transporter =nodemailer.createTransport({
    host:"smtp.gmail.com",
    port:587,
    auth:{
        user: process.env.EMAIL,
        pass: process.env.EMAIL_PASS,
    }
})


app.listen(5000,()=>console.log("Server running on port 5000"))


```

# Frontend

* CreateEvent.js

```

import React, { useState } from "react"
import axios from "axios"

const CreateForm=({onAddEvent})=>{
    const [eventData, setEventData] = useState({name: "",date: "",message: "",eventId: ""})
    
    const handleInputChange =(e)=>{
        setEventData({ ...eventData, [e.target.name]: e.target.value })
    }

    const handleSubmit=async(e)=>{
        e.preventDefault()
        try{
            await axios.post("http://localhost:5000/events", eventData)
            onAddEvent()
            setEventData({name:"",date:"",message:"",eventId:""})
        }catch(error){
            console.error("Error creating event:", error)
        }
    }

    return (
        <form onSubmit={handleSubmit}>
        <input type="text" name="name" value={eventData.name} onChange={handleInputChange} placeholder="Event Name" required/>
        <br />
        <input type="date" name="date" value={eventData.date} onChange={handleInputChange} required/>
        <br />
        <br />
        <input type="text" name="eventId" value={eventData.eventId} onChange={handleInputChange} placeholder="Event ID" required/>
        <br />
        <textarea name="message" value={eventData.message} onChange={handleInputChange} placeholder="Message" required/>
        <br />
        <button type="submit">Create Event</button>
        </form>
    )
}

export default CreateForm


```


* EventsCard.js

```

import React from "react"
import axios from "axios"

const EventsCard=({event,onSendMail})=>{
    const handleSendMail = async()=>{
        try{
            await axios.post(`http://localhost:5000/events/${event.eventId}/sendmail`)
            onSendMail()
        }catch(error){
            console.error("Error sending event emails:", error)
        }
    }

    return (
        <div style={{border:"5px solid", padding:"0px 20px"}}>
            <h4>{event.name}</h4>
            <p>Date: {new Date(event.date).toLocaleDateString()}</p>
            <p>Message: {event.message}</p>
            <button style={{border:"5px solid green", background:"green" }}onClick={handleSendMail}>Send Mail</button>
        </div>
    )
}

export default EventsCard


```


* EventsList.js

```

import React, { useEffect, useState } from "react"
import axios from "axios"
import EventsCard from "./EventsCard"

const EventsList=()=>{
    const [events,setEvents] =useState([])

    useEffect(()=>{
        const fetchEvents=async()=>{
            try{
                const response =await axios.get("http://localhost:5000/events")
                setEvents(response.data)
                console.log(response.data)
            }catch(error){
                console.error("Error fetching events:", error)
            }
        }
        fetchEvents()
    },[])

    const handleSendMail =async()=>{
        try{
            const response = await axios.get("http://localhost:5000/events")
            setEvents(response.data)
        }catch (error) {
            console.error("Error fetching events:",error)
        }
    }
    return (
        <div>
        <h3>Events List</h3>
            {events.map((event)=>(
                <EventsCard event={event} onSendMail={handleSendMail}/>
            ))}
        </div>
    )
}

export default EventsList


```

- Creating Event

![Screenshot (30)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/82aec57c-cccb-4219-995c-92263d910409)

- Displaying the Events

![Screenshot (31)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/552e5dca-0141-4fba-ad14-e1b0ea99a98c)

- Sending mail using "Send mail" button on EventCard

![Screenshot (32)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/de4e72aa-26ec-429d-9824-1375d5aed2fb)

- Mail received by the recipients

![Screenshot (33)](https://github.com/AnanDEswaran18/Datum-Daily-Report/assets/100366969/16a1bf22-d1d5-4243-bac9-990dd0201795)

