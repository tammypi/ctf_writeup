# 我的flag呢？

题目链接：https://www.nssctf.cn/problem/3861



解题思路：

直接f12查看源码，可以看到最后的部分即为flag:

```
<body id="page-top" data-bs-spy="scroll" data-bs-target="#mainNav" data-bs-offset="57">
    <nav class="navbar navbar-light navbar-expand-lg fixed-top" id="mainNav">
        <div class="container"><a class="navbar-brand" href="#page-top">啊哈？</a><button data-bs-toggle="collapse"
                data-bs-target="#navbarResponsive" class="navbar-toggler navbar-toggler-right" type="button"
                aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation"><i
                    class="fa fa-align-justify"></i></button>
            <div class="collapse navbar-collapse" id="navbarResponsive">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item"><a class="nav-link" href="#about">没用</a></li>
                    <li class="nav-item"><a class="nav-link" href="#services">点不了</a></li>
                    <li class="nav-item"><a class="nav-link" href="#portfolio">别尝试</a></li>
                    <li class="nav-item"><a class="nav-link" href="#contact">要不再看看题？</a></li>
                </ul>
            </div>
        </div>
    </nav>
    <header class="text-center text-white d-flex masthead"
        style="background-image: linear-gradient(rgba(0, 0, 0, 0.1), rgba(0, 0, 0, 0.3)),url('https://api.r10086.com/樱道随机图片api接口.php?自适应图片系列=原神');">
        <div class="container my-auto">
            <div class="row">
                <div class="col-lg-10 mx-auto">
                    <h1 class="text-uppercase"><strong>这是一个普普通通的页面</strong></h1>
                    <hr style="border-color: rgb(255,255,255);">
                </div>
            </div>
            <div class="col-lg-8 mx-auto">
                <p class="text-faded mb-5">听说你是来找flag的？但是哪里有flag呢？</p><a class="btn btn-primary btn-xl" role="button"
                    href="#services"
                    style="background: rgb(255,255,255);border-color: rgb(147,147,147);--bs-primary: #9a9a9a;--bs-primary-rgb: 154,154,154;transform-style: preserve-3d;"><span
                        style="color: rgb(0, 0, 0);">Find Out More</span></a>
            </div>
        </div>
    </header>
    <script src="assets/bootstrap/js/bootstrap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/baguettebox.js/1.11.1/baguetteBox.min.js"></script>
    <script src="assets/js/script.min.js"></script>
</body>

</html>
<!--flag is here flag=NSSCTF{dcce1c20-c17a-434f-ac01-03f63656627e} -->
```

