---
layout: post
title: form submit
categories: [HR_MANAGER]
comments: true
---

###### 문제
- 매니저 기능의 사원 검색 페이지에서 분명 아래의 코드를 추가하여 버튼을 마우스로 클릭하지 않고, 해당 입력칸에서 엔터를 누르면 함수가 실행되도록 만들었으나 아무것도 검색이 되지 않았다.

``` java
 $(document).ready(function (){
      $("#nameOrEmpNo").keyup(function(key) {
          if(key.keyCode === 13) {
              search();
          }
      });
  });
```

- 이유는 jsp파일에 있었다. 해당 파일의 form태그에서 submit이 이루어졌기 때문에 페이지 이동이 된 것이다.

``` java
<form class="form-inline">
    <input type="text" class="form-control" id = "nameOrEmpNo" name="nameOrEmpNo" placeholder="이름 or 사번" />
    <button type="button" class="btn btn-primary" onclick="search()">검색</button>
</form>
```

- form을 사용할 경우 엔터를 누르면 submit이 진행된다.


###### submit 막기
1. form 사용 안 하기
  - 해당 기능 같은 경우, form 태그를 굳이 사용할 필요 없이 div를 사용해도 된다.

``` java
<div class="form-inline">
    <input type="text" class="form-control" id = "nameOrEmpNo" name="nameOrEmpNo" placeholder="이름 or 사번" />
    <button type="button" class="btn btn-primary" onclick="search()">검색</button>
</div>
```


2. onsubmit 사용하기

``` java
<form class="form-inline" onsubmit="return false">
    <input type="text" class="form-control" id = "nameOrEmpNo" name="nameOrEmpNo" placeholder="이름 or 사번" />
    <button type="button" class="btn btn-primary" onclick="search()">검색</button>
</form>
```

- onsubmit : 양식 제출 이벤트가 발생할 때의 동작을 지정 -> 'flase' 자리에 메서드가 오는데, 이 메서드의 반환형이 true면 submit을 진행하고 false면 submit을 하지 않는다. 나는 애초에 submit을 사용할 생각이 없기 때문에 바로 함수 자리에 'false'를 사용했다.
