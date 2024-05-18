# Shared Wallet Smart Contract

This project demonstrates how to create a shared wallet on the Ethereum blockchain using Solidity and interact with it through a web interface.

## Functionality

* The wallet holds funds in ETH.
* An admin deploys the smart contract and funds it.
* The admin authorizes specific users to spend a certain amount of ETH within a time limit.
* Users can spend ETH up to their allowance and within the time limit.

### Optional Features

* A basic HTML and Javascript frontend for user interaction.
* Integration with the `ethers.js` library to interact with the smart contract through Javascript.

### Learning Objectives

* Develop and deploy a smart contract for a shared wallet.
* Create a basic frontend to interact with the smart contract.
* Integrate a frontend with a smart contract using `ethers.js`.

## Smart Contract

The smart contract is written in Solidity and uses the following functions:

* `receive()`: Allows the admin to fund the wallet (only callable by the owner).
* `getWalletBalance()`: Returns the current wallet balance.
* `renewAllowance(address _user, uint _allowance, uint _timeLimit)`: Admin renews the allowance for a user (specifies user address, allowance amount, and time limit).
* `myAllowance()`: Returns the user's pending allowance.
* `spendCoins(address payable _receiver, uint _amount)`: User spends coins (specifies receiver address and amount).

### Frontend 

The optional frontend features a basic HTML page with Javascript functionalities to:

* Connect the user's wallet (using MetaMask).
* Display the connected wallet address, pending allowance, and wallet balance.
* Allow the admin to renew a user's allowance (entering user address and allowance amount).
* Allow approved users to spend ETH from their allowance (entering receiver address and amount).

### Running the Project

1. **Deploy the Smart Contract:**
    * Use Remix or a similar tool to deploy the smart contract.
    * Note down the deployed contract address.

2. **Frontend (Optional):**
    * Create HTML and Javascript files for the frontend.
    * Replace the placeholder contract address with the deployed address.
    * Run a local server to view the frontend in your browser.

### Interaction

1. Connect your MetaMask wallet to the frontend.
2. **Admin:**
    * Use the frontend to renew user allowances by entering their address and allowance amount.
3. **Approved Users:**
    * View their pending allowance on the frontend.
    * Use the frontend to spend ETH from their allowance by specifying a receiver address and amount.

## Security Considerations

This is a basic example for learning purposes. Real-world implementations should consider security best practices for smart contracts and blockchain applications.

### Further Exploration

* Explore advanced functionalities like adding multiple admins or implementing more complex allowance rules.
* Enhance the frontend with a user-friendly interface and error handling.
* Learn about smart contract security best practices for production environments.
