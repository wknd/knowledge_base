---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: TechArticle
title: source coding
order: 2
date:   2018-02-26 17:42:09 +0100
math: true
---
Source encoding is the first step in our information transfer pipeline. We will go through various definitions needed to design and quantify an efficient compact code. After that we will use these to implement specific source coding methods.

A source generates a message, these messages consist of "**symbols**".  
Each symbol consists of "**characters**" from an **alphabet**. The alphabet for most electronic systems is {% raw %}{0,1}{% endraw %} (binary).

In a memoryless source, there is no correlation between the generated symbols (if I just saw an A that doesn't mean there is suddenly less probability the next symbol is not an A).  
If the probability distribution of the generated symbols is constant, the source is "stationary".

## properties
A source code is:
*    **non-singular**: if all codewords are different
*    **uniquely decodable**: if there is no message for which the decoding is ambiguous
*    **instantaneously decodable**: if it is uniquely decodable and if it is possible to decode each message without waiting for subsequent codewords
*    a **block code**: if all codewords have the same length
*    uniquely decodable if it is a **non-singular block code**

In general it is not trivial to determine if a code is uniquely decodable.

#### example source codes 1
An information source that has 4 possible values: s<sub>1</sub>, s<sub>2</sub>, s<sub>3</sub> and s<sub>4</sub>.  
We could represent this information using a binary alphabet {% raw %}{0,1}{% endraw %} so that:  
*    Code A: s<sub>1</sub>=0, s<sub>2</sub>=11, s<sub>3</sub>=00, s<sub>4</sub>=11.
*    Code B: s<sub>1</sub>=0, s<sub>2</sub>=11, s<sub>3</sub>=00, s<sub>4</sub>=010.
*    Code C: s<sub>1</sub>=00, s<sub>2</sub>=01, s<sub>3</sub>=10, s<sub>4</sub>=11.

Code A is not non-singular because A(s<sub>2</sub>) = A(s<sub>4</sub>) = 11.  
Code B is non-singular but not uniquely decodable because 00 can be decoded as s<sub>3</sub> or s<sub>1</sub>s<sub>1</sub>.  
Code C is a non-singular block code of length 2, so it is uniquely decodable.

#### example source codes 2
Again an information source that has 4 possible values: s<sub>1</sub>, s<sub>2</sub>, s<sub>3</sub> and s<sub>4</sub> represented using a binary alphabet {% raw %}{0,1}{% endraw %} 
*    Code A: s<sub>1</sub>=0, s<sub>2</sub>=10, s<sub>3</sub>=110, s<sub>4</sub>=1110.
*    Code B: s<sub>1</sub>=0, s<sub>2</sub>=01, s<sub>3</sub>=011, s<sub>4</sub>=0111.
*    Code C: s<sub>1</sub>=0, s<sub>2</sub>=01, s<sub>3</sub>=011, s<sub>4</sub>=111.

Codes A, B and C are uniquely decodable.  
Only code A is instantaneously decodable, e.g. 01011101100100010 = s<sub>1</sub>s<sub>2</sub>s<sub>4</sub>s<sub>3</sub>s<sub>1</sub>s<sub>2</sub>s<sub>1</sub>s<sub>1</sub>s<sub>2</sub>.  
Code B is not instantaneously decodable, e.g. 01101011100 = s<sub>3</sub>s<sub>2</sub>s<sub>4</sub>s<sub>1</sub>s<sub>1</sub>. (when you see the first 0, you don't know if its s<sub>1</sub> or part of s<sub>2</sub> or s<sub>3</sub>, when you see 01 you don't know if its s<sub>2</sub> or part of s<sub>3</sub>).  
Code C is not instantaneously decodable, e.g. 011111111….= ? (you won't know if its s<sub>1</sub>s<sub>4</sub>s<sub>4</sub>.. or s<sub>1</sub>s<sub>2</sub>s<sub>4</sub>... until you can finally see another 0 and can start counting)

## Prefix condition

A prefix of a codeword is a substring of the codeword consisting of consecutive characters of the codeword starting from the first character of the codeword. 

The prefix condition is a necessary and sufficient condition for a code to be **instantaneously decodable**. It states that none of the codewords is a prefix of another codeword.

#### exercise 1
Design an instantaneously decodable binary code with lengths: 3, 2, 3, 2, 2.

{% capture spoil_prefix_ex1 %}
3,2,3,2,2 -> 2, 2, 2, 3, 3 re-ordering the lengths makes it easier to quickly come up with codewords that match the prefix condition.  
00  
01  
10  
110  
111

A=110, B=00, C=111, D=01, E=10
{% endcapture %}
<details>
<summary> show answer </summary>
{{ spoil_prefix_ex1 | markdownify }}
</details>

#### exercise 2
Design an instantaneously decodable ternary code with lengths: 2, 3, 1, 1, 2.

{% capture spoil_prefix_ex2 %}
lengths: 1, 1, 2, 2, 3 (ternary code, 3 possible values)  
0  
1  
20  
21  
220

A=20, B=220, C=0, D=1, E=21

{% endcapture %}
<details>
<summary> show answer </summary>
{{ spoil_prefix_ex2 | markdownify }}
</details>

#### exercise 3
Design an instantaneously decodable binary code with lengths: 2, 3, 2, 2, 2.

{% capture spoil_prefix_ex3 %}
lengths: 2, 2, 2, 2, 3
00  
01  
10  
11  
ERROR

Can't create an other code that isn't a prefix of the already existing ones, so we cannot make an instantaneously decodable code with these lengths.

{% endcapture %}
<details>
<summary> show answer </summary>
{{ spoil_prefix_ex3 | markdownify }}
</details>

## Properties of instantaneously decodable codes
*    Easy to prove if a code is instantaneously decodable using the prefix condition
*    Easy to design an instantaneously decodable code with given lengths
*    Decoding is easy
*    Sensitivity to bit errors is much larger if the code is not a block codeword

## Kraft's inequality
Kraft's inequality is a necessary and sufficient condition for a code with alphabet size ```r``` consisting of ```q``` codewords with lengths l<sub>1</sub>, ..., l<sub>q</sub> to be **instantaneously decodable**. 

$${% raw %} K = \sum_{i=1}^q r^{-l_i} \le 1 {% endraw %}$$

Vice versa there exists an instantaneously decodable code with these codeword lengths if Kraft's inequality holds.

#### example
*    Code A: s<sub>1</sub> = 0, s<sub>2</sub> = 100, s<sub>3</sub> = 110, s<sub>4</sub> = 111.
*    Code B: s<sub>1</sub> = 0, s<sub>2</sub> = 100, s<sub>3</sub> = 110, s<sub>4</sub> = 11.
*    Code C: s<sub>1</sub> = 0, s<sub>2</sub> = 10, s<sub>3</sub> = 110, s<sub>4</sub> = 11.

Code A meets the prefix condition, so it is instantaneously decodable as expected. 
$${% raw %} K = 2^{-1} + 2^{-3} + 2^{-3} + 2^{-3} = 0.875 \le 1 {% endraw %}$$

Code B does not meet the prefix condition, so it is not instantaneously decodable. (s<sub>4</sub> is a prefix of s<sub>3</sub>). 
$${% raw %} K = 2^{-1} + 2^{-3} + 2^{-3} + 2^{-2} = 1 \le 1 {% endraw %}$$. This means it is possible to design an instantaneously decodable code with the given codeword lengths. For instance: 0, 110, 111, 10.

Code C does not meet the prefix condition, so it is not instantaneously decodable. $${% raw %} K = 2^{-1} + 2^{-2} + 2^{-3} + 2^{-2} = {9 \over 8} \gt 1 {% endraw %}$$. This means it is not possible to design an instantaneously decodable code with the given codeword lengths.

## Average codeword length
The average codeword length ```L``` is defined as:

$${% raw %} L = \sum_{i=1}^q p_i l_i {% endraw %}$$ 

with p<sub>i</sub> the probability of the symbols and l<sub>i</sub> the individual lengths of the ```q``` codewords.

A **compact code** has an average codeword length that is smaller than or equal to the average codeword length of all other uniquely decodable codes with the same source symbols and the same code alphabet.

## Lower bound of the average codeword length
Ever instantaneously decodable code of the source S={% raw %}{s<sub>1</sub>, ..., s<sub>q</sub>}{% endraw %} with alphabet {0, 1} has an average codeword length ```L```, that is equal to or larger than the entropy of the source:

$${% raw %} L \ge H(S) {% endraw %}$$

with p<sub>i</sub> the probability of the symbols, and l<sub>i</sub> the individual lengths of the ```q``` codewords and ```r``` the alphabet size. The inequality bcomes an equality when  
$${% raw %} p_i = r^{-l} {% endraw %}$$ with ```i=1,2,...,q```

## code efficiency
The efficiency of a code is calculated with:

$${% raw %} \eta = {H(S) \over L} * 100% {% endraw %}$$ 

A "special source" is a source with symbol probabilityies p<sub>i</sub> with ```i=1,2,...,q``` such that $${% raw %} log_2({1 \over p_i}) {% endraw %}$$ are integers (p=0.5, or 0.25, or 0.125, or ...). It is possible to design an instantaneously decodable code that is 100% efficient with codeword lengths $${% raw %} l_i = log_2({1 \over p_i}) {% endraw %}$$ 

#### examples

###### example code efficiency

| Source         |  p<sub>i</sub>  |  Code A  |  Code B  |
|:--------------:|:---------------:|:--------:|:--------:|
| s<sub>1</sub>  |  0.5            | 00       | 1        |
| s<sub>2</sub>  |  0.1            | 01       | 000      |
| s<sub>3</sub>  |  0.2            | 10       | 001      |
| s<sub>4</sub>  |  0.2            | 11       | 01       |

The entropy of the source is: 

$${% raw %}H( X ) = - \sum_{i=1}^n p_i log_2(p_i){% endraw %}$$  

$${% raw %}H( X ) = - (0.5 log_2(0.5) + 0.1 log_2(0.1) + 2*0.2 log_2(0.2)) = 1.76096 bits/symbol{% endraw %}$$ is equal to  
$${% raw %}H( X ) = 0.5 log_2( {1 \over 0.5} ) + 0.1 log_2( {1 \over 0.1} ) + 2*0.2 log_2( {1 \over 0.2} ) = 1.76096 bits/symbol{% endraw %}$$

Average codeword length is:

$${% raw %} L = \sum_{i=1}^q p_i l_i {% endraw %}$$ 

$${% raw %} L_a = (0.5*2)+(0.1*2)+2*(0.2*2) = 2 {% endraw %}$$  
$${% raw %} L_b = (0.5*1)+(0.1*3)+(0.2*3)+(0.2*2) = 1.8 {% endraw %}$$ 

Efficiency is:

$${% raw %} \eta = {H(S) \over L} * 100% {% endraw %}$$ 

$${% raw %} \eta_a = {1.76096 \over 2} * 100% = 88.04%{% endraw %}$$  
$${% raw %} \eta_b = {1.76096 \over 1.8} * 100% = 97.83%{% endraw %}$$ 

###### example special source

| Source         |  p<sub>i</sub>  |
|:--------------:|:---------------:|
| s<sub>1</sub>  |  0.125          |
| s<sub>2</sub>  |  0.25           |
| s<sub>3</sub>  |  0.5            |
| s<sub>4</sub>  |  0.125          |

This is a special source because $${% raw %}log_2({1 \over p_i}) {% endraw %}$$ are integers. This means that a 100% efficient compact code can be designed with $${% raw %} l_i = log_2({1 \over p_i}) {% endraw %}$$.

l<sub>1</sub>=3, l<sub>2</sub>=2, l<sub>3</sub>=1, l<sub>4</sub>=3.  
E.g. s<sub>1</sub>=110, s<sub>2</sub>=10, s<sub>3</sub>=0, s<sub>4</sub>=111.

#### exercises

###### exercise 1

Determine if the following codes are uniquely decodable. If not, give two message with the same code.  
Determine if the following codes are instantaneously decodable. If not, can you design an instantaneously decodable code with the same codeword lengths? If you can, design such a code.

*    A: 000, 001, 010, 011, 100, 101
*    B: 0, 01, 011, 0111, 01111, 011111
*    C: 0, 10, 110, 1110, 11110, 111110
*    D: 0, 10, 110, 1110, 1011, 1101
*    F: 0, 100, 101, 110, 111, 001
*    E: 0, 10, 1100, s1101, 1110, 1111
*    H: 1010, 001, 101, 0001, 1101, 1011
*    G: 01, 011, 10, 1000, 1100, 0111

###### exercise 2

Can you design an instantaneously decodable code with the following codeword lengths? Give an example.
*    A: 2 2 2 4 4 4
*    B: 1 1 2 3 3
*    C: 1 1 2 2 2 2

###### exercise 3

A source with 6 symbols has the following probability distribution and codewords:
*    s<sub>1</sub> = 0; p<sub>1</sub> = 0.3  
*    s<sub>2</sub> = 10; p<sub>2</sub> = 0.2  
*    s<sub>3</sub> = 1110; p<sub>3</sub> = 0.1  
*    s<sub>4</sub> = 1111; p<sub>4</sub> = 0.1  
*    s<sub>5</sub> = 1100; p<sub>5</sub> = 0.2  
*    s<sub>6</sub> = 1101; p<sub>6</sub> = 0.1  

What is the efficiency of the code? Is it possible to design a more efficient code? If yes, design such a code and calculate the efficiency of that code.

## N-th extension of a source

We refer to S<sup>n</sup> as the n-th extension of the source S if the alphabet of S<sup>n</sup> consists of all possible sequences of ```n``` symbols of S in which all symbols keep their probabilities.  
Then the following holds:

$${% raw %} H( S^n ) = nH(S) {% endraw %}$$

#### example
S = {% raw %}{s<sub>1</sub>, s<sub>2</sub>} {% endraw %} with p<sub>1</sub>=0.1 and p<sub>2</sub>=0.9.  
S<sup>2</sup> = {% raw %}{s<sub>1</sub>s<sub>1</sub>, s<sub>1</sub>s<sub>2</sub>, s<sub>2</sub>s<sub>1</sub>, s<sub>2</sub>s<sub>2</sub>} {% endraw %} with p<sub>11</sub>=0.01, p<sub>12</sub>=0.09, p<sub>21</sub>=0.09, p<sub>22</sub>=0.81.  
Then $${% raw %} H( S^n ) = nH(S) {% endraw %}$$.

## Shannon's coding theorem
*    If L is the average codeword length of a compact code for the source S, then: $${% raw %} H(S) \le L \lt H(S)+1 {% endraw %}$$.
*    If L<sub>n</sub> is the average codeword length of a compact code for the n-th extension of the source S, then:  $${% raw %} H(S) \le {L_n \over n} \lt H(S)+{1 \over n} {% endraw %}$$.
*    As a consequence, the code becomes 100% efficient in the limit: $${% raw %} \lim_{n \to \inf} {L_n \over n} = H(S) {% endraw %}$$.
