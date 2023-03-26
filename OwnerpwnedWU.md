# Davinci ctf 2023 : ownerpwned[337pts]

challange was worth 337pts with 33 solves,And we was top 3 for first solves


**writeup**

This challange focuses on the implementation constructors from older versions of solidity.The objective of this challenge is to become the smart contract's owner`me` by calling the "constructor"`initwallet` and eventually calling functions that could be called by only owner of the contract.
see another example of this type of problem:https://ethernaut.openzeppelin.com/level/2

**code review**

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.7.6;



contract Challenge1 {

    address public me;
    mapping(address => uint256) balances;

    //constructor
    
    function initWallet() public {
        me = msg.sender;
    }

    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    function withdraw(uint256 amount) public {
        require(amount <= balances[msg.sender]);
        payable(msg.sender).transfer(amount);
        balances[msg.sender] -= amount;
    }
     //If there is an emergency, i'm protected \o/
    function migrateTo(address to) public {
        require(msg.sender == me, "Only me can withdraw all the funds");
        payable(to).transfer(address(this).balance);
    }
     //getBalance returns the balance of the contract, it is always nice to check my fortune
    function getBalance() public view returns (uint)
    {
        return (address(this).balance);
    }

```
**implementation**

using remix we deploy the contract at the address of the challange
* call initwallet()
* call migrateTo() with our wallet address as input
* and finally call the withdraw function with a uint for amount . Thats it.we get the flag and some eth(DVC tokens) to follow .ez.
