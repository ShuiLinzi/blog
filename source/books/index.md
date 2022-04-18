---
title: books
date: 2022-04-18 10:52:39
layout: books
---
<style>
    @media screen and (max-width: 877px) {
        #book .page .content {
            flex-direction: column;
            align-items: center;
        }
    }

    #book {
        width: 100%;
    }

    #book .info .info-card {
        position: relative;
        width: 250px;
        overflow: hidden;
        transition: .3s
    }

    #book .info .info-card .hidden-content {
        position: absolute;
        /* border-radius: 50%; */
        display: flex;
        justify-content: center;
        align-items: center;
        top: 50%;
        left: 50%;
        height: 0%;
        transform: translate(-50%, -50%);
        filter: blur(12px);
        opacity: 0;
        background: #fff;
        width: 100%;
        transition: .5s;
    }

    #book .info .info-card .hidden-content .text {
        height: 80%;
        width: 80%;
        padding: 5px;
        overflow: hidden;
        text-overflow: ellipsis;
        font-family: Microsoft YaHei;
        font-size: 10px;
        color: #676767;
        float: left;
        clear: both;
        text-align: justify;
    }

    #book .info .info-card .hidden-content .text::first-letter {
        font-size: 20px;
        float: left;
        /* font-weight: bold; */
        margin: 0 .2em 0 0;

    }

    #book .info a:hover+.info-card .hidden-content {
        opacity: 1;
        height: 100%;
        width: 100%;
        filter: unset;
    }

    /* #book .info:hover .info-card{
    height:0;
} */

    #book .info-card h3 {
        position: unset;
        background: none;
        display: block;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
    }

    #book .info-card h3:before {
        display: none;
    }

    #book .page {
        overflow: hidden;
        border-radius: 3px;
        width: 100%;
    }

    @media screen and (min-width: 768px) {
        #book .content {
            grid-template-columns: 1fr 1fr;
        }
    }

    #book .content {
        /* background:pink; */
        /* display: grid; */
        /* grid-column-gap: 16px; */
        display: flex;
        align-items: center;
        width: 100%;
        margin: 0;
        justify-content: space-between;
        flex-wrap: wrap;
        padding: 16px;
        text-align: justify;
    }

    #book .content li {
        width: 380px;
        list-style: none;
        margin-bottom: 16px;
        border-radius: 8px;
        /* padding: 10px; */
        /* transform: matrix3d(1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1);
    box-shadow: 0 1px 2px 0px rgb(0 0 0 / 10%); */
        /* background-color: hsla(0, 0%, 100%, .7); */
        /* -webkit-box-shadow: 0 14px 38px rgba(0, 0, 0, .08), 0 3px 8px rgba(0, 0, 0, .06);
    box-shadow: 0 14px 38px rgba(0, 0, 0, .08), 0 3px 8px rgba(0, 0, 0, .06);
    -webkit-transition: all .3s ease 0s, -webkit-transform .6s cubic-bezier(.6, .2, .1, 1) 0s; */
        transition: all .3s ease 0s, -webkit-transform .6s cubic-bezier(.6, .2, .1, 1) 0s;
        transition: all .3s ease 0s, transform .6s cubic-bezier(.6, .2, .1, 1) 0s;
        transition: all .3s ease 0s, transform .6s cubic-bezier(.6, .2, .1, 1) 0s, -webkit-transform .6s cubic-bezier(.6, .2, .1, 1) 0s;
    }

    #book .content li .info {
        /* transform: matrix3d(1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1);
        box-shadow: 0 1px 2px 0px rgb(0 0 0 / 10%); */
        border-radius: 8px;
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: start;
        -ms-flex-pack: start;
        justify-content: flex-start;
        padding: 16px 12px;
        line-height: 1.7;
        list-style: none;
    }

    .book-container {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        -webkit-perspective: 600px;
        perspective: 600px;
    }

    .book {
        position: relative;
        width: 100px;
        height: 150px;
        -webkit-transform-style: preserve-3d;
        transform-style: preserve-3d;
        -webkit-transform: rotateY(-30deg);
        transform: rotateY(-30deg);
        -webkit-transition: 1s ease;
        transition: 1s ease;
        -webkit-animation: bookRotate 0s 1s ease 0s;
        animation: bookRotate 0s 1s ease 0s;
        list-style: none;
    }

    .book:before {
        content: " ";
        position: absolute;
        left: 0;
        top: 2px;
        width: 23px;
        height: 146px;
        -webkit-transform: translateX(84.5px) rotateY(90deg);
        transform: translateX(84.5px) rotateY(90deg);
        background: -webkit-gradient(linear, left top, right top, from(#fff), color-stop(5%, #f9f9f9), color-stop(10%, #fff), color-stop(15%, #f9f9f9), color-stop(20%, #fff), color-stop(25%, #f9f9f9), color-stop(30%, #fff), color-stop(35%, #f9f9f9), color-stop(40%, #fff), color-stop(45%, #f9f9f9), color-stop(50%, #fff), color-stop(55%, #f9f9f9), color-stop(60%, #fff), color-stop(65%, #f9f9f9), color-stop(70%, #fff), color-stop(75%, #f9f9f9), color-stop(80%, #fff), color-stop(85%, #f9f9f9), color-stop(90%, #fff), color-stop(95%, #f9f9f9), to(#fff));
        background: linear-gradient(90deg, #fff, #f9f9f9 5%, #fff 10%, #f9f9f9 15%, #fff 20%, #f9f9f9 25%, #fff 30%, #f9f9f9 35%, #fff 40%, #f9f9f9 45%, #fff 50%, #f9f9f9 55%, #fff 60%, #f9f9f9 65%, #fff 70%, #f9f9f9 75%, #fff 80%, #f9f9f9 85%, #fff 90%, #f9f9f9 95%, #fff);
    }

    .book>:first-child {
        position: absolute;
        top: 0;
        left: 0;
        width: 100px;
        height: 150px;
        -webkit-transform: translateZ(12.5px);
        transform: translateZ(12.5px);
        border-radius: 0 2px 2px 0;
        -webkit-box-shadow: 5px 5px 20px #666;
        box-shadow: 5px 5px 20px #666;
    }

    .book:after {
        content: " ";
        position: absolute;
        top: 0;
        left: 0;
        width: 100px;
        height: 150px;
        -webkit-transform: translateZ(-12.5px);
        transform: translateZ(-12.5px);
        background-color: #555;
        border-radius: 0 2px 2px 0;
    }

    #book .content li .info>div {
        margin-left: 26px;
    }

    #book .content li .info h3 {
        font-size: 16px;
    }

    #book .content li .info p a {
        position: relative;
        color: #b854d4;
    }

    #book .content li .info p a:before {
        content: " "attr(href);
        position: absolute;
        padding: 0 4px;
        width: -webkit-max-content;
        width: -moz-max-content;
        width: max-content;
        pointer-events: none;
        font-family: Fontello;
        font-size: 12px;
        -webkit-box-shadow: 0 14px 38px rgba(0, 0, 0, .08), 0 3px 8px rgba(0, 0, 0, .06);
        box-shadow: 0 14px 38px rgba(0, 0, 0, .08), 0 3px 8px rgba(0, 0, 0, .06);
        border-radius: 3px;
        background-color: #fff;
        opacity: 0;
        -webkit-transform: scale(.7) translateY(-75%);
        transform: scale(.7) translateY(-75%);
        -webkit-transform-origin: left center;
        transform-origin: left center;
        -webkit-transition: all .3s ease 0s;
        transition: all .3s ease 0s;
    }

    #book .content li .info p a:after {
        content: "";
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;
        height: 1.5px;
        background-color: #b854d4;
        -webkit-transform: scaleX(0);
        transform: scaleX(0);
        -webkit-transform-origin: bottom right;
        transform-origin: bottom right;
        -webkit-transition: -webkit-transform .3s ease-out;
        transition: -webkit-transform .3s ease-out;
        transition: transform .3s ease-out;
        transition: transform .3s ease-out, -webkit-transform .3s ease-out;
    }

    #book .content li .info .fa-star {
        margin-right: 4px;
        color: #d68fe9;
    }

    .icon-star:before {
        content: "\e80b";
    }

    [class*=" icon-"]:before,
    [class^=icon-]:before {
        font-family: Fontello;
        font-style: normal;
        font-weight: 400;
        display: inline-block;
        text-decoration: inherit;
        width: 1em;
        text-align: center;
        font-variant: normal;
        text-transform: none;
        line-height: 1em;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    #book .content li .description {
        padding: 12px;
        font-size: 14px;
        line-height: 1.7;
    }

    #book .content li .info p {
        font-size: 14px;
        line-height: 1.7;

    }

    /* @media screen and (min-width: 768px) {
    #book .content li:hover {
        -webkit-transform: translateY(-4px);
        transform: translateY(-4px);
        -webkit-box-shadow: 0 14px 38px rgba(0, 0, 0, .14), 0 3px 8px rgba(0, 0, 0, .12);
        box-shadow: 0 14px 38px rgba(0, 0, 0, .14), 0 3px 8px rgba(0, 0, 0, .12);
    }
} */


    #book .content li:hover .book {
        -webkit-transform: rotateY(0deg);
        transform: rotateY(0deg);
    }
</style>
<div id="book">
        <div class="page">
            <ul class="content">
                <!-- 每个li标签内容代表一本书籍的所有信息 -->
                <li>
                    <div class="info">
                        <a href="https://book.qidian.com/info/1023589917/" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="水浒传">
                                <img src="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/20211112007.jpg" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/20211112007.jpg" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="水浒传">
                            </div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">《水浒传》是一部以描写古代农民起义为题材的长篇小说。它形象地描绘了农民起义从发生、发展直至失败的全过程，深刻揭示了起义的社会根源，满腔热情地歌颂了起义英雄的反抗斗争和他们的社会理想，也具体揭示了起义失败的内在历史原因。</p>
                            </div>
                            <h3>《水浒传》</h3>
                            <p>作者：施耐庵</p>
                            <p>出版时间：2005-04-01</p>
                                <span>读书笔记：</span>
                                <a href="" target="_blank" rel="noopener noreferrer"> 暂无 </a>
                            <p>
                                <span>推荐指数：</span>
                                <span></span>
                                <!-- 推荐指数，多少个星星就有以下多少个i标签 -->
                                <i class="ic i-star"></i>
                                <i class="ic i-star"></i>
                                <i class="ic i-star"></i>
                                <i class="ic i-star"></i>
                                <i class="ic i-star"></i>
                            </p>
                        </div>
                    </div>
                </li>
                <!-- 每个li标签内容代表一本书籍的所有信息 -->
                <li>
                    <div class="info"><a href="https://book.qidian.com/info/1023589911/" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="三国演义"><img src="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/202111120010.jpg" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/202111120010.jpg" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="三国演义"></div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">《三国演义》描写了从东汉末年到西晋初年之间近百年的历史风云，以描写战争为主，诉说了东汉末年的群雄割据混战和魏、蜀、吴三国之间的政治和军事斗争，最终司马炎一统三国，建立晋朝的故事。反映了三国时代各类社会斗争与矛盾的转化，并概括了这一时代的历史巨变，塑造了一群叱咤风云的三国英雄人物。</p>
                            </div>
                            <h3>《三国演义》</h3>
                            <p>作者：罗贯中</p>
                            <p>出版时间：1986-06-01</p>
                            <span>读书笔记：</span><a href="javascript:;" target="" rel="noopener noreferrer"> 暂无 </a><p></p>
                            <p><span>推荐指数：</span><span></span><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i></p>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="info"><a href="http://book.sbkk8.com/gudai/sidawenxuemingzhu/xiyouji/" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="西游记">
                                <img src="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/20211112008.jpg" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/20211112008.jpg" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="西游记"></div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">《西游记》是中国古代魔幻小说中的巅峰之作，讲述了唐僧师徒四人一路降妖伏魔、历经艰险到西天求取真经的故事。书中借助惊险的故事充分展现了作者丰富的知识、惊人的想象力和高超的写作技巧。
                                </p>
                            </div>
                            <h3>《西游记》</h3>
                            <p>作者：吴承恩</p>
                            <p>出版时间：1994-04-01</p>
                            <span>读书笔记：</span><a href="javascript:;" target="" rel="noopener noreferrer">暂无</a><p></p>
                            <p><span>推荐指数：</span><span></span><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i></p>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="info"><a href="https://dushu.baidu.com/pc/detail?gid=4305592439" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="红楼梦">
                                <img src="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/20211112009.jpg" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/casteller/berrychenpic/20211112009.jpg" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="红楼梦"></div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">《红楼梦》是一部现实主义和浪漫主义高度结合的伟大著作，也是一个时代文学艺术的顶峰。全书通过对“贾、史、王、薛”四大家族从繁荣到衰败的描写，真实地展现了当时的社会生活及风俗人情，堪称封建末世的百科全书。
                                </p>
                            </div>
                            <h3>《 红楼梦 》</h3>
                            <p>作者：曹雪芹</p>
                            <p>出版时间：1996-12-01</p>
                            <span>读书笔记：</span><a href="javascript:;" target="" rel="noopener noreferrer"> 暂无 </a><p></p>
                            <p><span>推荐指数：</span><span></span><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i><i class="fa fa-star"></i></p>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
