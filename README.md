# Karma Vault - A Solana Anchor Program

The Karma Vault project is a simple example of how to interact with the Solana blockchain using Anchor. This program tracks user karma points and allows users to "give karma" to other users by modifying their stored score. It's a fun use case for building decentralized applications with Rust, Anchor, and Solana.

### Features
- **Karma Vault**: Stores and manages karma points for users.
- **Give Karma**: Allows users to give karma to others, increasing their stored score.
- **Anchor Framework**: Built with Anchor for Solana, simplifying smart contract development.
- **Devnet Deployment**: Test the program on Solana's Devnet before deploying to mainnet.

---

## Table of Contents
- [Project Setup](#project-setup)
- [Smart Contract Overview](#smart-contract-overview)
- [Testing & Deployment](#testing--deployment)
- [Usage](#usage)
- [Future Improvements](#future-improvements)
- [Contributing](#contributing)
- [License](#license)

---

## Project Setup

### 1. Install Dependencies

Ensure you have the necessary dependencies installed. If you haven't already done so, follow the steps below.

```bash
# Install Rust
curl https://sh.rustup.rs -sSf | sh
rustup component add rustfmt

# Install Anchor CLI globally
npm install -g @coral-xyz/cli
anchor --version  # check if Anchor CLI is installed

# Install Solana CLI
solana-install init stable
solana --version
