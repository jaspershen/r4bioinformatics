--- 
title: "R for Bioinformatics project"
author: "Xiaotao Shen"
date: "2020-8-30 and updated on 2024-01-30"
site: bookdown::bookdown_site
documentclass: book
output: bookdown::bs4_book
url: https://jaspershen.github.io/r4bioinformatics
cover-image: cover.png
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This is a summary of how to use R for bioinformatics projects.
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---

# Preface

这本书用来系统记录在学习过程中的遇到的问题,学到的知识.

You can contact me via the social websites below:

<a href="https://jaspershen.github.io/" target='_blank'><i class="fa fa-home"> Personal website</i></a> 

<a href="https://github.com/jaspershen" target='_blank'><i class="fa fa-github"> GitHub</i></a> 

<a href="https://jaspershen.github.io/image/wechat_QR.jpg" target='_blank'><i class="fa fa-weixin"> WeChat</i></a> 

<a href="https://www.shenxt.info/files/qq_QR.jpg" target='_blank'><i class="fa fa-qq"> QQ</i></a> 

<a href="shenxt@stanford.edu" target='_blank'><i class="fa fa-envelope"> shenxt@ stanford.edu</i></a> 

<a href="https://www.linkedin.com/in/shenxt/" target='_blank'><i class="fa fa-linkedin"> LinkedIn</i></a>


```{=html}
<div class="leaflet html-widget html-fill-item" id="htmlwidget-db78c9a4c419c5848c63" style="width:672px;height:480px;"></div>
<script type="application/json" data-for="htmlwidget-db78c9a4c419c5848c63">{"x":{"options":{"crs":{"crsClass":"L.CRS.EPSG3857","code":null,"proj4def":null,"projectedBounds":null,"options":{}}},"calls":[{"method":"addTiles","args":["https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",null,null,{"minZoom":0,"maxZoom":18,"tileSize":256,"subdomains":"abc","errorTileUrl":"","tms":false,"noWrap":false,"zoomOffset":0,"zoomReverse":false,"opacity":1,"zIndex":1,"detectRetina":false,"attribution":"&copy; <a href=\"https://openstreetmap.org/copyright/\">OpenStreetMap<\/a>,  <a href=\"https://opendatacommons.org/licenses/odbl/\">ODbL<\/a>"}]},{"method":"addPopups","args":[37.432748,-122.176758,"My office is <b>M339, Alway Building, Cooper Lane, Palo Alto, CA 94304",null,null,{"maxWidth":300,"minWidth":50,"autoPan":true,"keepInView":false,"closeButton":true,"className":""}]}],"setView":[[37.432748,-122.176758],17,[]],"limits":{"lat":[37.432748,37.432748],"lng":[-122.176758,-122.176758]}},"evals":[],"jsHooks":[]}</script>
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
