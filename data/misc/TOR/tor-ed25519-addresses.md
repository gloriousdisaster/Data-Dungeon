# Calculating Time - Generating Onion Addresses

[![Static Badge](https://img.shields.io/badge/certified-disaster-blue)](https://github.com/gloriousdisaster)

<span style="color: red;">If any of this is important to you, I'd check my math. It really could be disastrous.</span>

**If you find errors in this math, let me know.**

> **ℹ️ Note:**  
> Base-32 omits the numeral digits of 0, 1, 8, and 9.

> **📖 References:**
>
> - https://github.com/cathugger/mkp224o/issues/27 #github
> - https://mathway.com
> - Artificial Intelligence
>   - ChatGPT 3.5
>   - Ollama (Model: Llama3:8b)

# Equation Breakdown

> **⚠️ Warning:**  
> LaTeX and MathJax may not render correctly on GitHub.

### Formula

- **p** = Percentage of Probability
- **n** = Number of Characters (for prefix)
- **t** = Number of Calculations (or Trails)

**Formula**:  
`t = log(1-p) / log(1-1/(32^n))`

### Example

$$
\large \begin{align*}
& p = 99\% \\
& n = 2 \\
\\\
t = \frac{\log(1-p)}{\log\left(1-\left(\frac{1}{32}\right)^n\right)}
\end{align*}
$$

### Solving the Equation

<br>

#### Simplify the Numerator

<br>
Starting with parentheses, subtract .99 from 1.

$\large \log (1 - .99) = \log (0.01)$
<br><br>
$\large LOG10~~of~~0.01~\approx -2$
<br><br>

$$
\LARGE
\frac{\log (0.01)}{\log \left(1-\left(\frac{1}{32}\right)^{2}\right)} = \frac{-2} {\log \left(1-\left(\frac{1}{32}\right)^{2}\right)}


$$

<br>

#### Simplify the Denominator.

<br>

##### Simplify Each Term

<br><br>

Apply the product rule to $~~~\large ~~~\frac{1}{32}$ $~~~ \large \frac{-2}{\log \left(1-\frac{1^{2}}{32^{2}}\right)}$

One to any power is one. $~~~~~~~~~ \large \frac{-2}{\log \left(1-\frac{1}{32^{2}}\right)}$

Raise 32 to the power of 2 . $~~~~~~ \large \frac{-2}{\log \left(1-\frac{1}{1024}\right)}$

Write 1 as a fraction with a common denominator.

$\huge \log \left(\frac{1024}{1024}-\frac{1}{1024}\right)$

Combine the numerators over the common denominator.

$\huge \frac{-2}{\log \left(\frac{1024-1}{1024}\right)}$

Subtract 1 from 1024.

$\huge \frac{-2}{\log \left(\frac{1023}{1024}\right)}$

Move the negative in front of the fraction.

$\huge -\frac{2}{\log \left(\frac{1023}{1024}\right)}$

### Results

#### Exact Form:

$\huge -\frac{2}{\log \left(\frac{1023}{1024}\right)}$

#### Decimal Form:

$$
\frac{1023}{1024} ~~~ = ~~~
0.9990234375
$$

$$
\large \log (0.9990234375) =  -0.00042432292
$$

$$\large t~~= ~~~\frac{-2}{-0.00042432292}$$

$\huge t~\approx~4713.391395\ldots$

<br><br>

### Probable Data Calculations

**Excel Formula**
The following formula is used to create a data table using the above equation. The table can plot the probable calculations required based on the number of characters defined.

```
LOG10(1-'calculations')/LOG10(1-1/32^'probability')
```

#### Table A

###### Shows the Number of Possible Calculations by Probability and Character Count.

- **X-Axis**: Character Count (number of characters in the string)
- **Y-Axis**: Probability (percentage chance of obtaining a specific "string value")
- **Data**: The number of calculations required to reach the specified probability, given the character count
  <br><br>

|        | **50%**         | **99%**          |
| ------ | --------------- | ---------------- |
| **2**  | 709             | 4713             |
| **3**  | 22713           | 150900           |
| **4**  | 726817          | 4828869          |
| **5**  | 23258160        | 154523868        |
| **6**  | 744261118       | 4944763833       |
| **7**  | 23816355774     | 158232442728     |
| **8**  | 762123384785    | 5063438167379    |
| **9**  | 24387948313146  | 162030021356198  |
| **10** | 780414346020669 | 5184960683398420 |

#### Table B

Taking the information from Table A, we can assume how much time will be needed to obtain the defined character prefix by the probability level.

We will need an approximation of the compute resources calculations per second.
<br><br>

$$
\large \begin{align*}
& f=Probable~Calculations~Required \small ~~ (found~in~table~A) \\
& c=Calculation~Per~Second \\
\\

\\
& Seconds~ Needed = \frac{f}{c}
\end{align*}
$$

<br><br>

- **X-Axis**: Character Count (number of characters in the string)
- **Y-Axis**: Probability (percentage chance of obtaining a specific "string value")
- **Data**: Amount of time needed to process the required calculations. (Assuming the compute resource can process ~35 million calculations per second.)
  - _(Using a Dell XPS 15 | Intel i9-11900H | Thermal Throttling - 14 Treads @ ~ 3.3GHz | Resulting ~ 35M Calc Per Second_)

<br><br>

|        | **50%**            | **99%**                     |
| ------ | ------------------ | --------------------------- |
| **2**  | Less than 1 second | Less than 1 second          |
| **3**  | Less than 1 second | Less than 1 second          |
| **4**  | Less than 1 second | Less than 1 second          |
| **5**  | 1 Second           | 4 Seconds                   |
| **6**  | 21 Sec             | 2 Min, 21 Sec               |
| **7**  | 11 Min, 20 Sec     | 15 Min, 21 Sec              |
| **8**  | 6 hours            | 1 Day, 16 hours             |
| **9**  | 8 Days             | 53 Days                     |
| **10** | 8 Months, 14 Days  | 4 Years, 18 Months, 10 Days |

_(Excel Formula Examples for time)_

> [!note]
> Excel has issues when the value is under zero.

```
LOG10(1-$calculations)/LOG10(1-1/32^$probability)/$CalPerSec
```

```
IF("REF"=0,"0 M, 0 D, 0 hours, 0 M, 0 S",

IF("REF"<60,"0 M, 0 D, 0 hours, 0 M, "&"REF"&" S",

IF("REF"<3600,"0 M, 0 D, 0 hours, "&INT("REF"/60)&" M, "&MOD("REF",60)&" S",

IF("REF"<86400,"0 M, 0 D, "&INT("REF"/3600)&" hours, "&TEXT(INT(("REF"-INT("REF"/3600)*3600)/60),"00")&" M, "&MOD("REF",60)&" S",

IF("REF"<2629746,"0 M, "&INT("REF"/86400)&" D, "&TEXT(INT(("REF"-INT("REF"/86400)*86400)/3600),"00")&" hours, "&TEXT(INT(("REF"-INT("REF"/3600)*3600)/60),"00")&" M, "&MOD("REF",60)&" S",

IF("REF">=2629746,INT("REF"/2629746)&" M, "&TEXT(INT(("REF"-INT("REF"/2629746)*2629746)/86400),"00")&" D, "&TEXT(INT(("REF"-INT("REF"/86400)*86400)/3600),"00")&" hours, "&TEXT(INT(("REF"-INT("REF"/3600)*3600)/60),"00")&" M, "&MOD("REF",60)&" S"
)
)
)
)
))
```
