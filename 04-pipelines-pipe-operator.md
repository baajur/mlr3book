## The Pipeline Operator: `%>>%` {#pipe-operator}



It is possible to create intricate `Graphs` with edges going all over the place (as long as no loops are introduced).
Irrespective, there is usually a clear direction of flow between "layers" in the `Graph`.
It is therefore convenient to build up a `Graph` from layers.
This can be done using the **`%>>%`** ("double-arrow") operator.
It takes either a `PipeOp` or a `Graph` on each of its sides and connects all of the outputs of its left-hand side to one of the inputs each of its right-hand side.
The number of inputs therefore must match the number of outputs.


```r
library("magrittr")

gr = mlr_pipeops$get("scale") %>>% mlr_pipeops$get("pca")
gr$plot(html = FALSE)
```



\begin{center}\includegraphics{04-pipelines-pipe-operator_files/figure-latex/04-pipelines-pipe-operator-002-1} \end{center}