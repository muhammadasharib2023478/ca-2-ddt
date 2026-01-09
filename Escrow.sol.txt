// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract LegalEscrow {

    address public client;
    address public solicitor;
    uint public escrowAmount;
    bool public isCompleted;

    constructor(address _solicitor) payable {
        client = msg.sender;
        solicitor = _solicitor;
        escrowAmount = msg.value;
        isCompleted = false;
    }

    function confirmCompletion() public {
        require(msg.sender == client, "Only client can confirm completion");
        require(!isCompleted, "Already completed");

        isCompleted = true;
        payable(solicitor).transfer(escrowAmount);
    }

    function refundClient() public {
        require(msg.sender == solicitor, "Only solicitor can refund");
        require(!isCompleted, "Already completed");

        payable(client).transfer(escrowAmount);
    }
}
