# ZKMarketplace API Documentation

This document outlines the API endpoints and interactions that were used by the Andrometa NFT Marketplace.
We have since updated these API endpoints which you can find on our GitHub. We'll need to update the website once we finish integrating with Skale.

## Base URL

All API requests are made to the following base URL:

```
https://outkast.world:7777
```

## Authentication

Many API endpoints require authentication using a signature-based approach:

1. Request a nonce for the user's Ethereum address
2. Create a message containing the nonce and relevant transaction data
3. Sign the message with the user's Ethereum wallet
4. Include the signature in the API request

## Endpoints

### User Endpoints

#### Get User Data

```
GET /users/{eth_address}
```

Retrieves user data including nonce for authentication and token holdings.

**Response:**
```json
{
  "status": "Success",
  "data": {
    "result": {
      "nonce": "12345",
      "tokens": {
        "currencies": [
          {
            "address": "0xb4EEDeC5c01418A00570E4223D37cF3d9fb55562",
            "ticker": "ADS",
            "owned": "100000000000000000000"
          }
        ]
      }
    }
  }
}
```

### NFT Endpoints

#### Get NFT Holdings

```
GET /holdings/nft
```

Retrieves NFT holdings with various query parameters.

**Query Parameters:**
- `limit`: Maximum number of records to return (default: 20)
- `offset`: Offset for pagination (default: 0)
- `token_address`: Filter by token address
- `token_id`: Filter by token ID
- `owner`: Filter by owner address
- `listing`: Filter to only include NFTs with active listings (true/false)
- `bid`: Filter to only include NFTs with active bids (true/false)
- `creator_slug`: Filter by creator slug
- `creator`: Filter by creator address
- `category`: Filter by category
- `lower_cost`: Filter by minimum price
- `upper_cost`: Filter by maximum price
- `sort`: Sort field (creation, cost)

**Example Request:**
```
GET /holdings/nft?limit=20&offset=0&listing=true&creator_slug=andrometa
```

**Response:**
```json
{
  "result": {
    "data": [
      {
        "owner": "0x1234...",
        "token_id": 123,
        "metadata": {
          "name": "NFT Name",
          "image": "https://ipfs.io/ipfs/QmXyz...",
          "attributes": [
            {
              "trait_type": "Background",
              "value": "Blue"
            }
          ]
        },
        "collection": {
          "address": "0xabcd...",
          "name": "Collection Name",
          "ticker": "COLL",
          "category": "Avatar",
          "description": "Collection description",
          "base_uri": "ipfs://QmXyz...",
          "logo": "/assets/images/logo.png"
        },
        "listing": {
          "listing_id": "1234",
          "txn_id": "5678",
          "merchant_address": "0x1234...",
          "creation": "1649985600000",
          "expiration": "1682035200000",
          "featured": true,
          "status": "Pending",
          "currency": {
            "address": "0xb4EEDeC5c01418A00570E4223D37cF3d9fb55562",
            "name": "Andrometa",
            "ticker": "ADS",
            "description": "Andrometa platform token",
            "logo": "/assets/images/token-logo.png",
            "cost": "1000000000000000000"
          }
        },
        "bids": []
      }
    ],
    "info": {
      "returnedRows": 1,
      "totalRows": 100,
      "limit": 20,
      "offset": 0
    }
  }
}
```

#### Get Specific NFT

```
GET /holdings/nft/?token_address={address}&token_id={tokenId}
```

Retrieves data for a specific NFT.

**Example Request:**
```
GET /holdings/nft/?token_address=0xabcd...&token_id=123
```

### Marketplace Endpoints

#### Get Creators

```
GET /marketplace/creators
```

Retrieves all creators on the platform.

**Response:**
```json
{
  "result": [
    {
      "id": "1",
      "name": "Creator Name",
      "slug": "creator-name",
      "description": "Creator description",
      "banner": "/assets/images/banner.png",
      "logo": "/assets/images/creator-logo.png",
      "light_text_primary_color": "#160637",
      "light_text_secondary_color": "#270B5F",
      "light_primary_color": "#FFFFFF",
      "light_secondary_color": "#E2D6F9",
      "dark_text_primary_color": "#E2D6FA",
      "dark_text_secondary_color": "#C6AEF6",
      "dark_primary_color": "#160637",
      "dark_secondary_color": "#270B5F",
      "tokens": [
        {
          "name": "Collection Name",
          "category": "Avatar",
          "ticker": "COLL",
          "description": "Collection description",
          "logo": "/assets/images/collection-logo.png",
          "user_address": "0x1234...",
          "token_address": "0xabcd...",
          "token_index": "0"
        }
      ]
    }
  ]
}
```

#### Get Listings

```
GET /marketplace/listings
```

Retrieves marketplace listings with various query parameters.

**Query Parameters:**
- `token_address`: Filter by token address
- `token_index`: Filter by token index

**Example Request:**
```
GET /marketplace/listings?token_address=0xabcd...&token_index=123
```

#### Get Bids

```
GET /marketplace/bids
```

Retrieves marketplace bids with various query parameters.

**Query Parameters:**
- `token_address`: Filter by token address
- `token_index`: Filter by token index

**Example Request:**
```
GET /marketplace/bids?token_address=0xabcd...&token_index=123
```

#### Create Listing

```
POST /marketplace/list
```

Creates a new NFT listing.

**Request Body:**
```json
{
  "eth_address": "0x1234...",
  "token_address": "0xabcd...",
  "token_index": 123,
  "currency_address": "0xb4EEDeC5c01418A00570E4223D37cF3d9fb55562",
  "cost": "1000000000000000000",
  "expiration": 1682035200000,
  "signature": "0x..."
}
```

**Response:**
```json
{
  "status": "Success",
  "result": {
    "listing_id": "1234"
  }
}
```

#### Create Bid

```
POST /marketplace/bid
```

Creates a new bid on an NFT.

**Request Body:**
```json
{
  "eth_address": "0x1234...",
  "token_address": "0xabcd...",
  "token_index": 123,
  "currency_address": "0xb4EEDeC5c01418A00570E4223D37cF3d9fb55562",
  "cost": "1000000000000000000",
  "expiration": 1682035200000,
  "signature": "0x..."
}
```

**Response:**
```json
{
  "status": "Success",
  "result": {
    "bid_id": "5678"
  }
}
```

#### Buy Listing

```
POST /marketplace/buy
```

Purchases an NFT listing.

**Request Body:**
```json
{
  "eth_address": "0x1234...",
  "listing_id": "1234",
  "signature": "0x..."
}
```

**Response:**
```json
{
  "status": "Success",
  "result": {
    "txn_id": "9876"
  }
}
```

#### Tender Bid

```
POST /marketplace/tender
```

Accepts a bid on an NFT.

**Request Body:**
```json
{
  "eth_address": "0x1234...",
  "bid_id": "5678",
  "signature": "0x..."
}
```

**Response:**
```json
{
  "status": "Success",
  "result": {
    "txn_id": "9876"
  }
}
```

#### Cancel Listing

```
POST /marketplace/cancel_listing
```

Cancels an NFT listing.

**Request Body:**
```json
{
  "eth_address": "0x1234...",
  "listing_id": "1234",
  "signature": "0x..."
}
```

**Response:**
```json
{
  "status": "Success"
}
```

#### Cancel Bid

```
POST /marketplace/cancel_bid
```

Cancels a bid on an NFT.

**Request Body:**
```json
{
  "eth_address": "0x1234...",
  "bid_id": "5678",
  "signature": "0x..."
}
```

**Response:**
```json
{
  "status": "Success"
}
```

### Token Endpoints

#### Get ERC20 Tokens

```
GET /tokens
```

Retrieves all supported ERC20 tokens.

**Response:**
```json
{
  "result": [
    {
      "id": "1",
      "eth_address": "0xb4EEDeC5c01418A00570E4223D37cF3d9fb55562",
      "name": "Andrometa",
      "ticker": "ADS",
      "description": "Andrometa platform token",
      "logo": "/assets/images/token-logo.png",
      "type": "ERC20"
    }
  ]
}
```

## Message Signing Format

For signature-based authentication, messages must follow a specific format:

### Listing Creation
```
Signature Manager for Andrometa
Creating new listing by {eth_address}
Offering ERC721 Token {token_address} #{token_index}
Looking for {cost} {currency_address} ERC20 Tokens
Nonce: {nonce}
```

### Bid Creation
```
Signature Manager for Andrometa
Creating new bid by {eth_address}
Offering {cost} {currency_address}
Looking for ERC721 Token {token_address} #{token_index}
Nonce: {nonce}
```

### Buy Listing
```
Signature Manager for Andrometa
Buying listing #{listing_id}
Transfering ERC721 Token {token_address} #{token_index} from {owner} to {eth_address}
Transfering {cost} {currency_address} ERC20 Tokens from {eth_address} to {owner}
Nonce: {nonce}
```

### Tender Bid
```
Signature Manager for Andrometa
Buying bid #{bid_id}
Transfering {cost} {currency_address} ERC20 Tokens from {bidder_address} to {eth_address}
Transfering ERC721 Token {token_address} #{token_index} from {eth_address} to {bidder_address}
Nonce: {nonce}
```

### Cancel Listing
```
Signature Manager for Andrometa
Cancelling listing #{listing_id}
Nonce: {nonce}
```

### Cancel Bid
```
Signature Manager for Andrometa
Cancelling bid #{bid_id}
Nonce: {nonce}
```

## Error Handling

API errors are returned with a `Failure` status and an error message:

```json
{
  "status": "Failure",
  "error": {
    "message": "Error message describing the issue"
  }
}
```

Common error types:
- Authentication errors
- Invalid parameters
- Resource not found
- Permission errors
- Server errors
