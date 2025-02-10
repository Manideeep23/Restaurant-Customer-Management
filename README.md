Restaurant Customer Management

This project is a simple web application to manage restaurant customers. 
It allows users to add customers, track their check-in and check-out times, and monitor the available seats in the restaurant.

Features

- Add new customers with guest count, name, and phone number.
- Track check-in and check-out times for each customer.
- Display the current status of each customer (Pending, Checked In, Checked Out).
- Automatically update the available seats when customers check-in and check-out.
- Prevent adding more customers if there are no seats left.
- Visually appealing design with a playful color scheme.

Technologies Used

- React: JavaScript library for building user interfaces.
- CSS: Styling for the application.
- HTML: Markup language for structuring the web application.

Getting Started

Prerequisites

- Node.js: JavaScript runtime for building server-side applications.
- npm: Package manager for Node.js.

Codes

```
App.jsx:
import React, { useState } from 'react';
import './App.css';

function App() {
  const [guestCount, setGuestCount] = useState(1);
  const [name, setName] = useState('');
  const [phone, setPhone] = useState('');
  const [customers, setCustomers] = useState([]);
  const totalCapacity = 50;

  const addCustomer = (e) => {
    e.preventDefault();
    const seatsLeft = totalCapacity - customers.reduce((acc, curr) => acc + (curr.status !== 'Checked Out' ? curr.guestCount : 0), 0);
    if (guestCount > seatsLeft) {
      alert('No seats left!');
      return;
    }
    const newCustomer = { guestCount, name, phone, checkIn: '', checkOut: '', status: 'Pending' };
    setCustomers([...customers, newCustomer]);
    setGuestCount(1);
    setName('');
    setPhone('');
  };

  const checkInCustomer = (index) => {
    const updatedCustomers = [...customers];
    updatedCustomers[index].checkIn = new Date().toLocaleString();
    updatedCustomers[index].status = 'Checked In';
    setCustomers(updatedCustomers);
  };

  const checkOutCustomer = (index) => {
    const updatedCustomers = [...customers];
    updatedCustomers[index].checkOut = new Date().toLocaleString();
    updatedCustomers[index].status = 'Checked Out';
    setCustomers(updatedCustomers);
  };

  const seatsLeft = totalCapacity - customers.reduce((acc, curr) => acc + (curr.status !== 'Checked Out' ? curr.guestCount : 0), 0);

  return (
    <div className="App">
      <h1>Restaurant Customer Management</h1>
      <div className="capacity-info">
        <div className="box">Total Capacity: {totalCapacity}</div>
        <div className="box">Seats Left: {seatsLeft > 0 ? seatsLeft : 'No seats left'}</div>
      </div>
      <form onSubmit={addCustomer}>
        <label>
          Guest Count:
          <input type="number" value={guestCount} onChange={(e) => setGuestCount(Number(e.target.value))} min="1" required />
        </label>
        <label>
          Name:
          <input type="text" value={name} onChange={(e) => setName(e.target.value)} required />
        </label>
        <label>
          Phone:
          <input type="tel" value={phone} onChange={(e) => setPhone(e.target.value)} required />
        </label>
        <button type="submit">Add Customer</button>
      </form>
      <h2>Customer List</h2>
      <table>
        <thead>
          <tr>
            <th>Count</th>
            <th>Name</th>
            <th>Phone</th>
            <th>Check In</th>
            <th>Check Out</th>
            <th>Status</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          {customers.map((customer, index) => (
            <tr key={index}>
              <td>{customer.guestCount}</td>
              <td>{customer.name}</td>
              <td>{customer.phone}</td>
              <td>{customer.checkIn}</td>
              <td>{customer.checkOut}</td>
              <td>{customer.status}</td>
              <td>
                {customer.status === 'Pending' && <button onClick={() => checkInCustomer(index)}>Check In</button>}
                {customer.status === 'Checked In' && <button onClick={() => checkOutCustomer(index)}>Check Out</button>}
                <button onClick={() => setCustomers(customers.filter((_, i) => i !== index))}>
                  Remove
                </button>
              </td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default App;


App.css:
body {
  font-family: 'Courier New', monospace;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background-color: #FFFFFF; /* White */
  color: #000000; /* Black for readability */
}

.App {
  text-align: center;
  background-color: #FFA500; /* Bright Orange */
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

h1 {
  color: #40E0D0; /* Turquoise */
  font-size: 2.5em;
  margin-bottom: 20px;
}

.capacity-info {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
}

.box {
  background-color: #40E0D0; /* Turquoise */
  padding: 10px;
  border-radius: 5px;
  font-size: 1.2em;
  color: #FFFFFF; /* White */
}

form {
  background-color: #FFFFFF; /* White */
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 15px;
  font-weight: bold;
  color: #FFA500; /* Bright Orange */
}

input {
  margin-left: 10px;
  padding: 10px;
  border: 1px solid #40E0D0; /* Turquoise */
  border-radius: 5px;
  width: calc(100% - 22px);
  font-size: 1em;
  background-color: #FFFFFF; /* White */
  color: #000000; /* Black for readability */
}

button {
  margin-top: 10px;
  padding: 10px 20px;
  background-color: #FFA500; /* Bright Orange */
  color: #FFFFFF; /* White */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #40E0D0; /* Turquoise */
  color: #FFFFFF; /* White */
}

h2 {
  margin-top: 20px;
  color: #FFA500; /* Bright Orange */
  font-size: 2em;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: medium; /* Medium font size for the table */
}

th, td {
  padding: 10px;
  border: 1px solid #40E0D0; /* Turquoise */
  color: #000000; /* Black for readability */
}

th {
  background-color: #FFA500; /* Bright Orange */
  color: #FFFFFF; /* White */
}
```
Screenshots



![11](https://github.com/user-attachments/assets/822e2326-f8bd-449c-93af-8ae548c2873a)
![12](https://github.com/user-attachments/assets/ac4aae7b-e9e6-43f5-a2c5-18477dfdb85d)
![13](https://github.com/user-attachments/assets/5c1578ad-3676-4de2-9580-8f6f327fe764)
![14](https://github.com/user-attachments/assets/2d396bde-607d-4455-8d77-07142dd42633)
![15](https://github.com/user-attachments/assets/77810560-d846-4a75-bd69-112336335dca)
![16](https://github.com/user-attachments/assets/55def64b-40e7-4f38-9c29-aa272e24c7fe)
![17](https://github.com/user-attachments/assets/c21933a8-28b0-4166-8e3f-eb76d31f65c1)
![18](https://github.com/user-attachments/assets/189d1249-cd56-435d-9bb2-ed7043a0ffbe)


Usage

- Open the application in your browser.
- Use the form to add a new customer by entering the guest count, name, and phone number.
- Click "Add Customer" to add the customer to the list
- The customer will appear in the customer list with a status of "Pending."
- Click "Check In" to mark the customer as checked in.
- Click "Check Out" to mark the customer as checked out.
- The available seats will be updated automatically based on the customer status.

Code Explanation

App.jsx

The App.jsx file contains the main component of the application.
It manages the state and logic for adding customers, tracking check-ins and check-outs, and displaying the customer list.

State Variables:

- guestCount: Tracks the number of guests for each customer.
- name: Tracks the name of the customer.
- phone: Tracks the phone number of the customer.
- customers: Array of customer objects.
- totalCapacity: Total seating capacity of the restaurant.

Functions:

- addCustomer: Adds a new customer to the customer list. Checks if there are available seats before adding.
- checkInCustomer: Marks a customer as checked in and updates the check-in time.
- checkOutCustomer: Marks a customer as checked out and updates the check-out time.
- seatsLeft: Calculates the number of available seats based on the customer status.

App.css
The App.css file contains the styling for the application. It defines the appearance of various elements, including the form, buttons, and table.

Colors:

- Primary Color: Bright Orange (#FFA500)
- Accent Color: Turquoise (#40E0D0)
- Neutral Color: White (#FFFFFF)

Styling:

- The body is styled to center the content and set the background color.
- The .App class is styled to create a visually appealing container for the content.
- Headers, labels, inputs, and buttons are styled to match the chosen color scheme.
- The table is styled to ensure readability and consistent appearance.

Future Improvements

- Add validation for phone number input to ensure valid phone numbers.
- Implement pagination for the customer list to handle a large number of customers.
- Add search and filter functionality to quickly find specific customers.
- Enhance the design with additional visual elements and animations.
- Integrate with a backend database to persist customer data.

License

This project is licensed under the MIT License. See the LICENSE file for details.
