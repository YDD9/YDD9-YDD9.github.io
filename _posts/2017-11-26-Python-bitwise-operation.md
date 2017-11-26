# [Bitwise Operators](https://www.tutorialspoint.com/python/bitwise_operators_example.htm)


& 
Binary AND Operator: copies a bit to the result if it exists in both operands	(a & b) 

| 
Binary OR	Operator: It copies a bit if it exists in either operand.	(a | b) 

^ 
Binary XOR Operator: It copies the bit if it is set in one operand but not both.	(a ^ b)

~ 
Binary Ones Complement Operator: It is unary and has the effect of 'flipping' bits.	(~a )

<< 
Binary Left Shift Operator: The left operands value is moved left by the number of bits specified by the right operand.	a << 2

>> 
Binary Right Shift Operator: The left operands value is moved right by the number of bits specified by the right operand.    

  
[More about above operators:](https://wiki.python.org/moin/BitwiseOperators)     

x << y
Returns x with the bits shifted to the left by y places (and new bits on the right-hand-side are zeros). This is the same as multiplying x by 2\**y.  

x >> y
Returns x with the bits shifted to the right by y places. This is the same as //'ing x by 2\**y.

x & y
Does a "bitwise and". Each bit of the output is 1 if the corresponding bit of x AND of y is 1, otherwise it's 0.

x | y
Does a "bitwise or". Each bit of the output is 0 if the corresponding bit of x AND of y is 0, otherwise it's 1.

~ x
Returns the complement of x - the number you get by switching each 1 for a 0 and each 0 for a 1. This is the same as -x - 1.

x ^ y
Does a "bitwise exclusive or". Each bit of the output is the same as the corresponding bit in x if that bit in y is 0, and it's the complement of the bit in x if that bit in y is 1.

[Usefull examples with bitwise operators](https://discuss.leetcode.com/topic/50315/a-summary-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)
