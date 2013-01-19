#Bootstrap学习笔记-1

-tags:Bootstrap

----

>最近开始写[PotatoBox](https://github.com/PotatoBrother/PotatoBox)了，本来想先写后端的，不过发现不和前端一起并行开发是十分蛋疼的

Bootstrap创建了响应式的12列栅格系统，所以在网页排版和设计上十分简单
<html lang="en">
  <head>
    <meta charset="utf-8">
    <link href="assets/css/bootstrap.css" rel="stylesheet">
    <link href="assets/css/bootstrap-responsive.css" rel="stylesheet">
    <link href="assets/css/docs.css" rel="stylesheet">
    <link href="assets/js/google-code-prettify/prettify.css" rel="stylesheet">
  </head>
  <body>
    <div class="row show-grid">
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
      <div class="span1">1</div>
    </div>
    <div class="row show-grid">
      <div class="span4">4</div>
      <div class="span4">4</div>
      <div class="span4">4</div>
  </div>
  <div class="row show-grid">
      <div class="span4">4</div>
      <div class="span8">8</div>
  </div>
      <div class="row show-grid">
      <div class="span6">6</div>
      <div class="span6">6</div>
  </div>
  <div class="row show-grid">
      <div class="span12">12</div>
  </div>
</body>