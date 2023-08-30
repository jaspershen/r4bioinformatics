

# `Tidyverse`用于数据分析stringr {#tidyverse_stringr}

`stringr`是R中进行文本处理的包.cheatsheet可以看这里.

https://github.com/rstudio/cheatsheets/blob/master/strings.pdf

## 安装


```r
# Install the released version from CRAN:
install.packages("stringr")

# Install the cutting edge development version from GitHub:
# install.packages("devtools")
devtools::install_github("tidyverse/stringr")
```

## Rstudio Addin

使用Rstudio Addin,可以很方便的使用正则表达式.

需要安装一个包:


```r
if(!require(remotes)){
  install.packages("remotes")
}
if(!require(regexplain)){
remotes::install_github("gadenbuie/regexplain")
}
```

安装结束之后,可以在Addins中找到.

## Pattern matching

###  计算一个文本中符合要求的数目

使用`str_count()`函数:

比如要统计某个单词中某个字母的个数:


```r
library(stringr)
library(regexplain)
fruit <- c("apple", "banana", "pear", "pineapple")
str_count(fruit, "a")
#> [1] 1 3 1 1

str_count(fruit, c("a", "b", "p", "p"))
#> [1] 1 1 1 3
```

其中第二个参数`pattern`是支持正则表达式的.

### 判断一个文本中是否存在一个pattern

使用`str_detect()`函数.


```r
str_detect(string, pattern, negate = FALSE)
```

其中`negate`是在`stringr`中的很多函数都有的一个参数,如果设置为`TRUE`,就会返回没有match到的内容.


```r
fruit <- c("apple", "banana", "pear", "pinapple")
str_detect(fruit, "a")
#> [1] TRUE TRUE TRUE TRUE
```

### 从文本中提取pattern

有两个函数,`str_extract`和`str_extract_all`.

用法如下:


```r
str_extract(string, pattern)
str_extract_all(string, pattern, simplify = FALSE)
```

从文本中提取符合要求的文本.

例子:


```r
shopping_list <- c("apples x4", "bag of flour", "bag of sugar", "milk x2")
str_extract(shopping_list, "\\d")
#> [1] "4" NA  NA  "2"
```

`\\d`是一种正则表达式的写法,代表任意数字.上面的代码代表我们想要从每个文本中提取所有的数字.

如果没有数字,那么就会返回NA.


```r
shopping_list <- c("apples x4", "bag of flour", "bag of sugar", "milk x2")
str_extract(shopping_list, "[a-z]+")
#> [1] "apples" "bag"    "bag"    "milk"
```

`[a-z]+`代表任意一个小写字母,然后重复1次或者多次.


```r
shopping_list <- c("apples x4", "bag of flour", "bag of sugar", "milk x2")
str_extract(shopping_list, "\\b[a-z]{1,4}\\b")
#> [1] NA     "bag"  "bag"  "milk"
```

`\\b`代表字符的边界.

`\\b[a-z]{1,4}\\b`代表`一个字符的边界+重复1-4次的任意小写字母+一个字符的边界`.

可以看到,只会提取出第一个字母.

如果想要把一个字符的所有符合要求的pattern都提取出来,可以使用`str_extract_all`.


```r
shopping_list <- c("apples x4", "bag of flour", "bag of sugar", "milk x2")
str_extract_all(shopping_list, "[a-z]+")
#> [[1]]
#> [1] "apples" "x"     
#> 
#> [[2]]
#> [1] "bag"   "of"    "flour"
#> 
#> [[3]]
#> [1] "bag"   "of"    "sugar"
#> 
#> [[4]]
#> [1] "milk" "x"
str_extract_all(shopping_list, "\\b[a-z]+\\b")
#> [[1]]
#> [1] "apples"
#> 
#> [[2]]
#> [1] "bag"   "of"    "flour"
#> 
#> [[3]]
#> [1] "bag"   "of"    "sugar"
#> 
#> [[4]]
#> [1] "milk"
str_extract_all(shopping_list, "\\d")
#> [[1]]
#> [1] "4"
#> 
#> [[2]]
#> character(0)
#> 
#> [[3]]
#> character(0)
#> 
#> [[4]]
#> [1] "2"
```

可以看到,这时候得到的就是一个和string同样长度的list.会把每个string中的符合要求的pattern都提取出来.

我们注意到`str_extract_all()`有一个参数:`simplify`,它能够将返回的结果变为一个matrix:


```r
shopping_list <- c("apples x4", "bag of flour", "bag of sugar", "milk x2")
str_extract_all(shopping_list, "[a-z]+", simplify = TRUE)
#>      [,1]     [,2] [,3]   
#> [1,] "apples" "x"  ""     
#> [2,] "bag"    "of" "flour"
#> [3,] "bag"    "of" "sugar"
#> [4,] "milk"   "x"  ""
str_extract_all(shopping_list, "\\b[a-z]+\\b", simplify = TRUE)
#>      [,1]     [,2] [,3]   
#> [1,] "apples" ""   ""     
#> [2,] "bag"    "of" "flour"
#> [3,] "bag"    "of" "sugar"
#> [4,] "milk"   ""   ""
str_extract_all(shopping_list, "\\d", simplify = TRUE)
#>      [,1]
#> [1,] "4" 
#> [2,] ""  
#> [3,] ""  
#> [4,] "2"
```

### 确定某个pattern在文本中的位置

两个函数:


```r
str_locate(string, pattern)
str_locate_all(string, pattern)
```

例子:


```r
fruit <- c("apple", "banana", "pear", "pineapple")
str_locate(fruit, "$")
```

`$`代表以某个东西结尾.


```r
str_locate(fruit, "a")
str_locate(fruit, "e")
str_locate(fruit, c("a", "b", "p", "p"))
```

然后我们再查找字母`a`在所有字符中的位置.如果没有符合要求的pattern,则会返回NA.也可以设置pattern为一个长度和string相同的vector.

如果想要查找所有符合要求的pattern的位置,要使用`str_locate_all()`函数:


```r
str_locate(fruit, "a")
str_locate_all(fruit, "a")
```

返回是一个list.

### 从文本中提取(extract)匹配到的group

函数为:


```r
str_match(string, pattern)
str_match_all(string, pattern)
```

这两个函数跟`str_extract()`有些类似.

例子:


```r
strings <- c(" 219 733 8965", "329-293-8753 ", "banana", "595 794 7569",
  "387 287 6718", "apple", "233.398.9187  ", "482 952 3315",
  "239 923 8115 and 842 566 4692", "Work: 579-499-7527", "$1000",
  "Home: 543.355.3679")
phone <- "([2-9][0-9]{2})[- .]([0-9]{3})[- .]([0-9]{4})"
```

phone代表符合电话号码的格式:

* 第一个group: 小括号括起来的,数字2-9其中一个,重复一次(后面没有限定数量,则代表重复一次);数字0-9.重复两次. 然后中间是`-`或者空格,或者任意一个字符.

* group 2: 小括号括起来的.0-9任意数字重复三次.然后中间是`-`或者空格,或者任意一个字符.

* group 3: 小括号括起来的.0-9任意数字重复四次.


```r
str_extract(strings, phone)
#>  [1] "219 733 8965" "329-293-8753" NA            
#>  [4] "595 794 7569" "387 287 6718" NA            
#>  [7] "233.398.9187" "482 952 3315" "239 923 8115"
#> [10] "579-499-7527" NA             "543.355.3679"
```

这时候我们试试使用`str_match()`函数:


```r
str_match(strings, phone)
#>       [,1]           [,2]  [,3]  [,4]  
#>  [1,] "219 733 8965" "219" "733" "8965"
#>  [2,] "329-293-8753" "329" "293" "8753"
#>  [3,] NA             NA    NA    NA    
#>  [4,] "595 794 7569" "595" "794" "7569"
#>  [5,] "387 287 6718" "387" "287" "6718"
#>  [6,] NA             NA    NA    NA    
#>  [7,] "233.398.9187" "233" "398" "9187"
#>  [8,] "482 952 3315" "482" "952" "3315"
#>  [9,] "239 923 8115" "239" "923" "8115"
#> [10,] "579-499-7527" "579" "499" "7527"
#> [11,] NA             NA    NA    NA    
#> [12,] "543.355.3679" "543" "355" "3679"
```

可以看到,除了将整体pattern匹配出来之后,还将每个`group`(也就是括号括起来的部分)分别匹配出来了.


```r
str_extract_all(strings, phone)
#> [[1]]
#> [1] "219 733 8965"
#> 
#> [[2]]
#> [1] "329-293-8753"
#> 
#> [[3]]
#> character(0)
#> 
#> [[4]]
#> [1] "595 794 7569"
#> 
#> [[5]]
#> [1] "387 287 6718"
#> 
#> [[6]]
#> character(0)
#> 
#> [[7]]
#> [1] "233.398.9187"
#> 
#> [[8]]
#> [1] "482 952 3315"
#> 
#> [[9]]
#> [1] "239 923 8115" "842 566 4692"
#> 
#> [[10]]
#> [1] "579-499-7527"
#> 
#> [[11]]
#> character(0)
#> 
#> [[12]]
#> [1] "543.355.3679"
str_match_all(strings, phone)
#> [[1]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "219 733 8965" "219" "733" "8965"
#> 
#> [[2]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "329-293-8753" "329" "293" "8753"
#> 
#> [[3]]
#>      [,1] [,2] [,3] [,4]
#> 
#> [[4]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "595 794 7569" "595" "794" "7569"
#> 
#> [[5]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "387 287 6718" "387" "287" "6718"
#> 
#> [[6]]
#>      [,1] [,2] [,3] [,4]
#> 
#> [[7]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "233.398.9187" "233" "398" "9187"
#> 
#> [[8]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "482 952 3315" "482" "952" "3315"
#> 
#> [[9]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "239 923 8115" "239" "923" "8115"
#> [2,] "842 566 4692" "842" "566" "4692"
#> 
#> [[10]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "579-499-7527" "579" "499" "7527"
#> 
#> [[11]]
#>      [,1] [,2] [,3] [,4]
#> 
#> [[12]]
#>      [,1]           [,2]  [,3]  [,4]  
#> [1,] "543.355.3679" "543" "355" "3679"
```

另外一个例子:


```r
x <- c("<a> <b>", "<a> <>", "<a>", "", NA)
str_match(x, "<(.*?)> <(.*?)>")
#>      [,1]      [,2] [,3]
#> [1,] "<a> <b>" "a"  "b" 
#> [2,] "<a> <>"  "a"  ""  
#> [3,] NA        NA   NA  
#> [4,] NA        NA   NA  
#> [5,] NA        NA   NA
str_match_all(x, "<(.*?)>")
#> [[1]]
#>      [,1]  [,2]
#> [1,] "<a>" "a" 
#> [2,] "<b>" "b" 
#> 
#> [[2]]
#>      [,1]  [,2]
#> [1,] "<a>" "a" 
#> [2,] "<>"  ""  
#> 
#> [[3]]
#>      [,1]  [,2]
#> [1,] "<a>" "a" 
#> 
#> [[4]]
#>      [,1] [,2]
#> 
#> [[5]]
#>      [,1] [,2]
#> [1,] NA   NA
str_extract(x, "<.*?>")
#> [1] "<a>" "<a>" "<a>" NA    NA
str_extract_all(x, "<.*?>")
#> [[1]]
#> [1] "<a>" "<b>"
#> 
#> [[2]]
#> [1] "<a>" "<>" 
#> 
#> [[3]]
#> [1] "<a>"
#> 
#> [[4]]
#> character(0)
#> 
#> [[5]]
#> [1] NA
```

`<(.*?)>`其中的`*?`是贪婪用法,表示重复任意次,但尽可能少重复.请参考这一章[正则表达式](#regularexpression).

### 从文本中去除匹配到的pattern

函数:


```r
str_remove(string, pattern)
str_remove_all(string, pattern)
```

与`str_replace`用法作用一致.但是`str_replace`可以替换成其他pattern.

例子:


```r
fruits <- c("one apple", "two pears", "three bananas")
str_remove(fruits, "[aeiou]")
#> [1] "ne apple"     "tw pears"     "thre bananas"
str_remove_all(fruits, "[aeiou]")
#> [1] "n ppl"    "tw prs"   "thr bnns"
```

### 从文本中替换匹配到的pattern

函数:


```r
str_replace(string, pattern, replacement)
str_replace_all(string, pattern, replacement)
```

例子:


```r
fruits <- c("one apple", "two pears", "three bananas")
str_replace(fruits, "[aeiou]", "-")
#> [1] "-ne apple"     "tw- pears"     "thr-e bananas"
str_replace_all(fruits, "[aeiou]", "-")
#> [1] "-n- -ppl-"     "tw- p--rs"     "thr-- b-n-n-s"
```


```r
str_replace_all(fruits, "[aeiou]", toupper)
#> [1] "OnE ApplE"     "twO pEArs"     "thrEE bAnAnAs"
str_replace_all(fruits, "b", NA_character_)
#> [1] "one apple" "two pears" NA
```

从这个例子可以看出来,`replacement`也可以是一个函数,将匹配到的所有pattern使用该函数转换之后替换.


```r
str_replace(fruits, "([aeiou])", "\\1\\1")
#> [1] "oone apple"     "twoo pears"     "threee bananas"
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
                            
