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

```import React, { useState, useRef } from "react";
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
