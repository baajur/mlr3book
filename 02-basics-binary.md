## Binary classification {#binary-classification}

Classification problems with a target variable containing only two classes are called "binary".
For such binary target variables, you can specify the *positive class* within the [`classification task`](https://mlr3.mlr-org.com/reference/TaskClassif.html) object during task creation.
If not explicitly set during construction, the positive class defaults to the first level of the target variable.


```r
# during construction
data("Sonar", package = "mlbench")
task = TaskClassif$new(id = "Sonar", Sonar, target = "Class", positive = "R")

# switch positive class to level 'M'
task$positive = "M"
```

### ROC Curve and Thresholds {#binary-roc}

ROC Analysis, which stands for "receiver operating characteristics", is a subfield of machine learning which studies the evaluation of binary prediction systems.
We saw earlier that one can retrieve the confusion matrix of a [`Prediction`](https://mlr3.mlr-org.com/reference/Prediction.html) by accessing the `$confusion` field:


```r
learner = lrn("classif.rpart", predict_type = "prob")
pred = learner$train(task)$predict(task)
C = pred$confusion
print(C)
```

```
##         truth
## response  M  R
##        M 95 10
##        R 16 87
```

The confusion matrix contains the counts of correct and incorrect class assignments, grouped by class labels.
The columns illustrate the true (observed) labels and the rows display the predicted labels.
The positive is always the first row or column in the confusion matrix.
Thus, the element in $C_{11}$ is the number of times our model predicted the positive class and was right about it.
Analogously, the element in $C_{22}$ is the number of times our model predicted the negative class and was also right about it.
The elements on the diagonal are called True Positives (TP) and True Negatives (TN).
The element $C_{12}$ is the number of times we falsely predicted a positive label, and is called False Positives (FP).
The element $C_{21}$ is called False Negatives (FN).

We can now normalize in rows and columns of the confusion matrix to derive several informative metrics:

* **True Positive Rate (TPR)**: How many of the true positives did we predict as positive?
* **True Negative Rate (TNR)**: How many of the true negatives did we predict as negative?
* **Positive Predictive Value PPV**: If we predict positive how likely is it a true positive?
* **Negative Predictive Value NPV**: If we predict negative how likely is it a true negative?



\begin{center}\includegraphics[width=0.98\linewidth]{images/confusion_matrix} \end{center}

Source: [Wikipedia](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers)


It is difficult to achieve a high TPR and low FPR in conjunction, so one uses them for constructing the ROC Curve.
We characterize a classifier by its TPR and FPR values and plot them in a coordinate system.
The best classifier lies on the top-left corner.
The worst classifier lies at the diagonal.
<!-- FIXME Why is the best classifier on the top-left? The argument here only shows that classifiers at the diagonal are inferior but there is no argument in place, illustrating why the other (top left) classifiers are superior. -->
Classifiers lying on the diagonal produce random labels (with different proportions).
If each positive $x$ will be randomly classified with 25\% as "positive", we get a TPR of 0.25.
If we assign each negative $x$ randomly to "positive" we get a FPR of 0.25.
In practice, we should never obtain a classifier below the diagonal, as inverting the predicted labels will result in a reflection at the diagonal.
<!-- FIXME Why is that reflection bad as such ? One sentence to elaborate would add here-->

A scoring classifier is a model which produces scores or probabilities, instead of discrete labels.
To obtain probabilities from a learner in mlr3, you have to set `predict_type = "prob"` for a `ref("LearnerClassif")`.
Whether a classifier can predict probabilities is given in its `$predict_types` field.
Thresholding flexibly converts measured probabilities to labels.
Predict $1$ (positive class) if $\hat{f}(x) > \tau$ else predict $0$.
Normally, one could use $\tau = 0.5$ to convert probabilities to labels, but for imbalanced or cost-sensitive situations another threshold could be more suitable.
After thresholding, any metric defined on labels can be used.

For `mlr3` prediction objects, the ROC curve can easily be created with [mlr3viz](https://mlr3viz.mlr-org.com) which relies on the [precrec](https://cran.r-project.org/package=precrec) to calculate and plot ROC curves:


```r
library("mlr3viz")

# TPR vs FPR / Sensitivity vs (1 - Specificity)
ggplot2::autoplot(pred, type = "roc")
```



\begin{center}\includegraphics{02-basics-binary_files/figure-latex/02-basics-binary-004-1} \end{center}

```r
# Precision vs Recall
ggplot2::autoplot(pred, type = "prc")
```



\begin{center}\includegraphics{02-basics-binary_files/figure-latex/02-basics-binary-004-2} \end{center}

### Threshold Tuning

<!--
When we are interested in class labels based on scores or probabilities, we can set the classification threshold according to our target performance measure.
This threshold however can also be **tuned**, since the optimal threshold might differ for different (custom) measures or in situations like const-sensitive classification.

This can be also done with `mlr3`.
-->
