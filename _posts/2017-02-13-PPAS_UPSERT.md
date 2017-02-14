---
layout: post
title: "WIL : postgresql-upsert "
categories: ["WIL"]
---


1. SQL- upsert 만드는 법

* 기존 오라클에서 사용하던 merge 구문효과를 postgresql 에서 사용하기 위해 UPSERT 문을 만들어 사용한다.


  1)
  ```SQL
  INSERT INTO spider_count (spider, tally) VALUES ('Googlebot', 1);
  ```

  2)
  ```SQL
  UPDATE spider_count SET tally=tally+1 WHERE date='today' AND spider='Googlebot';
  ```

  이와 같은 2개의 구문이 있다고 할 때,

  변환 방법은 다음 처럼 한다.
  $insert, $upsert 라는 변수로 가정하고,
  ```SQL
  $insert = "INSERT INTO spider_count (spider, tally) SELECT 'Googlebot', 1";
  $upsert = "UPDATE spider_count SET tally=tally+1 WHERE date='today' AND spider='Googlebot'";
  ```

### 참고

[- http://www.the-art-of-web.com](http://www.the-art-of-web.com/sql/upsert/)
