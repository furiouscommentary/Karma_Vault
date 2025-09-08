# 🚀 Karma Vault Starter Setup (Anchor + Solana + TypeScript)

This project is a **starter boilerplate** for building a Karma Vault program on Solana using **Anchor framework** and interacting with it via **TypeScript**.
The contract demonstrates simple concepts like initializing a vault and updating a karma score for users.

---

## 📦 1. Install Dependencies (Skip if already installed)

```bash
# Install Rust
curl https://sh.rustup.rs -sSf | sh
rustup component add rustfmt

# Install Anchor CLI
npm install -g @coral-xyz/cli
anchor --version  # Verify installation

# Install Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"
solana --version
```

---

## ⚙️ 2. Set up Solana CLI environment

```bash
# Point CLI to devnet
solana config set --url devnet

# Generate a new keypair for devnet
solana-keygen new --outfile ~/.config/solana/devnet.json

# Configure CLI to use that keypair
solana config set --keypair ~/.config/solana/devnet.json
```

---

## 📂 3. Create the Anchor project

```bash
anchor init karma_vault --javascript
cd karma_vault
```

---

## 📝 4. Replace `lib.rs` (basic structure for Karma Vault)

```rust
use anchor_lang::prelude::*;

#[program]
pub mod karma_vault {
    use super::*;

    pub fn initialize(ctx: Context<Initialize>) -> Result<()> {
        let vault = &mut ctx.accounts.vault;
        vault.bump = *ctx.bumps.get("vault").unwrap();
        Ok(())
    }

    pub fn give_karma(ctx: Context<GiveKarma>, amount: u64) -> Result<()> {
        let recipient = &mut ctx.accounts.recipient;
        recipient.score += amount;
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = user, space = 8 + 8, seeds = [b"vault"], bump)]
    pub vault: Account<'info, Vault>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct GiveKarma<'info> {
    #[account(mut)]
    pub recipient: Account<'info, KarmaScore>,
}

#[account]
pub struct Vault {
    pub bump: u8,
}

#[account]
pub struct KarmaScore {
    pub score: u64,
}
```

---

## ⚡ 5. Update `Anchor.toml`

Modify the cluster for **localnet** or **devnet** as needed:

```toml
[provider]
cluster = "devnet"
wallet = "~/.config/solana/devnet.json"
```

---

## 🛠️ 6. Build and Deploy

```bash
anchor build
anchor deploy
```

---

## 📜 7. Interact with the Program (TypeScript)

Create a script inside `/tests` or `/scripts` (e.g., `giveKarma.ts`):

```ts
import * as anchor from "@coral-xyz/anchor";

async function main() {
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);

  const program = anchor.workspace.KarmaVault;

  // Example: Call the giveKarma instruction
  const recipient = anchor.web3.Keypair.generate();

  await program.methods
    .giveKarma(new anchor.BN(10))
    .accounts({
      recipient: recipient.publicKey,
    })
    .rpc();

  console.log("✅ Karma given to:", recipient.publicKey.toBase58());
}

main().catch((err) => {
  console.error(err);
});
```

---

## 🧪 8. Run Tests

```bash
anchor test
```

---

## 🎯 Next Steps

* Prevent multiple karma from the same wallet
* Implement karma decay over time
* Add leaderboard functionality
* Deploy to **mainnet-beta** after testing

---

## 📖 Resources

* [Solana Docs](https://docs.solana.com)
* [Anchor Book](https://book.anchor-lang.com/)
* [Coral Anchor CLI](https://github.com/coral-xyz/anchor)
