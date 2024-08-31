# IERC20 Token

//SPDX-License-Identifier :MIT

pragma solidity ^0.8.20;

interface IERC20 {
   
    event Transfer(address indexed from, address indexed to, uint256 value);

    
    event Approval(address indexed owner, address indexed spender, uint256 value);

    
    function totalSupply() external view returns (uint256);

   
    function balanceOf(address account) external view returns (uint256);

    
    function transfer(address to, uint256 value) external returns (bool);

    
    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract codeEater is IERC20{
   
 address public founder;
 uint public totalSupply=1000;
 uint public decimals=0;

  constructor (){
   founder=msg.sender;
   balanceOfUser[founder]=totalSupply;
  }
   
  mapping (address=>uint) public balanceOfUser;
  mapping (address=>mapping(address=>uint)) public allowedToken;
  
  /*function totalSupply() external view returns (uint256){
    return totalSupply;
  }*/

  function balanceOf(address account) external view returns (uint256){
   return  balanceOfUser[account];
  }

  function transfer(address to, uint256 value) external  returns (bool){
     require(to!=address(0)," Invalid Addresss");
    require(balanceOfUser[msg.sender]>=value, "Insufficent Balance");
    balanceOfUser[msg.sender]-=value;
     balanceOfUser[to] +=value;
     emit Transfer(msg.sender, to, value);
     return true;
    
  }

  
  function approve(address spender, uint256 value) external returns (bool){
    require(spender!=address(0)," Invalid Address");
    require(balanceOfUser[msg.sender]>=value, "Insufficient Token");
    allowedToken[msg.sender][spender]=value;
    emit Approval( msg.sender, spender,  value);
    return true;
   

  }

  function allowance(address owner, address spender) external view returns (uint256){
    return allowedToken[owner][spender];
  }

   function transferFrom(address from, address to, uint256 value) external returns (bool){
    require(to==msg.sender,"Not Authorized");
    require(from!=address(0), "Invalid Address");
    require(to!=address(0)," Invalid Address");
    require(allowedToken[from][to]>=value, "Insufficient Token");
    allowedToken[from][to]-=value;
    balanceOfUser[from]-=value;
    balanceOfUser[to]+=value;
    //allowedToken[from][to]=value;
    emit Transfer( from, to,  value);
    return true;
    

   }

}
