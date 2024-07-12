### Questions

How is XGBoost handling bias-variance tradeoff?

# Solution

**Difficulty: Medium** 

**Summary:** 

XGBoost is a popular choice for predictive modeling due to its ability to train on a variety of datasets. Whether your dataset is a mix of categorical, ordinal, or numeric data, even with missing fields, or if you have a nice and tidy numeric dataset, XGBoost has proven to perform well in both cases, making it an extremely versatile tool on a data scientist’s belt.

### Versatility as Bias-Variance Handling

What does it mean for a predictive model to be versatile? At face value, it means we can expect it to train and achieve reasonable accuracy in both validation and testing (not just training) in a wide set of circumstances. 

Training a model introduces a tradeoff between bias and variance, or a tradeoff between model complexity and predictive power. As we increase model complexity (or decrease bias) we risk worsening the predictive power of the model on unseen data. From another perspective, we can recognize the worse predictive power as greater variance in the final parameters of a given model.

For any given dataset and choice of predictive model, there is going to be an optimal equilibrium between the bias and the variance. This optimal point is subject to change based on your use case for the model, so this equilibrium is better understood conceptually as there aren’t simple statistical checks for this in a lot of cases. In any case, we always want a model to learn the general trends of a training set without getting bogged down in idiosyncrasies, as that results in overfitting which can decimate the predictive power of the model. On the other hand, indexing too much into predictive power backfires as the model learns too little from the training data, and often results in a severely underfit model that performs worse than random guessing.

XGBoost succeeds the most of any often used model amongst a lot of datasets, and that’s because almost every parameter that exists in the model serves this balancing act - they either enable the model to learn more from the training data, or they somehow restrict its complexity. This is exactly how one would create an algorithmic approach to balancing overfitting and underfitting - you construct a model that is capable of severe overfitting through its parameters, and then you add another class of (hyper)parameters to limit the complexity introduced by the former set of parameters.

There are well over 100 parameters in the XGBoost module, so if you want to learn about the ones that are relevant for bias-variance handling, I recommend referring to the docs, starting with the [Parameters for Tree Booster](https://xgboost.readthedocs.io/en/stable/parameter.html#parameters-for-tree-booster). Then it would be valuable to check out [Learning Task Parameters](https://xgboost.readthedocs.io/en/stable/parameter.html#learning-task-parameters), located on the same page. All of the parameters on the page are relevant for building performant models, but these two sections often have the biggest lift.

### Tree Booster Parameters

These parameters dictate the structure of the decision trees internal to the model before training, giving global limits to model complexity. A couple of parameters worth knowing:

- `max_depth`: maximum depth of a tree. A tree with a higher depth will learn more nuances of the training dataset, but will be subject to overfitting via increased bias.
- `colsample_bytree`: percentage of columns used for each tree construction. This percentage informs how many features of a dataset will be shown to any given decision tree in model training. Tuning this value down can minimize overfitting by intermittently excluding some training features.

### Learning Task Parameters

Once the model training begins, there are parameters that dictate the way a model converges to its final values. Consider the following hyperparameters:

- `eta`: Perhaps better known by its alias `learning_rate`, this parameter informs step size shrinkage used in update to prevents overfitting. By lowering this value, you can minimize the feature weight updates, creating a more conservative model training.
- `gamma`: minimum loss reduction required to make a further partition on a leaf node of the tree. Lower values make for a more conservative model, while higher values increase the regularization.

In conclusion, we’ve discussed how XGBoost is a staple model in any data science application, and that success in part is due to its wide array of hyperparameters that can be used to optimize the bias-variance tradeoff. Finally we talked about Tree Booster and Learning Task hyperparameters, which offer tuning methods that can push a model towards higher bias or higher variance, depending on the values used.
