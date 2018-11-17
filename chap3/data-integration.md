# Data Integration

Combining data from different sources

## Entity Identification Problem

Same entities from different sources should be matched up.

### Schema Integration

### Object Matching


## Redundancy and Correlation Analysis

### Methods to Detect Redundancy

#### Nominal Data: Chi-square Correlation Test

Suppose we are analyzing two columns A and B in the data table. A can take values a1, and a2, while B can take values b1 and b2.

```
A := {a1, a2}
B := {b1,b2,b3}
```

For example, for each row, we have

| index | A | B |
|--|---|---|
| 1 | a1 | b2 |
| 2 | a1 | b2 |
| 3 | a1 | b1 |
| 4 | a2 | b1 |
| 5 | a2 | b3 |
| 6 | a2 | b2 |
| 7 | a1 | b2 |
| 8 | a2 | b2 |



Now we construct a contigency table,

| | a1 | a2 |
|--|---|---|
| b1 | 1 | 1 |
| b2 | 3 | 2 |
| b3 | 0 | 1 |

where the cells are filled with the occurence of the combinations.

This table is called the observed frequencies, which we denote as O and each cell is denoted as o_ij.

This table tells us about the possible correlations already. Imagine we have two columns that are exactly the same, we would have a table that have large number of occurences on the diagonal elements.

However, those numbers in the table depends on the number of rows that we have in our original table. To find the actual correlation, we need to normalize it. We could simply divided everything by the total number of rows in the original table but this is not really normalization since the elements in O table will not be in the range 0 to 1.

Pearson's chi square correlation is a smart idea. We define an expectation table E. Each element of E is calculated as

$$
e_{ij} = \frac{ \text{number of} a_i * \text{ number of} b_j }{ \text{ total number of rows in original table } }
$$

Basically, we have the total number of possible occurrences divided by the total number of rows. Suppose we have A and B exactly the same, and they all have the same values, a1.

| index | A | B |
|--|---|---|
| 1 | a1 | a1 |
| 2 | a1 | a1 |
| 3 | a1 | a1 |
| 4 | a1 | a1 |
| 5 | a1 | a1 |
| 6 | a1 | a1 |
| 7 | a1 | a1 |
| 8 | a1 | a1 |

Then we expect e_11 = n, where n=8 is the number of rows.

Now if we compare the original table with this one,

$$
o_{ij} - e_{ij}
$$

we get the deviation from the expected table. With a few little twitches, we would define

$$
\chi^2 = \sum_{i,j} \frac{ (o_{ij} - e_{ij})^2 } { e_{ij} }
$$

If A and B are the same and each possible values occurred m times, then we would have

$$
o_{ij} = e_{ij} = \delta_{ij} * m.
$$

Then we get 
$$
\chi^2 = 0
$$

Then we say this chi-square analysis doesn't reject our hypothesis that these two columns are correlated.

The final question is how to use the result. We usually have a threshold $\chi_0^2$. Whenever our calculated value is larger than this one, we decide that our analysis reject the hypothesis that the two columns are correlated.
This value $\chi_0^2$ can be found in the textbooks.




#### Correlation Coefficient for Numeric Data

Pearson's product moment coefficient.


For a series of data A, we have the standard deviations
$$
\sigma_A = \sqrt{ \frac{ \sum (a_i - \bar A)^2 }{ n } },
$$
where $n$ is the number of elements in series A. Now imagine we have two series
$(a_i - \bar A)$ and $(a_j - \bar A)$. The geometric mean squared for $i=j$ is 
$$
M_i^2 = (a_i - \bar A)^2
$$
So standard deviation is in fact a measure of the **geometric mean of the deviation of each element**.


Similarly, we would imagine that for two series A and B of the same length, we could define a quantity to measure the geometric mean of the deviation of the two series correspondingly
$$
\sigma_{A,B}^2 = \frac{ \sum (a_i - \bar A) (b_i - \bar B) }{ n }.
$$

Of course the square in the definition is simply for notation purpose at this point.

What this tells us is actually the correlation of these two series. To understand this, we assume that we have two series A = B, then we have $\sigma_{A,B} = \sigma_{A}$. Suppose we have two series at a completely opposite phase, 

| index | A | B |
|--|---|---|
| 1 | 1 | -1 |
| 2 | -1 | 1 |
| 3 | 1 | -1 |
| 4 | -1 | 1 |
| 5 | 1 | -1 |
| 6 | -1 | 1 |
| 7 | 1 | -1 |

we have $\sigma_{A,B} = -1 $. The negative sign tells us that our series is anti-correlated.

However, we would find that the value of this correlation depends on the values of the standard deviation of each series. We would like to normalize it and define the correlation between the two series,
$$
r_{A,B} = \frac{ \sigma_{A,B}  } { \sigma_{A} \sigma_{B} } = \frac{ \sum (a_i - \bar A) (b_i - \bar B) }{ n\sigma_{A} \sigma_{B} }
$$

Another way of interpreting it is
$$
r_{A,B} = \frac{ \sum_{i} ( \text{Sign}(a_i - \bar A) M_{i}^a ) ( \text{Sign}(b_i - \bar B) M_i^b ) }{ \sum_i { M_i^a } \sum_j { M_j^b } }
$$
which is some kind of geometric mean of the geometric mean of each series.

#### Covariance for Numeric Data

Covariance is the correlation coefficient without normalization
$$
Cov{A,B} = \sum_{i} ( \text{Sign}(a_i - \bar A) M_{i}^a ) ( \text{Sign}(b_i - \bar B) M_i^b )
$$

Or
$$
Cov{A,B} = E( A,B ) - \bar A \bar B
$$

### Tuple Duplication

In database 1, we have

| index | Name | Address | Purchased  |  
|--|--|--|--|
| 1 | Aby | Mars | 10 |
| 2 | Bob | Earth | 12 |
| 3 | Eve | Mars | 5 |

In database 2, we have

| index | Name | Address | Is Gained by Email Marketing |  
|--|--|--|--|
| 1 | Aby | Earth | False |
| 2 | Aby | Mars | False |
| 3 | Bob | Earth | True |
| 4 | Eve | Mars | False |


In these two databases, we have different addresses for Aby. This should be detected and fixed when combining the two databases.

### Data Value Conflict Detection and Resolution

One simple example is that different unit systems are used in different tables. We might have been using emperical units in table A while using standard units in table B.