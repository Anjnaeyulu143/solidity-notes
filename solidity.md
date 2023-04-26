1. Sending ether to a contract B from a contract A(for e.g using send or transfer), executes the code of B's fallback function if it exists(and receive doesn't exist) - X

2. Division result is auto rounded towards zero - X

3. Division by zero causes panic error, escapes unchecked - X

4. Always check the return value of send. Send fails if call stack depth is 1024(can be forced by the caller) - X