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


# [Subtractor](http://www.cprogrammingcode.com/2014/06/write-program-to-add-two-numbers.html)   
To subtract two numbers here we are using 2's complement. To understand the logic let's take two number 4 and 3.

First calculate the 2's Complement of 3.

Binary representation of 3 is 0011

Now 1's complement can be obtained by inverting all the bits of binary representation of 3. And by adding 1 in 1's complement we get 2's complement.

1's complement of 3 is 1100.
2's complement of 3 is 1101. ( By adding 1 in 1's complement we get 2's complement).

Let's add 4 and 2's complement of 3.

0100 + 1101 = 0001 ( This is the binary representation of 1).

```
def subtractor(A, B)
    # A - B
    # 1\'s complement
    complement = ~B
    @ 2\'s complement
    comlement += 1
    return A + complement
    
# this is a fake solution, because operator '+' is still used!
# Make it work like a computer does adder first, just as CPU circuit was designed https://www.youtube.com/watch?v=VBDoT8o4q00
# First A&B to find carry number, then the last bit of carry was passed to one bit higher as the carry number << 1
# the higher bit is calculated with carry ^ A ^ B gives the current bit
    
```
0011  (carry = 3&5)
 0011 (3)
+0101 (5)
--------------
 1000 (8)

Correct solution based on the electronics circuit which subtractor really does as such: http://www.geeksforgeeks.org/subtract-two-numbers-without-using-arithmetic-operators/   

https://stackoverflow.com/questions/3430651/subtracting-two-numbers-without-using-operator


# max int for 32bits</br>
http://sqlity.net/en/858/how-to-calculate-maxint/</br>
**Signed Numbers**

A signed integer allows to store positive and negative numbers, while an unsigned integer can only represent positive numbers.
All signed integer data types use a single bit to indicate if the number is positive or negative. In a 32 bit integer like SQL Servers INT data type 31 bits are left to encode the actual number. That means we can store 2^31 different numbers. With zero being the smallest non negative number, the largest number for the INT data type is 2^31-1 = 2,147,483,647.</br>

Python 8bit, first bit is the sign, the maximum possible number is then a sum of 2^0, 2^1,..., 2^6 with n=7, a1=2^0=1, q=2</br>
https://en.wikipedia.org/wiki/Geometric_progression 
`Sn = a1*(1-q**n)/(1-q)`

<a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;\fn_cm&space;S_{n}=2^{0}&plus;2^{1}&plus;...&plus;2^{6}=2^{7}-1=127" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\inline&space;\fn_cm&space;S_{n}=2^{0}&plus;2^{1}&plus;...&plus;2^{6}=2^{7}-1=127" title="S_{n}=2^{0}+2^{1}+...+2^{6}=2^{7}-1=127" /></a></br>
Binary in python, left most bit is reserved for the sign, so 8bit maximum 1,111,111 with binary marker `0b` (when type in python)
```
1<<0 = 2^0 = 0b1      # zero  0 after 1
1<<1 = 2^1 = 0b10     # one   0 after 1
1<<2 = 2^2 = 0b100    # two   0 after 1
1<<3 = 2^3 = 0b1000   # three 0 after 1
...
1<<6 = 2^6 = 0b1000000

sum of above in binary is simply move one bit further to left and minus 1:
(1<<7)  -1 = 0b1111111
# 127
```
Other ways to confirm.
```
2**7 -1
# 127

0b1111111
# 127
```



