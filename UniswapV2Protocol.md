UniswapV2 Protocol
---------------------

1. How addLiquidity works ?
2. How addLiquidityETH works ?
3. How removeLiquidity works ?
4. How removeLiquidityETH works ?
5. How swapping works ?

1. Add Liquidity.
    -> it calls internal(FUN) 
    - _addLiquidity().
        1. First it checks pair contract of these tokens exists or not.If not then it creates a pair contract using these two tokens.
        2. If these tokens pair contract is exists, then it tries to add liquidity to pool;

            - it calls uniswapLibrary getReserves(FUN);
                getReserves();

                -1. It calls the sortTokens(FUN)
                    sortTokens() -> to sort tokenAddresses;
                -2. It calls the pairFor(FUN)
                    pairFor() -> It returns the pair address;
                -3. It calls uniswapV2pair getReserves(FUN);
                    getReserves() -> it returns the reserved balance of both tokens 

            - Finally uniswapLibrary getReserves(FUN) -> returns balances of the both tokens;

            - It validate the return value, If both tokens balance is 0;
                - Then user desired amount of the both tokens are add to the liquidity pool;

            - If return value (balances) is not equal 0;
                 Then it calculates the optimal value of the both tokens to added to the liquidity
                 -1. It calls the uniswapLibrary quote(FUN) - to calculate optimal value;

    - Finally this function is returns the the optimal value added to liquidity of the both tokens;

    - Again it calls the uniswapV2Library pairFor(FUN);
        - pairFor();
            - It calls the sortTokens(FUN);
                sortTokens() -> It sorts the tokens and returns the sorted tokens

    - Based on the sort tokes it returns the contract pair address;

    - Here both tokens amount are transfer to the pair contract.


    It calls the mint(FUN);
        - getReserves();

        it calls the internal _mintFee();

            -This function is executed when the first user added some liquidity and the second user added more liquidity then the first user; 

            if (_kLast != 0) {
                uint rootK = Math.sqrt(uint(_reserve0).mul(_reserve1));
                uint rootKLast = Math.sqrt(_kLast);
                if (rootK > rootKLast) {
                    uint numerator = totalSupply.mul(rootK.sub(rootKLast));
                    uint denominator = rootK.mul(5).add(rootKLast);
                    uint liquidity = numerator / denominator;
                    if (liquidity > 0) _mint(feeTo, liquidity);
                }
            }

        -_mintFee(); -> It returns the bool value. If the user adding liquidity first time then it mint tokens inside the mintFee function

        





        ---------------------------------------------

        doge/shiba

        First User            Second User

        100 - doge            150 - doge 
        100 - shiba           150 - shiba

        _kLast = 10000


        amount shiba = (150 * 100)/100 = 150 

        if 150 <= 150:
            require(150 >= 150);
            (doge,shiba) = (150,150)


        balance of pair contract in doge coin = 250 
        balance of pair contract in shiba coin = 250

        reserve of doge coin = 100
        reserve of shiba coin = 100

        amount0 = 250-100 = 150
        amount1 = 250-100 = 150

        rootK = 100
        rootKLast = 100

        
        
        
        
        




