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


## 🤝 Contributing

Contributions are welcome! If you have any ideas or suggestions to improve the project, feel free to open an issue or submit a pull request.

## 📄 License

This project is licensed under the -- License. See the LICENSE file for more details.

## 🙏 Acknowledgements

Thanks to the Difinity Foundation teams for their amazing libraries and tools.

