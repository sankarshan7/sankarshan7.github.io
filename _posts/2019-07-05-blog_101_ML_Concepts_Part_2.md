---
layout: post
title:  "Machine Learning Concepts (Part 2)"
date:   2019-07-05 00:00:10 -0030
categories: jekyll update
mathjax: true
---

# Content

1. TOC
{:toc}
---

# How to Choose a Feature Selection Method For Machine Learning?

Feature selection is the process of reducing the number of input variables when developing a predictive model.

Feature-based feature selection methods involve evaluating the relationship between each input variable and the target variable using statistics and selecting those input variables that have the strongest relationship with the target variable. These methods can be fast and effective, although the choice of `statistical measures` depends on the data type of both the input and output variables.

## Feature Selection Algorithms

There are three general classes of feature selection algorithms: filter methods, wrapper methods and embedded methods.

**Filter Methods:** Filter feature selection methods apply a statistical measure to `assign a scoring` to each feature. The features are ranked by the score and either selected to be kept or removed from the dataset. The methods are often **univariate** and `consider the feature independently`, or with regard to the dependent variable. **Example:** Chi squared test, information gain and correlation coefficient scores.

**Wrapper Methods:** Wrapper methods consider the selection of a set of features as a **search problem**, where different combinations are prepared, evaluated and compared to other combinations. A predictive model is used to evaluate a combination of features and assign a score based on model accuracy. **Example:** [sklearn.feature_selection.RFE](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html)

**Main difference** of filter method with wrapper method is that, in filter method, before applying the model we are filtering the feature. This is quite helpful if running the model is a costly affair and also our data set is quite huge. Because in wrapper method to evaluate each combination of feature, we need to build and train the model first and only then we can evaluate.  

**Embedded Methods:** Embedded methods **learn** which **features** best contribute to the accuracy of the model while the model is being created. The most common type of embedded feature selection methods are **regularization** methods. Automatic feature selection. 

## Feature Selection Checklist


1. **Do you have domain knowledge?** 
   1. If yes, construct a better set of **ad hoc** features
2. **Are your features commensurate** (i.e comparable)?
   1. If no, consider `normalizing` them.
3. **Do you suspect interdependence of features?**
   1. If yes, expand your feature set by constructing `conjunctive features` or `products of features`, as much as your computer resources allow you.
4. **Do you need to prune the input variables** (e.g. for cost, speed or data understanding reasons)? 
   1. If no, construct disjunctive features or weighted sums of feature
5. **Do you need to assess features individually** (e.g. to understand their influence on the system or because their number is **so large that you need to do a first filtering**)? 
   1. If yes, use a `variable ranking method`; else, do it anyway to get baseline results.
6. Do you need a predictor? If no, stop
7. **Do you suspect your data is** `dirty` (has a few meaningless input patterns and/or noisy outputs or wrong class labels)? 
   1. If yes, `detect the outlier` examples using the top ranking variables obtained in step 5 as representation; check and/or discard them.
8. **Do you know what to try first ?** 
   1. If no, use a `linear predictor`. Use a forward selection method with the “probe” method as a stopping criterion or use the 0-norm embedded method for comparison, following the ranking of step 5, construct a sequence of predictors of same nature using increasing subsets of features. Can you match or improve performance with a smaller subset? 
   2. If yes, try a `non-linear predictor` with that subset.
9. **Do you have new ideas, time**, computational resources, and enough examples? 
   1.  If yes, compare several feature selection methods, including your new idea, `correlation coefficients`, `backward selection` and `embedded methods`. Use linear and non-linear predictors. Select the best approach with model selection
10. **Do you want a stable solution** (to improve performance and/or understanding)? 
    1.  If yes, subsample your data and redo your analysis for several `bootstrap`.

----

## Filter Method

>> Filter methods evaluate the relevance of the predictors outside of the predictive models and subsequently model only the predictors that pass some criterion.

Statistics for Filter Feature Selection Methods

```r
## Numerical Input, Numerical Output
## Numerical Input, Categorical Output
## Categorical Input, Numerical Output
## Categorical Input, Categorical Output
```

Common input variable data types:

1. Numerical Variables
   1. Integer Variables.
   2. Floating Point Variables.
2. Categorical Variables.
   1. Boolean Variables (dichotomous).
      1. `TRUE/FALSE` or `0/1` 
   2. **Ordinal Variables.**
      1. Example: Economic status ("low income",”middle income”,”high income”)
   3. **Nominal Variables:** no intrinsic order
      1. Example:  Types of houses: ("regular", "condos", "co-ops", "bungalows")

## 2. Statistics for Filter-Based Feature Selection Methods


- **Numerical Output:** Regression predictive modeling problem.
- **Categorical Output:** Classification predictive modeling problem.

![image](https://3qeqpr26caki16dnhd19sv6by6v-wpengine.netdna-ssl.com/wp-content/uploads/2019/11/How-to-Choose-Feature-Selection-Methods-For-Machine-Learning.png)

- [image_source](https://machinelearningmastery.com/feature-selection-with-real-and-categorical-data/)

1. Numerical Input, Numerical Output
   1. Pearson’s correlation coefficient (linear)
   2. Spearman’s rank coefficient (nonlinear)

2. Numerical Input, Categorical Output
   1. ANOVA correlation coefficient (linear).
      1. Check this implementation [video](https://www.youtube.com/watch?v=PWZLhr3FfIM)
      2. Understand [ANOVA](https://www.youtube.com/watch?v=-yQb_ZJnFXw)
   2. Kendall’s rank coefficient (nonlinear). Kendall does assume that the categorical variable is `ordinal`.


3. Categorical Input, Numerical Output

This is a strange example of a regression problem (e.g. you would not encounter it often).

Nevertheless, you can use the same “Numerical Input, Categorical Output” methods (described above), but in reverse.

4. Categorical Input, Categorical Output
   1. Chi-Squared test (contingency tables).
   2. Mutual Information.

In fact, mutual information is a powerful method that may prove useful for both categorical and numerical data, e.g. it is **agnostic to the data types**.

## Correlation Statistics

The scikit-learn library provides an implementation of most of the useful statistical measures.

For example:

- Pearson’s Correlation Coefficient: [f_regression()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_regression.html)
- ANOVA: [f_classif()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.f_classif.html)
- Chi-Squared: [chi2()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.chi2.html)
- Mutual Information: [mutual_info_classif()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_classif.html) and
[mutual_info_regression()](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.mutual_info_regression.html)



Also, the SciPy library provides an implementation of many more statistics, such as Kendall’s tau ([kendalltau](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html)) and Spearman’s rank correlation ([spearmanr](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html)).

- For more details check [sklearn feature selection](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectPercentile.html)


## Selection Method

Two of the more popular methods include:

- Select the top k variables: [SelectKBest](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html)
- Select the top percentile variables: [SelectPercentile](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectPercentile.html)

## Other

- **Transform Variables:** 
  1. transform a categorical variable to ordinal, even if it is not, and see if any interesting results come out.
  2. make a numerical variable discrete (e.g. bins); try categorical-based measures.

## Assumptions:

- Pearson’s that assumes a Gaussian probability distribution to the observations and a linear relationship


## Worked Examples of Feature Selection

**Numerical Input, Numerical Output**

```python
# pearson's correlation feature selection for numeric input and numeric output
from sklearn.datasets import make_regression
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import f_regression
# generate dataset
X, y = make_regression(n_samples=100, n_features=100, n_informative=10)
# define feature selection
fs = SelectKBest(score_func=f_regression, k=10)
# apply feature selection
X_selected = fs.fit_transform(X, y)
print(X_selected.shape)
# (100, 10)
```

**Numerical Input, Categorical Output**

```python
# ANOVA feature selection for numeric input and categorical output
from sklearn.datasets import make_classification
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import f_classif
# generate dataset
X, y = make_classification(n_samples=100, n_features=20, n_informative=2)
# define feature selection
fs = SelectKBest(score_func=f_classif, k=2)
# apply feature selection
X_selected = fs.fit_transform(X, y)
print(X_selected.shape)
# (100, 2)
```

**Categorical Input, Categorical Output**

- **use of chi-squared test**

```py
def select_features(X_train, y_train, X_test):
  """
  Use chi-squared test
  """
  fs = SelectKBest(score_func=chi2, k='all')
  fs.fit(X_train, y_train)
  X_train_fs = fs.transform(X_train)
  X_test_fs = fs.transform(X_test)
```

- **Mutual Information Feature Selection**

```py
# feature selection
def select_features(X_train, y_train, X_test):
  """
  Use mutual information based approach
  """
  fs = SelectKBest(score_func=mutual_info_classif, k='all')
  fs.fit(X_train, y_train)
  X_train_fs = fs.transform(X_train)
  X_test_fs = fs.transform(X_test)
  return X_train_fs, X_test_fs, fs
```

## Other methods

- Feature Selection with Filtering Method- Constant, Quasi Constant and Duplicate Feature Removal
  - [Implementation video](https://www.youtube.com/watch?v=nPHU1CpX4jg&list=PLc2rvfiptPSQYzmDIFuq2PqN2n28ZjxDH&index=2)
- _for more detailed coding example follow this_ _[link](https://machinelearningmastery.com/feature-selection-with-categorical-data/)_.
- [Very good explanation video](https://www.youtube.com/watch?v=kA4mD3y4aqA&list=PLc2rvfiptPSQYzmDIFuq2PqN2n28ZjxDH)


**Reference:**

- [feature-selection-with-real-and-categorical-data](https://machinelearningmastery.com/feature-selection-with-real-and-categorical-data/)
- [an-introduction-to-feature-selection](https://machinelearningmastery.com/an-introduction-to-feature-selection/)
- [Feature Selection with Categorical Data](https://machinelearningmastery.com/feature-selection-with-categorical-data/)


<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Different Statistical tests for feature selection

## ANOVA

## CHI Square test

The chi-square independence test is a procedure for testing if two categorical variables are related in some population. 

- [blog](https://www.spss-tutorials.com/chi-square-independence-test/#what-is-it)

----

# What are the parameters in training a decision tree?

- `max_depth`: How deep the tree can be
- `min_samples_split`: Min number of samples needed to split a node
- `min_samples_leaf`: Min number of samples needed to be at the leaf node.
- `max_features`: Max number of features to consider when looking for the best split.

**Reference:**

- [source](http://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)

----

# What is the philosophy behind Decision Tree?

> Tree based methods involve stratifying or segmenting the Predictor space into number of region. 

- A decision tree is a tree where each `node` represents a feature(attribute), each `link`(branch) represents a decision(rule) and each `leaf` represents an outcome(categorical or continues value)
- Find the feature that best splits the target class into the `purest possible` children nodes (ie: nodes that don't contain a mix of both classes, rather pure nodes with only one class).
- `Entropy` on the other hand it is a measure of impurity. It is defined for a classification problem with N classes as:
- **Entropy:** $-\sum_i c_i * \log(c_i)$, where `i=1,...,N`

Say we have a dataset `D` and we are looking for a potential feature `f`, on which we will split the dataset w.r.t `f` into 2 parts `Dl` and `Dr` for left and right dataset respectively, such that those two datasets are at their purest form. Finally we use information gain to decide how good that feature is i.e how much pure the split is w.r.t `f` using `Information Gain`. 
    
```py
   Df
 /    \
Dl     Dr
```
- **Information Gain:** It is the difference of entropy before the split and after the split.
`EntropyBefore_f = Entropy(Df)` and entropy after is 
`EntropyAfter_f = Entropy(Dl)+Entropy(Dr)` and finaly 
`InformationGain_f = EntropyBefore_f - EntropyAfter_f`.

**Reference:**

- [SO](https://stackoverflow.com/questions/1859554/what-is-entropy-and-information-gain)
- [Medium](https://medium.com/deep-math-machine-learning-ai/chapter-4-decision-trees-algorithms-b93975f7a1f1)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>


----

# How to build decision tree?

There are couple of algorithms there to build a decision tree. Some of the important ones are

+ **CART (Classification and Regression Trees)** → uses Gini Index(Classification) as metric. Lower the Gini Index, higher the purity of the split.
+ **ID3 (Iterative Dichotomiser 3)** → uses Entropy function and Information gain as metrics. Higher the Information Gain, better the split is.

## What are the criteria for splitting at a node in decision trees ?

**Gini Index** [[link](https://dni-institute.in/blogs/cart-decision-tree-gini-index-explained/)]
  + CART uses Gini index as a split metric. For `N` classes, the Gini Index is defined as: 
  $1-\sum_i p_i^2$, where `i=1,...,N` and $p_i=p(target=i)$ [[source](https://medium.com/deep-math-machine-learning-ai/chapter-4-decision-trees-algorithms-b93975f7a1f1)]

**Information Gain**
  
  $$
  IG = Entropy(parent) - weight_{avg}*Entropy(children)
  $$

  $$
  Gain(S, A) = H(S) - \sum_{v \in values(A)} \frac{\vert S_v \vert}{\vert S \vert}H(S_v)
  $$

where, 

- $V$ - possible values of categorical feature $A$
- $S$ - set of examples $\{X\}$
- $S_v$ - subset where $X_A = V$

**Cross Entropy**
+ Cross-entropy loss, or log loss, measures the performance of a classification model whose output is a probability value between 0 and 1.
+ In binary classification, where the number of classes $M=2$, cross-entropy can be calculated as:
$-{(y\log(p) + (1 - y)\log(1 - p))}$
+ If $M \gt 2$ (i.e. multiclass classification), we calculate a separate loss for each class label per observation and sum the result.

$$
H(y, \hat{y}) = \sum_i y_i \log \frac{1}{\hat{y}_i} = -\sum_i y_i \log \hat{y}_i
$$

Entropy

**CHI Square:** It is an algorithm to find out the statistical significance between the differences between sub-nodes and parent node.

Reduction of Variance

**Reference:**

- [Mathisfun](https://www.mathsisfun.com/data/chi-square-test.html)
- [Medium blog](https://medium.com/greyatom/decision-trees-a-simple-way-to-visualize-a-decision-dc506a403aeb)
- [Decision tree](https://clearpredictions.com/Home/DecisionTree)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# What is the formula of Gini index criteria?

<center>
<img src="/assets/images/image_22_Tree_1.png" height="350" alt="image">
</center>


<center>
<img src="/assets/images/image_22_Tree_2.png" height="190" alt="image">
</center>

## How is it decided that on which features it has to split?

Based on for which feature the information gain is maximum.


**Reference:**

- [Gini index](http://dni-institute.in/blogs/cart-decision-tree-gini-index-explained/)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>


----

# What is the formula for Entropy criteria?

Entropy is nothing but `Expectation` with negative sign. 

Expectation Formula: $E[g(x)] = \sum p(x)g(x)$

In entropy, $g(x)$ is $\log (p(x))$, and combining with the negative sign (which is apparent as for $0 \leq x \leq 1$ , $\log (x)$ is negative), which makes it positive, the  entropy ( or expectation) formula becomes:

$$H[x] = -\sum p(x) \log (p(x))$$


<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

## How do you calculate information gain mathematically? 

- If `H` is the entropy of the original data D and it has undergone `N` splits for feature `f`, then Information Gain: 

$$IG(D,f) = H - \Sigma \frac{S_i}{S}H_i$$

where `i=1,...,N` and $S$ is the size of total datasets and $S_i$ is the size of the $i_{th}$ split data.  

**Reference:**

- [clear explanation, slides](https://www3.nd.edu/~rjohns15/cse40647.sp14/www/content/lectures/23%20-%20Decision%20Trees%202.pdf)


<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>


----

## Pros and Cons of Decision Trees:

<center>
<img src="/assets/images/image_22_Tree_3.png" height="350" alt="image">
</center>


<center>
<img src="/assets/images/image_22_Tree_4.png" height="230" alt="image">
</center>


- Decision Tree also suffers from `high variance`.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Philosophy behind Bagging?

- Say we have N independent observations $Z_1, \dots Z_N$, each with variance $\sigma^2$. Then the variance of the mean $\bar{Z}$ of the observation is given by $\sigma^2/n$. That is, averaging a set of observations reduce variance. 
- Hence a natural way to reduce the variance and hence increase the prediction accuracy of a statistical learning method is to take many training sets from the population, build a separate prediction model using each training set, and average the resulting predictions.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

-----

# What is KL Divergence?

- KL Divergence is the measure of `relative entropy`. It is a measure of the `distance between two distributions`. 
- In statistics, it arises as an `expected logarithm` of the likelihood ratio. 
  
The relative entropy  ${KL}(p\sim||\sim q)$ 
is a measure of the `inefficiency` of assuming that the distribution is q, when the true distribution is p. 

- The KL divergence from $p$ to $q$ is simply the difference between cross entropy and entropy:

$${KL}(y~||~\hat{y}) = \sum_i y_i \log \frac{1}{\hat{y}_i} - \sum_i y_i \log \frac{1}{y_i}= \sum_i y_i \log \frac{y_i}{\hat{y}_i}$$ 

Where $y_i \sim p$ and $\hat{y}_i \sim q$, i.e. they come from two different probability distribution.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----
# What is the advantage with random forest ?

- Random forest is an ensemble method in which a classifier is constructed by `combining several different Independent base classifiers`. 
- The independence is theoretically enforced by training each base classifier on a training set sampled with replacement from the original training set. 
- This technique is known as `bagging`, or `bootstrap aggregation`. 
- In Random Forest, further randomness is introduced by identifying the best split feature from a random subset of available features.
- `Reduction in overfitting`: by averaging several trees, there is a significantly lower risk of overfitting.
- `Less variance`: By using multiple trees, you reduce the chance of stumbling across a classifier that doesn’t perform well because of the relationship between the train and test data.

## Why ensemble is good?

Q: Suppose we have $10$ independent classifiers, each with error rate of $0.3$. What will be the final error rate if we ensemble these $10$ independent classifiers?

$\epsilon=0.3$

In this setting, the error rate of the ensemble can be computed as below: 

**Assumption:** We are taking a majority vote on the predictions. 

An ensemble makes a wrong prediction only when more than half of the base classifiers are wrong.

$$\epsilon_{ensemble}= \sum_{i=6}^{i=10} \binom{10}{i} \epsilon^i (1-\epsilon)^{10-i} \approx 0.05$$

It can be seen that with the theoretical guarantees stated above an ensemble model performs significantly well.

However in practice it is not possible to guarantee such classifier independence as they are trained from the same data, but still introduction of randomness helps achieve independence to a certain degree and it has been empirically observed that ensembles perform significantly well over individual base classifiers.


**Reference:**

- [Quora](https://www.quora.com/What-are-some-advantages-of-using-a-random-forest-over-a-decision-tree-given-that-a-decision-tree-is-simpler)



## Ensemble Learning algorithm

- [introduction-to-ensembling-along-with-implementation-in-r](https://www.analyticsvidhya.com/blog/2017/02/introduction-to-ensembling-along-with-implementation-in-r/)
- [introduction-ensemble-learning](https://www.analyticsvidhya.com/blog/2015/08/introduction-ensemble-learning/)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Characteristics of Different Learning Methods

<center>
<img src="/assets/images/image_22_Algorithms_1.png" height="450" alt="image">
</center>

MARS: Multivariate Adaptive Regression Splines 

**Reference:**

- [Book: ESL C10 P351]()

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Boosting algorithms

The term `Boosting` refers to a family of algorithms which **boosts** (i.e converts) weak learner to strong learners.

1. **AdaBoost (Adaptive Boosting)**

>> At the end of every model prediction we end up boosting the weights of the misclassified instances so that the next model does a better job on them, and so on.

Adaptive Boosting, or most commonly known AdaBoost, is a Boosting algorithm :rocket:. The method this algorithm uses to correct its predecessor is by **paying more attention to underfitted training instances by the previous model**.

<center>
<img src="https://miro.medium.com/max/850/0*paPv7vXuq4eBHZY7.png" width="500" alt="image">
</center>

2. **Gradient Tree Boosting:** It works by **sequentially adding the previous predictors underfitted predictions to the ensemble**, ensuring the erros made previously are corrected.

The difference lies in what it does with the underfitted values of its predecessor. 
- AdaBoost: Tweaks the instance weights at every interaction, 
- Gradient Boosting: Tries to **fit the new predictor to the residual errors** made by the previous predictor.

3. **XGBoost:** Extreme Gradient Boosting is an **advanced implementation of the Gradient Boosting**. This algorithm has **high predictive power** and is **ten times faster** than any other gradient boosting techniques. Moreover, includes a **variety of regularisation** which reduces overfitting and improves overall performance.

4. **Light GB:** For datasets which are extremely large Light Gradient Boosting is the best, compared to all of the other, since it takes less time to run.

- The motivation behind the `Boosting` algorithm is, there are `n weak classifiers`. Combining together gives a `powerful committee` who decides the final verdict of the classifier.
- A week classifier is one whose error rate is slightly better than `random guessing` 

**Resource:**

- [quick-introduction-boosting-algorithms-machine-learning](https://www.analyticsvidhya.com/blog/2015/11/quick-introduction-boosting-algorithms-machine-learning/)
- [Boosting-with-adaboost-and-gradient-boosting](https://medium.com/diogo-menezes-borges/boosting-with-adaboost-and-gradient-boosting-9cbab2a1af81)



<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Do you know about Adaboost algorithm ? How and why does it work ?

**AdaBoost:**

Also called Adaptive Boosting, where boosting is applied in a gradual way in the form of combining new learners on the misclassified data.

- First: A weak learner is applied and all the training examples, which are misclassified, are given higher weight. 
- Second: While building the dataset for training the next learner, the previously misclassified training examples will appear in the dataset (as high weight has been given to them). Now on this new dataset another learner is trained. Obviously this learner will correctly classify those previously misclassified examples plus some more misclassification in this step. 
- Repeat first and second.  


<center>
<img src="/assets/images/image_21_AdaBoost_1.png" width="500" alt="image">
</center>

<center>
<img src="/assets/images/image_21_AdaBoost_2.png" width="500" alt="image">
</center>

<center>
<img src="/assets/images/image_21_AdaBoost_3.png" width="500" alt="image">
</center>

- In AdaBoost you need to define a `base classifier`.
- `Classification Tree` acts as the best off the shelf base classifier for Adaboost.

**Resource:**

- [MIT Resource](http://math.mit.edu/~rothvoss/18.304.3PM/Presentations/1-Eric-Boosting304FinalRpdf.pdf)
- [Book: ESL, Chapter 10, Page 339]()

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>


----

# How does gradient boosting works ?
   
1.  Bagging and Boosting both are ensemble learning algorithm, where a collection of weak learner builds the strong learner. 
2. Bagging works on `re-sampling data with replacement` and create different dataset and the week learners are learnt on them, and final predictions are taken by 
averaging or majority voting. E.g. Random Forest.
    
**Bagging:** It is a simple ensembling technique in which we build many independent predictors/models/learners on sampled data with replacement from the original data (**Bootstrap Aggregatoin**) and combine them using some model averaging techniques. (e.g. weighted average, majority vote or normal average). E.g: `Random Forest`

**Boosting:** Also an ensemble learning method in which the predictors are not made independently, but sequentially. [[link](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d)]

   
   
**Gradient Boosting:** 

- Gradient Boosting is also a boosting algorithm(Duh!), hence it also tries to create a strong learner from an ensemble of weak learners. This is algorithm is similar to Adaptive Boosting (AdaBoost) but differs from it on certain aspects. In this method we try to `visualize the boosting problem as an optimization problem`, i.e we take up a loss function and try to optimise it. 
- We take up a weak learner(in previous case it was decision stump) and at each step, we add another weak learner to increase the performance and build a strong learner. This reduces the loss of the loss function. We iteratively add each model and compute the loss. The loss represents the error residuals(the difference between actual value and predicted value) and using this loss value the predictions are updated to minimise the residuals.
- **Important:** It learns a weak learner $F(x)+\epsilon_1$ ($\epsilon$ is the noise). Then on the noise, i.e. the residual, 
it builds another weak learner $H(x)+\epsilon_2$ and so on. Thus it becomes $F(x)+H(x)+G(x)+....+\epsilon$, where $\epsilon = \sum_i \epsilon_i$. Gradient boosting a sequential approach.


**Reference:**

- [link](https://www.analyticsvidhya.com/blog/2015/09/complete-guide-boosting-methods/)
- [Difference of Gradient Boosting and XGBoost](https://medium.com/hackernoon/gradient-boosting-and-xgboost-90862daa6c77)
- [Jeremy Howard: How to explain gradient boosting](https://explained.ai/gradient-boosting/index.html)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

## Difference of AdaBoost, Gradient Boost and XGBoost

Both AdaBoost and Gradient Boosting build weak learners in a sequential fashion. Originally, AdaBoost was designed in such a way that at every step the **sample distribution was adapted to put more weight on misclassified samples and less weight on correctly classified samples.** The final prediction is a weighted average of all the weak learners, where more weight is placed on stronger learners. 

AdaBoost can also be expressed as in terms of the more general framework of `additive models` with a particular loss function (the exponential loss) [_chapter 10 in (Hastie) ESL_]. AdaBoost.M1 (Algorithm 10.1, from the book) is equivalent to forward stagewise additive modeling (Algorithm 10.2) using the loss function 

$$
L(y, f(x)) = exp(-yf(x))
$$

>> In `Gradient Boosting`, **shortcomings** (of existing weak learners) are identified by gradients a.k.a **residuals**. In `Adaboost`, ‘shortcomings’ are identified by **high-weight data points**.


The **main differences** therefore are that Gradient Boosting is a generic algorithm to find approximate solutions to the additive modeling problem, while AdaBoost can be seen as a special case with a particular loss function. Hence, gradient boosting is much more flexible.

Second, AdaBoost can be interepted from a much more intuitive perspective and can be implemented without the reference to gradients by reweighting the training samples based on classifications from previous learners. 

**Reference:**

- [Quora: What-is-the-difference-between-gradient-boosting-and-adaboost](https://www.quora.com/What-is-the-difference-between-gradient-boosting-and-adaboost) 
- [Math Explanation: Imp_link](http://www.ccs.neu.edu/home/vip/teach/MLcourse/4_boosting/slides/gradient_boosting.pdf)
- [Stack Exchange](https://datascience.stackexchange.com/questions/39193/adaboost-vs-gradient-boosting)


### Bagging boosting difference:

- [link](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# XGBoost: Extreme Gradient Boosting 

XGBoost, a scalable machine learning system for tree boosting. The most important factor behind the success of XGBoost is its **scalability** in all scenarios. The system runs more than **ten times faster** than existing popular solutions on a single machine and **scales to billions of examples** in distributed or memory-limited settings. 

The scalability of XGBoost is due to several important systems and algorithmic optimizations. These innovations include: 

- A novel tree learning algorithm is for handling sparse data
- A theoretically justified weighted quantile sketch procedure enables handling instance weights in approximate tree learning. 
- Parallel and distributed computing makes learning faster which enables quicker model exploration.

## Objective Function

Read section $2.1$ and $2.2$ of the original XGBoost paper.

**Regularized Learning Objective**

For a given data set with $n$ examples and $m$ features
$D = \{(x_i, y_i)\}$ $( \vert D \vert = n, x_i \in \mathbb{R}^m, y_i \in \mathbb{R})$, a tree ensemble model uses $K$-**additive functions** to predict the output.

$$
\hat{y_i} = \phi(\mathbf{x_i}) = \sum\limits_{k=1}^K f_k(\mathbf{x_i})  
$$

where $f_k \in \mathcal{F}$. Also $\mathcal{F} = {f(\mathbf{x}) = w_{q(\mathbf{x})}}(q : \mathbb{R}^m \rightarrow T, w \in \mathbb{R}^T)$ is the space of regression trees (also known as CART). Here $q$ represents the structure of each tree that maps an example to the corresponding leaf index. $T$ is the number of leaves in the tree. Each $f_k$ corresponds to an independent tree structure $q$ and leaf weights $w$. Unlike decision trees, each regression tree contains a continuous score on each of the leaf, we use $w_i$ to represent score on $i^{th}$ leaf. For a given example, we will use the decision rules in the trees (given by q) to classify it into the leaves and calculate the final prediction by summing up the score in the corresponding leaves (given by $w$). To learn the set of functions used in the model, we minimize the following regularized objective. 

$$
\mathcal{L}(\phi) = \sum_i l(\hat{y_i}, y_i) + \sum_k \Omega(f_k)
$$


where $\Omega(f)= \gamma T + \frac{1}{2} \lambda \vert \vert w \vert \vert^2$

Here $l$ is a differentiable convex loss function that measures the difference between the prediction $\hat{y_i}$ and the target $y_i$. The second term $\Omega$ penalizes the complexity of the model (i.e., the regression tree functions). The additional regularization term helps to smooth the final learnt weights to avoid over-fitting. Intuitively, the regularized objective will tend to select a model employing simple and predictive functions. When the regularization parameter is set to zero, the objective falls back to the traditional gradient tree boosting.


**Gradient Tree Boosting**

The tree ensemble model in the above equation includes functions as parameters and cannot be optimized using traditional optimization methods in Euclidean space. Instead, the model is **trained in an additive manner**. Formally, let $\hat{y_i}$ be the prediction of the $i^{th}$ instance at the $t^{th}$ iteration, we will need to add $f_t$ to minimize the following objective.

$$
\mathcal{L}^{(t)} = \sum\limits_{i=1}^{n} l(\hat{y_i}, y_i^{(t-1)} + f_t(\mathbf{x_i})) + \Omega(f_t)
$$


This means we greedily add the ft that most improves our model according to the above equation. Second-order approximation can be used to quickly optimize the objective in the general setting.

Fore more in-depth math, read the section $2.2$ of the [paper](https://arxiv.org/pdf/1603.02754.pdf). 



## System design

**Column Block for Parallel Learning:**

The most time consuming part of tree learning is to get
the data into sorted order. In order to reduce the cost of sorting, we propose to store the data in in-memory units, which we called `block`. Data in each block is stored in the compressed column (`CSC`) format, with each column sorted by the corresponding feature value. This input data layout only needs to be computed once before training, and can be reused in later iterations.

**Cache-aware Access:** 

While the proposed block structure helps optimize the
computation complexity of split finding, the new algorithm requires indirect fetches of gradient statistics by row index, since these values are accessed in order of feature. This is a non-continuous memory access. A naive implementation of split enumeration introduces immediate read/write de- pendency between the accumulation and the non-continuous memory fetch operation. This slows down split finding when the gradient statistics do not fit into CPU cache and cache miss occur. For the exact greedy algorithm, we can alleviate the problem by a cache-aware prefetching algorithm. Specifically, we allocate an internal buffer in each thread, fetch the gradient statistics into it, and then perform accumulation in a mini-batch manner.

**Blocks for Out-of-core Computation:**

One goal of our system is to fully utilize a machine’s re-
sources to achieve scalable learning. Besides processors and memory, it is important to utilize disk space to handle data that does not fit into main memory. To enable out-of-core computation, we divide the data into multiple blocks and store each block on disk. During computation, it is important to use an independent thread to pre-fetch the block into a main memory buffer, so computation can happen in concurrence with disk reading.

- **Block Compression:** The first technique we use is block compression. The block is compressed by columns, and decompressed on the fly by an independent thread when loading into main memory. This helps to trade some of the computation in decompression with the disk reading cost.

- **Block Sharding:** The second technique is to shard the data onto multiple disks in an alternative manner. A pre-fetcher thread is assigned to each disk and fetches the data into an in-memory buffer. The training thread then alternatively reads the data from each buffer. This helps to increase the throughput of disk reading when multiple disks are available.

(_**SHARD:** A database shard is a horizontal partition of data in a database or search engine. Each individual partition is referred to as a shard or database shard. Each shard is held on a separate database server instance, to spread load._)

**Reference:**

- [arXiv: XGBoost: A Scalable Tree Boosting System](https://arxiv.org/pdf/1603.02754.pdf)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>


----


# Logistic Regression

## What is the loss function for logistic regression?

This is a very tricky question. 

**Case 1:**

When $y \in ({0,1})$

$$NLL(w) = - \sum_{i=1}^{N}[y_i log (\hat{y_i}) + (1-y_i)log (1-\hat{y_i})] $$

This is also called `cross entropy` error function.

**Case 2:**

When $y \in ({-1,+1})$

$$NLL(w)= \sum_{i=1}^{N}log(1+\exp(-y_iw^Tx_i))$$

Though these 2 equations look different. But if you pay attention for **case 1**, it's written in terms of $\hat{y_i}$ but for **case 2** there is no such term. Now what is $\hat{y_i}$ ??

$$
\hat{y_i} = \frac{exp(w^Tx_i)}{1+exp(w^Tx_i)}
$$

So if we substitute this in case 1, then we will find case 2. So they are same. 

![image](/assets/images/image_27_LR_loss_1.png)
![image](/assets/images/image_27_LR_loss_2.png)

![image](/assets/images/image_25_loss_2.png)

The above image shows, that how we approximate `0-1 loss` in SVM and in Logistic regression.

**Resource:**

- Probabilistic Perspective:  Murphy - Chapter 8.3.1
- [Very IMP, ML Course: Prof. Piyush Rai](https://www.cse.iitk.ac.in/users/piyush/courses/ml_autumn18/material/771_A18_lec9_print.pdf)
- [Very IMP, ML Course: Prof. Piyush Rai, slide 13](https://cse.iitk.ac.in/users/piyush/courses/ml_autumn16/771A_lec6_slides.pdf) 
 

## Comparison of SVM and Logistic Loss?

![image](/assets/images/image_27_LR_loss_3.png)

**Resource:**

- [Slide 15](https://www.robots.ox.ac.uk/~az/lectures/ml/2011/lect4.pdf)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>


---

# Why is logistic regression considered as a linear model? 

Q. Is it always necessary the decision boundary is linear / plane always?

The short answer is: Logistic regression is considered a generalized linear model because the outcome always depends on the sum of the inputs and parameters. Or in other words, the output cannot depend on the product (or quotient, etc.) of its parameters! $z = \sum_i w_ix_i$

$$f(x) = \frac{1}{1+e^{-\sum_i w_ix_i}}$$

The key is that our model is `additive`.  Our outcome z depends on the additivity of the weight parameter values, e.g., : $z = w_1x_1 + w_2x_2$

There’s no interaction between the weight parameter values,nothing like $w_1x_1 * w_2x_2$ or so, which would make our model non-linear!

However we can use non-linear feature s.t $z = \sum_i w_if(x_i)$ where $f()$ is a non linear function of $x$. But still z is linear in terms of parameter $w_i$

- In general the decision boundary is linear in `x`. To be more specific, the decision boundary in this case is given by $w^Tx=0$ (a hyperplane). But then you go on to say `but we can generate non-linear decision boundaries as well`.
- Well, of course you can, but then that'll be called a `non-linear instance` of logistic regression (the exact same way we have linear SVMs and non-linear SVMs). In other words, you can start with your original data x and see/decide that it's not linearly separable. What you can do next is introduce a feature transformation h(x) and use that in place of x. 
- For example, if you decide to apply a quadratic feature transformation on say for simplicity, your 2-dimensional data then h(x) in this case is simply given by
$h(x) = [x_1, x_2, x_1^2, x_2^2, x_1x_2]$
and your logistic model is now $y=f(w^Th(x))$ with the decision boundary given by $w^Th(x)=0$ (which is now a `non-linear quadratic curve` in the **original data space**).

**Resource**

- [logistic_regression_linear](https://sebastianraschka.com/faq/docs/logistic_regression_linear.html)
- [Quora](https://www.quora.com/Why-is-logistic-regression-considered-a-linear-model)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# SVM Summary:

A Support Vector Machine (SVM) performs classification by finding the hyperplane that maximizes the margin between the two classes. The vectors (cases) that define the hyperplane are the support vectors.

## Algorithm 		


1. Define an optimal hyperplane: maximize margin
2. Extend the above definition for non-linearly separable problems: have a penalty term for misclassifications.
3. Map data to high dimensional space where it is easier to classify with linear decision surfaces: reformulate problem so that data is mapped implicitly to this space.

<center>
<img src="https://saedsayad.com/images/SVM_optimize.png" alt="image" width="400"/>
</center>

<center>
<img src="https://saedsayad.com/images/SVM_optimize_1.png" alt="image" width="400" />
</center>

We find w and b by solving the following objective function using Quadratic Programming.

## Hard Margin

$$min \frac{1}{2}w^Tw$$ 

s.t $y_i(w.x_i+b)\ge 1, \forall x_i$ 

## Soft Margin

The beauty of SVM is that if the data is linearly separable, there is a unique global minimum value. An ideal SVM analysis should produce a hyperplane that completely separates the vectors (cases) into two non-overlapping classes. However, perfect separation may not be possible, or it may result in a model with so many cases that the model does not classify correctly. In this situation SVM finds the hyperplane that maximizes the margin and minimizes the misclassifications. 		
<center>
<img src="https://saedsayad.com/images/SVM_3.png" alt="image" width="400" />
</center>

<center>
<img src="https://saedsayad.com/images/SVM_optimize_3.png" alt="image" width="400"/>
</center>

The simplest way to separate two groups of data is with a straight line (1 dimension), flat plane (2 dimensions) or an N-dimensional hyperplane. However, there are situations where a nonlinear region can separate the groups more efficiently. SVM handles this by using a **kernel function** (nonlinear) to map the data into a different space where a hyperplane (linear) cannot be used to do the separation. 

- It means a non-linear function is learned by a linear learning machine in a high-dimensional **feature space** while the capacity of the system is controlled by a parameter that does not depend on the dimensionality of the space. This is called kernel trick which means the kernel function transform the data into a higher dimensional feature space to make it possible to perform the linear separation. 

- [Blog](https://saedsayad.com/support_vector_machine.htm)

## Formulate SVM with loss function and solve by gradient decent 

**Alternative question:** How do you adjust the cost parameter for the SVM regularizer? 

Regularization problems are typically formulated as optimization problems involving the desired objective(classification loss in our case) and a regularization penalty.The regularization penalty is used to help stabilize the minimization of the ob­jective or infuse prior knowledge we might have about desirable solutions.Many machine learning methods can be viewed as regularization methods in this manner.For later utility we will cast SVM optimization problem as a 
regularization problem.


Re write the soft margin problem using `hinge loss` $(z)$ defined as the positive part of $1-z$, written as $(1-z)^+$. The relaxed optimization problem (soft margin) can be reformulated as 

$$min \frac{1}{2}\vert \vert w \vert \vert^2 + C \sum\limits_{t=1}^{n}(1 - y_t(w^T x_t + w_0))^+ $$

Here $\frac{1}{2}\vert \vert w \vert \vert^2$, the `inverse squared` **geometric margin** is viewed as a regularization penalty that helps stabilizes the objective $C \sum\limits_{t=1}^{n}(1 - y_t(w^T x_t + w_0))^+$ . 

- [MIT OCW Notes](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-867-machine-learning-fall-2006/lecture-notes/lec4.pdf)

## What sort of optimization problem would you be solving to train a support vector machine?

- `Maximize Margin` (best answer), quadratic program, quadratic with linear constraints, reference to solving the primal or dual form.

## What are the kernels used in SVM ?

Kernel $K(X_i, X_j)$ are:
- Linear Kernel: $X_i.X_j$
- Polynomial Kernel: $(\gamma X_i.X_j + C)^d$
- RBF Kernel: $\exp (-\gamma\vert X_i - X_j\vert ^2)$
- Sigmoid Kernel: $\tanh(\gamma X_i.X_j + C)$

where $K(X_i, X_j) = \phi(X_i).\phi(X_j)$

that is, the kernel function, represents a dot product of input data points mapped into the higher dimensional feature
space by transformation $\phi(.)$

$\gamma$ is an adjustable parameter of certain kernel functions.

The `RBF` is by far the **most popular choice of kernel** types used in Support Vector Machines. This is mainly because of their localized and finite responses across the entire range of the real x-axis.

- [Blog](http://www.statsoft.com/Textbook/Support-Vector-Machines)

## What is the optimization technique of SVM?


<object data="https://drona.csa.iisc.ac.in/~shivani/Teaching/E0370/Aug-2011/Lectures/2.pdf" width="750px" height="750px">
    <embed src="https://drona.csa.iisc.ac.in/~shivani/Teaching/E0370/Aug-2011/Lectures/2.pdf" type="application/pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="https://drona.csa.iisc.ac.in/~shivani/Teaching/E0370/Aug-2011/Lectures/2.pdf">Download PDF</a>.</p>
    </embed>
</object>


## Why bring Lagrange Multiplier for solving the SVM problem?

- Constrained Optimization Problem easier to solve with Lagrange Multiplier
- The existing constraints will be replaced by the constraints of the Lagrange Multiplier, which are easier to handle
- By this `reformulation of the problem`, the data will appear only as `dot product`, which will be very helpful while `generalizing the SVM for non linearly separable class`. 

- [Youtube:Lagrange Multiplier Intuition](https://www.youtube.com/watch?v=yuqB-d5MjZA&list=PLg9_rXni6UXmjc7Cxw8HpYWRlFg72v15a&index=7)

## KKT Condition for SVM?

![image](/assets/images/image_25_svm_kkt_1.png)

- [Prof. SB IIT KGP, course](http://cse.iitkgp.ac.in/~sourangshu/coursefiles/ML15A/svm.pdf)
- [Imp Lecture Notes](http://www.csc.kth.se/utbildning/kth/kurser/DD3364/Lectures/KKT.pdf)


## Geometric analysis of Lagrangian, KKT, Dual

- [link](http://anie.me/Lagrangian-And-Dual-Problem/)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

### How does SVM learns non-linear boundaries ? Explain.

- Using `kernel trick`, it maps the examples from `input space` to `feature space`. 
In the higher dimension, they are separated linearly.


## SVM: Regularized Loss Function View

![image](/assets/images/image_25_svm_1.png)


<object data="https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-867-machine-learning-fall-2006/lecture-notes/lec4.pdf" type="application/pdf" width="750px" height="750px">
    <embed src="https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-867-machine-learning-fall-2006/lecture-notes/lec4.pdf" type="application/pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-867-machine-learning-fall-2006/lecture-notes/lec4.pdf">Download PDF</a>.</p>
    </embed>
</object>

**Resource:**

- [Prof. Piyush Rai, IIT K](https://www.cse.iitk.ac.in/users/piyush/courses/ml_autumn18/material/771_A18_lec11_print.pdf)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Constrained optimization (`Lagrangian`)

- **minimize** $f(x)$ such that **`g(x)<=0`** and $h(x)=0$. 
  - Our target is to bring a new equation where we will combine $f(x), g(x), h(x)$ in a single equation. We will do this by introducing Lagrange Multiplier $\lambda$ and $\mu$. The new equation looks like: $L(x,\lambda,\mu)=f(x)+\lambda g(x)+ \mu h(x)$.
+ **maximize** $f(x)$ such that **`g(x)>=0`** and $h(x)=0$. 
  + Our target is to bring a new equation where we will combine $f(x), g(x), h(x)$ in a single equation. We will do this by introducing Lagrange Multiplier $\lambda$ and $\mu$. The new equation looks like: $L(x,\lambda,\mu)=f(x)+\lambda g(x)+ \mu h(x)$.

**NOTE:** In the above formulation, pay special attention to the `minimize` and `maximize` kewords and the change in inequality constrains. So given any minimization or maximization problem, convert its constraints to $g(x)<=0$ or $g(x)>=0$ accordingly and then formulate the Lagrangian form. The $h(x)=0$ may or may not be there. Finally apply KKT conditions for finding the solution. 


![image](/assets/images/image_25_svm_lagrange_1.png)
![image](/assets/images/image_25_svm_lagrange_2.png)


**KKT Conditions:**
- Stationarity $\nabla_x L(x,\lambda,\mu)=0$
- Primal feasibility, $g(x)<=0$ (for minimization problem)
- Dual feasibility, $\lambda>=0, \mu>=0$
- Complementary slackness, $\lambda g(x) = 0$ and $\mu h(x)=0$

**Resource**

- [Prof. Piyush Rai, Lecture 10](https://www.cse.iitk.ac.in/users/piyush/courses/ml_autumn18/material/771_A18_lec10_print.pdf)
- [notes](http://mat.gsia.cmu.edu/classes/QUANT/NOTES/chap4.pdf)
- [sb slides](http://cse.iitkgp.ac.in/~sourangshu/coursefiles/ML15A/svm.pdf)
- [iitK_notes](https://www.cse.iitk.ac.in/users/rmittal/prev_course/s14/notes/lec11.pdf)
- [Khan Academy - Constrained Optimization](https://www.youtube.com/watch?v=vwUV2IDLP8Q&list=PLSQl0a2vh4HC5feHa6Rc5c0wbRTx56nF7&index=92)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Talking about unsupervised learning? What are the algorithms ?

- Clustering
- [K Means](http://www.saedsayad.com/clustering_kmeans.htm)
  - K-Means clustering intends to partition n objects into k clusters in which each object belongs to the cluster with the nearest mean. This method produces exactly k different clusters of greatest possible distinction. The best number of clusters k leading to the greatest separation (distance) is not known as a priori and must be computed from the data. The objective of K-Means clustering is to minimize total intra-cluster variance, or, the squared error function:
  - $J = \Sigma_j \Sigma_i \vert\vert x_i - c_j \vert\vert^2$ where `j=1,...,K` and `i=1,...,N`. `N` total number of observations and `K` total number of classes

![image](/assets/images/image_13_KMeans_1.png)
![image](/assets/images/image_13_KMeans_2.png)
![image](/assets/images/image_13_KMeans_3.png)
![image](/assets/images/image_13_KMeans_6.png)


**Resource:**

- [Prof. Piyush Rai, Lecture 13](https://www.cse.iitk.ac.in/users/piyush/courses/ml_autumn18/material/771_A18_lec13_print.pdf)
- [Prof. Piyush Rai, Lecture 13](https://www.cse.iitk.ac.in/users/piyush/courses/ml_autumn18/material/771_A18_lec14_print.pdf)


**Algorithm:**

1. Clusters the data into k groups where k  is predefined.
2. Select k points at random as cluster centers.
3. Assign objects to their closest cluster center according to the Euclidean distance function.
4. Calculate the centroid or mean of all objects in each cluster.
5. Repeat steps b, c and d until the same points are assigned to each cluster in consecutive rounds. 


- **Seed K-Means:** For seeding, i.e, to decide the insitial set of `K` centroids, use **K-Means++** algorithm. 
- K Medoids
- Agglomerative Clustering
  - Hierarchichal Clustering
  - Dimensionality Reduction
- PCA
- ICA

## How do you decide K in K-Means clustering algorithm ?Tell me at least 

3 ways of deciding K in clustering ?

- Elbow Method
- Average Silhouette Method
- Gap Statistics

**Reference:**

- [link1](http://www.sthda.com/english/articles/29-cluster-validation-essentials/96-determining-the-optimal-number-of-clusters-3-must-know-methods/)
- [link2](https://uc-r.github.io/kmeans_clustering)


## How do you `seed` k-means algorithm,i.e. how to decide the first `k` clusters?

- `k-means++` is an algorithm for choosing the initial values (or "seeds") for the k-means clustering algorithm. It was proposed in 2007 by David Arthur and Sergei Vassilvitskii, as an approximation algorithm for the `NP-hard k-means` problem - a way of avoiding the sometimes poor clustering found by the standard k-means algorithm.
- The intuition behind this approach is that spreading out the k initial cluster centers is a good thing: the first cluster center is chosen uniformly at random from the data points that are being clustered, after which each subsequent cluster center is chosen from the remaining data points with probability proportional to its squared distance from the point's closest existing cluster center.
- The exact algorithm is as follows:

**Algorithm**

1. Choose one center uniformly at random from among the data points.
2. For each data point x, compute D(x), the distance between x and the nearest center that has already been chosen.
3. Choose one new data point at random as a new center, using a weighted probability distribution where a point x is chosen with probability proportional to D(x)^2.
4. Repeat Steps 2 and 3 until k centers have been chosen.
5. Now that the initial centers have been chosen, proceed using standard k-means clustering.

**Reference:**

- [resource_wiki](https://en.wikipedia.org/wiki/K-means%2B%2B)
- [link2](https://datasciencelab.wordpress.com/2014/01/15/improved-seeding-for-clustering-with-k-means/)


## When K-means will fail?
- When data is **non linearly separable**. It works best when data clusters are discrete and spherically distributed.  

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# What other clustering algorithms do you know?

- [link](https://sites.google.com/site/dataclusteringalgorithms/)
- Unsupervised **linear clustering algorithm**
- **k-means clustering algorithm** [link](https://home.deib.polimi.it/matteucc/Clustering/tutorial_html/kmeans.html)
- Fuzzy c-means clustering algorithm
- **[Hierarchical clustering algorithm](https://sites.google.com/site/dataclusteringalgorithms/hierarchical-clustering-algorithm)**  
- Hierarchical Agglomerative Clustering (bottom up)
- Hierarchical DIvisive Clustering (top down)
- Gaussian(EM) clustering algorithm
- Quality threshold clustering algorithm      
- Unsupervised **non-linear clustering algorithm**
- **MST based clustering algorithm**
  - **Basic Idea:** Apply MST on the data points. Use the _Euclidean_ distance as the weight between two data points. After building the MST removes the longest edge, then the 2nd longest and so on. And thus clusters will be formed. [source](http://shodhganga.inflibnet.ac.in/bitstream/10603/9728/10/10_chapter%203.pdf)
- kernel k-means clustering algorithm
- Density based clustering algorithm
- **[DBSCAN](https://en.wikipedia.org/wiki/DBSCAN)**
- [code](https://plot.ly/scikit-learn/plot-dbscan/)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# What is DB-SCAN algorithm ?

- It is a density-based clustering algorithm: given a set of points in some space, it groups together points that are closely packed together (points with many nearby neighbors), marking as outliers points that lie alone in low-density regions
- A point p is a core point if at least `minPts` points are within distance `ε` (`ε` is the maximum radius of the neighborhood from p) of it (including p). Those points are said to be directly reachable from p.
- A point q is `directly reachable` from p if point q is within distance `ε` from point p and p must be a core point.
- A point q is `reachable` from p if there is a path `p1, ..., pn` with `p1 = p` and `pn = q`, where each `pi+1` is directly reachable from `pi` (all the points on the path must be core points, with the possible exception of q).
- All points not reachable from any other point are outliers. 

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# How does HAC (Hierarchical Agglomerative clustering) work ?
 
- [link1](https://newonlinecourses.science.psu.edu/stat505/node/143/)
- [link2](https://nlp.stanford.edu/IR-book/html/htmledition/hierarchical-agglomerative-clustering-1.html)
- [code](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# The Inductive Biases of Various Machine Learning Algorithms

That is, there is some fundamental assumption or set of assumptions that the learner makes about the target function that enables it to generalize beyond the training data

**Linear Regression**
- The relationship between the attributes x and the output y is linear. The goal is to minimize the sum of squared errors.

**Decision Trees**
- Shorter trees are preferred over longer trees. Trees that place high information gain attributes close to the root are preferred over those that do not.

**Single-Unit Perceptron:**
- Each input votes independently toward the final classification (interactions between inputs are not possible).

**Neural Networks with Backpropagation:**
- Smooth interpolation between data points.

**K-Nearest Neighbors:**
- The classification of an instance x will be most similar to the classification of other instances that are nearby in Euclidean distance.

**Support Vector Machines:**
- Distinct classes tend to be separated by wide margins.

**Naive Bayes:**
- Each input depends only on the output class or label; the inputs are independent from each other.

**Reference:**

- [Blog](http://www.lauradhamilton.com/inductive-biases-various-machine-learning-algorithms)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

---

# How do you deploy Machine Learning models ?

- Microservice
- Docker
- Kubernetes

Lot of times, we may have to write ML models from scratch in C++ ? Will you be able to do that?

+ [quora](https://www.quora.com/I-want-to-use-C++-to-learn-Machine-Learning-instead-of-Python-or-R-is-it-fine)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# How Stochastic Gradient Decent with momentum works?

+ An SGD can be thought of as a ball rolling down the hill where the velocity of the ball is influenced by the gradient of the 
curve. However, in this approach, the ball has a chance to get stuck in any ravine. So if the ball can have enough momentum to get past
get past the ravine would have been better. based on this idea, SGD with Momentum works. Where the ball has been given 
some added momentum which is based on the previous velocity and gradient. 
   + `velocity = momentum*past_velocity + learning_rate*gradient`
   +  `w=w-velocity`
   +  `past_velocity = velocity`
+ [easy explanation](http://ufldl.stanford.edu/tutorial/supervised/OptimizationStochasticGradientDescent/)
+ book: deep learning in Python - by Cholet, page 51, but the equation looks suspicious 
+ [through explanation, distill pub](https://distill.pub/2017/momentum/)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# How the model varies in KNN for K=1 and K=N?

- When K equals 1 or other small number the model is prone to `overfitting (high variance)`, while when K equals number of data points or other large number the model is prone to `underfitting (high bias)`.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

# Generative model vs Discriminative model.

- Discriminative algorithms model `P(y|x; w)`, that is, given the dataset and learned parameter, what is the probability of y belonging to a specific class. A discriminative algorithm doesn't care about how the data was generated, it simply categorizes a given example.
- Generative algorithms try to model `P(x|y)`, that is, the distribution of features given that it belongs to a certain class. A generative algorithm models how the data was generated. [source](https://github.com/ShuaiW/data-science-question-answer#knn)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

------

# Scenario based Question

Let’s say, you are given a scenario where you have terabytes of data files consisting of pdfs, text files, images, scanned pdfs etc. What approach will you take in understanding or classifying them ?

**Q.** How will you read the content of scanned pdfs or written documents in image formats?

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Why is naive bayes called “naive”? Tell me about naive bayes classifier?

- Because it's assumed that all the features are independent of each other. This is a very _naive_ assumption.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----


# Logistic Regression loss function?

+ [ml-cheatsheet quick summary](http://ml-cheatsheet.readthedocs.io/en/latest/logistic_regression.html)
+ [CMU ML slides](https://www.cs.cmu.edu/~mgormley/courses/10701-f16/slides/lecture5.pdf)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# What do you mean by mutable and immutable objects in python ?

+ [Everything in Python is an object](https://medium.com/@meghamohan/mutable-and-immutable-side-of-python-c2145cf72747). 

Since everything in Python is an Object, every variable holds an object instance. When an object is initiated, it is assigned a unique object id. Its type is 
defined at runtime and once set can never change, however its state can be changed if it is 
mutable. Simple put, a mutable object can be changed after it is created, and an immutable object can’t.

+ Objects of built-in types like (`int, float, complex, bool, str, tuple, unicode, frozen set`) are immutable. Objects of 
built-in types like (`list, set, dict,byte array`) are mutable. Custom classes are generally mutable.

## What are the data structures you have used in python ?

+ set,list,tuple,dictionary, string, frozen set. [link1](https://docs.python.org/3/tutorial/datastructures.html),
[link2](http://thomas-cokelaer.info/tutorials/python/data_structures.html) 

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

------

# How do you handle multi-class classification with unbalanced dataset ?

+ [link1](https://www.linkedin.com/pulse/multi-class-classification-imbalanced-data-using-random-burak-ozen/)
+ handling imbalanced data by resampling original data to provide balanced classes. 
    + Random under sampling
    + Random over sampling
    + Cluster based over sampling
    + Informed Over Sampling: Synthetic Minority Over-sampling Technique (**SMOTE**)
+ Modifying existing classification algorithms to make them appropriate for imbalanced data sets.
   + Bagging based
   + Boosting based: AdaBoost, Gradient Boost, XGBoost
+ [imp source](https://www.analyticsvidhya.com/blog/2017/03/imbalanced-classification-problem/)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----


# How do you select between 2 models (Model Selection techniques)?

To choose between 2 model generally `AIC` or `BIC` are used.
Generally, the most commonly used metrics, for measuring regression model quality and for comparing models, are: `Adjusted R2`, `AIC`, `BIC` and `Cp`.

- **AIC** stands for (Akaike’s Information Criteria), a metric developped by the Japanese Statistician, Hirotugu Akaike, 1970. The basic idea of AIC is to `penalize the inclusion of additional variables` to a model. It adds a penalty that increases the error when including additional terms. `The lower the AIC, the better the model.`
`AICc` is a version of AIC corrected for small sample sizes.
- **BIC** (or Bayesian information criteria) is a variant of AIC with a stronger penalty for including additional variables to the model.
- **Mallows Cp:** A variant of AIC developed by Colin Mallows
- $R^2$ not a good criterion.Always increase with model size –> `optimum` is to take the biggest model.
- `Adjusted` $R^2$: better. It `penalized` bigger models.



**Reference:**

- [Blog](http://www.sthda.com/english/articles/38-regression-model-validation/158-regression-model-accuracy-metrics-r-square-aic-bic-cp-and-more/)
- [Blog](https://www.sciencedirect.com/topics/medicine-and-dentistry/akaike-information-criterion)
- [ppt-Stanford](https://statweb.stanford.edu/~jtaylo/courses/stats203/notes/selection.pdf)


## How does it work mathematically? Explain the intuition behind BIC or AIC ?

In general, it might be best to use AIC and BIC together in model selection. 
- For example, in selecting the number of latent classes in a model, if BIC points to a three-class model and AIC points to a five-class model, it makes sense to select from models with 3, 4 and 5 latent classes.
   
- `AIC is better in situations when a false negative finding would be considered more misleading than a false positive`, 
- `BIC is better in situations where a false positive is as misleading as, or more misleading than, a false negative`.


$$AIC=-2\log L(\hat \theta) + 2k$$

where, 

- $\theta$= the set (vector) of model parameters
- $L(\theta)$ =  the  likelihood  of  the  candidate  model  given  the  data  when  evaluated at the maximum likelihood estimate of $\theta$
- $k$ = the number of estimated parameters in the candidate model

The first compo-nent, $−2\log L(\hat \theta)$, is the value of the likelihood function, $\log L(\theta)$, which is the probability of obtaining the data given the candidate model.
The  more  parameters,  the  greater the amount added to the first component, increasing the value for the AIC and penalizing the model. 

**BIC** is  another  model  selection  criterion  based  on  infor-mation theory but set within a Bayesian context. The difference between the BIC and the AIC is the greater penalty imposed for the number of param-eters  by  the BIC  than the AIC.

$$BIC=-2\log L(\hat \theta) + k \log n$$

**Reference:**

- [Blog](https://www.methodology.psu.edu/resources/aic-vs-bic/)
- [Paper](https://onlinelibrary.wiley.com/doi/pdf/10.1002/9781118856406.app5)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

-----

# What is precision and recall? Which one of this do you think is important in medical diagnosis?

## Type I and Type II Errors

>> One fine morning, Jack got a phone call. It was a stranger on the line. Jack, still sipping his freshly brewed morning coffee, was barely in a position to understand what was coming for him. The stranger said, “Congratulations Jack! You have won a lottery of $10 Million! I just need you to provide me your bank account details, and the money will be deposited in your bank account right way…”

What are the odds of that happening? What should Jack do? What would you have done?

<img src="https://miro.medium.com/max/454/1*t_t7cMq3FGqDk6gbwfA4EA.png" alt="image" width="400"/>

- **Type I: False Positive**
- **Type II: False Negative**

<img src="https://miro.medium.com/max/700/1*pOtBHai4jFd-ujaNXPilRg.png" alt="image" width="600"/>

- [blog](https://towardsdatascience.com/precision-vs-recall-386cf9f89488)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# ROC Curve Analysis

## ROC curve

An ROC curve (receiver operating characteristic curve) is a graph showing the performance of a classification model at all classification thresholds. This curve plots two parameters:
- True Positive Rate:
  - True Positive Rate (TPR) is a synonym for recall and is therefore defined as follows:

  $$TPR = \frac{TP} {TP + FN}$$

- False Positive Rate

$$FPR = \frac{FP} {FP + TN}$$

To compute the points in an ROC curve, we could evaluate a logistic regression model many times with different classification thresholds, but this would be inefficient. Fortunately, there's an efficient, sorting-based algorithm that can provide this information for us, called AUC.

## AUC: Area Under the ROC Curve

AUC stands for "Area under the ROC Curve." That is, AUC measures the entire two-dimensional area underneath the entire ROC curve (think integral calculus) from (0,0) to (1,1).

AUC provides an aggregate measure of performance across all possible classification thresholds. One way of interpreting AUC is as the probability that the model ranks a random positive example more highly than a random negative example. 
[(source)](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc)


## What does AUC-ROC curve signify ?

AUC - ROC curve is a performance measurement for classification problem `at various thresholds settings`. ROC is a **probability curve** and AUC represents **degree or measure of separability**. It tells how much model is capable of distinguishing between classes. Higher the AUC, better the model is at predicting 0s as 0s and 1s as 1s. By analogy, Higher the AUC, better the model is at distinguishing between patients with disease and no disease.

### How do you draw AUC-ROC curve ?

<img src="https://miro.medium.com/max/700/1*k65OKy7TOhBWRIfx0u6JqA.png" alt="image" width="600"/>

<img src="https://miro.medium.com/max/700/1*hf2fRUKfD-hCSw1ifUOCpg.png" alt="image" width="600"/>

- `True positive` is the area designated as “bad” on the right side of the threshold (mild sky blue region). `False positive` denotes the area designated as “good” on the right of the threshold. 
- `Total positive` is the total area under the “bad” curve while total negative is the total area under the “good” curve.
- We divide the value as shown in the diagram to derive TPR and FPR. 
- We derive the TPR and FPR at different threshold values (by sliding the black vertical bar in the above image) to get the ROC curve. Using this knowledge, we create the ROC plot function.

**Bonus Question:** Write pseudo-code to generate the data for such a curve. [Check the below blog]

- [Imp Blog](https://towardsdatascience.com/receiver-operating-characteristic-curves-demystified-in-python-bd531a4364d0)

The ROC curve is plotted with `TPR` against the `FPR` where TPR is on y-axis and FPR is on the x-axis.

<img src="https://miro.medium.com/max/361/1*pk05QGzoWhCgRiiFbz-oKQ.png" alt="image" width="250"/>

- `TPR == RECALL`

$$\frac{TP}{TP+FN}$$

- Specificity:

$$\frac{TN}{TN+FP}$$

- `FPR == 1-Specificity`

$$\frac{FP}{TN+FP}$$

### How will you draw ROC for multi class classification problem

In multi-class model, we can plot N number of AUC ROC Curves for N number classes using One vs ALL methodology. So for Example, If you have three classes named X, Y and Z, you will have one ROC for X classified against Y and Z, another ROC for Y classified against X and Z, and a third one of Z classified against Y and X.

**Reference**

- [Blog](https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5)
- [ICML 2004: Tutorial on Many Faces of ROC Analysis in Machine Learning](http://people.cs.bris.ac.uk/~flach/ICML04tutorial//)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# What is random about Random Forest?

+ For different `tree`, a different datasets (build from original using _random resampling with replacement_) are given as input.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Metric to measure multi-class classification result?

We can generalize all the binary performance metrics such as precision, recall, and F1-score etc. to multi-class settings. In the binary case, we have:

![image](https://sebastianraschka.com/images/faq/multiclass-metric/conf_mat.png)
![image](https://sebastianraschka.com/images/faq/multiclass-metric/pre-rec.png)
![image](https://sebastianraschka.com/images/faq/multiclass-metric/mcc.png)

 And to generalize this to `multi-class`, assuming we have a `One-vs-All` (OvA) classifier, we can either go with the **micro average** or the **macro average**. 
 
 - `Micro averaging`: we’d calculate the performance, e.g., precision, from the `individual` (assuming One-vs-All) true positives, true negatives, false positives, and false negatives of the the k-class model:

 ![image](https://sebastianraschka.com/images/faq/multiclass-metric/micro.png)

`Macro-averaging`: We average the performances of each individual class.

![image](https://sebastianraschka.com/images/faq/multiclass-metric/macro.png)

**Reference:**

- [Sebastian Rachka](https://sebastianraschka.com/faq/docs/multiclass-metric.html)

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# How is using a logistic regression different from using a random forest ?
  
+ If your data is linearly separable, go with logistic regression. However, in real world, data is rarely linearly separable. Most of the time data would be a jumbled mess.
In such scenarios, Decision trees would be a better fit as DT essentially is a non-linear classifier. As DT is prone to over fitting, Random Forests are used in practice to better generalize the fitment. RF provide a good balance between precision and overfitting.
+ If your problem/data is linearly separable, then first try logistic regression. If you don’t know, then still start with logistic regression because that will be your baseline, followed by non-linear classifier such as random forest. Do not forget to tune the parameters of logistic regression / random forest for maximizing their performance on your data.
+ If your data is categorical, then random forest should be your first choice; however, logistic regression can be dealt with categorical data.
+ If you want to understand results easily, logistic regression is a better choice because it leads to simple interpretation of the explanatory variables.
+ If `speed` is your criteria, then `logistic regression` should be your choice.
+ If your `data` is `unbalanced`, then `random forest` may be a better choice.
+ If number of data objects are less than the number of features, logistic regression should not be used.
+ Lastly, as noted in this paper, either of the random forest or logistic regression “models appear to perform similarly across the datasets with performance more influenced by choice of dataset rather than model selection”

**Reference:**

- [Quora](https://www.quora.com/When-should-random-forest-be-used-over-logistic-regression-for-classification-and-vice-versa)


<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----


# Which model would you use in case of unbalanced dataset: Random Forest or Boosting ? Why ?

+ Gradient boosting is also a good choice here. You can use the gradient boosting classifier in sci-kit learn for example. Gradient boosting is a principled method of dealing with class imbalance by constructing successive training sets based on incorrectly classified examples.
+ [link1](https://www.analyticsvidhya.com/blog/2017/03/imbalanced-classification-problem/)
+ An alternative could be a cost-sensitive algorithm like C5.0 that doesn't need balanced data. You could also think about applying Markov chains to your problem.

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# How to prepare for ML Interview?

> In general, for an interview that you think will be machine learning focused, I would make sure you knew the following techniques, and an approach you would use for each, and how they are different from each other:

- Regression
- Classification
- Ranking
- Recommendation
- Clustering
- Unsupervised Learning

# Question source: 

- [link_1](https://appliedmachinelearning.wordpress.com/2018/04/13/my-data-science-machine-learning-job-interview-experience-list-of-ds-ml-dl-questions/) 

<a href="#Top" style="color:#2F4F4F;background-color: #c8f7e4;float: right;">Content</a>

----

# Exercise 

1. What is sensitivity and specificity ?
2. **Name the package of scikit-learn that implements logistic regression** ?
3. What is mean and variance of standard normal distribution ?
4. **What is central limit theoram**?
5. **Law of Large Number**?
6. What are the data structures you have used in python ?
 
7.  What is naive bayes classifier ?
8.  What is the probability of heads coming 4 times when a coin is tossed 10 number of times ?
9.  **How do you get an index of an element of a list in python** ?
    + [link1](https://stackoverflow.com/questions/176918/finding-the-index-of-an-item-given-a-list-containing-it-in-python)
10. How do you merge two data-set with pandas?

```py
frames = [df1, df2, df3]
result = pd.concat(frames)
```
+ [link1](https://pandas.pydata.org/pandas-docs/stable/merging.html)

11. From user behavior, you need to model fraudulent activity. How are you going to solve this ? May be anomaly detection problem or a classification problem !!
12. What will you prefer a decision tree or a random forest ?

13. Will you use decision tree or random forest for a classification problem ? What is advantage of using  random forest ?
14. 1. What are the boosting techniques you know ?
15. **Which model would you like to choose if you have many classes in a supervised learning problem ? Say 40-50 classes !!**
16. How do you perform ensemble technique?
17. How does SVM work ?
18. What is Kernel ? Explain a few.
19. **How do you perform non-linear regression**?
   + [link1](http://www.statisticshowto.com/nonlinear-regression/)
20. What are Lasso and Ridge regression ?
21. What is Gaussian Mixture model ? How does it perform clustering ?
22. How is Expectation Maximization performed ? Explain both the steps ?
23. How is likelihood calculated in GMM ?

----

<a href="#Top" style="color:#023628;background-color: #f7d06a;float: right;">Back to Top</a>