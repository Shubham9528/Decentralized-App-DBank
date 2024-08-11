### 🏦 DBank: A Decentralized Banking System

DBank is a decentralized banking application built using the Internet Computer by the Dfinity Foundation. This project allows users to top up their balance, withdraw funds, and automatically compound their interest over time.

![difinity logo](https://github.com/user-attachments/assets/0bfac58b-8fa9-497a-a796-c187407d15ee)


## 🚀 Project Overview

The project consists of a frontend built with HTML, CSS, and JavaScript, and a backend that runs on the Internet Computer using the Motoko programming language. The frontend interacts with the backend to perform various banking operations like topping up, withdrawing, and compounding interest.


## 🌐 Internet Computer

The Internet Computer, developed by the Dfinity Foundation, is a decentralized computing platform that enables the development and deployment of dApps (decentralized applications) with web-like speed and scalability. DBank leverages this platform to provide a seamless and secure banking experience.



## 🏗️ Project Structure
Frontend

 The frontend consists of an HTML page with a simple form to top up and withdraw amounts, and JavaScript code that interacts with the backend.
 
![dbank](https://github.com/user-attachments/assets/7bea0d2c-011f-4f3b-a428-e59e69b0e609)


## ✨JavaScript Intermediator
The JavaScript code is responsible for handling user input, disabling the form during transactions, and updating the balance display.
Sudo
```
import { dbank } from "../../declarations/dbank"

window.addEventListener("load", async function() {
  update();
});

document.querySelector("form").addEventListener("submit", async function(event) {
  event.preventDefault();

  const button = event.target.querySelector("#submit-btn");
  const button2 = event.target.querySelector("#withdrawal-amount");

  const topUp = parseFloat(document.getElementById("input-amount").value);
  const withdraw = parseFloat(document.getElementById("withdrawal-amount").value);

  button.setAttribute("disabled", true);
  button2.setAttribute("disabled", true);

  if (topUp) {
    await dbank.topUp(topUp);
  }

  if (withdraw) {
    await dbank.withdraw(withdraw);
  }

  await dbank.compound();

  update();

  document.getElementById("input-amount").value = "";
  document.getElementById("withdrawal-amount").value = "";
  button.removeAttribute("disabled");
  button2.removeAttribute("disabled");
});

async function update() {
  const currAmount = await dbank.checkBalance();
  document.getElementById("value").innerText = Math.round(currAmount * 100) / 100;
}

```

## 🛠️ Backend (Motoko)

The backend is written in Motoko and runs on the Internet Computer. It handles the core banking logic, including topping up, withdrawing, and compounding interest.

![voter regitration](https://github.com/user-attachments/assets/f7d2bd35-bc2e-40c9-8632-8b514fdecd33)
Sudo
```
import Debug "mo:base/Debug";
import Time "mo:base/Time";
import Float "mo:base/Float";

actor DBank {
  stable var currValue: Float = 500;
  stable var startTime = Time.now();

  public func topUp(amount: Float) {
    currValue += amount;
    Debug.print(debug_show(currValue));
  };

  public func withdraw(amount: Float) {
    let tempValue = currValue - amount;
    if (tempValue >= 0) {
      currValue -= amount;
    } else {
      Debug.print("Amount too large, currentValue less than zero.");
    }
  };

  public query func checkBalance(): async Float {
    return currValue;
  };

  public func compound() {
    let currentTime = Time.now();
    let timeElapsedNS = currentTime - startTime;
    let timeElapsedS = timeElapsedNS / 1000000000;
    currValue := currValue * (1.01 ** Float.fromInt(timeElapsedS));
    startTime := currentTime;
  };
}

```

## 🔍 Pseudocode Explanation
Top-Up Functionality
- 1.Input: User enters an amount to top up.
- 2.Process: The backend adds the amount to the current balance.
- 3.Output: The new balance is displayed on the frontend.

Withdrawal Functionality
- 1.Input: User enters an amount to withdraw.
- 2.Process: The backend checks if the balance is sufficient.
-- If yes, it subtracts the amount from the current balance.
-- If no, it displays an error message.
-- 3.Output: The new balance is displayed on the frontend.

Compound Interest Calculation
- 1.Input: Triggered automatically after a transaction.
- 2.Process: The backend calculates the time elapsed since the last transaction and compounds the interest based on this duration.
- 3.Output: The updated balance is displayed on the frontend.
## 📚 Additional Resources

To learn more about the Internet Computer and the Dfinity Foundation, check out the following resources:
- https://dfinity.org/

Sudo
```
<!-- HTML Structure for Voter Registration -->
<form action="/register" method="POST">
    <div class="form">
        <!-- Input for First and Last Name -->
        <label>Firstname:</label>
        <input type="text" name="fname" required>
        
        <label>Lastname:</label>
        <input type="text" name="lname" required>

        <!-- Dropdown for ID Proof Selection -->
        <label>Choose ID Proof:</label>
        <select name="idname">
            <option value="Aadhar">Aadhar</option>
            <option value="Pan Card">Pan Card</option>
            <!-- Other Options -->
        </select>

        <!-- Input for ID Number -->
        <label>ID No:</label>
        <input type="text" name="idnum" required>

        <!-- Input for Institute ID, DOB, Gender, Phone, and Address -->
        <label>Institute Id No:</label>
        <input type="text" name="instidnum" required>
        <!-- Other Inputs -->

        <!-- Registration Button -->
        <button name="register">Register</button>
    </div>
</form>

```
## 🗳️ Voting System Module

Candidate Selection: Voters can view and select a candidate to vote for.

![voting page](https://github.com/user-attachments/assets/d1677126-0216-45a0-a6ec-e3b6cb3f95a3)

## ✨Key Features
- Voter Registration: Collects essential voter details for registration.
- Vote Submission: Once a candidate is selected, voters can submit their vote.

``` Code
<!-- HTML Structure for Voting -->
<form id="candidate" class="container" action="/voteCount" method="POST">
    <table class="table text-center">
        <thead>
            <!-- Display Candidates and Their Parties -->
            <tr>
                <th>Name</th>
                <th>Party</th>
            </tr>
            <!-- Candidate List Generated Dynamically -->
            <% votingData.forEach(data => { %>
                <tr>
                    <td><input type="radio" name="candidate" value="<%= data.candidate_name %>">
                        <label><%= data.candidate_name %></label></td>
                    <td><%= data.party_name %></td>
                </tr>
            <% }) %>
        </thead>
    </table>
    <!-- Vote Button -->
    <button id="voteButton" onclick="App.vote()">Vote</button>
</form>

```

## 🛠️ Backend Server

The backend server handles all the database operations, API routes, and logic to support the online voting system. It is built using Node.js, Express.js, and PostgreSQL.

![database](https://github.com/user-attachments/assets/c1805c78-3587-4459-af6a-0e62adaf98d3)



## ✨Key Features
- Voter Registration: Stores voter details in the database.
- Admin Login: Authenticates admin users.
- Candidate Management: Adds candidates and manages their information.
- Voting Logic: Handles the voting process and vote counting.

``` Sudo
import express from "express";
import bodyParser from "body-parser";
import uniqueId from "generate-unique-id";
import pg from "pg";

const app = express();
const port = 3000;
app.use(express.static("public"));
app.use(bodyParser.urlencoded({ extended: true }));

// PostgreSQL Database Connection
const db = new pg.Client({
    user: "postgres",
    host: "localhost",
    database: "voting system",
    password: "Shubham@123",
    port: 5432,
});
db.connect();

// Register Functionality
async function register(FirstName, LastName, idNumber, instituteId, dob, gender, phone, address, votecount) {
    try {
        const voter_id = uniqueId({ length: 11, useLetters: false });
        await db.query("INSERT INTO register VALUES($1,$2,...)", [FirstName, LastName, idNumber, ...]);
    } catch (err) {
        console.log(err);
    }
}

// Admin Login Functionality
async function adminLogin(email, password) {
    try {
        const result = await db.query("SELECT * FROM adminLogin WHERE email = $1 AND password = $2", [email, password]);
        return result.rows.length > 0;
    } catch (err) {
        console.log(err);
    }
}

// Add Candidate Functionality
async function addCandidate(name, party, stdate, endate) {
    try {
        await db.query("INSERT INTO addCandidate VALUES($1, $2, $3, $4)", [name, party, stdate, endate]);
        await db.query("INSERT INTO votecount VALUES($1, $2)", [name, party]);
    } catch (err) {
        console.log(err);
    }
}

// Server Initialization
app.listen(port, () => {
    console.log(`App listening on http://localhost:${port}`);
});


```


## 🏗️ How to Set Up the Project

## 1. Clone the Repository

```bash
git clone https://github.com/your-repo/online-voting-system.git
```
## 2.  Install Dependencies
Navigate to the project directory and install the required dependencies:
```bash
npm install

```
## 3. Set Up PostgreSQL Database
Create a new PostgreSQL database named voting system:
Update the database credentials in the backend server script.
```bash
const db = new pg.Client({
    user: "your-username",
    host: "localhost",
    database: "voting system",
    password: "your-password",
    port: 5432,
});


```
## 4.Run the Application
bash
```
npm start
```

## 5. Access the Application
Open your browser and navigate to:
```bash
http://localhost:3000
```





## 🤝 Contributing

Contributions are welcome! If you have any ideas or suggestions to improve the project, feel free to open an issue or submit a pull request.

## 📄 License

This project is licensed under the -- License. See the LICENSE file for more details.

## 🙏 Acknowledgements

Thanks to the bootstrap teams for their amazing libraries and tools.

