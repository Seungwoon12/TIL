# 인덱스(index)
빠른 검색을 위해 항목에 설치하는 도구
<br><br>
primary key는 기본적으로 인덱스가 부여되어 있다.
```
select * from student where student_no = 1;
select * from student where student_name = '홍길동';
```
위의 쿼리중 student_no로 조회하는게 더 빠르다.
<br><br>

## 인덱스 생성하는 법
아래의 쿼리로 index를 생성할 수 있다.
```
create index 인덱스명 on 테이블명(항목);
```
<br>

## 인덱스 리빌딩
-index가 부여된 항목이 수시로 변경될 경우 index 자체의 성능이 느려진다.
<br>
-트리의 깊이가 4가 넘어가면 리빌딩하는 것이 권장된다.(정기적으로 수행)
<br><br>

**인덱스 트리의 깊이 파악하는 법**
```
select
    tablespace_name, table_name, index_name, blevel
from user_indexes
where blevel >= 4;   
```
**인덱스 리빌딩 하는법**
```
alter index 인덱스명 rebuild;
```

