# ModifiedErc20 Token

The `ModifiedErc20` contract is an implementation of the ERC20 standard token with additional functionalities. This token is based on the OpenZeppelin ERC20 contract and adds administrative capabilities for minting tokens, as well as a public function for burning tokens.

## Features

- **Minting**: Only the admin can mint new tokens.
- **Burning**: Any token holder can burn their own tokens.
- **Token Transfer**: Standard ERC20 token transfer functionality.

## Prerequisites

To work with this contract, you'll need:

- Solidity ^0.8.13
- OpenZeppelin Contracts library

## Installation

1. **Install OpenZeppelin Contracts**:
   ```sh
   npm install @openzeppelin/contracts
   ```

2. **Clone this repository**:
   ```sh
   git clone <repository-url>
   ```

## Usage

### Deployment

To deploy the `ModifiedErc20` contract, you need to specify the initial supply of tokens. The deployer will be set as the admin.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract ModifiedErc20 is ERC20 {
    address public admin;

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this function");
        _;
    }

    constructor(uint256 initialTokens) ERC20("RagToken", "RTK") {
        admin = msg.sender;
        _mint(admin, initialTokens * 10 ** uint256(decimals()));
    }

    function mint(address recipient, uint256 tokenAmount) public onlyAdmin {
        _mint(recipient, tokenAmount * 10 ** uint256(decimals()));
    }

    function burn(uint256 tokenAmount) public {
        _burn(msg.sender, tokenAmount * 10 ** uint256(decimals()));
    }

    function sendTokens(address recipient, uint256 tokenAmount) public returns (bool) {
        return transfer(recipient, tokenAmount * 10 ** uint256(decimals()));
    }
}
```

### Functions

1. **Minting Tokens**
   ```solidity
   function mint(address recipient, uint256 tokenAmount) public onlyAdmin
   ```
   - `recipient`: Address to receive the newly minted tokens.
   - `tokenAmount`: Amount of tokens to mint.

   Only the admin can call this function to mint new tokens.

2. **Burning Tokens**
   ```solidity
   function burn(uint256 tokenAmount) public
   ```
   - `tokenAmount`: Amount of tokens to burn.

   Any token holder can burn their own tokens using this function.

3. **Sending Tokens**
   ```solidity
   function sendTokens(address recipient, uint256 tokenAmount) public returns (bool)
   ```
   - `recipient`: Address to receive the tokens.
   - `tokenAmount`: Amount of tokens to send.

   This function allows users to transfer tokens to another address.

### Example

Here's an example of how to interact with the `ModifiedErc20` contract after deployment:

```js
const ModifiedErc20 = artifacts.require("ModifiedErc20");

module.exports = async function(deployer) {
  const initialSupply = 1000;
  await deployer.deploy(ModifiedErc20, initialSupply);

  const instance = await ModifiedErc20.deployed();
  console.log("Contract deployed at address:", instance.address);

  // Mint new tokens
  await instance.mint(recipientAddress, 500, { from: adminAddress });

  // Burn tokens
  await instance.burn(200, { from: tokenHolderAddress });

  // Send tokens
  await instance.sendTokens(recipientAddress, 100, { from: tokenHolderAddress });
};
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- OpenZeppelin for providing secure and community-vetted contracts.
