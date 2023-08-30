

# 正则表达式 {#regularexpression}

正则表达式(regular expression)是文本处理中经常用到的方法.正则表达式描述了一种字符串匹配的模式（pattern）.在R中,正则表达式跟其他语言基本一致,要写成文本的格式,也就是需要使用双引号或者单引号括起来.

## 特殊字符

在正则表达式中有一些字符是有特殊含义的,比如点(.),如果只写点,它代表任意字符,这些称之为特殊字符,或者元字符.正则表达式中就有很多这样元字符.总结如下:

|正则表达式|含义           |
|----------|---------------|
|.         |任意字符|
|^         |匹配字符的开始位置|
|$         |匹配字符的结束位置|
|()         |括号中的内容作为一个整体进行匹配|
|*         |重复其前面字符,零次或者多次|
|+         |重复其前面字符,一次或者多次|
|?         |重复其前面字符,零次或者一次|
|[]         |括号中的字符表示任意一个|
||         |表示或者|

这里面有很多是和后面的内容重复的,因此这里写的比较简略.

如果想要匹配特殊字符,那么需要对其进行转义,这时候就用到了转义符,也就是`\`.但是因为转义符本身也是特殊字符,所以它本身还需要一个转义符,就是`\\`.因此比如我们想要匹配点本身,那在pattern中,需要写作`\\.`.

## 匹配(match)

有一些特殊含义的匹配,在正则表达式有固定表达方式,比如任意一个数字,任意字母等.

|正则表达式|含义           |
|----------|---------------|
|`\\.`         |匹配点.|
|`\\n`         |匹配换行符|
|`\\t`         |匹配tab|
|`\\s`         |匹配空格(white space)|
|`\\d`         |匹配任意数字|
|`\\w`         |匹配任意字符|
|`\\b`         |匹配所有字符的边界|
|`[:digit:]`   |任意数字|
|`[:alpha:]`   |任意字母|
|`[:lower:]`   |任意小写字母|
|`[:upper:]`   |任意大写字母|
|`[:alnum:]`   |任意数字和字母|
|`[:punct:]`   |任意标点符号|
|`[:graph:]`   |任意数字字母或者标点符号|
|`[:space:]`   |空格|
|`[:blank:]`   |空格和tab,但是不包括新行|

## 替换 (Alternates)

|正则表达式|含义           |
|----------|---------------|
|`a|b`         |或者,a或者b|
|`[abc]`       |abc中的任意一个|
|`[^abc]`      |除abc之外的任意字符|
|`[a-c]`       |range,abc|


## 锚点(anchors)

锚点,又称为定位符,能够将正则表达式固定到行首或行尾。它们还使您能够创建这样的正则表达式，这些正则表达式出现在一个单词内、在一个单词的开头或者一个单词的结尾。

### 以固定要求开头

以固定要求开头的正则表达式是`^`,比如我们要查找那些以字母a开头的字符串,就可以写作:

```markdonw
'^a'
```

在R语言中:


```r
grep(pattern = "^a", x = c("abc", "bcd", "cde", "ade"))
#> [1] 1 4
```

### 以固定要求结尾

以固定要求结尾的正则表达式是`$`,比如我们要查找那些以字母a结尾的字符串,就可以写作:

```markdonw
'a$'
```

在R语言中:


```r
grep(pattern = "a$", x = c("abc", "bcd", "cde", "ada"))
#> [1] 4
```

## 数量(Quantifiers)

正则表达式中经常需要有一些表达数量的内容,比如重复1次,重复多于一次等等.一般是使用一些特殊字符在某个要重复的内容之后.称之为限定符:

|正则表达式|含义           |
|----------|---------------|
|?         |重复0次或者一次|
|*         |0次或者多次    |
|+         |1次或者多次    |
|{n}         |正好n次    |
|{n,}         |n次或者多于n次|
|{n,m}         |大于等于n次小于等于m次|

重复或者数量还涉及到贪婪和懒惰的问题:

|正则表达式|含义           |
|----------|---------------|
|*?         |重复任意次,但尽可能少重复|
|+?         |重复一次或者更多次,但尽可能少重复 |
|??         |重复零次或者1次,但是尽可能少重复|
|{n,}?         |重复n次及以上,但是尽可能少重复|
|{n,m}         |重复n次到m次,但是尽可能少重复|


## 整体匹配 (groups)

如果需要将某个对象最为整体进行匹配,一般是使用小括号将其括起来.

比如:

```markdown
(ab|d)e
```

小括号中就代表一个整体,也即是`ab`或者`d`然后后面带着`e`.

## 反义

有时需要查找不属于某个能简单定义的字符类的字符。比如想查找除了数字以外，其它任意字符都行的情况，这时需要用到反义:

|正则表达式|含义           |
|----------|---------------|
|`\W`         |匹配任意不是字母,数字,下划线和汉字的字符|
|`\S`         |匹配任意不是空白符的字符|
|`\D`         |匹配任意不是数字的字符|
|`\B`         |匹配不是单词开头或者结束的位置|
|`[^x]`         |匹配除了x以外的任意字符|
|`[^abc]`         |匹配除了abc以外的任意字符|

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
                            
