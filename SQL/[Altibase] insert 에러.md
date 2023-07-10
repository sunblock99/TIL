![](https://velog.velcdn.com/images/sunblock99/post/51c9f6ac-cf39-4a1a-87ee-c4a0d9e9b305/image.png)



알티베이스에서 Failed to append record(row: 1001)
Error : # 69720, The row already exists in a unique index
라고 에러 발생하였고 

insert과정에서 PK 설정된 컬럼의 값이 중복될때 나오는 에러다.