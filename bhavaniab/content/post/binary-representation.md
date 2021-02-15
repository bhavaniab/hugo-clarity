+++
author = "Bhavani Anantapur Bache"
title = "Representation of fractional numbers on Hardware – The fixed point representation"
date = "2015-07-28"
description = "C++11-Significance of override keyword"
featured = true
tags = [
    "FPGA",
    "DSP",
]
categories = [
    "FPGA",
    "DSP"
]
series = ["DSP"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/fpga.jpg"
+++

In the hardware world, a number can be represented only by ones and zeros. How do we represent fractional numbers if that’s the case? How do we represent negative numbers in Hardware?
While designing hardware systems, engineers often overlook the range and end up losing some of the bits especially the most significant bits due to overflow, resulting in inconsistent results in the simulation.Therefore, it’s important for DSP Engineers and Hardware designers to know about different ways to represent the fractional numbers.

Fractional numbers can be represented in different ways like Fixed point representation, Unsigned fixed point representation, IEEE754 representation etc.

The current blog-post discusses in detail about the Fixed point representation of integers, fractional numbers and negative numbers.

<!--more-->

<h4>Binary representation of a number</h4>
The number 45 can be represented in binary as (101101)<sub>2</sub>

$$
 \(2^0 \times 1\) + \(2^1 \times 0\) + \(2^2 \times 1\) + \(2^3 \times 1\) + \(2^4 \times 0\) + \(2^5 \times 1\) \newline
   = 1 + 4 + 8 + 32
  = 45_{10}
$$

<h3>Binary representation of fractional numbers</h3>
Let’s take the example of (1011.01)<sub>2</sub>

$$
= \(2^{-1} \times 0\) + \( 2^{-2} \times 1) + \(2^{0} \times 1\) + \(2^{1} \times 1\) + \(2^{2} \times 0\) + \(2^{3} \times 1\) \newline
= 0 + 0.25 + 1+2 + 8 \newline
=  11.25_{10}
$$

<h4>Representation of a negative numbers – the two’s complement representation.</h4>

Let’s take the example of 8 bit two’s complement representation.

$$
  -45_{10} = 11010011_{2}
$$

The two complement of a number can be calculated as follows:

1. Write the binary representation of the number
$$
  00101101_2
$$

2. Invert the bits, 0 becomes 1 and 1 becomes 0.
$$
  11010010_2
$$

3. Add 1 to the bits inverted
$$
  11010011_2
$$

<h4>Fixed point representation</h4>
Representation of fixed point number, the width of the number and the binary point position of the number.
The fixed point numbers are represented using two parameters, the width of the number representation and the binary point position within the number. We use the notation fixed<w,b> where w denotes the number of bits used and b denotes the binary point counting from the least significant bit.
Let’s take the example of 11.25 discussed above.
$$
  11.25_{10} = 001011.01_2 \newline
fixed<8,2> = 00101101_2 \newline
$$
fixed<8,2> represents 8 bit fixed point number, where the two rightmost bits are fractional and 6 bits are for decimal representation.
$$
  5.625_{10} = 00101.101_2 \\
  fixed<8,3> = 00101101_2 \\
$$
The same bit pattern 00101101 can also be used to represent number 5.625, where 5 bits and 3 are decimal bits. Integer numbers can be represented with 0 bits as fractional.
$$
 5.625_{10} = 00101.101_2 \\
 45_{10} = 00101101_2 \\ 
 fixed<8,0> = 00101101_2 \\ 
$$

<h4>Representation of negative numbers in fixed point representation, the 2’s compliment </h4>
2’s compliment is used in representing numbers in the fixed point notation.
$$
 -45_{10} = 11010011_2 \\ 
 fixed<8,0> = 11010011_2 \\ 
$$

Similarly -11.25 can be represented as
$$
 fixed<8,2> = (11010011)2
$$

