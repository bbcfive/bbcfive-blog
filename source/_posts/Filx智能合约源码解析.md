---
title: Filx智能合约源码解析
date: 2021-05-07 10:06:08
tags: 
  - 智能合约
  - 以太坊
  - 数字货币
  - solidity
categories: 区块链
---

## 一、Token
token.sol
```
// 使用solidity0.5.0及以上版本
pragma solidity ^0.5.0;

contract FilxToken {
    string public name = "FILX Token"; //Optional
    string public symbol = "FILX"; //Optional
    uint256 public totalSupply;

    // 注册Transfer和Approval两个交易事件
    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(
        address indexed _owner,
        address indexed _spender,
        uint256 _value
    );

    // 创建balanceOf和allowance两个键值对
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    constructor(uint256 _initialSupply) public {
        // 令当前交易地址的余额等于_initialSupply
        balanceOf[msg.sender] = _initialSupply;
        totalSupply = _initialSupply;
    }

    // 创建转账函数
    function transfer(address _to, uint256 _value)
        public
        returns (bool success)
    {
        // 判断当前交易地址的余额是否大于等于_value，否则抛出错误"Not enough balance"
        require(balanceOf[msg.sender] >= _value, "Not enough balance");
        // 是则当前交易地址的余额减去_value
        balanceOf[msg.sender] -= _value;
        // 转出地址的余额加_value
        balanceOf[_to] += _value;
        // 触发Transfer事件
        emit Transfer(msg.sender, _to, _value);
        // 返回true
        return true;
    }

    // 创建准许函数
    function approve(address _spender, uint256 _value)
        public
        returns (bool success)
    {   
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    // 创建自定义转出账户的转账函数
    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public returns (bool success) {
        // 判断当前转出账户的余额是否大于等于_value
        require(
            balanceOf[_from] >= _value,
            "_from does not have enough tokens"
        );
        // 判断当前津贴转出账户的余额是否大于等于_value
        require(
            allowance[_from][msg.sender] >= _value,
            "Spender limit exceeded"
        );
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }
}
```

## 二、TokenSale
tokenSale.sol
```
pragma solidity ^0.5.0;

// 引入文件
import "./token.sol";

contract FILXTokenSale {
    address payable admin;
    FilxToken public tokenContract;

    constructor(FilxToken _tokenContract) public {
        admin = msg.sender;
        tokenContract = _tokenContract;
    }

    function buyTokens(uint256 _numberOfTokens) public payable{
        
        require(
            // 判断_numberOfTokens与msg.value匹配
            _numberOfTokens == msg.value / 10**14,
            "Number of tokens does not match with the value"
        );
        
        require(
            // 判断当前地址的余额大于等于_numberOfTokens
            tokenContract.balanceOf(address(this)) >= _numberOfTokens,
            "Contact does not have enough tokens"
        );
        require(
            // 购买token
            tokenContract.transfer(msg.sender, _numberOfTokens),
            "Some problem with token transfer"
        );
    }

    function endSale() public {
        require(msg.sender == admin, "Only the admin can call this function");
        require(
            tokenContract.transfer(
                address(0),
                tokenContract.balanceOf(address(this))
            ),
            "Unable to transfer tokens to 0x0000"
        );
        // destroy contract
        selfdestruct(admin);
    }
}
```

源码链接：https://filxcoin.com/#/sol