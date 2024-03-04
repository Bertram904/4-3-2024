# 4-3-2024
~~~solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.3;

contract UserManager{
    address private Adminstrator;
    address private Manager;
    constructor(address admin) {
        Adminstrator = admin;
        UserRoles[admin] = 1;
    }
    mapping (address => uint) UserRoles;
    mapping (address => uint) Balances;
    modifier checkAdmin{
        require(msg.sender==Adminstrator, "You can not solve!");
        _;
    }
    modifier checkManager{
        require(msg.sender==Manager, "You can not solve!");
        _;
    }
    //Admin
    function adding_roles(address _user, uint roles) public checkAdmin{
        /*
            1: Admin
            2: Manager
            3: User
        */
        if(roles==1) {
            Adminstrator = _user;
            UserRoles[_user] = 1;
        } else if(roles==2) {
            Manager = _user;
            UserRoles[_user] = 2;
        } else {
            UserRoles[_user] = 3;
        }
    }
    // All
    function checking_roles(address _user) public view returns(string memory) {
        if(UserRoles[_user]==1) {
            return "Admin";
        } else if(UserRoles[_user]==2) {
            return "Manager";
        } else if(UserRoles[_user]==3) {
            return "User";
        } else {
            return "Can not find data";
        }
    }
}

contract Bank {
    mapping (address => uint) Balance_Bank;
    modifier checkValue(uint value) {
        require(value <= Balance_Bank[msg.sender], "You not enough money!");
        _;
    }
}

contract Financial_Operations is UserManager(msg.sender), Bank {
    function deposit(uint value) public {
        Balance_Bank[msg.sender] += value; 
        Balances[msg.sender] -= value;       
    }

    function withDraw(uint value) public checkValue(value) {
        Balance_Bank[msg.sender] -= value;
        Balances[msg.sender] += value;
    }
}
