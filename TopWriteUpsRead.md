
DEEP DIVE TO UNDERSTAND UNISWAP V2 SMART CONTRACTS
In order to understand the different components that we are going to go through while analysing the code first it is important to know which are the main concepts and which is their role. So, bare with me because it is going to be worth it.

I have summarised in 5 paragraphs the main important concepts that you need to know exist and that you will understand by the end of this article.

Uniswap is a decentralised exchange protocol. This protocol is a suite of persistent, non-upgradable smart contracts that together create an automated market maker.

The Uniswap ecosystem consists of liquidity providers who contribute to liquidity, traders who swap the tokens and developers who interact with smart contracts to develop new interactions for the tokens.

Each Uniswap smart contract, or pair, manages a liquidity pool made up of reserves of two ERC-20 tokens.

Every liquidity pool rebalances to maintain a 50/50 proportion of cryptocurrency assets, which in turn determines the price of the assets.

Liquidity providers can be anyone who is able to supply equal values of ETH and an ERC-20 token to a Uniswap exchange contract. In return they are given Liquidity Provider Tokens (LP tokens represent the share of the pool owned by a liquidity provider) from the exchange contract which can be used to withdraw their proportion of the liquidity pool at any time.

The main smart contracts in their repository are these:

UniswapV2ERC20 — an extended ERC20 implementation that’s used for LP-tokens. It additionally implements EIP-2612 to support off-chain approval of transfers.

UniswapV2Factory — similarly to V1, this is a factory contract that creates pair contracts and serves as a registry for them. The registry uses create2 to generate pair addresses–we’ll see how it works in details.

UniswapV2Pair — the main contract that’s responsible for the core logic. It’s worth noting that the factory allows to create only unique pairs to not dilute liquidity.

UniswapV2Router — the main entry point for the Uniswap UI and other web and decentralized applications working on top of Uniswap.

UniswapV2Library — a collection of helper functions that implement important calculations.

In this article we will be mentioning all of these but we will focus mainly on going through UniswapV2Router and UniswapV2Factory code, although UniswapV2Pair and UniswapV2Library are going to be very involved.

UniswapV2Router02.sol
This contract makes it easier to create pairs, add and remove liquidity, calculate prices for all possible swap variations and perform actual swaps. Router works with all pairs deployed via the Factory contract

You will need to create an instance in your contract in order to call addLiquidity, removeLiquidity and swapExactTokensForTokens functions

address private constant ROUTER = 0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D;

IUniswapV2Router02 public uniswapV2Router;
uniswapV2Router = IUniswapV2Router02();
Let’s now look into liquidity management:

FUNCTION addLiquidity():

function addLiquidity(
    address tokenA,
    address tokenB,
    uint amountADesired,
    uint amountBDesired,
    uint amountAMin,
    uint amountBMin,
    address to,
    uint deadline
) external returns (uint amountA, uint amountB, uint liquidity);
tokenA and tokenB: are the tokens we need to get or create the pair we want to add liquidity to.

amountADesired and amountBDesired are the amounts we want to deposit into the liquidity pool.

amountAMin and amountBMin are the minimal amounts we want to deposit.

to address is the address that receives LP-tokens.

deadline, would most commonly be the block.timestamp

Inside internal _addLiquidity() it will check if a pair of those two tokens already exists and in case it doesn’t it will create a new one

if (IUniswapV2Factory(factory).getPair(tokenA, tokenB) == address(0)) {
    IUniswapV2Factory(factory).createPair(tokenA, tokenB);
}
Then it needs to get the existing amount of the tokens or also known as reserveA and reserveB, which we can access through UniswapV2Pair contract

IUniswapV2Pair(pairFor(factory, tokenA, tokenB)).getReserves()
Now, the external function addLiquidity, returns (uint amountA, uint amountB, uint liquidity), so how does it calculate it?

After getting the reserves mentioned above through UniswapV2Library, there are a series of checks

If the pair didn’t exist and a new one was newly created amountA and amountB returned will be amountADesired and amountBDesired which were passed as parameters (See above).

Otherwise, it will do this operation

amountBOptimal = amountADesired.mul(reserveB) / reserveA;
if the amountB is smaller or equal to the amountBDesired, then it will return:

(uint amountA, uint amountB) = (amountADesired, amountBOptimal)
Otherwise, it will return

(uint amountA, uint amountB) = (amountAOptimal, amountBDesired)
where amountAOptimal is calculated the same way as amountBOptimal

Then, to calculate the liquidity to return will go through the following:

First of all, it is going to deploy the UniswapV2Pair contract with the address of the existing/newly created pair.

How does it do that? It calculates the CREATE2 address for a pair without making any external call with: (Read more about CREATE2 Opcode)

pair = address(uint(keccak256(abi.encodePacked(
    hex'ff',
    factory,
    keccak256(abi.encodePacked(token0, token1)),
   hex'96e8ac4277198ff8b6f785478aa9a39f403cb768dd02cbee326c3e7da348845f' 
))));
Then, it gets the address of the newly deployed contract, which we will need in order to mint the tokens from this pair of tokens.

When you add liquidity to a pair, the contract mints LP-tokens; when you remove liquidity, LP-tokens get burned.

So, first we get the address using pairFor from UniswapV2Library:

address pair = UniswapV2Library.pairFor(factory, tokenA, tokenB);
So, that later can mint the ERC20 tokens and calculate the liquidity to return:

liquidity = IUniswapV2Pair(pair).mint(to);
In case you’re wondering why it ends up being an ERC20, inside the mint function it’s storing it like this

uint balance0 = IERC20(token0).balanceOf(address(this));
uint balance1 = IERC20(token1).balanceOf(address(this));
FUNCTION removeLiquidity():

function removeLiquidity(
    address tokenA,
    address tokenB,
    uint liquidity,
    uint amountAMin,
    uint amountBMin,
    address to,
    uint deadline
) external returns (uint amountA, uint amountB);