# Andrometa NFT Marketplace

![Andrometa Logo](/assets/images/logo.png)

Andrometa is a next-generation NFT marketplace built on Ethereum, enabling users to browse, buy, sell, and bid on digital assets. The platform provides a seamless experience for NFT collectors and creators alike.

![Andrometa Marketplace](/assets/images/preview.png)

## 🚀 Features

- **NFT Browsing & Discovery**: Explore NFTs with advanced filtering and sorting options
- **Creator Profiles**: Dedicated pages for creators with customizable themes
- **Collection Management**: View collections and individual assets with detailed metadata
- **Trading System**: Full support for listings, bids, purchases and cancellations
- **Wallet Integration**: Connect with Web3Modal for seamless wallet connectivity
- **Responsive Design**: Mobile-friendly interface that works across all devices
- **Dark/Light Mode**: Toggle between themes for optimal viewing experience

## 🔧 Tech Stack

- **Frontend**: Next.js 13 (App Router), React 18, TypeScript
- **Styling**: Tailwind CSS
- **Web3**: Wagmi, ethers.js, Web3Modal
- **Authentication**: Signature-based authentication

## 🛠️ Setup & Installation

### Prerequisites

- Node.js 16+ 
- npm or yarn
- MetaMask or other Web3 wallet

### Installation

1. Clone the repository
```bash
git clone https://github.com/yourusername/zkmarketplace.git
cd andrometa-marketplace
```

2. Install dependencies
```bash
npm install
# or
yarn install
```

3. Create a `.env.local` file in the root directory with the following variables:
```
WALLETCONNECT_PROJECT_ID=your_walletconnect_project_id
API_URL=https://yourbackend.com
```

4. Start the development server
```bash
npm run dev
# or
yarn dev
```

5. Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

## 🔍 Key Components

### Next.js App Router

The project uses Next.js App Router for routing.

### Web3 Integration

Wallet connection is handled through Web3Modal and wagmi. The main integration is in `components/providers/index.tsx`.

### NFT Trading

The marketplace supports all core NFT trading functions:
- Creating listings (`createListing` in `utils/calls.ts`)
- Placing bids (`createBid` in `utils/calls.ts`)
- Buying listings (`buyListing` in `utils/calls.ts`)
- Accepting bids (`tenderBid` in `utils/calls.ts`)
- Cancelling listings/bids (`cancelListing`/`cancelBid` in `utils/calls.ts`)

## 📝 API Integration

The platform communicates with a backend API at `https://outkast.world:7777` (Deprecated) which provides:

- NFT metadata
- Creator information
- Listing management
- Bid management
- User authentication
- Transaction verification

## 🧩 Smart Contract Interaction

All blockchain interactions are signature-based, where:
1. User signs a message with their Ethereum wallet
2. Signature is sent to the backend
3. Backend verifies the signature and processes the transaction
