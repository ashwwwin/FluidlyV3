# Liquidify V3

Liquidify V3 is a decentralized protocol for tokenizing NFTs into tradable ERC20 tokens, allowing for fractional ownership, improved liquidity, and trading opportunities.

## Overview

Liquidify V3 allows NFT owners to wrap their NFTs (ERC721 or ERC1155) into ERC20 tokens, creating a liquid market for otherwise relatively illiquid assets. This enables:

- **Fractional Ownership** - Split NFT value across multiple owners
- **Improved Liquidity** - Trade fractional NFT values without selling the entire asset
- **Market Access** - Lower entry barriers for valuable NFT collections
- **Price Discovery** - Establish fair market values through trading activity

## Key Components

The protocol consists of several key components:

### LiquidifyV3Factory

The central contract that deploys and manages Liquidify pairs. The factory:

- Creates new Liquid ERC721 and ERC1155 token contracts
- Manages protocol-wide parameters and fees
- Maintains registry of created contracts

### LiquidERC721

Handles the wrapping and unwrapping of ERC721 NFTs:

- Locks NFTs in the contract and mints ERC20 tokens
- Allows token redemption for the underlying NFTs
- Supports tiered token issuance based on NFT rarity
- Handles protocol and creator fees

### LiquidERC1155

Similar to LiquidERC721 but for ERC1155 tokens:

- Supports multi-token standards
- Manages quantities of fungible tokens
- Integrates with the same fee and Uniswap structures

### LiquidifyStandard

Base contract that implements:

- ERC20 token standards
- Fee distribution mechanics
- Uniswap pair integration

## Key Features

- **Tiered Tokenization**: Configure different token amounts for NFTs based on rarity or traits
- **Creator Fees**: NFT creators receive a percentage of trades (up to 5%)
- **Protocol Fees**: Small fees support protocol development and maintenance
- **Uniswap Integration**: Automatic liquidity pool creation
- **Storage Fees**: Small fees for wrapping/unwrapping to prevent spam

## Technical Integration

Liquidify V3 integrates with:

- Uniswap V2 for automated liquidity
- OpenZeppelin contracts for secure implementation
- ERC721 and ERC1155 NFT standards
- ERC20 token standard

## Usage Examples

### Creating an ERC721 Liquidify Pair

```solidity
// Deploy a Liquidify pair for an ERC721 collection
address liquidPair = liquidifyFactory.createLiquidERC721(
    "Wrapped CryptoPunks",  // token name
    "WPUNK",               // token symbol
    0xb47e3cd837dDF8e4c57F05d70Ab865de6e193BBB, // NFT contract address
    100,                   // tokens per NFT (100 tokens per punk)
    300                    // creator fee (3%)
);
```

### Wrapping NFTs

```solidity
// Approve the Liquidify contract to transfer your NFT
IERC721(nftContract).approve(liquidPairAddress, tokenId);

// Wrap your NFT (receive tokens)
uint256[] memory tokenIds = new uint256[](1);
tokenIds[0] = 1234; // Your NFT ID
LiquidERC721(liquidPairAddress).wrapERC721{value: storageFee}(tokenIds);
```

### Unwrapping NFTs

```solidity
// Approve tokens to be burned
IERC20(liquidPairAddress).approve(liquidPairAddress, tokensPerNft);

// Unwrap to receive an NFT
LiquidERC721(liquidPairAddress).unwrapERC721{value: storageFee}(tokensPerNft);
```

## Fee Structure

- **Creator Sell Fee**: 0-5% (set at deployment)
- **Protocol Fee**: 0.25% on sales
- **Storage Fee**: Small ETH fee for wrap/unwrap operations

## Security Considerations

The protocol implements multiple security features:

- Reentrancy guards
- Owner controls with renouncement options
- Fee limitations
- Safe transfer mechanisms

## License

This project is licensed under the MIT License - see the [License](./License) file for details.

## OP Retro Funding

This project is a part of the OP Retro funding program with project ID: `0xb0c8fe8e359a91bc2fa4b7498e2c7f6a50f730ee1dc821bee016a4b7feb7639e`
