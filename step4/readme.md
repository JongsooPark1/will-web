## step4

### DB에 test data 넣기

src/main/resources에 import.sql 파일 만들고 아래와 같이 작성

```sql
INSERT INTO USER (USER_ID, PASSWORD, NAME, EMAIL) VALUES ('will33', '1111', 'will', 'will@naver.com');
```

