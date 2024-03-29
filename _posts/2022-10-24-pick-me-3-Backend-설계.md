---
layout: single
title:  "Pick me - 3. Backend 설계"
excerpt: "개인 담당 파트 Backend 설계 방법"
categories: 
    - Pick me
tag: 
    - [Backend, Node.js, Express, JS, JQ, DB, Mysql, Sequelize, Multer]
toc: true
toc_sticky : true
author_profile: true
sidebar:
    nav: "docs"
date: 2022-10-24
last_modified_at: 2022-10-24
---

**💡 Link :** [Pick me](http://118.67.130.161:8000/ "Pick me"){:target="_blank"}  

### 개요
#### 순서

1. 작품 출력 <a class="header-link" href="#작품-출력" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
> - 1줄에 3개 씩 반복 출력<a class="header-link" href="#1줄에-3개-씩-반복-출력" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
> - 작품 클릭 시 <a class="header-link" href="#작품-클릭-시" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
>> - SweetAlert2 로 작품 확대 및 설명 출력<a class="header-link" href="#sweetalert2-로-작품-확대-및-설명-출력" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>  
>> - 작품 설명 출력 <a class="header-link" href="#작품-설명-출력" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a> 
>> - 네브바 fadein/fadeout <a class="header-link" href="#네브바-fadeinfadeout" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
2. 사이드바 ( Bootstrap - scrollspy ) <a class="header-link" href="#사이드바--bootstrap---scrollspy-" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
> - 작품과 시대 매칭 <a class="header-link" href="#작품과-시대-매칭" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
> - 시대 클릭 시 해당 작품으로 이동 <a class="header-link" href="#시대-클릭 시-해당-작품으로-이동" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>
> - 해당 시대에 맞게 스크롤 바 상하 이동 <a class="header-link" href="#해당-시대에-맞게-스크롤-바-상하-이동" title="Permalink"><span class="sr-only">Permalink</span><i class="fas fa-link"></i></a>

---

#### 🛟 구조
> - 작품이 출력되는 공간  
>> 열 ( ROW )  
>>> 열 3 등분  
>>>> - 작품  
>>>> - 작가명, 작품명
>
> - 사이드 바 공간  
>> 연대  
>>> 시대  

![print3](/videos/frame.gif)

---

### 작품 출력


---

#### 1줄에 3개 씩 반복 출력

출력이 되어야 할 착품의 개수 = art_list.length  
현재 출력이 완료된 작품수 = art_count

반복문과 조건문을 통해 모든 작품이 출력 될 때 까지
한 열에 3개씩 작품을 출력.


<details markdown="1">
  <summary>📝 코드</summary>

``` js
<% while (art_count <= art_list.length) { %>

    # 작품 3개를 담을 row <div> 생성

    <% for(var three=0; three<3; three++, art_count++) { %>
        <% if(art_count >= art_list.length) { %>
            <% break; %>
        <% } %>

        # row 1/3 크기의 <div> 생성  
            # 작품 <div> 와 작가, 작가명 <div>  

    <% } %>
<% } %>
```
</details>

<br>

![print3](/videos/div3.gif)

---

#### 작품 클릭 시

---

##### SweetAlert2 로 작품 확대 및 설명 출력

onclick 이벤트를 통해 함수를 호출하여 sweetalert2 실행

<details markdown="1">
  <summary>📝 코드</summary>

``` js
function sweet_alert(get_pic_src) {
    desc_title = decodeURI(get_pic_src.src.split('/').reverse()[0]).split('.')[0];
    art_title = desc_title.replace(/_/g, " ").replace(/-/g, " - ");

    Swal.fire({
        imageUrl: get_pic_src.src,
        html: "<b>" + art_title + "</b>" + "<br><br>" + art_desc[desc_title]
    })
}
```

</details>

---

##### 네브바 fadein/fadeout  

&nbsp; 사진을 확대시 sweetalert2 가 실행되며  
div 태그 내부에 swal2-container 라는 클래스가 생기는 것을 확인.  

&nbsp; eventlistener 를 통해 클릭 시에 div 태그 내부에 swal2-container 유무를 통해  
네브바를 fadeout 해준다.

&nbsp; sweetalert2 가 사라진 이후 다시 원상 복구를 해야 한다.  
하지만 eventlistener가 onclick에 걸려있기 때문에 클릭을 하지 않을 경우   
navbar가 복구되지 않는 경우가 있기 때문에  
eventlistener 에 onscroll을 추가하여 예상치 못한 상황에 대처.

<details markdown="1">
  <summary>📝 코드</summary>

``` js
document.addEventListener('click', nav_fade);
document.addEventListener('scroll', nav_fade);

function nav_fade(){
    if($("div").hasClass("swal2-container")){
        $("#navbar").fadeOut(500);
    }else{
        $("#navbar").fadeIn(500);
    }
}
```
</details>

<br/>

![print3](/videos/sweetalert2.gif)

---

### 사이드바 ( Bootstrap - scrollspy )

---

##### 작품과 시대 매칭
##### 시대 클릭 시 해당 작품으로 이동

<br/>

&nbsp; 위 두 개는 bootstrap - scrollspy 사용법에 나온대로 속성을 추가해 주고  
id를 타겟팅 해 주면 자동으로 작동 하였다.

<details markdown="1">
  <summary>📝 코드</summary>

``` js
// 작품이 출력되는 div 
// scrollspy 의 타겟으로 설정
<div class="col-8 col-lg-10" id="img_container" data-bs-spy="scroll" data-bs-target="#navbar-example3" data-bs-offset="0" tabindex="0">

----------------------------------------

// scrolspy 출력 부분
<div class="col-4 col-lg-2" id="scrollspy_container" >

// scrollspy 부분의 설정된 타겟을 id에 입력
    <nav id="navbar-example3" class="navbar navbar-light bg-light flex-column align-items-stretch p-3">
        <nav class="nav nav-pills flex-column">


            // 연대 출력 하는 반복문 ( 고대, 중세, 근세, ... )
            // id를 통해 해당 연대과 매칭
            <%for(var i=0; i< artbox_data[0].length; i++) { %>
                <% var first_id="#" + artbox_data[0][i] %>
                <a class="nav-link a_tag" href= <%= first_id %> > <%= artbox_data[0][i] %> </a>
                <nav class="nav nav-pills flex-column">
                    <% var big_title = artbox_data[1][i]%>

                    // 시대 출력하는 반복문 ( 르네상스, 바로크, ... )
                    // id를 통해 해당 시대와 매칭
                    <%for(var mini_title in big_title) { %>
                        <% var second_id="#" + mini_title %>
                        <a class="nav-link ms-3 my-1 a_tag" href= <%=  second_id %> > <%= mini_title %> </a>
                    <% } %>
                </nav>
            <% } %>
        </nav>
    </nav>
</div>
```
</details>

<br/>

![print3](/videos/scrollspy1.gif)

---

##### 해당 시대에 맞게 스크롤 바 상하 이동

&nbsp; 현재 화면의 위치해 있는 시대에 맞춰 scrollspy의 시대가 이동은 하지만 스크롤을 해주지는 않아 시대가 보이지 않는 경우가 있다.  
&nbsp; 이를 해결하기 위해 **현재 화면의 눂이/전체화면의 높이** 를 통해 퍼센테이지를 구하고 scrollspy의 스크롤 위치를 퍼센테이지에 맞춰 주면 해결이 될 거라 생각을 했지만 scroll 이벤트가 중복 발생 할 경우 동기처리 때문인지 확실하지는 않지만 제대로 작동하지 않는 경우 발생.  
&nbsp; 이를 해결하기 위해 나온 방법으로 scrollspy 내부 컨텐츠의 양이 많지 않기 때문에 **현재 화면의 눂이/전체화면의 높이** 가 50% 를 기점으로 scrollspy의 스크롤을 최하단 최상단으로 옮겨주는 것으로 해결 하였다.

<details markdown="1">
  <summary>📝 코드</summary>

``` js
window.addEventListener("scroll", (event) => {
    var current_height = this.scrollY;
    var total_height = $("html").height();
    var current_height_percent = scrollY/total_height;

    if (current_height_percent < 0.5)
    {
        check_scrollbar = 0;
    }else{
        check_scrollbar = 1;
    }

    if (check_scrollbar != save_result){
        setTimeout(function() {
            if (check_scrollbar == 0){
                $("#navbar-example3").scrollTop(0);
                save_result = 0;
            }else{
                $("#navbar-example3").scrollTop(1000);
                save_result = 1;
            }
        }, 700);
    }
});
```
</details>

<br/>

![print3](/videos/scrollspy2.gif)

---

**💡 Link :** [Pick me](http://118.67.130.161:8000/ "Pick me"){:target="_blank"}  