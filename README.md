// SPDX-License-Identifier: MIT
pragma solidity ^0.8.25;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Module3 is ERC20{

    address public OwnerWalletAddress;

    constructor() ERC20("ChanToken", "CTN"){
        OwnerWalletAddress = msg.sender;
    }

    mapping (address Wallet => uint CTN) public CheckBalance;

    function CTNMinting(address WalletAddress, uint256 CTN) public {
        require(msg.sender == OwnerWalletAddress, "unable to mint");
        _mint(WalletAddress, CTN);
        CheckBalance[WalletAddress] += CTN;
    }

    function GTNBurning(address WalletAddress, uint256 CTN) public{
        if(CheckBalance[WalletAddress] >= CTN){
            _burn(WalletAddress, CTN);
            CheckBalance[WalletAddress] -= CTN;
        }else{
            revert("unavailable amount to burn");
        }
    }

    function CTNTransferring(address spender, address receiver, uint256 CTN)public{
        if(CheckBalance[spender] >= CTN){
            _approve(spender, receiver, CTN);
            _transfer(spender, receiver, CTN);

            CheckBalance[spender] -= CTN;
            CheckBalance[receiver] += CTN;
        }else{
            revert("unavailable amount to transfer");
        }
    }
}
