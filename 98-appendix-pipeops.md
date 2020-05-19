## Integrated Pipe Operators {#list-pipeops}



\begin{tabular}{l|l|l|l}
\hline
Id & Packages & Train & Predict\\
\hline
[`boxcox`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_boxcox.html) & [bestNormalize](https://cran.r-project.org/package=bestNormalize) & Task \$
\hline
[`branch`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_branch.html) &  & * \$
\hline
[`chunk`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_chunk.html) &  & Task \$
\hline
[`classbalancing`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_classbalancing.html) &  & TaskClassif \$
\hline
[`classifavg`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_classifavg.html) & [stats](https://cran.r-project.org/package=stats) & NULL \$
\hline
[`classweights`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_classweights.html) &  & TaskClassif \$
\hline
[`colapply`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_colapply.html) &  & Task \$
\hline
[`collapsefactors`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_collapsefactors.html) &  & Task \$
\hline
[`copy`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_copy.html) &  & * \$
\hline
[`crankcompose`](https://mlr3proba.mlr-org.com/reference/PipeOpCrankCompositor.html) & [distr6](https://cran.r-project.org/package=distr6) & NULL \$
\hline
[`distrcompose`](https://mlr3proba.mlr-org.com/reference/PipeOpDistrCompositor.html) & [distr6](https://cran.r-project.org/package=distr6) & NULL, NULL \$
\hline
[`encode`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_encode.html) & [stats](https://cran.r-project.org/package=stats) & Task \$
\hline
[`encodeimpact`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_encodeimpact.html) &  & Task \$
\hline
[`encodelmer`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_encodelmer.html) & [lme4](https://cran.r-project.org/package=lme4), [nloptr](https://cran.r-project.org/package=nloptr) & Task \$
\hline
[`featureunion`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_featureunion.html) &  & Task \$
\hline
[`filter`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_filter.html) &  & Task \$
\hline
[`fixfactors`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_fixfactors.html) &  & Task \$
\hline
[`histbin`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_histbin.html) & [graphics](https://cran.r-project.org/package=graphics) & Task \$
\hline
[`ica`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_ica.html) & [fastICA](https://cran.r-project.org/package=fastICA) & Task \$
\hline
[`imputehist`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_imputehist.html) & [graphics](https://cran.r-project.org/package=graphics) & Task \$
\hline
[`imputemean`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_imputemean.html) &  & Task \$
\hline
[`imputemedian`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_imputemedian.html) & [stats](https://cran.r-project.org/package=stats) & Task \$
\hline
[`imputenewlvl`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_imputenewlvl.html) &  & Task \$
\hline
[`imputesample`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_imputesample.html) &  & Task \$
\hline
[`kernelpca`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_kernelpca.html) & [kernlab](https://cran.r-project.org/package=kernlab) & Task \$
\hline
[`learner`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_learner.html) &  & TaskClassif \$
\hline
[`learner\_cv`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_learner\_cv.html) &  & TaskClassif \$
\hline
[`missind`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_missind.html) &  & Task \$
\hline
[`modelmatrix`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_modelmatrix.html) & [stats](https://cran.r-project.org/package=stats) & Task \$
\hline
[`mutate`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_mutate.html) &  & Task \$
\hline
[`nop`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_nop.html) &  & * \$
\hline
[`pca`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_pca.html) &  & Task \$
\hline
[`quantilebin`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_quantilebin.html) & [stats](https://cran.r-project.org/package=stats) & Task \$
\hline
[`regravg`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_regravg.html) &  & NULL \$
\hline
[`removeconstants`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_removeconstants.html) &  & Task \$
\hline
[`scale`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_scale.html) &  & Task \$
\hline
[`scalemaxabs`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_scalemaxabs.html) &  & Task \$
\hline
[`scalerange`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_scalerange.html) &  & Task \$
\hline
[`select`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_select.html) &  & Task \$
\hline
[`smote`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_smote.html) & [smotefamily](https://cran.r-project.org/package=smotefamily) & Task \$
\hline
[`spatialsign`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_spatialsign.html) &  & Task \$
\hline
[`subsample`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_subsample.html) &  & Task \$
\hline
[`unbranch`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_unbranch.html) &  & * \$
\hline
[`yeojohnson`](https://mlr3pipelines.mlr-org.com/reference/mlr\_pipeops\_yeojohnson.html) & [bestNormalize](https://cran.r-project.org/package=bestNormalize) & Task \$
\hline
\end{tabular}