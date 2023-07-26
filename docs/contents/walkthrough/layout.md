---
layout: page
#title: About
#permalink: /test.h
---

# 애플리케이션 레이아웃의 작성

프로젝트를 생성할 때 콘솔의 출력 내용에는 아래와 같은 내용을 볼 수 있었을 것이다.

```bash
$ rails new rcafe
...중략~
      create  app/views/layouts/application.html.erb
...중략 ~
```

레일스에서는 `app/views/layouts/` 디렉토리에서 애플리케이션의 레이아웃을 관리한다.

특히 `application.html.erb` 파일은 전체 애플리케이션의 레이아웃을 만들어 주는데, 그 소스코드는 아래와 같다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Rcafe</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

</body>
</html>
```

`.html.erb` 확장자로 끝나는 뷰 템플릿 파일들은 파일 내부에 `ERB(Embeded Ruby)` 코드를 포함하고 있다. 즉, HTML 파일에 루비코드를 삽입해 놓은 것이며 렌더링 과정 중에 루비 코드에 대한 처리 결과가 삽입된다. `<%= %>`와 같은 형태를 가지면 루비의 처리결과로 대체되며, `<% %>`와 같은 경우에는 삽입된 루비 코드를 실행만 한다.

위의 HTML 코드 중 하단에 있는 `<%= yield %>` 부분을 주목하자.
우선은 각 액션이 실행된 후 HTML로 렌더링되는 결과가 이 부분에 삽입된다고 알아 두자. 나중에 `yield`에 대해서 자세히 다루도록 하겠다.

언급한 바와 같이, 애플리케이션 레이아웃은 애플리케이션의 전체 레이아웃을 만들어 준다. 따라서 전체 애플리케이션의 페이지 모습을 일관성 있게 변경하고자 할 때 바로 이 `application.html.erb` 파일에서 작업을 해주면 된다. `<body></body>` 태그의 내용을 다음과 같이 수정하자. 모든 페이지에 `Bootstrap`의 `navbar` 메뉴가 추가될 것이다.

```html
<body>
    <!-- Fixed navbar -->
    <div class="navbar navbar-default navbar-fixed-top" role="navigation">
      <div class="container">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="#">Rails<i>Cafe</i></a>
        </div>
        <div class="navbar-collapse collapse">
          <ul class="nav navbar-nav">
            <li class="active"><a href="#">Home</a></li>
            <li><a href="#about">About</a></li>
            <li><a href="#contact">Contact</a></li>
            <li class="dropdown">
              <a href="#" class="dropdown-toggle" data-toggle="dropdown">Dropdown <b class="caret"></b></a>
              <ul class="dropdown-menu">
                <li><a href="#">Action</a></li>
                <li><a href="#">Another action</a></li>
                <li><a href="#">Something else here</a></li>
                <li class="divider"></li>
                <li class="dropdown-header">Nav header</li>
                <li><a href="#">Separated link</a></li>
                <li><a href="#">One more separated link</a></li>
              </ul>
            </li>
          </ul>
        </div><!--/.nav-collapse -->
      </div>
    </div>

    <div class="container">

      <%= yield %>

    </div> <!-- /container -->
</body>
```

![](./layout/2015-01-30_16-53-09_zps8a496008.webp)

브라우저의 폭을 줄이면 브라우저 상단의 navbar 메뉴가 모바일 화면에 적합하게 자동으로 축소된다.

![](./layout/2015-01-30_16-55-43_zps8cdb3caa.webp)

이 메뉴 아이콘을 클릭하면 감춰진 메뉴항목들이 보인다.

### Twitter-Bootstrap

위에서 언급한 바와 같이 애플리케이션의 레이아웃을 작성하기 위해서는 `HTML`과 `CSS`에 대한 기본적인 지식이 있어야 한다. 이것에 대한 자세한 내용은 이 책의 범위를 벗어나는 것이므로 관련 서적을 참고하기 바란다. 또한 `.css` 파일과 `.scss` 파일의 차이에 대해서도 알아야 하는데 이에 대한 자세한 내용은 [여기](http://stackoverflow.com/a/5654471)를 참고하기 바란다. 애플리케이션 스타일 파일로 사용하는 `application.scss`와 같이 `.scss` 확장로 끝나는 파일은 `Sassy SCSS`라고 말하는데, `CSS3`의 확장기능(태그 중첩, 변수 기능 등)이 추가된 것이다.


### 헬퍼메소드

루트페이지(루트 URL로 접속하면 보이는 페이지, `app/views/welcome/index.html.erb`)에서 게시물(posts)을 작성하는 페이지로 이동하는 링크를 아래와 같이 추가하고 브라우저에서 확인해 보자.

```
<h2>레일스카페<small>에 오신 것을 환영합니다.</small></h2>
<p>: 본 서비스는 "초보자를 위한 레일스 가이드라인"의 샘플 프로젝트입니다.</p>
<br />
<%= link_to "글작성", posts_path, class:'btn btn-default' %>
```

![](./layout/2015-01-30_17-05-54_zps065e8c02.webp)


컨트롤러의 특정 메소드를 뷰 템플릿 파일에서도 사용할 수 있게 해 놓은 것을 [`헬퍼메소드`](http://stackoverflow.com/a/3993323)라고 한다.


위에서 사용한 `link_to` 헬퍼메소드는 링크태그(`<a>`)를 만들어 준다.

```
<%= link_to "글작성", posts_path, class:'btn btn-default' %>
```

위의 `erb` 코드는 렌더링 후 아래와 같이 HTML 코드로 변경된다.

```
<a class="btn btn-default" href="/posts">글작성</a>
```

이제 `글작성` 링크 버튼을 클릭하여 이동해 보자.

> **Note** `Bootstrap 3`는 `모바일 우선` 정책을 지원한다. 따라서 자동으로 `responsive` 또는 `fluid` 페이지 레이아웃이 적용되어 화면크기에 따라 레이아웃이 최적화되어 변경된다. 그러나 실제로 `HTML` 페이지의 `<head></head>` 태그 사이에 `viewport`에 대한 메터데이터를 추가하지 않으면 제대로 적용되지 않는다. 따라서 아래와 같은 메타 데이터를 추가해 주어야 한다.

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0" />
```


---
> **Git소스** https://github.com/rorlakr/rcafe/tree/chapter_05_04
