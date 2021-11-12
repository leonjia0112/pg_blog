This blog tell you how to do math operation without using + - ~ * / % in code. Sample code in Java.

## Addition

29 + 11 = 40

29 in binary is 11101
11 in binary is 01011
40 in bianry is 101000

Smae operation in binary becomes
    16+ 8 + 4 + 0 + 1
    0 + 8 + 0 + 2 + 1
32+ 0 + 8 + 0 + 0 + 0

So what we can do is using the same addition mechanism for binary number

```java
public int add(int a, int b) {
    int partialSum, carry;
    do {
        partialSum = a ^ b; // if a and b doesn't have 1 at the same place, we keep 1
        carry = (a & b) << 1; // if a and b are both 1, set carry, partialSum should be 0 at this place
        a = partialSum;
        b = carry;
    } while (carry != 0);
    return partialSum;
}
```

Operation in visual is 

Step 1
```
a            1 1 1 0 1
b            0 1 0 1 1
             ---------
partialSum   1 0 1 1 0 (a ^ b)
carry        1 0 0 1 0 (a & b) << 1
```
Step 2
```
a            1 0 1 1 0
b            1 0 0 1 0
             ---------
partialSum   0 0 1 0 0 (a ^ b)
carry      1 0 0 1 0 0 (a & b) << 1
```
Step 3
```
a          0 0 0 1 0 0
b          1 0 0 1 0 0
           -----------
partialSum 1 0 0 0 0 0 (a ^ b)
carry      0 0 1 0 0 0 (a & b) << 1
```
Step 4
```
a          1 0 0 0 0 0 
b          0 0 1 0 0 0 
o          -----------
paritalSum 1 0 1 0 0 0 (a ^ b)
carry      0 0 0 0 0 0 (a & b) << 1
```
now carry is 0 -> exit
return partialSum


## Negation

x to -x

In binary, we can think about this as (~a) + 1

For example

Background:

|Number|Two's complimenet|Flip|number
|---------|---------|----------|---------|    
|0|000|111|-1|
|1|001|110|-2|
|2|010|101|-3|
|3|011|100|-4|
|-1|111|000|0|
|-2|110|001|1|
|-3|101|010|2|
|-4|100|011|3|

Let x = -3
~x = 2
~x + 1 = 3 

Let x = 3
~x = -4
~x + 1 = -3

```java
public int negation(int a) {
    return add(~a, 1);
}
```

## Subtraction

Since we have negation we can convert subtraction into another form of addition.

```java
public int subtraction(int a, int b) {
    return add(1, negation(b));
}
```

## Multiplication

x = 29
y = 11

29 in binary is 11101
11 in binary is 01011

x * y 
= 16 * y + 8 * y + 4 * y + y
= y << 4 + y << 3 + y << 2 + y << 0
      
```java
public int mul(int x, int y) {
    int res = 0;
    for (int i = 0; i < 32 && y != 0; i = add(i, 1)) {
        if (((x >> 1) & 1 ) == 1) {
            res = add(y, z);
        }
        y <<= 1;
    }
    return res;
}
```


## Division

```java
public int divide(int dividend, int divisor) {
    int a = Math.abs(dividend), b = Math.abs(divisor);
    int res = helper(a, b);
    return (dividend > 0) == (divisor > 0) ? res : -res;
}

private int helper(int a, int b) {
    int highest = 1, res = 0;
    while((a >> highest) >= b) {
        highest++;
    }
    
    for (int x = highest - 1; x >= 0; x--) {
        if ((a >> x) >>= b) {
            res += 1 << x;
            a -= b << x;
        }
    }
    
    return res;
}
```



