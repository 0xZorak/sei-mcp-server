# MCP EVM Server

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![EVM Networks](https://img.shields.io/badge/Networks-30+-green)
![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-3178C6)
![Viem](https://img.shields.io/badge/Viem-1.0+-green)

A comprehensive Model Context Protocol (MCP) server that provides blockchain operations across multiple EVM-compatible networks. This server enables AI agents to interact with Ethereum, Optimism, Arbitrum, Base, Polygon, and many other EVM chains with a unified interface.

## 📋 Contents

- [Overview](#overview)
- [Features](#features)
- [Supported Networks](#supported-networks)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Reference](#api-reference)
  - [Tools](#tools)
  - [Resources](#resources)
- [Security Considerations](#security-considerations)
- [Project Structure](#project-structure)
- [Development](#development)
- [License](#license)

## 🔭 Overview

The MCP EVM Server leverages the Model Context Protocol to provide blockchain operations to AI agents. It supports a wide range of operations including:

- Reading blockchain state (balances, transactions, blocks, etc.)
- Interacting with smart contracts
- Transferring tokens (native, ERC20, ERC721, ERC1155)
- Querying token metadata and balances
- Chain-specific operations across dozens of EVM networks

All operations are exposed through a consistent interface of MCP tools and resources, making it easy for AI agents to discover and use blockchain functionality.

## ✨ Features

### Blockchain Data Access

- **Multi-chain support** for 30+ EVM-compatible networks
- **Chain information** including blockNumber, chainId, and RPCs
- **Block data** access by number, hash, or latest
- **Transaction details** and receipts with decoded logs
- **Address balances** for native tokens and all token standards

### Token Operations

- **ERC20 Tokens**
  - Get token metadata (name, symbol, decimals, supply)
  - Check token balances
  - Transfer tokens between addresses
  - Approve spending allowances

- **NFTs (ERC721)**
  - Get collection and token metadata
  - Verify token ownership
  - Transfer NFTs between addresses
  - Retrieve token URIs and count holdings

- **Multi-tokens (ERC1155)**
  - Get token balances and metadata
  - Transfer tokens with quantity
  - Access token URIs

### Smart Contract Interactions

- **Read contract state** through view/pure functions
- **Write operations** with private key signing
- **Contract verification** to distinguish from EOAs
- **Event logs** retrieval and filtering

### Comprehensive Transaction Support

- **Native token transfers** across all supported networks
- **Gas estimation** for transaction planning
- **Transaction status** and receipt information
- **Error handling** with descriptive messages

## 🌐 Supported Networks

### Mainnets
- Ethereum (ETH)
- Optimism (OP)
- Arbitrum (ARB)
- Arbitrum Nova
- Base
- Polygon (MATIC)
- Polygon zkEVM
- Avalanche (AVAX)
- Binance Smart Chain (BSC)
- zkSync Era
- Linea
- Celo
- Gnosis (xDai)
- Fantom (FTM)
- Filecoin (FIL)
- Moonbeam
- Moonriver
- Cronos
- Scroll
- Mantle
- Manta
- Blast
- Fraxtal
- Mode
- Metis
- Kroma
- Zora
- Aurora
- Canto

### Testnets
- Sepolia
- Optimism Sepolia
- Arbitrum Sepolia
- Base Sepolia
- Polygon Amoy
- Avalanche Fuji
- BSC Testnet
- zkSync Sepolia
- Linea Sepolia
- Scroll Sepolia
- Mantle Sepolia
- Manta Sepolia
- Blast Sepolia
- Fraxtal Testnet
- Mode Testnet
- Metis Sepolia
- Kroma Sepolia
- Zora Sepolia
- Celo Alfajores
- Goerli
- Holesky

## 🛠️ Prerequisites

- [Bun](https://bun.sh/) 1.0.0 or higher
- Node.js 18.0.0 or higher (if not using Bun)

## 📦 Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/mcp-evm-server.git
cd mcp-evm-server

# Install dependencies with Bun
bun install

# Or with npm
npm install
```

## ⚙️ Configuration

The server uses public RPC endpoints by default for all supported networks. For production use, you can configure custom RPC providers by:

1. Create a `.env` file in the project root
2. Add custom RPC URLs for any networks you want to override:

```env
# Example .env configuration
ETHEREUM_RPC_URL=https://your-custom-ethereum-rpc.com
OPTIMISM_RPC_URL=https://your-custom-optimism-rpc.com
ARBITRUM_RPC_URL=https://your-custom-arbitrum-rpc.com
```

## 🚀 Usage

### Running the Server

Start the server using stdio (for embedding in CLI tools):

```bash
# Start the stdio server
bun start

# Development mode with auto-reload
bun dev
```

Or start the HTTP server with SSE for web applications:

```bash
# Start the HTTP server
bun start:http

# Development mode with auto-reload
bun dev:http
```

### Connecting to the Server

Connect to this MCP server using any MCP-compatible client. For testing and debugging, you can use the [MCP Inspector](https://github.com/modelcontextprotocol/inspector).

### Example: Getting a Token Balance

```javascript
// Example of using the MCP client to check a token balance
const mcp = new McpClient("http://localhost:3000");

const result = await mcp.invokeTool("get-token-balance", {
  tokenAddress: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48", // USDC on Ethereum
  ownerAddress: "0x1234567890abcdef1234567890abcdef12345678",
  network: "ethereum"
});

console.log(result);
// {
//   tokenAddress: "0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48",
//   owner: "0x1234567890abcdef1234567890abcdef12345678",
//   network: "ethereum",
//   raw: "1000000000",
//   formatted: "1000",
//   symbol: "USDC",
//   decimals: 6
// }
```

## 📚 API Reference

### Tools

The server provides the following MCP tools for agents:

#### Token Operations

| Tool Name | Description | Key Parameters |
|-----------|-------------|----------------|
| `get-token-info` | Get ERC20 token metadata | `tokenAddress`, `network` |
| `get-token-balance` | Check ERC20 token balance | `tokenAddress`, `ownerAddress`, `network` |
| `transfer-token` | Transfer ERC20 tokens | `privateKey`, `tokenAddress`, `toAddress`, `amount`, `network` |
| `approve-token-spending` | Approve token allowances | `privateKey`, `tokenAddress`, `spenderAddress`, `amount`, `network` |
| `get-nft-info` | Get NFT metadata | `tokenAddress`, `tokenId`, `network` |
| `check-nft-ownership` | Verify NFT ownership | `tokenAddress`, `tokenId`, `ownerAddress`, `network` |
| `transfer-nft` | Transfer an NFT | `privateKey`, `tokenAddress`, `tokenId`, `toAddress`, `network` |
| `get-nft-balance` | Count NFTs owned | `tokenAddress`, `ownerAddress`, `network` |
| `get-erc1155-token-uri` | Get ERC1155 metadata | `tokenAddress`, `tokenId`, `network` |
| `get-erc1155-balance` | Check ERC1155 balance | `tokenAddress`, `tokenId`, `ownerAddress`, `network` |
| `transfer-erc1155` | Transfer ERC1155 tokens | `privateKey`, `tokenAddress`, `tokenId`, `amount`, `toAddress`, `network` |

#### Blockchain Operations

| Tool Name | Description | Key Parameters |
|-----------|-------------|----------------|
| `get-chain-info` | Get network information | `network` |
| `get-balance` | Get native token balance | `address`, `network` |
| `transfer-eth` | Send native tokens | `privateKey`, `to`, `amount`, `network` |
| `get-transaction` | Get transaction details | `txHash`, `network` |
| `read-contract` | Read smart contract state | `contractAddress`, `abi`, `functionName`, `args`, `network` |
| `write-contract` | Write to smart contract | `contractAddress`, `abi`, `functionName`, `args`, `privateKey`, `network` |
| `is-contract` | Check if address is a contract | `address`, `network` |

### Resources

The server exposes blockchain data through the following MCP resource URIs:

#### Blockchain Resources

| Resource URI Pattern | Description |
|-----------|-------------|
| `evm://{network}/chain` | Chain information for a specific network |
| `evm://chain` | Ethereum mainnet chain information |
| `evm://{network}/block/{blockNumber}` | Block data by number |
| `evm://{network}/block/latest` | Latest block data |
| `evm://{network}/address/{address}/balance` | Native token balance |
| `evm://{network}/tx/{txHash}` | Transaction details |
| `evm://{network}/tx/{txHash}/receipt` | Transaction receipt with logs |

#### Token Resources

| Resource URI Pattern | Description |
|-----------|-------------|
| `evm://{network}/token/{tokenAddress}` | ERC20 token information |
| `evm://{network}/token/{tokenAddress}/balanceOf/{address}` | ERC20 token balance |
| `evm://{network}/nft/{tokenAddress}/{tokenId}` | NFT (ERC721) token information |
| `evm://{network}/nft/{tokenAddress}/{tokenId}/isOwnedBy/{address}` | NFT ownership verification |
| `evm://{network}/erc1155/{tokenAddress}/{tokenId}/uri` | ERC1155 token URI |
| `evm://{network}/erc1155/{tokenAddress}/{tokenId}/balanceOf/{address}` | ERC1155 token balance |

## 🔒 Security Considerations

- **Private keys** are used only for transaction signing and are never stored by the server
- Consider implementing additional authentication mechanisms for production use
- Use HTTPS for the HTTP server in production environments
- Implement rate limiting to prevent abuse
- For high-value operations, consider adding confirmation steps

## 📁 Project Structure

```
mcp-evm-server/
├── src/
│   ├── index.ts                # Main stdio server entry point
│   ├── http-server.ts          # HTTP server with SSE
│   ├── core/
│   │   ├── chains.ts           # Chain definitions and utilities
│   │   ├── resources.ts        # MCP resources implementation
│   │   ├── tools.ts            # MCP tools implementation
│   │   ├── prompts.ts          # MCP prompts implementation
│   │   └── operations/         # Core blockchain operations
│   │       ├── index.ts        # Operation exports
│   │       ├── balance.ts      # Balance operations
│   │       ├── blocks.ts       # Block operations
│   │       ├── clients.ts      # RPC client utilities
│   │       ├── contracts.ts    # Contract interactions
│   │       ├── tokens.ts       # Token metadata operations
│   │       ├── transfer.ts     # Token transfer operations
│   │       ├── transactions.ts # Transaction operations
│   │       └── utils.ts        # Utility functions
├── package.json
├── tsconfig.json
└── README.md
```

## 🛠️ Development

To modify or extend the server:

1. Add new operations in the appropriate file under `src/core/operations/`
2. Register new tools in `src/core/tools.ts`
3. Register new resources in `src/core/resources.ts`
4. Add new network support in `src/core/chains.ts`

## 📄 License

MIT
