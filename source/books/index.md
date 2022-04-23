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
# 读完
<div id="book">
        <div class="page">
            <ul class="content">
                <!-- 每个li标签内容代表一本书籍的所有信息 -->
                  <li>
                    <div class="info">
                        <a href="" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="被讨厌的勇气">
                                <img src="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/image.4eu6y0whcwc.webp" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/image.4eu6y0whcwc.webp" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="水浒传">
                            </div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">「被讨厌的勇气」并不是要去吸引被讨厌的负向能量，而是，如果这是我生命想绽放出最美的光彩，那么，即使有被讨厌的可能，我都要用自己的双手双脚往那里走去。」</p>
                            </div>
                            <h3>《被讨厌的勇气》</h3>
                            <p>作者： [日] 岸见一郎 / [日] 古贺史健</p>
                            <p>出版时间：2015-3-1</p>
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
                    <div class="info">
                        <a href="" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="人类简史">
                                <img src="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/人类简史.webp" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/人类简史.webp" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="水浒传">
                            </div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">十万年前，地球上至少有六种不同的人但今日，世界舞台为什么只剩下了我们自己？从只能啃食虎狼吃剩的残骨的猿人，到跃居食物链顶端的智人，从雪维洞穴壁上的原始人手印，到阿姆斯壮踩上月球的脚印，从认知革命、农业革命，到科学革命、生物科技革命，我们如何登上世界舞台成为万物之灵的？</p>
                            </div>
                            <h3>《人类简史》</h3>
                            <p>作者：  [以色列] 尤瓦尔·赫拉利</p>
                            <p>出版时间：2014-11</p>
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
                  <li>
                    <div class="info">
                        <a href="" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="富爸爸，穷爸爸">
                                <img src="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/穷爸爸富爸爸.webp" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/穷爸爸富爸爸.webp" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="富爸爸，穷爸爸">
                            </div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">《富爸爸，穷爸爸》是一个真实的故事，作者罗伯特・清崎的亲生父亲和朋友的父亲对金钱的看法截然不同，这使他对认识金钱产生了兴趣，最终他接受了朋友的父亲的建议，也就是书中所说的。“富爸爸”的观念，即不要做金钱的奴隶，要让金钱为我们工作，并由此成为一名极富传奇色彩的成功的投资家。</p>
                            </div>
                            <h3>《富爸爸，穷爸爸》</h3>
                            <p>作者：（美）罗伯特・T・清崎 / 莎伦・L・莱希特</p>
                            <p>出版时间：2000-09</p>
                                <span>读书笔记：</span>
                                <a href="https://shuilinzi.github.io/blog/posts/2021/book/%E5%AF%8C%E7%88%B8%E7%88%B8%E7%A9%B7%E7%88%B8%E7%88%B8/" target="_blank" rel="noopener noreferrer"> 富爸爸，穷爸爸 </a>
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
                 <li>
                    <div class="info">
                        <a href="" target="_blank" rel="noreferrer noopener" class="book-container">
                            <div class="book" title="围城">
                                <img src="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/围城.webp" class="lazyload" data-srcset="https://cdn.jsdelivr.net/gh/Kerisu/image@master/书/围城.webp" srcset="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAABGdBTUEAALGPC/xhBQAAADhlWElmTU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAAAAaADAAQAAAABAAAAAQAAAADa6r/EAAAAC0lEQVQIHWNgAAIAAAUAAY27m/MAAAAASUVORK5CYII=" alt="富爸爸，穷爸爸"">
                            </div>
                        </a>
                        <div class="info-card">
                            <div class="hidden-content">
                                <p class="text">《围城》是钱钟书仅有的一部长篇小说，堪称中国现当代长篇小说的经典。小说塑造了抗战开初一类知识分子的群像，生动反映了在国家特定时期，特殊人群的行为操守、以及困惑。从另一个角度记述了当时的情景、氛围。虽然有具体的历史背景，但这部小说揭示的只是人群的弱点，在今天依然能够引起人们的共鸣。第1版于1947年由上海晨光出版公司出版。《围城》是中国现代文学史上一部风格独特的讽刺小说。被誉为“新儒林外史”。</p>
                            </div>
                            <h3>《围城》</h3>
                            <p>作者：钱锺书</p>
                            <p>出版时间：2017-6</p>
                                <span>读书笔记：</span>
                                <a href="" target="_blank" rel="noopener noreferrer">  </a>
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
            </ul>
        </div>
    </div>
