

# 功能化函数编程`purrr` {#tidyverse_purrr}

`purrr`是R进行函数化编程(functional programing)的包.

## 安装

`purrr`包在`tidyverse`包中,所以可以直接通过安装`tidyverse`包进行安装.当然,也可以单独安装.


```r
# The easiest way to get purrr is to install the whole tidyverse:
install.packages("tidyverse")

# Alternatively, install just purrr:
install.packages("purrr")

# Or the the development version from GitHub:
# install.packages("devtools")
devtools::install_github("tidyverse/purrr")
```

## `map`函数系列

`purrr`包提供了很多`map`系列的函数,跟R的base apply家族函数有点像,都是对向量化的对象的每一个元素进行处理.`map(.x, .f)`,其中`.x`是向量化的对象,比如向量,list等,然后对每一个元素使用`.f`函数进行处理,得到结果之后会返回一个跟原来`.x`长度相同的对象.返回对象的格式跟`map`函数的后缀有关,比如`_lgl`,`_chr()`等.

> 在R中,向量包括两类,atomic和list,二者区别在于前者的元素类型必须相同,而后者可以不同,前者的代表为向量和矩阵,而后者为list和数据框.

比如`map()`函数,用法如下:



```r
map(.x, .f, ...)
```

`.x`是第一个参数,是一个list或者一个`atomic vector`.`.f`是一个函数,一个公式或者是向量.`...`则是函数的其他参数.

我们举个例子:


```r
library(tidyverse)
#> ── Attaching core tidyverse packages ──── tidyverse 2.0.0 ──
#> ✔ dplyr     1.1.2     ✔ readr     2.1.4
#> ✔ forcats   1.0.0     ✔ stringr   1.5.0
#> ✔ ggplot2   3.4.2     ✔ tibble    3.2.1
#> ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
#> ✔ purrr     1.0.1     
#> ── Conflicts ────────────────────── tidyverse_conflicts() ──
#> ✖ dplyr::filter() masks stats::filter()
#> ✖ dplyr::lag()    masks stats::lag()
#> ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
1:3 %>%
  map(rnorm, mean = 3)
#> [[1]]
#> [1] 5.245912
#> 
#> [[2]]
#> [1] 1.528066 2.986734
#> 
#> [[3]]
#> [1] 3.7782173 3.1711538 0.5922197
```

这时候`.x`是一个向量.函数是`rnorm`.如果不对函数进行特别说明,则向量的每个元素默认都是函数的第一个参数,然后后面的参数是其他参数.如果在后面对第一参数进行设置,这时候前面的向量则按顺序向后移动.比如:


```r
1:3 %>%
  map(rnorm, n = 3)
#> [[1]]
#> [1]  1.770773 -0.883499  2.069894
#> 
#> [[2]]
#> [1] 1.795669 1.890548 1.202950
#> 
#> [[3]]
#> [1] 1.207715 1.400057 3.971091
```

这时候,因为在后面设置了`n = 3`,因此这时候向量就是`rnorm`的第二个参数mean了.

对于比较复杂的参数,不太推荐这样的写法,更推荐使用匿名函数,然后将参数位置进行固定:


```r
map(.x = 1:3, .f = function(x) {rnorm(n = 3, mean = x)})
#> [[1]]
#> [1] 2.445230 1.761792 2.122510
#> 
#> [[2]]
#> [1] 2.321425 1.726930 2.684708
#> 
#> [[3]]
#> [1] 4.116747 3.490825 3.264808
```

当然,也可以是一个公式:


```r
map(.x = 1:3, ~ rnorm(n = 3, mean = .x))
#> [[1]]
#> [1] 0.7689384 0.9160738 0.5829471
#> 
#> [[2]]
#> [1] 0.7865884 1.4290659 1.6078502
#> 
#> [[3]]
#> [1] 3.703991 3.724092 2.606857
```

这时候参数需要使用`.x`作为占位符.

对于数据框来说,其实也可以看做是一个list,每一列就是一个list中的一个元素.


```r
df <- matrix(1:16, nrow = 4) %>% 
  as.data.frame()
df
#>   V1 V2 V3 V4
#> 1  1  5  9 13
#> 2  2  6 10 14
#> 3  3  7 11 15
#> 4  4  8 12 16
map(.x = df, .f = mean)
#> $V1
#> [1] 2.5
#> 
#> $V2
#> [1] 6.5
#> 
#> $V3
#> [1] 10.5
#> 
#> $V4
#> [1] 14.5
```

也可以做一些复杂的运算,比如进行scale.


```r
df <- matrix(1:16, nrow = 4) %>% 
  as.data.frame()
df
#>   V1 V2 V3 V4
#> 1  1  5  9 13
#> 2  2  6 10 14
#> 3  3  7 11 15
#> 4  4  8 12 16
map(.x = df, .f = function(x) {(x - mean(x)) / sd(x)})
#> $V1
#> [1] -1.1618950 -0.3872983  0.3872983  1.1618950
#> 
#> $V2
#> [1] -1.1618950 -0.3872983  0.3872983  1.1618950
#> 
#> $V3
#> [1] -1.1618950 -0.3872983  0.3872983  1.1618950
#> 
#> $V4
#> [1] -1.1618950 -0.3872983  0.3872983  1.1618950
```


```r
set_names(c("foo", "bar")) %>% map(paste0, ":suffix")
#> $foo
#> [1] "foo:suffix"
#> 
#> $bar
#> [1] "bar:suffix"
```

从上面的可以看到,使用`map`函数,最后返回的结果毕竟是一个list.如果想要返回其他的类型呢?这时候需要使用`map_`加上后缀名的函数,在`purrr`中一共有以下几种:

* `map_lgl()`, `map_int()`, `map_dbl()`和`map_chr()`返回的分别是logical,integer,dbl和character类型的数据.


```r
l1 <- list(list(a = 1L), list(a = NULL, b = 2L), list(b = 3L))
map(.x = l1, .f = "a", .default = "???")
#> [[1]]
#> [1] 1
#> 
#> [[2]]
#> [1] "???"
#> 
#> [[3]]
#> [1] "???"
```

另外,函数还可以是一个列名或者向量某个元素的名字,也就是使用位置参数对其进行提取.比如上面的例子,就是使用函数"a",也就是提取对象中名字为"a"的元素的名字,其实也就是函数`[[`,如果没有该元素,则使用函数`.default`的值进行填充.当然,还可以使用位置来进行提取.


```r
l1 %>% map_int("b", .default = NA)
#> [1] NA  2  3
l1 %>% map_int(2, .default = NA)
#> [1] NA  2 NA
```



```r
data <- 
mtcars %>%
  split(.$cyl)

data <- map(.x = data, .f = ~ lm(mpg ~ wt, data = .x))
  
data <- map(.x = data, .f = summary)
  
map(.x = data, .f = "r.squared")
#> $`4`
#> [1] 0.5086326
#> 
#> $`6`
#> [1] 0.4645102
#> 
#> $`8`
#> [1] 0.4229655
map_dbl(.x = data, .f = "r.squared")
#>         4         6         8 
#> 0.5086326 0.4645102 0.4229655
```

所以,可以看出来,其实这几个函数都是在`map`的基础上对返回的数值进行一定的修饰得到不同类型结果而已.

* `map_dfr()`和`map_dfc()`函数.

这两个函数其实就是将`map()`返回的list对象整理为data frame对象.分别按照行和列进行整合.

举个例子:


```r
df <- matrix(sample(1:20), nrow = 4) %>% 
  as.data.frame()
df
#>   V1 V2 V3 V4 V5
#> 1  6  8  9 19 17
#> 2 14 11  1 12 16
#> 3 10  7  4  3  5
#> 4 18 20  2 13 15
map(.x = df, .f = function(x) {(x - mean(x)) / sd(x)})
#> $V1
#> [1] -1.1618950  0.3872983 -0.3872983  1.1618950
#> 
#> $V2
#> [1] -0.59160798 -0.08451543 -0.76063883  1.43676223
#> 
#> $V3
#> [1]  1.4048787 -0.8429272  0.0000000 -0.5619515
#> 
#> $V4
#> [1]  1.09819076  0.03786865 -1.32540264  0.18934323
#> 
#> $V5
#> [1]  0.6744270  0.4945798 -1.4837394  0.3147326
map_dfr(.x = df, .f = function(x) {(x - mean(x)) / sd(x)})
#> # A tibble: 4 × 5
#>       V1      V2     V3      V4     V5
#>    <dbl>   <dbl>  <dbl>   <dbl>  <dbl>
#> 1 -1.16  -0.592   1.40   1.10    0.674
#> 2  0.387 -0.0845 -0.843  0.0379  0.495
#> 3 -0.387 -0.761   0     -1.33   -1.48 
#> 4  1.16   1.44   -0.562  0.189   0.315
map_dfc(.x = df, .f = function(x) {(x - mean(x)) / sd(x)})
#> # A tibble: 4 × 5
#>       V1      V2     V3      V4     V5
#>    <dbl>   <dbl>  <dbl>   <dbl>  <dbl>
#> 1 -1.16  -0.592   1.40   1.10    0.674
#> 2  0.387 -0.0845 -0.843  0.0379  0.495
#> 3 -0.387 -0.761   0     -1.33   -1.48 
#> 4  1.16   1.44   -0.562  0.189   0.315
```

* `walk()`函数,称之为游走函数

当使用函数的目的是向屏幕提供输出或将文件保存到磁盘——重要的是操作过程而不是返回值，我们应该使用游走函数，而不是映射函数。

比如最简单的例子:


```r
walk(.x = 1:10, .f = print)
#> [1] 1
#> [1] 2
#> [1] 3
#> [1] 4
#> [1] 5
#> [1] 6
#> [1] 7
#> [1] 8
#> [1] 9
#> [1] 10
```

另外一种是保存文件到本地.这时候就可以使用`walk`函数.


## `map`变种函数

和`map()`函数类似,`purrr`提供了一系列`map`函数的变种函数.

### `map2`系列函数

`map2()`可以多提供一个输入变量.然后对其同时进行处理.

用法如下:


```r
map2(.x, .y, .f, ...)

map2_lgl(.x, .y, .f, ...)

map2_int(.x, .y, .f, ...)

map2_dbl(.x, .y, .f, ...)

map2_chr(.x, .y, .f, ...)

map2_raw(.x, .y, .f, ...)

map2_dfr(.x, .y, .f, ..., .id = NULL)

map2_dfc(.x, .y, .f, ...)
```

`.x`和`.y`是两个长度相等的向量.如果其中一个长度为1,那么会对其进行循环.`.f`还是对向量进行处理的函数.

举个例子:


```r
x <- list(1, 10, 100)
y <- list(1, 2, 3)
z <- list(5, 50, 500)
map2(.x = x, .y = y, .f = ~ .x + .y)
#> [[1]]
#> [1] 2
#> 
#> [[2]]
#> [1] 12
#> 
#> [[3]]
#> [1] 103
map2(.x = x, .y = y, .f = `+`)
#> [[1]]
#> [1] 2
#> 
#> [[2]]
#> [1] 12
#> 
#> [[3]]
#> [1] 103
```


### `pmap`和`pwalk`系列函数

`pmap()`和`pwalk()`比较特殊,他们用法如下:


```r
pmap(.l, .f, ...)

pmap_lgl(.l, .f, ...)

pmap_int(.l, .f, ...)

pmap_dbl(.l, .f, ...)

pmap_chr(.l, .f, ...)

pmap_raw(.l, .f, ...)

pmap_dfr(.l, .f, ..., .id = NULL)

pmap_dfc(.l, .f, ...)

pwalk(.l, .f, ...)
```

其中`.l`是指一个列表(list).长度可以为任意值.如果输入的是一个data frame,那么他们是按照行进行处理的.这时候需要和`map`系列函数区分,以为他们是按照列进行处理的.

比如下面的例子:


```r
pmap(.l = list(x, y, z), .f = sum)
#> [[1]]
#> [1] 7
#> 
#> [[2]]
#> [1] 62
#> 
#> [[3]]
#> [1] 603
```

分别将x,y和z的第一个元素,第二个元素和第三个元素相加得到最终的结果.


```r
pmap(list(x, y, z), function(a, b, c) a / (b + c))
#> [[1]]
#> [1] 0.1666667
#> 
#> [[2]]
#> [1] 0.1923077
#> 
#> [[3]]
#> [1] 0.1988072
```



---

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://r-cookbook-shen.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
