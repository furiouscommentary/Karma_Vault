## Karma Vault Starter Setup (Anchor + Solana + TypeScript)

# 1. Install Dependencies (Skip if already installed)
curl https://sh.rustup.rs -sSf | sh
rustup component add rustfmt

npm install -g @coral-xyz/cli
anchor --version  # check if Anchor CLI is installed

solana-install init stable
solana --version

# 2. Set up Solana CLI environment
solana config set --url devnet
solana-keygen new --outfile ~/.config/solana/devnet.json
solana config set --keypair ~/.config/solana/devnet.json

# 3. Create the Anchor project
anchor init karma_vault --javascript
cd karma_vault

# 4. Replace lib.rs (basic structure for the Karma Vault contract)
cat > programs/karma_vault/src/lib.rs << 'EOF'
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
EOF

# 5. Update Anchor.toml for localnet/devnet as needed

# 6. Build and deploy (test on devnet)
anchor build
anchor deploy

# 7. Create TypeScript script to interact (inside /tests or /scripts)
# Example: giveKarma.ts (optional follow-up)

# 8. Test it using Anchor's Mocha test framework
anchor test

# Ready to expand: Add logic like preventing multiple karma from same wallet, or decay over time
