## Logging {#logging}

We use the [lgr](https://cran.r-project.org/package=lgr) package for logging and progress output.

### Changing mlr3 logging levels

To change the setting for [mlr3](https://mlr3.mlr-org.com) for the current session, you need to retrieve the logger (which is a [R6](https://cran.r-project.org/package=R6) object) from [lgr](https://cran.r-project.org/package=lgr), and then change the threshold of the like this:


```r
requireNamespace("lgr")

logger = lgr::get_logger("mlr3")
logger$set_threshold("<level>")
```

The default log level is `"info"`.
All available levels can be listed as follows:


```r
getOption("lgr.log_levels")
```

```
## fatal error  warn  info debug trace 
##   100   200   300   400   500   600
```

To increase verbosity, set the log level to a higher value, e.g. to `"debug"` with:

```r
lgr::get_logger("mlr3")$set_threshold("debug")
```

To reduce the verbosity, reduce the log level to warn:


```r
lgr::get_logger("mlr3")$set_threshold("warn")
```

[lgr](https://cran.r-project.org/package=lgr) comes with a global option called `"lgr.default_threshold"` which can be set via `options()` to make your choice permanent across sessions.

Also note that extension packages such as [mlr3tuning](https://mlr3tuning.mlr-org.com) define their own logger which inherits from the [mlr3](https://mlr3.mlr-org.com) logger.
To disable the output from [mlr3](https://mlr3.mlr-org.com), but keep the output from [mlr3tuning](https://mlr3tuning.mlr-org.com), first reduce the verbosity for the parent logger [mlr3](https://mlr3.mlr-org.com), then restore verbosity for the child logger [mlr3tuning](https://mlr3tuning.mlr-org.com):

```r
lgr::get_logger("mlr3")$set_threshold("warn")
lgr::get_logger("mlr3tuning")$set_threshold("info")
```

### Redirecting output

Redirecting output is already extensively covered in the documentation and vignette of [lgr](https://cran.r-project.org/package=lgr).
Here is just a short example which adds an additional appender to log events into a temporary file in [JSON](https://en.wikipedia.org/wiki/JSON) format:

```r
tf = tempfile("mlr3log_", fileext = ".json")

# get the logger as R6 object
logger = lgr::get_logger("mlr")

# add Json appender
logger$add_appender(lgr::AppenderJson$new(tf), name = "json")

# signal a warning
logger$warn("this is a warning from mlr3")
```

```
## WARN  [01:32:07.102] this is a warning from mlr3
```

```r
# print the contents of the file
cat(readLines(tf))
```

```
## {"level":300,"timestamp":"2020-05-19 01:32:07","logger":"mlr","caller":"eval","msg":"this is a warning from mlr3"}
```

```r
# remove the appender again
logger$remove_appender("json")
```