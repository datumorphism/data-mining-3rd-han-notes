# Model of data mining

1. data cleaning
2. data integration
3. data reduction
   1. dimensional reduction
   2. numerosity reduction
4. data transformation


## Data Cleaning

### Missing values

1. ignore whole value
2. fill with
   1. actual data from somewhere
   2. labels like "missing", "default", "none", etc
   3. central tendency such as mean or median
   4. central tendency of the group/class/category
   5. most probable value: using regression, inference-based tools using a Bayesian formalism or decision tree to come up with a most probable value

Rule of thumb: one should determine whether indicating the missing value makes sense.

### Noisy data


1. binning procedure (local smoothing):
   1. partitioning:
      1. into equal frequency bins: each bin contains same number of values
      2. into equal width: into same interval ranges of values
   2. binning:
      1. by mean/median
      2. by boundaries
2. regression
3. outlier analysis
4. concept hierarchies: map values to some concepts such as "expensive", "moderate", and "cheap"

### Cleaning as a process

steps

1. discrepancy detection
2. unique rules, consecutive rules, and null rules.


Good to know:

1. data decay may also cause inaccuracies: outdated data
2. data overloading: overwrite data into existing fields in database that is for a different purpose
2. data cleaning is related to the purpose of the data