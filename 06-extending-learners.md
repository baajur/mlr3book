## Adding new Learners {#extending-learners}

Here, we show how to create a custom mlr3learner step-by-step.
If you want to add a new learner, please follow the instructions on how to ["Add a new learner"](https://github.com/mlr-org/mlr3learners#requestingadding-additional-learners).

**Resources for adding a new learner (summary)**

- [mlr3learners.template](https://github.com/mlr-org/mlr3learners.template)
- [mlr3book section "Adding new learners" including FAQ](https://mlr3book.mlr-org.com/extending-learners.html)
- [Checklist prior to requesting a review](https://github.com/mlr-org/mlr3learners.template/issues/5)

This section gives insights on how a mlr3learner is constructed and how to troubleshoot issues.
See the [Learner FAQ subsection](#learner-faq) for help.

**(Do not copy/paste the code shown in this section. Use the {mlr3learners.template} to start.)**


```r
Learner<type><algorithm> = R6Class("Learner<type><algorithm>",
  inherit = Learner<type>,

  public = list(

    #' @description
    #' Creates a new instance of this [R6][R6::R6Class] class.
    initialize = function() {
      ps = ParamSet$new(
        params = list(
          <params>
        )
      )

      super$initialize(
        id = "<type>.<algorithm>",
        packages = "<package>",
        feature_types = "<feature types>"
        predict_types = "<predict types>"
        param_set = ps,
        properties = "<properties>",
        man = "<pkgname>::<help file name>"
      )
    },

    # optional
    importance = function() {

    },

    # optional
    oob_error = function() {

    }
  ),

  private = list(

    .train = function()
    .predict = function()
```

In the first line of the template, we create a new [R6](https://cran.r-project.org/package=R6) class of the learner.
The next line determines the parent class:
Depending on which type of learner we are creating, we need o inherit from its parent class, e.g. for a classification learner from [`LearnerClassif`](https://mlr3.mlr-org.com/reference/LearnerClassif.html).

A learner consists of the following parts:

1. [Meta information](#learner-meta-information) about the learners
1. [`.train()`](#learner-train) which takes a (filtered) [`TaskClassif`](https://mlr3.mlr-org.com/reference/TaskClassif.html) and returns a model
1. [`.predict()`](#learner-predict) which operates on the model in `self$model` (stored during `$train()`) and a (differently subsetted) [`TaskClassif`](https://mlr3.mlr-org.com/reference/TaskClassif.html) to return a named list of  predictions.
1. (optional) A public `importance()` or `oob_error()` method if the learner supports this.

### Meta-information {#learner-meta-information}

In the constructor (`initialize()`) the constructor of the super class (e.g. [`LearnerClassif`](https://mlr3.mlr-org.com/reference/LearnerClassif.html)) is called with meta information about the learner which should be constructed.
This includes:

* `id`: The ID of the new learner. Usually consists of `<type>.<algorithm>`, for example: `"classif.C5.0"`.
* `packages`: The upstream package name of the implemented learner.
* `param_set`: A set of hyperparameters and their descriptions provided as a [`paradox::ParamSet`](https://paradox.mlr-org.com/reference/ParamSet.html).
  For each hyperparameter the appropriate class needs to be chosen:
  * [`paradox::ParamLgl`](https://paradox.mlr-org.com/reference/ParamLgl.html) for scalar logical hyperparameters.
  * [`paradox::ParamInt`](https://paradox.mlr-org.com/reference/ParamInt.html) for scalar integer hyperparameters.
  * [`paradox::ParamDbl`](https://paradox.mlr-org.com/reference/ParamDbl.html) for scalar numeric hyperparameters.
  * [`paradox::ParamFct`](https://paradox.mlr-org.com/reference/ParamFct.html) for scalar factor hyperparameters (this includes characters).
  * [`paradox::ParamUty`](https://paradox.mlr-org.com/reference/ParamUty.html) for everything else (e.g. vector paramters or list parameters).
* `predict_types`: Set of predict types the learner is able to handle.
  These differ depending on the type of the learner.
  * `LearnerClassif`
    * `response`: Only predicts a class label for each observation in the test set.
    * `prob`: Also predicts the posterior probability for each class for each observation in the test set.
  * `LearnerRegr`
    * `response`: Only predicts a numeric response for each observation in the test set.
    * `se`: Also predicts the standard error for each value of response for each observation in the test set.
* `feature_types`: Set of feature types the learner is able to handle.
  See [`mlr_reflections$task_feature_types`](https://mlr3.mlr-org.com/reference/mlr_reflections.html) for feature types supported by `mlr3`.
* `properties`: Set of properties of the learner. Possible properties include:
  * `"twoclass"`: The learner works on binary classification problems.
  * `"multiclass"`: The learner works on multi-class classification problems.
  * `"missings"`: The learner can natively handle missing values.
  * `"weights"`: The learner can work on tasks which have observation weights / case weights.
  * `"parallel"`: The learner supports internal parallelization in some way.
    Currently not used, this is an experimental property.
  * `"importance"`: The learner supports extracting importance values for features.
    If this property is set, you must also implement a public method `importance()` to retrieve the importance values from the model.
  * `"selected_features"`: The learner supports extracting the features which where used.
    If this property is set, you must also implement a public method `selected_features()` to retrieve the set of used features from the model.
* `man`: The roxygen identifier of the learner.
  This is used within the `$help()` method of the super class to open the help page of the learner.
  This argument follows the structure `"mlr3learners.<package>::mlr_learners_<type>.<algorithm>"`.

For a simplified [`rpart::rpart()`](https://www.rdocumentation.org/packages/rpart/topics/rpart), the initialization could look like this:


```r
initialize = function(id = "classif.rpart") {
    ps = ParamSet$new(list(
      ParamDbl$new(id = "cp", default = 0.01, lower = 0, upper = 1, tags = "train"),
      ParamInt$new(id = "xval", default = 10L, lower = 0L, tags = "train")
    ))

    super$initialize(
        id = id,
        packages = "rpart",
        feature_types = c("logical", "integer", "numeric", "factor"),
        predict_types = c("response", "prob"),
        param_set = ps,
        properties = c("twoclass", "multiclass", "weights", "missings")
        man = "mlr3learners.rpart::mlr_learners_classif.rpart"
    )
}
```

We only have specified a small subset of the available hyperparameters:

* The complexity `"cp"` is numeric, has a feasible range of `[0,1]` and defaults to `0.01`.
  The parameter is used during `"train"`.
* The complexity `"xval"` is integer has a lower bound of `0`, a default of `0` and the parameter is used during `"train"`.

### Train function {#learner-train}

Let's talk about the `.train()` method.
The train function takes a [`Task`](https://mlr3.mlr-org.com/reference/Task.html) as input and must return a model.

Let's say we want to translate the following call of `rpart::rpart()` into code that can be used inside the `.train()` method.

First, we write something down that works completely without `mlr3`:


```r
data = iris
model = rpart::rpart(Species ~ ., data = iris, xval = 0)
```

We need to pass the formula notation `Species ~ .`, the data and the hyperparameters.
To get the hyperparameters, we call `self$param_set$get_values()` and query all parameters that are using during `"train"`.

The dataset is extracted from the [`Task`](https://mlr3.mlr-org.com/reference/Task.html).

Last, we call the upstream function `rpart::rpart()` with the data and pass all hyperparameters via argument `.args` using the `mlr3misc::invoke()` function.
The latter is simply an optimized version of `do.call()` that we use within the mlr3 ecosystem.


```r
.train = function(task) {
  pars = self$param_set$get_values(tags = "train")
  mlr3misc::invoke(rpart::rpart, task$formula(),
    data = task$data(), .args = pars)
}
```

### Predict function {#learner-predict}

The internal predict method `.predict()` also operates on a [`Task`](https://mlr3.mlr-org.com/reference/Task.html) as well as on the fitted model that has been created by the `train()` call previously and has been stored in `self$model`.

The return value is a [`Prediction`](https://mlr3.mlr-org.com/reference/Prediction.html) object.
We proceed analogously to what we did in the previous section.
We start with a version without any `mlr3` objects and continue to replace objects until we have reached the desired interface:


```r
# inputs:
task = tsk("iris")
self = list(model = rpart::rpart(task$formula(), data = task$data()))

data = iris
response = predict(self$model, newdata = data, type = "class")
prob = predict(self$model, newdata = data, type = "prob")
```

The [`rpart::predict.rpart()`](https://www.rdocumentation.org/packages/rpart/topics/predict.rpart) function predicts class labels if argument `type` is set to to `"class"`, and class probabilities if set to `"prob"`.

Next, we transition from `data` to a `task` again and construct a proper [`PredictionClassif`](https://mlr3.mlr-org.com/reference/PredictionClassif.html) object which should be returned.
Additionally, as we do not want to run the prediction twice, we differentiate what type of prediction is requested by querying the chosen predict type of the learner.
The predict type is stored in the `$predict_type` slot of a learner class.

The final `.predict()` method looks like this:


```r
.predict = function(task) {
  self$predict_type = "response"

  if (self$predict_type == "response") {
    response = predict(self$model, newdata = task$data(), type = "class")
  } else {
    prob = predict(self$model, newdata = task$data(), type = "prob")
  }

  PredictionClassif$new(task, response = response, prob = prob)
}
```

Note that if the learner would need to handle hyperparameters during the predict step, we would proceed analogously to the `.train()` step and use `self$params$get_values(tags = "predict")` in combination with [`mlr3misc::invoke()`](https://mlr3misc.mlr-org.com/reference/invoke.html).

Also note that you cannot rely on the column order of the data returned by `task$data()`: The order of columns may be different from the order of the columns during `$train()`.
You have to make sure that your learner accesses columns by name, not by position (like some algorithms with a matrix interface do).
You may have to restore the order manually here, see learner ["classif.svm"](https://github.com/mlr-org/mlr3learners/blob/master/R/LearnerClassifSVM.R) for an example.

### Control objects/functions of learners {#learner-control}

Some learners rely on a "control" object/function such as `glmnet::glmnet.control()`.
Accounting for such depends on how the underlying package works:

- If the package forwards the control parameters via `...` and makes it possible to just pass control parameters as additional parameters directly to the train call, there is no need to distinguish both `"train"` and `"control"` parameters.
  Both can be tagged with "train" in the ParamSet and just be handed over as shown previously.
- If the control parameters need to be passed via a separate argument, the parameters should also be tagged accordingly in the ParamSet.
  Afterwards they can be queried via their tag and passed separately to `mlr3misc::invoke()`.
  See example below.

```r
control_pars = mlr3misc::(<package>::<function>,
   self$param_set$get_values(tags = "control"))

train_pars = self$param_set$get_values(tags = "train"))

mlr3misc::invoke([...], .args = train_pars, control = control_pars)
```

### Testing the learner {#learner-test}

#### Train and Predict

For a bare-bone check you can just try to run a simple `train()` call locally.


```r
task = tsk("iris") # assuming a Classif learner
lrn$train(task)
p = lrn$predict(task)
p$confusion
```

To ensure that your learners is able to handle all kinds of different properties and feature types, we have written an "autotest" that checks the learner for different combinations of such.

The "autotest" setup is already included in the mlr3learners.template skeleton.
For some learners that have required parameters, it is needed to set some values for required parameters after construction so that the learner can be run in the first place.

You can also exclude some specific test arrangements within the "autotest" via argument `exclude` in the `run_autotest()` function.
Currently the `run_autotest()` function lives in [inst/testthat](https://github.com/mlr-org/mlr3/blob/f16326bf34bcac59c3b0a2fdbcf90dbebb3b4bbc/inst/testthat/helper_autotest.R) of the `mlr_plkg("mlr3")` and still lacks documentation.
This should change in the near future.

To finally run the test suite, call `devtools::test()` or hit `CTRL + Shift + T` if you are using RStudio.

#### Checking Parameters

Some learners have a high number of parameters and it is easy to miss out on some during the creation of a new learner.
In addition, if the maintainer of the upstream package changes something with respect to the arguments of the algorithm, the learner is in danger to break.
Also, new arguments could be added upstream and manually checking for new additions all the time is tedious.

Therefore we have written a "Parameter Check" that runs for every learner asynchronously to the R CMD Check of the package itself.
This "Parameter Check" compares the parameters of the mlr3 ParamSet against all arguments available in the upstream function that is called during `$train()`.

It comes with an `exclude` argument that should be used to _exclude and explain_ why certain arguments of the upstream function are not within the ParamSet of the mlr3learner.
This argument needs essentially to be used by every learner because arguments like `x`, `target` or `data` are handled by the mlr3 interface and are therefore not included within the ParamSet.

However, there might be more parameters that need to be excluded:

- Type dependent parameters, i.e. parameters that only apply for classification or regression learners.
- Parameters that are actually deprecated by the upstream package and which were therefore not included in the mlr3 ParamSet.

The "Parameter Check" should run for each implemented learner.
All excluded parameters should have a comment justifying their exclusion.

Whenever the "Parameter Check" breaks in the CI run, maintainers can be sure that a new parameter was added or removed in the upstream package.
Also, people taking over a learner package can instantly see why certain parameters were not included into the ParamSet initially.

### Learner FAQ {#learner-faq}

**Question 1**

How to deal with Parameters which have no default?

**Answer**

Do not set a default in the ParamSet and add `tags = "required"` to the Parameter.

**Question 2**

Where to add the package of the upstream package in the DESCRIPTION file?

Add it to the "Imports" section.
This will install the upstream package during the installation of the mlr3learner if it has not yet been installed by the user.

**Question 3**

How to handle arguments from external "control" functions such as `glmnet::glmnet_control()`?

**Answer**

See ["Control objects/functions of learners"](https://mlr3book.mlr-org.com/extending-learners.html#learner-control).

**Question 4**

How to document if my learner uses a custom default value that differs to the default of the upstream package?

**Answer**

If you set a custom default for the mlr3learner that does not cope with the one of the upstream package (think twice if this is really needed!), add this information to the help page of the respective learner.

You can use the following skeleton for this:

```r
#' @section Custom mlr3 defaults:
#' - `<parameter>`:
#'   - Actual default: <value>
#'   - Adjusted default: <value>
#'   - Reason for change: <text>
```

**Question 5**

When should the `"required"` tag be used when defining Params and what is its purpose?

**Answer**

The `"required"` tag should be used when the following conditions are met:

- The upstream function cannot be run without setting this parameter, i.e. it would throw an error.
- The parameter has no default in the upstream function.

In mlr3 we follow the principle that every learner should be constructable without setting custom parameters.
Therefore, if a parameter has no default in the upstream function, a custom value is usually set for this parameter in the mlr3learner (remember to document such changes in the help page of the learner).

Even though this practice ensures that no parameter is unset in an mlr3learner and partially removes the usefulness of the `"required"` tag, the tag is still useful in the following scenario:

If a user sets custom parameters after construction of the learner

```r
lrn = lrn("<id>")
lrn$param_set$values = list("<param>" = <value>)
```

Here, all parameters besides the ones set in the list would be unset.
See `paradox::ParamSet` for more information.
If a parameter is tagged as `"required"` in the ParamSet, the call above would error and prompt the user that required parameters are missing.
