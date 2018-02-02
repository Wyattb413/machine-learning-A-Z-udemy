# Machine Learning A-Z Notes

[**Udemy Course Link**](https://www.udemy.com/machinelearning/learn/v4/overview) - https://www.udemy.com/machinelearning/learn/v4/overview

[**R**](https://cran.r-project.org/) - https://cran.r-project.org/

[**R Studio**](https://www.rstudio.com/) - https://www.rstudio.com/

[**Anaconda - Python Distrubution**](https://www.anaconda.com/) - https://www.anaconda.com/

[**Course Data Sets**](https://www.superdatascience.com/machine-learning) - https://www.superdatascience.com/machine-learning

## Table of Contents

Lecture Topic | Link
--- | ---
**Shortcuts** | [**Shortcuts**](#shortcuts)
**SECTION 2** | [**Section 2**](#section-2)
Importing the Dataset | [**Lecture 10**](#section-2-lecture-10)
Summary of Object-Oriented Programing | [**Lecture 11**](#section-2-lecture-11)
Missing Data | [**Lecture 12**](#section-2-lecture-12)
Categorical Data | [**Lecture 13**](#section-2-lecture-13)
Splitting the Dataset into the Training set and Test set | [**Lecture 15**](#section-2-lecture-15)
Feature Scaling | [**Lecture 16**](#section-2lecture-16)


---

<!-- ################################################################################################################ -->
<!--                                                     Shortcuts                                                    -->
<!-- ################################################################################################################ -->

## Shortcuts

In _Spyder_ |
--- |
`Ctrl+i` - Inspect (open docs for) |
`Ctrl+Enter` - Execute selected lines |
`F5` - Execute (Order 66) File |

In _RStudio_ |
--- |
`F1` - Inspect (open docs for)

---

<!-- ################################################################################################################ -->
<!--                                                     SECTION 2                                                    -->
<!-- ################################################################################################################ -->

## SECTION 2

---

### Section 2 Lecture 10

#### Importing the dataset and setting the working directory

- **NOTE** - Python indexes start at 0, while R indexes start at 1

- **Matrix of Features** - Independant variables
- **Dependent Variable Vector** - Dependant variables

### Section 2 Lecture 11

#### Object Orientated Programming

- **Class** - The model of something in which build
- **Object** - An instance of the `Class`
- **Method** - A way to use functions of your `Object`. A method can also be seen as a function that is applied onto the object, takes some input (that were defined in the class) and returns some output.

### Section 2 Lecture 12

- Filling in missing data, by finding and subsituting with mean of column

##### **Python**

- `Imputer` (Class) - Imputation transformer for completeing missing values

```python
import pandas as pd

dataset = pd.read_csv('Data.csv')
x = dataset.iloc[:, :-1].values # [:, :-1] means from the start to the very last

from sklearn.preprocessing import Imputer
imputer = Imputer(missing_values = 'Nan', strategy = 'mean', axis = 0) # Replacing Nan values, with the mean of the columns
imputer = imputer.fit(x[:, 1:3]) # Selecting columns with indexes 1 and 2
x[:, 1:3] = Imputer.transform([:, 1:3]) # Replace x's values with the transformed version
```

##### **R**

- `data$Column` - Get the Column of data

```R
dataset = read.csv('Data.csv')

dataset$Age = ifelse(is.na(dataset$Age), # returns Boolean for if argument has no value
                     ave(dataset$Age, FUN = function(x) mean(x, na.rm = TRUE)), # Gets the mean of the column, and subsitutes values that are not defined
                     dataset$Age) # Use existing value

dataset$Salary = ifelse(is.na(dataset$Salary),
                     ave(dataset$Salary, FUN = function(x) mean(x, na.rm = TRUE)),
                     dataset$Salary)
```

### Section 2 Lecture 13

#### Categorical data

- **Categorical date** - Data that can be put into categories

- Data given as string can be difficult to process, encode the string to an integer

##### **Python**

- `LabelEncoder` - Encode labels with value between 0 and n_classes-1 (subsitute non-integer values with integers)

- `OneHotEncoder` - Encode categorical integer features using a one-hot aka one-of-K scheme (to avoid LabelEncoded string from being seen as "greater" than another, break the integers into their own columns, so that all the values for either 1 or 0 for the column)

- Encoding column with three options (e.g. 'France', 'Germany, 'Spain')

```python
import pandas as pd

dataset = pd.read_csv('Data.csv')
x = dataset.iloc[:, :-1].values # [:, :-1] means from the start to the very last

from sklearn.preprocessing import LabelEncoder, OneHotEncoder
labelencoder_x = LabelEncoder()
onehotencoder = OneHoteEncoder(categorical_features = [0]) # Argument specifies index for column to be encoded
x = onehotencoder.fit_transform(x)
```

- Encoding column with two options (e.g. yes/no)

```python
import pandas as pd

dataset = pd.read_csv('Data.csv')
y = dataset.iloc[:, 3].values # [:, 3] means the last column

from sklearn.preprocessing import LabelEncoder
labelencoder_y = LabelEncoder()
y = labelencoder_y.fit_transform(y)
```

##### **R**

- `factor` - Used to encode a vector as a factor (encode a non-integer value into an integer)

```R
dataset = read.csv('Data.csv')

dataset$Country = factor(dataset$Country,
                        levels = c('France', 'Spain', 'Germany'),
                        labels = c(1, 2, 3))

dataset$Purchased = factor(dataset$Purchased,
                        levels = c('No', 'Yes'),
                        labels = c(0, 1))
```

### Section 2 Lecture 15

It's key that you use two data sets when creating a machine learning model:

- **Traing Set** - This will be the data set you're actively using to train you ML model
- **Test Set** - This will be the data set that is slightly different from your training set, to test that your model isn't too tightly coupled with the data in the **Training Set**

##### **Python**

**NOTE** The code exmaple only include relavant code for the new topic introduced in **Lecture 15**

- `train_test_split` - Split arrays or matrices into random train and test subsets

```python
from sklearn.cross_validation import train_test_split
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = 0.2, random_state =0) # Splitting data set based on parameter test_size, here we are saying 20% data is reserved for test data set
```

##### **R**

- `install.packages` - To install packages lol
- `libary(<PACKAGE_NAME>)` - To import package

```R
# install.packages('caTools')
library(caTools)
set.seed(123) # Setting random interval based on int argument
split = sample.split(dataset$Purchased, SplitRatio = 0.8) # Returns set of TRUE/FALSE dictating what values are designated for the test or training data set
traing_set = subset(dataset, split == TRUE) # Assign values designated as TRUE to training set
test_set = subset(dataset, split == FALSE) # Assign values designated as FALSE to test set
```

### Section 2 Lecture 16

Feature Scaling is used to normalize data, as to not have your ML model hava data that's blown out of proportion comparatively speaking

![Feature Scaling Equations](/pictures/feature_scaling.png?raw=true "Feature Scaling Equations")

##### Python

- `StandardScaler` - Standardizes data utilizing the mean and standard deviation

**NOTE** When scaling `x_train` we are using `fit_transform`, because we are fitting the `StandardScaler` to the data in `x_train` so the mean and standard deviation are calculating based on the data provided by `x_train`.
However, we only use `transform` on `x_test` when scaling the data, because we want to use the same mean and standard deviation used to scale `x_train`.

```python
from sklearn.preprocessing import StandardScaler
sc_x = StandardScaler()
x_train = sc_x.fit_transform(x_train)
x_test = sc_x.transform(x_test)
```