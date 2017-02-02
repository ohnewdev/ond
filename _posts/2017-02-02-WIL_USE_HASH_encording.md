---
layout: post
title: "WIL USE_HASH, "
categories: ["WIL"]
---


1. SQL hint USE_HASH   
   [link](http://www.oraclejavanew.kr/bbs/board.php?bo_table=LecHINT&wr_id=110)
  : 테이블 조인시 속도저하로 사용함.

```
[형식]
/*+ USE_HASH ( table [table]... ) */
select /*+ ordered use_hash(큰테이블) */ … from 작은테이블, 큰테이블  

SQL> select /*+ ORDERED USE_HASH(e) */
  2        e.ename,
  3        d.dname
  4  from  mydept1 d, myemp1 e
  5  where  e.deptno = d.deptno  ;
```

2. encording 관련

-  JSP 에서 get 방식으로 한글 전달시 (post는 잘 전달 된다)

보내는 쪽에서

```javascript
  encodeURIComponent( v_text );
```

  받는 쪽은 필요 없음.


-  잘 전달했음에도 불구하고 request 에서 읽지 못한다면,   

```java
  request.setCharacterEncoding("UTF-8");
```

3. jquery 에서 data-info 에 값 지정시 한글 인식 불가
아래와 같이 URI 인코딩을 하던지, 또는 escape() 로 감싸서 처리한다.
```javascript
encodeURIComponent( name_str );
html += '<a href="#" data-info=\'{"name":"' + name_str + '"}\' ><button type="button">버튼</button>';
```
