1. Sending ether to a contract B from a contract A(for e.g using send or transfer), executes the code of B's fallback function if it exists(and receive doesn't exist). - X

2. Division result is auto rounded towards zero. - X

3. Division by zero causes panic error, escapes unchecked. - X

4. Always check the return value of send. Send fails if call stack depth is 1024(can be forced by the caller). - X

5. Using low level functions to call a contract, handing over control on it. - X

6. Before using delegate call, ensure that storage layout is in same order in both contracts. - X

7. enum types are not part of the ABI, they are just solidity abstraction. - (not understanded)

8. Delete keyword is simply a reassignment of elements to their default value(ie.zero). - X

9. .call bypasses function existence check, type checking and argument packing. - (not understanded)

10. The evm considers a call to non-existing contract to always succeed, so there is a check of extcodesize > 0 when making an external call. But call, static call, delegate call, send, transfer do not include this check. - X

11. On receiving funds from selfdestruct / coinbase miner reward, the contract can not react to it, and it doesn't require a contract to have receive or fallback functions. - X

12. extcodesize(evm opcode checks the size of the code ) > 0 check is skipped by the complier if the function call expects return data. the ABI decoder will catch the case of a non-existing contract Because such calls are followed up by abi decoding the return data, which has a check returndatasize(evm opcode checks the size of return data) is being at least non-zero number. So for empty contracts, they would always revert in the end. - X 

13. Always send 1 wei to precompiled contracts to activate them when testing in private blockchains, otherwise it may lead to OOG (out of gas) - X

    precompiled contracts
        Ex: 
            SHA-256: 0x2
            RIPEMD-160: 0x3
            ECRECOVER: 0x1b
            ECADD: 0x20 

14. Functions called from within an unchecked block do not inherit the property. Bitwise do not perform the overflow or underflow checks. - X

15. Calling a function on a different contract (instance) will perform an EVM function call and thus switch the context such that state variables in the calling contract are inaccessible during that call (except if you use delegatecall). - X

16. After contract creation, The deployed code does not include the constructor code or internal functions only called from the constructor. - X

17. Dont use this.f inside constructor. - X

18. Internal is the default visibility level for state variables. - X

19. Internal function calls do not create an EVM message call. They are called using simple jump statements. Same for functions of inherited contracts. - X

20. If you have a public state variable of array type, then you can only retrieve single elements of the array via the generated getter function. - X


