<h5>Flyway</h5>
opensource Database Migration Tool

<h2>왜?</h2>
운영 배포를 했는데 NPE가 발생한다! 왜? Dev까지만 DB를 반영하고 Prod에는 반영을 안했다!
이런경우가 심심치 않게 발생한다.

코드는 형상관리툴(git, svn, cvs...)을 통해 변경이력이 관리가 가능한데, db schema는 변경에대한 이력관리 방법이 존재하지 않는다.
flyway는 이런 수동관리요소를 해결해주는 Tool이다.

<h2>동작방식</h2>
![image](https://user-images.githubusercontent.com/28725848/104923439-66128180-59df-11eb-946f-2da8ffcc3d38.png)
(출처 : Link : [https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-flyway-db-%EB%A7%88%EC%9D%B4%EA%B7%B8%EB%A0%88%EC%9D%B4%EC%85%98-tool/]) (이하 동일)

두개의 마이그레이션(db schema변경 sql문이 기록된)을 가지고, 오른쪽의 DB는 비워져있을때,
flyway를 통해 migration을 처리하고자 하면, db에 SCHEMA_VERSION(flyway meta table)이 존재하는지 판단하고, 없다면 flyway가 자동생성.
![image](https://user-images.githubusercontent.com/28725848/104923767-dc16e880-59df-11eb-83ec-1522c88c6ba5.png)

SCHEMA_VERSION 이 생성되거나 존재한다면, flyway는 마이그레이션을 위해 지정된 파일 (sql or java)을 지정된 classpath에서 탐색하여 버전 순서대로 실행한다.
![image](https://user-images.githubusercontent.com/28725848/104924031-39129e80-59e0-11eb-9b60-bf7b2e880cc1.png)

migration이 실행되고 나면, SCHEMA_VERSION table에 실행 이력을 저장하게 된다.
![image](https://user-images.githubusercontent.com/28725848/104924101-521b4f80-59e0-11eb-9176-f7ba5f9ba028.png)

파일명 작성규칙은 다음과 같다.
![image](https://user-images.githubusercontent.com/28725848/104924207-724b0e80-59e0-11eb-959c-ebea40b64c21.png)

prefix : default로 <b>V</b> 는 migration, R은 반복 migration용 접두사이다. V or R로 시작해야만 flyway가 인식한다.
version : version은 version migration에서만 사용되며 숫자와 Dot이나 underscore(언더바) 조합으로 구성. (반복 마이그레이션에서 version을 명시하면 filename제약위반으로 에러발생)
separator : 설명 구분을위한 구분자며, 반드시 __ (언더바 2개)를 써야한다.
description : schema_version테이블 저장시 설명으로 사용된다.
suffix : 확장자 기본은 .sql


....추가..
