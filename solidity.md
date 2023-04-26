1. Sending ether to a contract B from a contract A(for e.g using send or transfer), executes the code of B's fallback function if it exists(and receive doesn't exist) - X

2. Division result is auto rounded towards zero - X

3. Division by zero causes panic error, escapes unchecked - X

4. Always check the return value of send. Send fails if call stack depth is 1024(can be forced by the caller) - X

5. Using low level functions to call a contract handing over control on it - X

6. Before using delegate call, ensure that storage layout is in same order in both contracts - X

7. enum types are not part of the ABI, they are just solidity abstraction - (not understanded)

8. Delete keyword is simply a reassignment of elements to their default value(ie.zero) - X

9. .call bypasses function existence check, type checking and argument packing - (not understanded)

10. The evm considers a call to non-existing contract to always succeed, so there is a check of extcodesize > 0 when making an external call. But call, static call, delegate call, send, transfer do not include this check - X