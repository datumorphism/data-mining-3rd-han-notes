# Data Transformation

## Smoothing

Remove noisy data

### Binning

### Regression

### Clustering

## Attribute Construction/Feature Construction

## Aggregation

Produce weekly data instead of daily data

## Normalization

Important for classification.

### Min-max Normalization

$$
\frac{(v'_i - v_{min}')}{ ( v'_{max} - v'_{min}  ) } = \frac{(v_i - v_{min})}{ ( v_{max} - v_{min}  ) }
$$
where everything on the right hand side is known and $v_{min}'$ and $v_{max}'$ are chosen as the new min and max to be scaled to.

### Z-score Normalization

$$
v'_i = \frac{ (v_i - \bar A) }{ \sigma_A }
$$

For a series with only one value, $v'_i = 0$. For series of the form $ (v, -v, v, -v) $ where $v> 0$,

$$
\sigma_A = v.
$$

Then we have
$$
( 1, -1, 1, -1 )
$$

### Decimal Scaling

Basically

$$
v'_i = v_i/ 10^j
$$

choose $j$ so that the new values are not larger than 1.

## Discretization

Turn acutual age into age ranges


### Binning

### Histogram

### Cluster

### Decision Tree

### Correlative Analysis

ChiMerge


## Concept hierarchy generation for nominal data

Street < City < Country




