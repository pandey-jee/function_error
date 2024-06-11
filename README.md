# ETH-AVAX PROOF- Intermediate- EVM - Course
Hello everyone this is the overview of Solidity Functions and Errors
# Description 
This Solidity smart contract for Ethereum offers basic functionalities such as withdrawal, deposit, and balance checking. It illustrates error handling and validation through the use of require(), revert(), and assert() statements.
# These are the requirements and usage of it 
To engage with the smart contract, you require:

1.A development setup for Ethereum smart contracts.
2.An Ethereum wallet or client like MetaMask for compiling, deploying, and interacting with the smart contract.
# Smart Contract Details 
The FunctionalityExample smart contract, written in Solidity, includes key functions demonstrating the use of require(), assert(), and revert() statements. It features a public state variable value that stores an unsigned integer. The setValue(uint _value) function sets this variable, using require(_value > 0, "Value must be greater than zero") to ensure the input is greater than zero, reverting the transaction with an error message if the condition fails. The checkValue() function showcases assert(value != 0) to verify that value is not zero, which would indicate a critical error if false, thereby reverting the transaction and consuming all remaining gas. Lastly, the revertExample() function uses revert("Value is 1, operation not allowed") to deliberately cause a transaction failure if value equals 1, providing a specific error message. This contract effectively illustrates input validation, internal consistency checks, and error handling through these essential Solidity statements.
# Executing program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.
The code: 
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Donation {

    uint256 public minimumDonation;
    uint256 public totalDonations;
    address public owner;
    mapping(address => uint256) public donations;

    event DonationReceived(address indexed donor, uint256 amount);
    event MinimumDonationChanged(uint256 newMinimumDonation);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    constructor(uint256 _minimumDonation) {
        require(_minimumDonation > 0, "Minimum donation must be greater than zero");
        minimumDonation = _minimumDonation;
        owner = msg.sender;
    }

    function setMinimumDonation(uint256 _minimumDonation) public onlyOwner {
        require(_minimumDonation > 0, "Minimum donation must be greater than zero");
        minimumDonation = _minimumDonation;
        emit MinimumDonationChanged(_minimumDonation);
    }

    function donate() public payable {
        require(msg.value >= minimumDonation, "Donation must be greater than or equal to the minimum donation");
        require(msg.value != 1, "Donation of 1 wei is not allowed");

        donations[msg.sender] += msg.value;
        totalDonations += msg.value;
        emit DonationReceived(msg.sender, msg.value);
    }

    function checkTotalDonations() public view returns (uint256) {
        require(totalDonations != 0, "Total donations must not be zero");
        return totalDonations;
    }

    function withdrawFunds() public onlyOwner {
        uint256 amount = address(this).balance;
        require(amount > 0, "No funds available for withdrawal");
        payable(owner).transfer(amount);
    }
}
