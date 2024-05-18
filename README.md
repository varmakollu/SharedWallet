# Shared Wallet Smart Contract

#### Project Overview

This project demonstrates the creation and deployment of a shared wallet smart contract on the Ethereum blockchain. The contract facilitates:

- Admin deployment to act as the shared wallet.
- Admin funding with ETH to establish the wallet's total balance.
- Admin authorization of specific users to spend a designated amount of ETH within a time limit.
- Users spending their allocated ETH, adhering to the admin-set limits.

#### Key Learnings

- Crafting and deploying a smart contract as the shared wallet.
- Building a basic HTML and JavaScript front-end for user interaction with the contract (optional).
- Integrating the front-end with the smart contract using the ethers.js library (optional).
- Creating ethers.js scripts for smart contract function interaction via terminal commands (optional).

**Optional Sections (Frontend and Ethers.js Integration) are outlined for those seeking a complete solution.**

## Smart Contract Development**

### Prerequisites**

- Basic understanding of Solidity, the Ethereum smart contract programming language.
- Familiarity with Remix, an online IDE for Ethereum development.

### Smart Contract Functions**

**1. `receive()`:**

   - Purpose: Accepts Ether funds into the shared wallet contract.
   - Implementation:
     ```solidity
     fallback() external payable {}
     ```

**2. `addAllowance(address user, uint amount, uint deadline)` (Admin-only):**

   - Purpose: Grants an allowance to a specified user, allowing them to spend up to a certain amount within a time frame.
   - Parameters:
     - `user`: Address of the authorized user.
     - `amount`: Maximum ETH the user can spend.
     - `deadline`: Unix timestamp representing the spending deadline.
   - Implementation:
     ```solidity
     mapping(address => UserAllowance) public allowances;

     struct UserAllowance {
         uint amount;
         uint deadline;
     }

     function addAllowance(address user, uint amount, uint deadline) public onlyOwner {
         require(deadline > block.timestamp, "Deadline must be in the future");
         allowances[user] = UserAllowance(amount, deadline);
     }
     ```

**3. `spend(uint amount)`:**

   - Purpose: Enables a user to spend their allocated ETH from the wallet.
   - Implementation:
     ```solidity
     function spend(uint amount) public {
         UserAllowance storage allowance = allowances[msg.sender];
         require(allowance.amount >= amount, "Insufficient allowance");
         require(allowance.deadline > block.timestamp, "Allowance expired");

         allowance.amount -= amount;
         payable(msg.sender).transfer(amount); // Transfer ETH to the user
     }
     ```

**4. `checkAllowance()`:**

   - Purpose: Allows a user to retrieve their remaining allowance and spending deadline.
   - Implementation:
     ```solidity
     function checkAllowance() public view returns (uint, uint) {
         UserAllowance storage allowance = allowances[msg.sender];
         return (allowance.amount, allowance.deadline);
     }
     ```

**5. `getBalance()`:**

   - Purpose: Returns the current balance of the shared wallet contract.
   - Implementation:
     ```solidity
     function getBalance() public view returns (uint) {
         return address(this).balance;
     }
     ```

#### Optional Frontend Development (HTML, JavaScript, and ethers.js)

- Create a basic HTML page with input fields for user interaction.
- Use JavaScript and ethers.js to connect to the deployed smart contract and call its functions.
- Consider incorporating user interface elements and error handling for a more user-friendly experience.

#### Example Frontend Interaction (using ethers.js):

```javascript
async function checkAllowance() {
  const provider = new ethers.providers.JsonRpcProvider("..."); // Replace with your provider URL
  const contract = new ethers.Contract(contractAddress, abi, signer); // Replace with contract address and ABI

  try {
    const allowance = await contract.checkAllowance();
    console.log("Remaining Allowance:", allowance[0].toString());
    console.log("Spending Deadline:", new Date(allowance[1].toNumber() * 1000));
  } catch (error) {
    console.error("Error:", error);
  }
}
```

### Deployment and Testing

1. Deploy the smart contract to a test network (e.g., Rinkeby) using Remix.
2. Thoroughly test the contract
