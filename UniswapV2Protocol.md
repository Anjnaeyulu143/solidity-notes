UniswapV2 Protocol
---------------------

Uniswap is a decentralised exchange protocol. This protocol is a suite of persistent, non-upgradable smart contracts that together create an automated market maker.

The Uniswap ecosystem consists of liquidity providers who contribute to liquidity, traders who swap the tokens and developers who interact with smart contracts to develop new interactions for the tokens.

Each Uniswap smart contract, or pair, manages a liquidity pool made up of reserves of two ERC-20 tokens.

Every liquidity pool rebalances to maintain a 50/50 proportion of cryptocurrency assets, which in turn determines the price of the assets.

Liquidity providers can be anyone who is able to supply equal values of ETH and an ERC-20 token to a Uniswap exchange contract. In return they are given Liquidity Provider Tokens (LP tokens represent the share of the pool owned by a liquidity provider) from the exchange contract which can be used to withdraw their proportion of the liquidity pool at any time.

--------------------------------------------------------------------------------------------------

The main smart contracts in their repository are these:
    . UniswapV2ERC20 — an extended ERC20 implementation that’s used for LP-tokens. It additionally implements EIP-2612 to support off-chain approval of transfers.